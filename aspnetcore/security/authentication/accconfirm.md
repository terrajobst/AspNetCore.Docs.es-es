---
title: Confirmación de la cuenta y la recuperación de contraseñas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo compilar una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803277"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="49016-103">Confirmación de la cuenta y la recuperación de contraseñas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49016-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="49016-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="49016-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="49016-105">Este tutorial muestra cómo crear una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="49016-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="49016-106">Este tutorial es **no** un tema de principio.</span><span class="sxs-lookup"><span data-stu-id="49016-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="49016-107">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="49016-107">You should be familiar with:</span></span>

* [<span data-ttu-id="49016-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49016-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="49016-109">Autenticación</span><span class="sxs-lookup"><span data-stu-id="49016-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="49016-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="49016-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="49016-111">Consulte [este archivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para las versiones 1.1 de ASP.NET Core MVC y 2.x.</span><span class="sxs-lookup"><span data-stu-id="49016-111">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49016-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="49016-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="49016-113">Cree un nuevo proyecto de ASP.NET Core con la CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="49016-113">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="49016-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="49016-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* <span data-ttu-id="49016-115">`--auth Individual` Especifica la plantilla de proyecto de cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="49016-115">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="49016-116">En Windows, agregue el `-uld` opción.</span><span class="sxs-lookup"><span data-stu-id="49016-116">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="49016-117">Especifica que se debería usar LocalDB en lugar de SQLite.</span><span class="sxs-lookup"><span data-stu-id="49016-117">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="49016-118">Ejecute `new mvc --help` para obtener ayuda sobre este comando.</span><span class="sxs-lookup"><span data-stu-id="49016-118">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="49016-119">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="49016-119">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="49016-120">Si usa la CLI o SQLite, ejecute lo siguiente en una ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="49016-120">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="49016-121">`--auth Individual` Especifica la plantilla de proyecto de cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="49016-121">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="49016-122">En Windows, agregue el `-uld` opción.</span><span class="sxs-lookup"><span data-stu-id="49016-122">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="49016-123">Especifica que se debería usar LocalDB en lugar de SQLite.</span><span class="sxs-lookup"><span data-stu-id="49016-123">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="49016-124">Ejecute `new mvc --help` para obtener ayuda sobre este comando.</span><span class="sxs-lookup"><span data-stu-id="49016-124">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="49016-125">Como alternativa, puede crear un nuevo proyecto de ASP.NET Core con Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="49016-125">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="49016-126">En Visual Studio, cree un nuevo **aplicación Web** proyecto.</span><span class="sxs-lookup"><span data-stu-id="49016-126">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="49016-127">Seleccione **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="49016-127">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="49016-128">**.NET core** está seleccionado en la siguiente imagen, pero puede seleccionar **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="49016-128">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="49016-129">Seleccione **Cambiar autenticación** y establezca en **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="49016-129">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="49016-130">Mantenga el valor predeterminado **en la aplicación de cuentas de usuario de Store**.</span><span class="sxs-lookup"><span data-stu-id="49016-130">Keep the default **Store user accounts in-app**.</span></span>

![Cuadro de diálogo proyecto nuevo que muestra "Radio de cuentas de usuario individuales" seleccionado](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="49016-132">Nuevo registro de usuario de prueba</span><span class="sxs-lookup"><span data-stu-id="49016-132">Test new user registration</span></span>

<span data-ttu-id="49016-133">Ejecute la aplicación, seleccione la **registrar** vincular y registrar un usuario.</span><span class="sxs-lookup"><span data-stu-id="49016-133">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="49016-134">Siga las instrucciones para ejecutar las migraciones de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="49016-134">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="49016-135">En este momento, es la única validación en el correo electrónico con el [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo.</span><span class="sxs-lookup"><span data-stu-id="49016-135">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="49016-136">Después de enviar el registro, se registran en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="49016-136">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="49016-137">Más adelante en el tutorial, el código se actualiza para que nuevos usuarios no pueden iniciar sesión hasta que se ha validado su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="49016-137">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="49016-138">Vista de la base de datos de identidad</span><span class="sxs-lookup"><span data-stu-id="49016-138">View the Identity database</span></span>

<span data-ttu-id="49016-139">Consulte [trabajar con SQLite en un proyecto de ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) para obtener instrucciones sobre cómo ver la base de datos de SQLite.</span><span class="sxs-lookup"><span data-stu-id="49016-139">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="49016-140">Para Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="49016-140">For Visual Studio:</span></span>

* <span data-ttu-id="49016-141">Desde el **vista** menú, seleccione **Explorador de objetos de SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="49016-141">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="49016-142">Vaya a **MSSQLLocalDB (13 de SQL Server) (localdb)**.</span><span class="sxs-lookup"><span data-stu-id="49016-142">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="49016-143">Haga doble clic en **dbo. AspNetUsers** > **ver datos**:</span><span class="sxs-lookup"><span data-stu-id="49016-143">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menú contextual de la tabla AspNetUsers en el Explorador de objetos de SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="49016-145">Tenga en cuenta la tabla `EmailConfirmed` campo es `False`.</span><span class="sxs-lookup"><span data-stu-id="49016-145">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="49016-146">Es posible que desee volver a usar este correo electrónico en el paso siguiente cuando la aplicación envía un correo electrónico de confirmación.</span><span class="sxs-lookup"><span data-stu-id="49016-146">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="49016-147">Haga doble clic en la fila y seleccione **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="49016-147">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="49016-148">Eliminar el alias de correo electrónico resulta más fácil en los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="49016-148">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="49016-149">Requerir HTTPS</span><span class="sxs-lookup"><span data-stu-id="49016-149">Require HTTPS</span></span>

<span data-ttu-id="49016-150">Consulte [requiere HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="49016-150">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="49016-151">Requerir confirmación por correo electrónico</span><span class="sxs-lookup"><span data-stu-id="49016-151">Require email confirmation</span></span>

<span data-ttu-id="49016-152">Es una práctica recomendada para confirmar el correo electrónico de un nuevo registro de usuario.</span><span class="sxs-lookup"><span data-stu-id="49016-152">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="49016-153">Enviar por correo electrónico de confirmación le ayuda a comprobar que no están suplantando persona (es decir, se hayan registrado con el correo electrónico de otra persona).</span><span class="sxs-lookup"><span data-stu-id="49016-153">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="49016-154">Supongamos que tuviera un foro de discusión y desea evitar "yli@example.com"al registrar como"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="49016-154">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="49016-155">Sin confirmación por correo electrónico, "nolivetto@contoso.com" podría recibir el correo no deseado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="49016-155">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="49016-156">Supongamos que el usuario registrado accidentalmente como "ylo@example.com" y no se vio el error ortográfico de "yli".</span><span class="sxs-lookup"><span data-stu-id="49016-156">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="49016-157">No podrán usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto.</span><span class="sxs-lookup"><span data-stu-id="49016-157">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="49016-158">Confirmación por correo electrónico proporciona una protección limitada solo desde los bots.</span><span class="sxs-lookup"><span data-stu-id="49016-158">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="49016-159">Confirmación por correo electrónico no proporciona protección contra usuarios malintencionados con varias cuentas de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="49016-159">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="49016-160">Por lo general desea impedir que todos los datos del sitio web de registro antes de que tengan un correo electrónico confirmado nuevos usuarios.</span><span class="sxs-lookup"><span data-stu-id="49016-160">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="49016-161">Actualización `ConfigureServices` para requerir un correo electrónico de confirmación:</span><span class="sxs-lookup"><span data-stu-id="49016-161">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="49016-162">`config.SignIn.RequireConfirmedEmail = true;` impide que los usuarios registrados iniciar sesión hasta que se confirme su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="49016-162">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="49016-163">Configurar el proveedor de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="49016-163">Configure email provider</span></span>

<span data-ttu-id="49016-164">En este tutorial, se usa SendGrid para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="49016-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="49016-165">Necesita una cuenta de SendGrid y una clave para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="49016-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="49016-166">Puede usar otros proveedores de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="49016-166">You can use other email providers.</span></span> <span data-ttu-id="49016-167">ASP.NET Core 2.x incluye `System.Net.Mail`, lo que permite enviar correo electrónico desde la aplicación.</span><span class="sxs-lookup"><span data-stu-id="49016-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="49016-168">Se recomienda que usar SendGrid u otro servicio de correo electrónico para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="49016-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="49016-169">SMTP es difícil proteger y configurado correctamente.</span><span class="sxs-lookup"><span data-stu-id="49016-169">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="49016-170">El [patrón de opciones](xref:fundamentals/configuration/options) se usa para acceder a la configuración de cuenta y clave de usuario.</span><span class="sxs-lookup"><span data-stu-id="49016-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="49016-171">Para obtener más información, consulte [configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="49016-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="49016-172">Cree una clase para recuperar la clave de protección del correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="49016-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="49016-173">Para este ejemplo, el `AuthMessageSenderOptions` se crea una clase en el *Services/AuthMessageSenderOptions.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="49016-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="49016-174">Establecer el `SendGridUser` y `SendGridKey` con el [herramienta secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="49016-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="49016-175">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="49016-175">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="49016-176">En Windows, Secret Manager almacena los pares de claves/valor en un *secrets.json* de archivos en el `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="49016-176">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="49016-177">El contenido de la *secrets.json* archivo no se cifran.</span><span class="sxs-lookup"><span data-stu-id="49016-177">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="49016-178">El *secrets.json* archivo se muestra a continuación (el `SendGridKey` se ha quitado el valor.)</span><span class="sxs-lookup"><span data-stu-id="49016-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="49016-179">Configurar el inicio para usar AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="49016-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="49016-180">Agregar `AuthMessageSenderOptions` al contenedor de servicios al final de la `ConfigureServices` método en el *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="49016-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="49016-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="49016-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="49016-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="49016-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="49016-183">Configurar la clase AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="49016-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="49016-184">Este tutorial muestra cómo agregar notificaciones de correo electrónico a través de [SendGrid](https://sendgrid.com/), pero puede enviar correo electrónico mediante SMTP y otros mecanismos.</span><span class="sxs-lookup"><span data-stu-id="49016-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="49016-185">Instalar el `SendGrid` paquete NuGet:</span><span class="sxs-lookup"><span data-stu-id="49016-185">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="49016-186">Desde la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="49016-186">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="49016-187">Desde la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="49016-187">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="49016-188">Consulte [comience gratis con SendGrid](https://sendgrid.com/free/) para registrar una cuenta gratuita de SendGrid.</span><span class="sxs-lookup"><span data-stu-id="49016-188">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="49016-189">Configuración de SendGrid</span><span class="sxs-lookup"><span data-stu-id="49016-189">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="49016-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="49016-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="49016-191">Para configurar SendGrid, agregue código similar al siguiente en *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="49016-191">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="49016-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="49016-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="49016-193">Agregue código en *Services/MessageServices.cs* similar a la siguiente configuración de SendGrid:</span><span class="sxs-lookup"><span data-stu-id="49016-193">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="49016-194">Habilitar la recuperación de confirmación y la contraseña de cuenta</span><span class="sxs-lookup"><span data-stu-id="49016-194">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="49016-195">La plantilla tiene el código para la recuperación de confirmación y la contraseña de cuenta.</span><span class="sxs-lookup"><span data-stu-id="49016-195">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="49016-196">Buscar el `OnPostAsync` método *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="49016-196">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="49016-197">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="49016-197">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="49016-198">Impedir que los usuarios recién registrados que se ha iniciado sesión automáticamente al marcar como comentario la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="49016-198">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="49016-199">El método completo se muestra con la línea cambiada resaltada:</span><span class="sxs-lookup"><span data-stu-id="49016-199">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="49016-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="49016-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="49016-201">Para habilitar la confirmación de la cuenta, quite el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="49016-201">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="49016-202">**Nota:** el código impide que un usuario recién registrado que se ha iniciado sesión automáticamente al marcar como comentario la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="49016-202">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="49016-203">Habilitar la recuperación de contraseña al quitar el comentario del código en el `ForgotPassword` acción de *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="49016-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="49016-204">Quite el elemento de formulario en *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="49016-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="49016-205">Es posible que desea quitar el `<p> For more information on how to enable reset password ... </p>` elemento, que contiene un vínculo a este artículo.</span><span class="sxs-lookup"><span data-stu-id="49016-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="49016-206">Registrar, confirmar correo electrónico y restablecimiento de contraseña</span><span class="sxs-lookup"><span data-stu-id="49016-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="49016-207">Ejecutar la aplicación web y probar el flujo de recuperación de contraseña y confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="49016-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="49016-208">Ejecute la aplicación y registrar un nuevo usuario</span><span class="sxs-lookup"><span data-stu-id="49016-208">Run the app and register a new user</span></span>

  ![Vista de cuenta registrar la aplicación Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="49016-210">Compruebe su correo electrónico para el vínculo de confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="49016-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="49016-211">Consulte [depurar correo electrónico](#debug) si no recibe el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="49016-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="49016-212">Haga clic en el vínculo para confirmar su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="49016-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="49016-213">Inicie sesión con su correo electrónico y contraseña.</span><span class="sxs-lookup"><span data-stu-id="49016-213">Log in with your email and password.</span></span>
* <span data-ttu-id="49016-214">Cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="49016-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="49016-215">Ver la página de administración</span><span class="sxs-lookup"><span data-stu-id="49016-215">View the manage page</span></span>

<span data-ttu-id="49016-216">Seleccione el nombre de usuario en el explorador: ![ventana del explorador con el nombre de usuario](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="49016-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="49016-217">Es posible que deba expandir la barra de navegación para ver el nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="49016-217">You might need to expand the navbar to see user name.</span></span>

![barra de navegación](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="49016-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="49016-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="49016-220">Se muestra la página de administración con el **perfil** pestaña seleccionada.</span><span class="sxs-lookup"><span data-stu-id="49016-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="49016-221">El **correo electrónico** muestra una casilla de verificación que indica el correo electrónico se ha confirmado.</span><span class="sxs-lookup"><span data-stu-id="49016-221">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![página de administración](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="49016-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="49016-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="49016-224">Esto se menciona más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="49016-224">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="49016-225">![página de administración](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="49016-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="49016-226">Restablecimiento de contraseña de la prueba</span><span class="sxs-lookup"><span data-stu-id="49016-226">Test password reset</span></span>

* <span data-ttu-id="49016-227">Si ha iniciado sesión, seleccione **Logout**.</span><span class="sxs-lookup"><span data-stu-id="49016-227">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="49016-228">Seleccione el **iniciarla** vínculo y seleccione el **¿olvidó su contraseña?** vínculo.</span><span class="sxs-lookup"><span data-stu-id="49016-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="49016-229">Escriba el correo electrónico usado para registrar la cuenta.</span><span class="sxs-lookup"><span data-stu-id="49016-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="49016-230">Se envía un correo electrónico con un vínculo para restablecer su contraseña.</span><span class="sxs-lookup"><span data-stu-id="49016-230">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="49016-231">Compruebe su correo electrónico y haga clic en el vínculo para restablecer la contraseña.</span><span class="sxs-lookup"><span data-stu-id="49016-231">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="49016-232">Después de que se ha restablecido correctamente su contraseña, puede iniciar sesión con su correo electrónico y la nueva contraseña.</span><span class="sxs-lookup"><span data-stu-id="49016-232">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="49016-233">Depurar el correo electrónico</span><span class="sxs-lookup"><span data-stu-id="49016-233">Debug email</span></span>

<span data-ttu-id="49016-234">Si no se puede obtener el trabajo de correo electrónico:</span><span class="sxs-lookup"><span data-stu-id="49016-234">If you can't get email working:</span></span>

* <span data-ttu-id="49016-235">Crear un [aplicación de consola para enviar correo electrónico](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="49016-235">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="49016-236">Revise el [actividad de correo electrónico](https://sendgrid.com/docs/User_Guide/email_activity.html) página.</span><span class="sxs-lookup"><span data-stu-id="49016-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="49016-237">Compruebe la carpeta de correo basura.</span><span class="sxs-lookup"><span data-stu-id="49016-237">Check your spam folder.</span></span>
* <span data-ttu-id="49016-238">Pruebe otro alias de correo electrónico en un proveedor de correo electrónico diferentes (Microsoft, Yahoo, Gmail, etcetera.)</span><span class="sxs-lookup"><span data-stu-id="49016-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="49016-239">Pruebe a enviar a las cuentas de correo electrónico diferente.</span><span class="sxs-lookup"><span data-stu-id="49016-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="49016-240">**Una práctica recomendada de seguridad** es **no** use secretos de producción en desarrollo y pruebas.</span><span class="sxs-lookup"><span data-stu-id="49016-240">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="49016-241">Si publica la aplicación en Azure, puede establecer los secretos de SendGrid como configuración de la aplicación en el portal de Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="49016-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="49016-242">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="49016-242">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="49016-243">Combinar las cuentas de inicio de sesión social y local</span><span class="sxs-lookup"><span data-stu-id="49016-243">Combine social and local login accounts</span></span>

<span data-ttu-id="49016-244">Para completar esta sección, primero debe habilitar a un proveedor de autenticación externo.</span><span class="sxs-lookup"><span data-stu-id="49016-244">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="49016-245">Consulte [Facebook, Google y la autenticación de proveedor externo](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="49016-245">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="49016-246">Puede combinar las cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="49016-246">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="49016-247">En la siguiente secuencia, "RickAndMSFT@gmail.com" en primer lugar se crea como un inicio de sesión local; sin embargo, puede crear la cuenta como un inicio de sesión social en primer lugar y luego agregar un inicio de sesión local.</span><span class="sxs-lookup"><span data-stu-id="49016-247">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplicación Web: RickAndMSFT@gmail.com usuario autenticado](accconfirm/_static/rick.png)

<span data-ttu-id="49016-249">Haga clic en el **administrar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="49016-249">Click on the **Manage** link.</span></span> <span data-ttu-id="49016-250">Tenga en cuenta la externa 0 (inicios de sesión sociales) asociado con esta cuenta.</span><span class="sxs-lookup"><span data-stu-id="49016-250">Note the 0 external (social logins) associated with this account.</span></span>

![Administrar la vista](accconfirm/_static/manage.png)

<span data-ttu-id="49016-252">Haga clic en el vínculo a otro servicio de inicio de sesión y acepte las solicitudes de aplicación.</span><span class="sxs-lookup"><span data-stu-id="49016-252">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="49016-253">En la siguiente imagen, Facebook es el proveedor de autenticación externos:</span><span class="sxs-lookup"><span data-stu-id="49016-253">In the following image, Facebook is the external authentication provider:</span></span>

![Administrar la vista de inicios de sesión externos listado de Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="49016-255">Se han combinado las dos cuentas.</span><span class="sxs-lookup"><span data-stu-id="49016-255">The two accounts have been combined.</span></span> <span data-ttu-id="49016-256">Es posible iniciar sesión con las cuentas.</span><span class="sxs-lookup"><span data-stu-id="49016-256">You are able to log on with either account.</span></span> <span data-ttu-id="49016-257">Es posible que desee que los usuarios agreguen cuentas locales en caso de su servicio de autenticación de inicio de sesión social esté inactivo o que es más probable hayan perdido el acceso a su cuenta de redes sociales.</span><span class="sxs-lookup"><span data-stu-id="49016-257">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="49016-258">Habilitar la confirmación de cuenta después de un sitio tiene usuarios</span><span class="sxs-lookup"><span data-stu-id="49016-258">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="49016-259">Habilitar confirmación de la cuenta en un sitio con usuarios bloquea todos los usuarios existentes.</span><span class="sxs-lookup"><span data-stu-id="49016-259">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="49016-260">Los usuarios existentes están bloqueados porque sus cuentas no confirmadas.</span><span class="sxs-lookup"><span data-stu-id="49016-260">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="49016-261">Para evitar el bloqueo de usuario existente, use uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="49016-261">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="49016-262">Actualizar la base de datos para marcar todos los usuarios existentes, como se ha confirmado.</span><span class="sxs-lookup"><span data-stu-id="49016-262">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="49016-263">Confirme que los usuarios existentes.</span><span class="sxs-lookup"><span data-stu-id="49016-263">Confirm exiting users.</span></span> <span data-ttu-id="49016-264">Por ejemplo, envío por lotes de mensajes de correo electrónico con vínculos de confirmación.</span><span class="sxs-lookup"><span data-stu-id="49016-264">For example, batch-send emails with confirmation links.</span></span>
