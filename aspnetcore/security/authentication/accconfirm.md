---
title: "Confirmación de la cuenta y la recuperación de contraseña en el núcleo de ASP.NET"
author: rick-anderson
description: "Obtener información sobre cómo compilar una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico."
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: e8f73d58bdf626910b2101ef310385f588315e26
ms.sourcegitcommit: 725cb18ad23013e15d3dbb527958481dee79f9f8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="8b883-103">Confirmación de la cuenta y la recuperación de contraseña en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b883-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="8b883-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="8b883-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="8b883-105">Este tutorial muestra cómo compilar una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="8b883-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="8b883-106">Este tutorial es **no** un tema de principio.</span><span class="sxs-lookup"><span data-stu-id="8b883-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="8b883-107">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="8b883-107">You should be familiar with:</span></span>

* [<span data-ttu-id="8b883-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b883-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="8b883-109">Autenticación</span><span class="sxs-lookup"><span data-stu-id="8b883-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="8b883-110">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="8b883-110">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="8b883-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="8b883-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="8b883-112">Vea [este archivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para las versiones 1.1 de MVC de ASP.NET Core y 2.x.</span><span class="sxs-lookup"><span data-stu-id="8b883-112">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8b883-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="8b883-113">Prerequisites</span></span>

<span data-ttu-id="8b883-114">[.NET core 2.1.4 SDK](https://www.microsoft.com/net/core) o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="8b883-114">[.NET Core 2.1.4 SDK](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="8b883-115">Cree un nuevo proyecto de ASP.NET Core con la CLI de núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="8b883-115">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8b883-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8b883-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

* <span data-ttu-id="8b883-117">`--auth Individual` Especifica la plantilla de proyecto de cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="8b883-117">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="8b883-118">En Windows, agregue el `-uld` opción.</span><span class="sxs-lookup"><span data-stu-id="8b883-118">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="8b883-119">Especifica que LocalDB debe usarse en lugar de SQLite.</span><span class="sxs-lookup"><span data-stu-id="8b883-119">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="8b883-120">Ejecute `new mvc --help` para obtener ayuda sobre este comando.</span><span class="sxs-lookup"><span data-stu-id="8b883-120">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8b883-121">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8b883-121">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8b883-122">Si usa la CLI o SQLite, ejecute lo siguiente en una ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="8b883-122">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="8b883-123">`--auth Individual` Especifica la plantilla de proyecto de cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="8b883-123">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="8b883-124">En Windows, agregue el `-uld` opción.</span><span class="sxs-lookup"><span data-stu-id="8b883-124">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="8b883-125">Especifica que LocalDB debe usarse en lugar de SQLite.</span><span class="sxs-lookup"><span data-stu-id="8b883-125">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="8b883-126">Ejecute `new mvc --help` para obtener ayuda sobre este comando.</span><span class="sxs-lookup"><span data-stu-id="8b883-126">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="8b883-127">Como alternativa, puede crear un nuevo proyecto de ASP.NET Core con Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="8b883-127">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="8b883-128">En Visual Studio, cree un nuevo **aplicación Web** proyecto.</span><span class="sxs-lookup"><span data-stu-id="8b883-128">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="8b883-129">Seleccione **principales de ASP.NET 2.0**.</span><span class="sxs-lookup"><span data-stu-id="8b883-129">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="8b883-130">**.NET core** está seleccionado en la siguiente imagen, pero puede seleccionar **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="8b883-130">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="8b883-131">Seleccione **Cambiar autenticación** y establezca en **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="8b883-131">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="8b883-132">Mantenga el valor predeterminado **en la aplicación de cuentas de usuario de almacén**.</span><span class="sxs-lookup"><span data-stu-id="8b883-132">Keep the default **Store user accounts in-app**.</span></span>

![Cuadro de diálogo proyecto nuevo que muestra "Radio de cuentas de usuario individuales" seleccionado](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="8b883-134">Probar el nuevo registro de usuario</span><span class="sxs-lookup"><span data-stu-id="8b883-134">Test new user registration</span></span>

<span data-ttu-id="8b883-135">Ejecutar la aplicación, seleccione la **registrar** vincular y registrar un usuario.</span><span class="sxs-lookup"><span data-stu-id="8b883-135">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="8b883-136">Siga las instrucciones para ejecutar migraciones de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="8b883-136">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="8b883-137">En este punto, es la única validación en el correo electrónico con el [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo.</span><span class="sxs-lookup"><span data-stu-id="8b883-137">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="8b883-138">Después de enviar el registro, se registran en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8b883-138">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="8b883-139">Más adelante en el tutorial, el código se actualiza para que los usuarios nuevos no se pueden iniciar sesión hasta que se ha validado el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="8b883-139">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="8b883-140">Vista de la base de datos de identidad</span><span class="sxs-lookup"><span data-stu-id="8b883-140">View the Identity database</span></span>

<span data-ttu-id="8b883-141">Vea [trabajar con código en un proyecto de MVC de ASP.NET Core](xref:tutorials/first-mvc-app-xplat/working-with-sql) para obtener instrucciones sobre cómo ver la base de datos de SQLite.</span><span class="sxs-lookup"><span data-stu-id="8b883-141">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="8b883-142">Para Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="8b883-142">For Visual Studio:</span></span>

* <span data-ttu-id="8b883-143">Desde el **vista** menú, seleccione **Explorador de objetos de SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="8b883-143">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="8b883-144">Vaya a **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="8b883-144">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="8b883-145">Haga doble clic en **dbo. AspNetUsers** > **ver datos**:</span><span class="sxs-lookup"><span data-stu-id="8b883-145">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menú contextual en la tabla de AspNetUsers en el Explorador de objetos de SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="8b883-147">Tenga en cuenta la tabla `EmailConfirmed` campo es `False`.</span><span class="sxs-lookup"><span data-stu-id="8b883-147">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="8b883-148">Puede volver a usar este correo electrónico en el paso siguiente cuando la aplicación envía un correo electrónico de confirmación.</span><span class="sxs-lookup"><span data-stu-id="8b883-148">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="8b883-149">Haga doble clic en la fila y seleccione **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="8b883-149">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="8b883-150">Eliminar el alias de correo electrónico facilita en los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="8b883-150">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="8b883-151">Requerir HTTPS</span><span class="sxs-lookup"><span data-stu-id="8b883-151">Require HTTPS</span></span>

<span data-ttu-id="8b883-152">Vea [requieren HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="8b883-152">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="8b883-153">Requerir confirmación por correo electrónico</span><span class="sxs-lookup"><span data-stu-id="8b883-153">Require email confirmation</span></span>

<span data-ttu-id="8b883-154">Es una práctica recomendada para confirmar el correo electrónico de un nuevo registro de usuario.</span><span class="sxs-lookup"><span data-stu-id="8b883-154">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="8b883-155">Enviar por correo electrónico de confirmación le ayuda a comprobar que no están suplantando otra persona (es decir, no ha registrado con el correo electrónico de otra persona).</span><span class="sxs-lookup"><span data-stu-id="8b883-155">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="8b883-156">Imagine que tuviera un foro de discusión, y desea evitar "yli@example.com"al registrar como"nolivetto@contoso.com."</span><span class="sxs-lookup"><span data-stu-id="8b883-156">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="8b883-157">Sin confirmación por correo electrónico, "nolivetto@contoso.com" podría recibir correos electrónicos no deseados de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8b883-157">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="8b883-158">Suponga que el usuario registrado accidentalmente como "ylo@example.com" y no se vio el error ortográfico de "yli".</span><span class="sxs-lookup"><span data-stu-id="8b883-158">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="8b883-159">No podrían usar recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto.</span><span class="sxs-lookup"><span data-stu-id="8b883-159">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="8b883-160">Confirmación por correo electrónico proporciona sólo una protección limitada de robots.</span><span class="sxs-lookup"><span data-stu-id="8b883-160">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="8b883-161">Confirmación por correo electrónico no proporciona protección de los usuarios malintencionados con varias cuentas de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="8b883-161">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="8b883-162">En general conveniente evitar que los nuevos usuarios se registren todos los datos del sitio web antes de que tengan un correo electrónico confirmado.</span><span class="sxs-lookup"><span data-stu-id="8b883-162">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="8b883-163">Actualización `ConfigureServices` para requerir un correo electrónico confirmadas:</span><span class="sxs-lookup"><span data-stu-id="8b883-163">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="8b883-164">`config.SignIn.RequireConfirmedEmail = true;` impide que los usuarios registrados el registro hasta que se ha confirmado el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="8b883-164">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="8b883-165">Configurar el proveedor de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="8b883-165">Configure email provider</span></span>

<span data-ttu-id="8b883-166">En este tutorial, se usa SendGrid para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="8b883-166">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="8b883-167">Necesita una cuenta de SendGrid y la clave para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="8b883-167">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="8b883-168">Puede usar otros proveedores de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="8b883-168">You can use other email providers.</span></span> <span data-ttu-id="8b883-169">ASP.NET Core 2.x incluye `System.Net.Mail`, que le permite enviar correo electrónico desde la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8b883-169">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="8b883-170">Se recomienda que usar SendGrid u otro servicio de correo electrónico para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="8b883-170">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="8b883-171">SMTP es difícil proteger y configurado correctamente.</span><span class="sxs-lookup"><span data-stu-id="8b883-171">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="8b883-172">El [patrón opciones](xref:fundamentals/configuration/options) se usa para acceder a la configuración de cuenta y clave de usuario.</span><span class="sxs-lookup"><span data-stu-id="8b883-172">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="8b883-173">Para obtener más información, consulte [configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="8b883-173">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="8b883-174">Cree una clase para obtener la clave de proteger el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="8b883-174">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="8b883-175">En este ejemplo, el `AuthMessageSenderOptions` se crea una clase en el *Services/AuthMessageSenderOptions.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="8b883-175">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="8b883-176">Establecer el `SendGridUser` y `SendGridKey` con el [herramienta Administrador de secreto](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="8b883-176">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="8b883-177">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8b883-177">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="8b883-178">En Windows, el Administrador de secreto almacena pares de claves/valor en un *secrets.json* un archivo en el `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="8b883-178">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="8b883-179">El contenido de la *secrets.json* archivos no están cifrados.</span><span class="sxs-lookup"><span data-stu-id="8b883-179">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="8b883-180">El *secrets.json* archivo se muestra a continuación (el `SendGridKey` se ha quitado el valor.)</span><span class="sxs-lookup"><span data-stu-id="8b883-180">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="8b883-181">Configurar el inicio para usar AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="8b883-181">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="8b883-182">Agregar `AuthMessageSenderOptions` al contenedor de servicios al final de la `ConfigureServices` método en el *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="8b883-182">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8b883-183">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8b883-183">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8b883-184">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8b883-184">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="8b883-185">Configurar la clase de AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="8b883-185">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="8b883-186">Este tutorial muestra cómo agregar notificaciones de correo electrónico a través de [SendGrid](https://sendgrid.com/), pero puede enviar correo electrónico mediante SMTP y otros mecanismos.</span><span class="sxs-lookup"><span data-stu-id="8b883-186">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="8b883-187">Instalar el `SendGrid` paquete NuGet:</span><span class="sxs-lookup"><span data-stu-id="8b883-187">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="8b883-188">Desde la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="8b883-188">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="8b883-189">Desde la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8b883-189">From the Package Manager Console, enter the following command:</span></span>

 `Install-Package SendGrid`

<span data-ttu-id="8b883-190">Vea [empiece de forma gratuita con SendGrid](https://sendgrid.com/free/) para registrar una cuenta gratuita de SendGrid.</span><span class="sxs-lookup"><span data-stu-id="8b883-190">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="8b883-191">Configurar SendGrid</span><span class="sxs-lookup"><span data-stu-id="8b883-191">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8b883-192">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8b883-192">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8b883-193">Para configurar SendGrid, agregue código similar al siguiente en *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="8b883-193">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8b883-194">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8b883-194">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="8b883-195">Agregue código en *Services/MessageServices.cs* similar al siguiente para configurar SendGrid:</span><span class="sxs-lookup"><span data-stu-id="8b883-195">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="8b883-196">Habilitar la recuperación de confirmación y la contraseña de cuenta</span><span class="sxs-lookup"><span data-stu-id="8b883-196">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="8b883-197">La plantilla tiene el código para la recuperación de confirmación y la contraseña de cuenta.</span><span class="sxs-lookup"><span data-stu-id="8b883-197">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="8b883-198">Buscar el `OnPostAsync` método *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="8b883-198">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8b883-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8b883-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8b883-200">Impedir que los usuarios recién registrados que se iniciará automáticamente la sesión como comentario la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="8b883-200">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="8b883-201">El método completo se muestra con la línea cambiada resaltada:</span><span class="sxs-lookup"><span data-stu-id="8b883-201">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8b883-202">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8b883-202">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8b883-203">Para habilitar la confirmación de la cuenta, quite el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="8b883-203">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="8b883-204">**Nota:** el código impide que un usuario recién registrado que se iniciará automáticamente la sesión como comentario la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="8b883-204">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="8b883-205">Habilitar la recuperación de contraseña por el comentario de código en el `ForgotPassword` acción de *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="8b883-205">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="8b883-206">Quite el elemento de formulario en *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8b883-206">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="8b883-207">Desea quitar el `<p> For more information on how to enable reset password ... </p>` elemento, que contiene un vínculo a este artículo.</span><span class="sxs-lookup"><span data-stu-id="8b883-207">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="8b883-208">Registrar, confirma el correo electrónico y restablecer contraseña</span><span class="sxs-lookup"><span data-stu-id="8b883-208">Register, confirm email, and reset password</span></span>

<span data-ttu-id="8b883-209">Ejecutar la aplicación web y probar la confirmación de la cuenta y el flujo de recuperación de contraseña.</span><span class="sxs-lookup"><span data-stu-id="8b883-209">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="8b883-210">Ejecutar la aplicación y registrar un nuevo usuario</span><span class="sxs-lookup"><span data-stu-id="8b883-210">Run the app and register a new user</span></span>

 ![Vista de cuenta registrar la aplicación Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="8b883-212">Compruebe su correo electrónico para el vínculo de confirmación de cuenta.</span><span class="sxs-lookup"><span data-stu-id="8b883-212">Check your email for the account confirmation link.</span></span> <span data-ttu-id="8b883-213">Vea [depurar correo electrónico](#debug) si no recibe el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="8b883-213">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="8b883-214">Haga clic en el vínculo para confirmar tu correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="8b883-214">Click the link to confirm your email.</span></span>
* <span data-ttu-id="8b883-215">Inicie sesión con su correo electrónico y contraseña.</span><span class="sxs-lookup"><span data-stu-id="8b883-215">Log in with your email and password.</span></span>
* <span data-ttu-id="8b883-216">Cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="8b883-216">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="8b883-217">Ver la página de administración</span><span class="sxs-lookup"><span data-stu-id="8b883-217">View the manage page</span></span>

<span data-ttu-id="8b883-218">Seleccione el nombre de usuario en el explorador: ![ventana del explorador con el nombre de usuario](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="8b883-218">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="8b883-219">Debe expandir la barra de navegación para ver el nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="8b883-219">You might need to expand the navbar to see user name.</span></span>

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8b883-221">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8b883-221">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8b883-222">Se muestra la página de administración con el **perfil** pestaña seleccionada.</span><span class="sxs-lookup"><span data-stu-id="8b883-222">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="8b883-223">El **correo electrónico** muestra una casilla de verificación que indica el correo electrónico se ha confirmado.</span><span class="sxs-lookup"><span data-stu-id="8b883-223">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![página de administración](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8b883-225">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8b883-225">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8b883-226">Esto se menciona más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="8b883-226">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="8b883-227">![página de administración](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="8b883-227">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="8b883-228">Restablecimiento de contraseña de prueba</span><span class="sxs-lookup"><span data-stu-id="8b883-228">Test password reset</span></span>

* <span data-ttu-id="8b883-229">Si ha iniciado sesión, seleccione **Logout**.</span><span class="sxs-lookup"><span data-stu-id="8b883-229">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="8b883-230">Seleccione el **sesión** de vínculo y seleccione el **¿olvidó su contraseña?** vínculo.</span><span class="sxs-lookup"><span data-stu-id="8b883-230">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="8b883-231">Escriba el correo electrónico que usa para registrar la cuenta.</span><span class="sxs-lookup"><span data-stu-id="8b883-231">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="8b883-232">Se envía un correo electrónico con un vínculo para restablecer su contraseña.</span><span class="sxs-lookup"><span data-stu-id="8b883-232">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="8b883-233">Compruebe su correo electrónico y haga clic en el vínculo para restablecer la contraseña.</span><span class="sxs-lookup"><span data-stu-id="8b883-233">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="8b883-234">Después de que la contraseña se restableció correctamente, puede iniciar sesión con su correo electrónico y la contraseña nueva.</span><span class="sxs-lookup"><span data-stu-id="8b883-234">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="8b883-235">Depurar el correo electrónico</span><span class="sxs-lookup"><span data-stu-id="8b883-235">Debug email</span></span>

<span data-ttu-id="8b883-236">Si no se puede obtener el trabajo de correo electrónico:</span><span class="sxs-lookup"><span data-stu-id="8b883-236">If you can't get email working:</span></span>

* <span data-ttu-id="8b883-237">Crear un [aplicación de consola para enviar correo electrónico](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="8b883-237">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="8b883-238">Revise el [actividad de correo electrónico](https://sendgrid.com/docs/User_Guide/email_activity.html) página.</span><span class="sxs-lookup"><span data-stu-id="8b883-238">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="8b883-239">Compruebe la carpeta de correo basura.</span><span class="sxs-lookup"><span data-stu-id="8b883-239">Check your spam folder.</span></span>
* <span data-ttu-id="8b883-240">Pruebe otro alias de correo electrónico en un proveedor de correo electrónico diferente (Microsoft, Yahoo, Gmail, etcetera.)</span><span class="sxs-lookup"><span data-stu-id="8b883-240">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="8b883-241">Vuelva a enviar a las cuentas de correo electrónico diferente.</span><span class="sxs-lookup"><span data-stu-id="8b883-241">Try sending to different email accounts.</span></span>

<span data-ttu-id="8b883-242">**Una práctica recomendada de seguridad** es **no** utilice secretos de producción en pruebas y desarrollo.</span><span class="sxs-lookup"><span data-stu-id="8b883-242">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="8b883-243">Si publica la aplicación en Azure, puede establecer los secretos de SendGrid como configuración de la aplicación en el portal de la aplicación Web de Azure.</span><span class="sxs-lookup"><span data-stu-id="8b883-243">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="8b883-244">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="8b883-244">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="8b883-245">Combinar las cuentas de inicio de sesión locales y redes sociales</span><span class="sxs-lookup"><span data-stu-id="8b883-245">Combine social and local login accounts</span></span>

<span data-ttu-id="8b883-246">Para completar esta sección, primero debe habilitar a un proveedor de autenticación externo.</span><span class="sxs-lookup"><span data-stu-id="8b883-246">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="8b883-247">Vea [habilitar la autenticación con Facebook, Google y otros proveedores externos](social/index.md).</span><span class="sxs-lookup"><span data-stu-id="8b883-247">See [Enabling authentication using Facebook, Google, and other external providers](social/index.md).</span></span>

<span data-ttu-id="8b883-248">Puede combinar cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="8b883-248">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="8b883-249">En la siguiente secuencia, "RickAndMSFT@gmail.com" en primer lugar se crea como un inicio de sesión local; sin embargo, puede crear la cuenta como un inicio de sesión social primero y luego agregar un inicio de sesión local.</span><span class="sxs-lookup"><span data-stu-id="8b883-249">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplicación Web: RickAndMSFT@gmail.com usuario autenticado](accconfirm/_static/rick.png)

<span data-ttu-id="8b883-251">Haga clic en el **administrar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="8b883-251">Click on the **Manage** link.</span></span> <span data-ttu-id="8b883-252">Tenga en cuenta el externo 0 (inicios de sesión sociales) asociados con esta cuenta.</span><span class="sxs-lookup"><span data-stu-id="8b883-252">Note the 0 external (social logins) associated with this account.</span></span>

![Administrar la vista](accconfirm/_static/manage.png)

<span data-ttu-id="8b883-254">Haga clic en el vínculo a otro servicio de inicio de sesión y Aceptar las solicitudes de aplicación.</span><span class="sxs-lookup"><span data-stu-id="8b883-254">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="8b883-255">En la siguiente imagen, Facebook es el proveedor de autenticación externos:</span><span class="sxs-lookup"><span data-stu-id="8b883-255">In the following image, Facebook is the external authentication provider:</span></span>

![Administrar la vista de inicios de sesión externos enumerar Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="8b883-257">Se han combinado las dos cuentas.</span><span class="sxs-lookup"><span data-stu-id="8b883-257">The two accounts have been combined.</span></span> <span data-ttu-id="8b883-258">Es posible iniciar sesión con cualquiera de estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="8b883-258">You are able to log on with either account.</span></span> <span data-ttu-id="8b883-259">Puede que los usuarios agreguen cuentas locales en caso de que su servicio de autenticación de inicio de sesión social está inactivo o, más probablemente ha perdido acceso a su cuenta sociales.</span><span class="sxs-lookup"><span data-stu-id="8b883-259">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="8b883-260">Habilitar la confirmación de la cuenta después de un sitio tiene usuarios</span><span class="sxs-lookup"><span data-stu-id="8b883-260">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="8b883-261">Habilitar confirmación de la cuenta en un sitio con usuarios bloquea todos los usuarios existentes.</span><span class="sxs-lookup"><span data-stu-id="8b883-261">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="8b883-262">No tienen acceso a los usuarios existentes porque no se ha confirmado sus cuentas.</span><span class="sxs-lookup"><span data-stu-id="8b883-262">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="8b883-263">Para evitar salir del bloqueo de usuario, use uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="8b883-263">To work around exiting user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="8b883-264">Actualizar la base de datos para marcar todos los usuarios existentes como está confirmado</span><span class="sxs-lookup"><span data-stu-id="8b883-264">Update the database to mark all existing users as being confirmed</span></span>
* <span data-ttu-id="8b883-265">Confirme que los usuarios existentes.</span><span class="sxs-lookup"><span data-stu-id="8b883-265">Confirm exiting users.</span></span> <span data-ttu-id="8b883-266">Por ejemplo, lote enviar correos electrónicos con vínculos de confirmación.</span><span class="sxs-lookup"><span data-stu-id="8b883-266">For example, batch-send emails with confirmation links.</span></span>
