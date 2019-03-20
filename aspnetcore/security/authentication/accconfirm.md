---
title: Confirmación de la cuenta y la recuperación de contraseñas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo compilar una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.
ms.author: riande
ms.date: 3/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 3bfc2ce46cfbc2ee308940f9e04eb2ffeec09073
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265499"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="4352b-103">Confirmación de la cuenta y la recuperación de contraseñas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4352b-103">Account confirmation and password recovery in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="4352b-104">Consulte [este archivo PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) para el núcleo de ASP.NET 1.1 y versión 2.1.</span><span class="sxs-lookup"><span data-stu-id="4352b-104">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4352b-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="4352b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="4352b-106">Este tutorial muestra cómo crear una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4352b-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="4352b-107">Este tutorial es **no** un tema de principio.</span><span class="sxs-lookup"><span data-stu-id="4352b-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="4352b-108">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="4352b-108">You should be familiar with:</span></span>

* [<span data-ttu-id="4352b-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4352b-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="4352b-110">Autenticación</span><span class="sxs-lookup"><span data-stu-id="4352b-110">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="4352b-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4352b-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="4352b-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="4352b-112">Prerequisites</span></span>

[<span data-ttu-id="4352b-113">SDK de .NET core 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="4352b-113">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="4352b-114">Crear una aplicación web y aplicar la técnica scaffolding identidad</span><span class="sxs-lookup"><span data-stu-id="4352b-114">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="4352b-115">Ejecute los comandos siguientes para crear una aplicación web con la autenticación.</span><span class="sxs-lookup"><span data-stu-id="4352b-115">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="4352b-116">Nuevo registro de usuario de prueba</span><span class="sxs-lookup"><span data-stu-id="4352b-116">Test new user registration</span></span>

<span data-ttu-id="4352b-117">Ejecute la aplicación, seleccione la **registrar** vincular y registrar un usuario.</span><span class="sxs-lookup"><span data-stu-id="4352b-117">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="4352b-118">En este momento, es la única validación en el correo electrónico con el [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo.</span><span class="sxs-lookup"><span data-stu-id="4352b-118">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="4352b-119">Después de enviar el registro, se registran en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4352b-119">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="4352b-120">Más adelante en el tutorial, se actualiza el código para que los nuevos usuarios no pueden iniciar sesión hasta que se valida su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4352b-120">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="4352b-121">Tenga en cuenta la tabla `EmailConfirmed` campo es `False`.</span><span class="sxs-lookup"><span data-stu-id="4352b-121">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="4352b-122">Es posible que desee volver a usar este correo electrónico en el paso siguiente cuando la aplicación envía un correo electrónico de confirmación.</span><span class="sxs-lookup"><span data-stu-id="4352b-122">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="4352b-123">Haga doble clic en la fila y seleccione **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="4352b-123">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="4352b-124">Eliminar el alias de correo electrónico resulta más fácil en los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="4352b-124">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="4352b-125">Requerir confirmación por correo electrónico</span><span class="sxs-lookup"><span data-stu-id="4352b-125">Require email confirmation</span></span>

<span data-ttu-id="4352b-126">Es una práctica recomendada para confirmar el correo electrónico de un nuevo registro de usuario.</span><span class="sxs-lookup"><span data-stu-id="4352b-126">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="4352b-127">Enviar por correo electrónico de confirmación le ayuda a comprobar que no están suplantando persona (es decir, se hayan registrado con el correo electrónico de otra persona).</span><span class="sxs-lookup"><span data-stu-id="4352b-127">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="4352b-128">Supongamos que tuviera un foro de discusión y desea evitar "yli@example.com"al registrar como"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="4352b-128">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="4352b-129">Sin confirmación por correo electrónico, "nolivetto@contoso.com" podría recibir el correo no deseado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4352b-129">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="4352b-130">Supongamos que el usuario registrado accidentalmente como "ylo@example.com" y no se vio el error ortográfico de "yli".</span><span class="sxs-lookup"><span data-stu-id="4352b-130">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="4352b-131">No podrán usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto.</span><span class="sxs-lookup"><span data-stu-id="4352b-131">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="4352b-132">Confirmación por correo electrónico proporciona una protección limitada de bots.</span><span class="sxs-lookup"><span data-stu-id="4352b-132">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="4352b-133">Confirmación por correo electrónico no proporciona protección contra usuarios malintencionados con varias cuentas de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4352b-133">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="4352b-134">Por lo general desea impedir que todos los datos del sitio web de registro antes de que tengan un correo electrónico confirmado nuevos usuarios.</span><span class="sxs-lookup"><span data-stu-id="4352b-134">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="4352b-135">Actualización `Startup.ConfigureServices` para requerir un correo electrónico de confirmación:</span><span class="sxs-lookup"><span data-stu-id="4352b-135">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="4352b-136">`config.SignIn.RequireConfirmedEmail = true;` impide que los usuarios registrados iniciar sesión hasta que se confirme su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4352b-136">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="4352b-137">Configurar el proveedor de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="4352b-137">Configure email provider</span></span>

<span data-ttu-id="4352b-138">En este tutorial, [SendGrid](https://sendgrid.com) se usa para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4352b-138">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="4352b-139">Necesita una cuenta de SendGrid y una clave para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4352b-139">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="4352b-140">Puede usar otros proveedores de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4352b-140">You can use other email providers.</span></span> <span data-ttu-id="4352b-141">ASP.NET Core 2.x incluye `System.Net.Mail`, lo que permite enviar correo electrónico desde la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4352b-141">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="4352b-142">Se recomienda que usar SendGrid u otro servicio de correo electrónico para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4352b-142">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="4352b-143">SMTP es difícil proteger y configurado correctamente.</span><span class="sxs-lookup"><span data-stu-id="4352b-143">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="4352b-144">Cree una clase para recuperar la clave de protección del correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4352b-144">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="4352b-145">Para este ejemplo, crear *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="4352b-145">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="4352b-146">Configurar secretos de usuario de SendGrid</span><span class="sxs-lookup"><span data-stu-id="4352b-146">Configure SendGrid user secrets</span></span>

<span data-ttu-id="4352b-147">Establecer el `SendGridUser` y `SendGridKey` con el [herramienta secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="4352b-147">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="4352b-148">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4352b-148">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="4352b-149">En Windows, Secret Manager almacena los pares de claves/valor en un *secrets.json* de archivos en el `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="4352b-149">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="4352b-150">El contenido de la *secrets.json* archivo no se cifran.</span><span class="sxs-lookup"><span data-stu-id="4352b-150">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="4352b-151">El marcado siguiente se muestra el *secrets.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="4352b-151">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="4352b-152">El `SendGridKey` se ha quitado el valor.</span><span class="sxs-lookup"><span data-stu-id="4352b-152">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="4352b-153">Para obtener más información, consulte el [patrón de opciones](xref:fundamentals/configuration/options) y [configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="4352b-153">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="4352b-154">Instalar SendGrid</span><span class="sxs-lookup"><span data-stu-id="4352b-154">Install SendGrid</span></span>

<span data-ttu-id="4352b-155">Este tutorial muestra cómo agregar notificaciones de correo electrónico a través de [SendGrid](https://sendgrid.com/), pero puede enviar correo electrónico mediante SMTP y otros mecanismos.</span><span class="sxs-lookup"><span data-stu-id="4352b-155">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="4352b-156">Instalar el `SendGrid` paquete NuGet:</span><span class="sxs-lookup"><span data-stu-id="4352b-156">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4352b-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4352b-157">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4352b-158">Desde la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="4352b-158">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4352b-159">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="4352b-159">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4352b-160">En la consola, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="4352b-160">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="4352b-161">Consulte [comience gratis con SendGrid](https://sendgrid.com/free/) para registrar una cuenta gratuita de SendGrid.</span><span class="sxs-lookup"><span data-stu-id="4352b-161">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="4352b-162">Implementar IEmailSender</span><span class="sxs-lookup"><span data-stu-id="4352b-162">Implement IEmailSender</span></span>

<span data-ttu-id="4352b-163">Para implementar `IEmailSender`, crear *Services/EmailSender.cs* con código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="4352b-163">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="4352b-164">Configurar el inicio para admitir correo electrónico</span><span class="sxs-lookup"><span data-stu-id="4352b-164">Configure startup to support email</span></span>

<span data-ttu-id="4352b-165">Agregue el código siguiente a la `ConfigureServices` método en el *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="4352b-165">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="4352b-166">Agregar `EmailSender` como un servicio transitorio.</span><span class="sxs-lookup"><span data-stu-id="4352b-166">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="4352b-167">Registrar el `AuthMessageSenderOptions` instancia de configuración.</span><span class="sxs-lookup"><span data-stu-id="4352b-167">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="4352b-168">Habilitar la recuperación de confirmación y la contraseña de cuenta</span><span class="sxs-lookup"><span data-stu-id="4352b-168">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="4352b-169">La plantilla tiene el código para la recuperación de confirmación y la contraseña de cuenta.</span><span class="sxs-lookup"><span data-stu-id="4352b-169">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="4352b-170">Buscar el `OnPostAsync` método *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="4352b-170">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="4352b-171">Impedir que los usuarios recién registrados que se va a iniciar sesión automáticamente en marcando como comentario la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="4352b-171">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="4352b-172">El método completo se muestra con la línea cambiada resaltada:</span><span class="sxs-lookup"><span data-stu-id="4352b-172">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="4352b-173">Registrar, confirmar correo electrónico y restablecimiento de contraseña</span><span class="sxs-lookup"><span data-stu-id="4352b-173">Register, confirm email, and reset password</span></span>

<span data-ttu-id="4352b-174">Ejecutar la aplicación web y probar el flujo de recuperación de contraseña y confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="4352b-174">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="4352b-175">Ejecute la aplicación y registrar un nuevo usuario</span><span class="sxs-lookup"><span data-stu-id="4352b-175">Run the app and register a new user</span></span>
* <span data-ttu-id="4352b-176">Compruebe su correo electrónico para el vínculo de confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="4352b-176">Check your email for the account confirmation link.</span></span> <span data-ttu-id="4352b-177">Consulte [depurar correo electrónico](#debug) si no recibe el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4352b-177">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="4352b-178">Haga clic en el vínculo para confirmar su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4352b-178">Click the link to confirm your email.</span></span>
* <span data-ttu-id="4352b-179">Inicie sesión con su correo electrónico y contraseña.</span><span class="sxs-lookup"><span data-stu-id="4352b-179">Sign in with your email and password.</span></span>
* <span data-ttu-id="4352b-180">Cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="4352b-180">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="4352b-181">Ver la página de administración</span><span class="sxs-lookup"><span data-stu-id="4352b-181">View the manage page</span></span>

<span data-ttu-id="4352b-182">Seleccione el nombre de usuario en el explorador: ![ventana del explorador con el nombre de usuario](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="4352b-182">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="4352b-183">Se muestra la página de administración con el **perfil** pestaña seleccionada.</span><span class="sxs-lookup"><span data-stu-id="4352b-183">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="4352b-184">El **correo electrónico** muestra una casilla de verificación que indica el correo electrónico se ha confirmado.</span><span class="sxs-lookup"><span data-stu-id="4352b-184">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="4352b-185">Restablecimiento de contraseña de la prueba</span><span class="sxs-lookup"><span data-stu-id="4352b-185">Test password reset</span></span>

* <span data-ttu-id="4352b-186">Si ha iniciado sesión, seleccione **Logout**.</span><span class="sxs-lookup"><span data-stu-id="4352b-186">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="4352b-187">Seleccione el **iniciarla** vínculo y seleccione el **¿olvidó su contraseña?** vínculo.</span><span class="sxs-lookup"><span data-stu-id="4352b-187">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="4352b-188">Escriba el correo electrónico usado para registrar la cuenta.</span><span class="sxs-lookup"><span data-stu-id="4352b-188">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="4352b-189">Se envía un correo electrónico con un vínculo para restablecer su contraseña.</span><span class="sxs-lookup"><span data-stu-id="4352b-189">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="4352b-190">Compruebe su correo electrónico y haga clic en el vínculo para restablecer la contraseña.</span><span class="sxs-lookup"><span data-stu-id="4352b-190">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="4352b-191">Una vez que se ha restablecido correctamente su contraseña, puede iniciar sesión con su correo electrónico y la contraseña nueva.</span><span class="sxs-lookup"><span data-stu-id="4352b-191">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="4352b-192">Cambiar el tiempo de espera de correo electrónico y la actividad</span><span class="sxs-lookup"><span data-stu-id="4352b-192">Change email and activity timeout</span></span>

<span data-ttu-id="4352b-193">El tiempo de espera de inactividad predeterminado es 14 días.</span><span class="sxs-lookup"><span data-stu-id="4352b-193">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="4352b-194">El código siguiente establece el tiempo de espera de inactividad en 5 días:</span><span class="sxs-lookup"><span data-stu-id="4352b-194">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="4352b-195">Cambiar todas las duraciones de token protección de datos</span><span class="sxs-lookup"><span data-stu-id="4352b-195">Change all data protection token lifespans</span></span>

<span data-ttu-id="4352b-196">El código siguiente cambia el tiempo de espera de los tokens de protección de datos todas a 3 horas:</span><span class="sxs-lookup"><span data-stu-id="4352b-196">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="4352b-197">Integrado en tokens de identidad de usuario (consulte [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) tiene un [tiempo de espera de un día](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="4352b-197">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="4352b-198">Cambiar la duración del token de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="4352b-198">Change the email token lifespan</span></span>

<span data-ttu-id="4352b-199">La duración del token predeterminado de [los tokens de identidad de usuario](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) es [un día](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="4352b-199">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="4352b-200">En esta sección se muestra cómo cambiar la duración del token de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4352b-200">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="4352b-201">Agregar un personalizado [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) y <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="4352b-201">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="4352b-202">Agregue el proveedor personalizado para el contenedor de servicios:</span><span class="sxs-lookup"><span data-stu-id="4352b-202">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="4352b-203">Reenviar correo electrónico de confirmación</span><span class="sxs-lookup"><span data-stu-id="4352b-203">Resend email confirmation</span></span>

<span data-ttu-id="4352b-204">Consulte [este problema de GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="4352b-204">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="4352b-205">Depurar el correo electrónico</span><span class="sxs-lookup"><span data-stu-id="4352b-205">Debug email</span></span>

<span data-ttu-id="4352b-206">Si no se puede obtener el trabajo de correo electrónico:</span><span class="sxs-lookup"><span data-stu-id="4352b-206">If you can't get email working:</span></span>

* <span data-ttu-id="4352b-207">Establecer un punto de interrupción en `EmailSender.Execute` comprobar `SendGridClient.SendEmailAsync` se llama.</span><span class="sxs-lookup"><span data-stu-id="4352b-207">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="4352b-208">Crear un [aplicación de consola para enviar correo electrónico](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) mediante código similar a `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="4352b-208">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="4352b-209">Revise el [actividad de correo electrónico](https://sendgrid.com/docs/User_Guide/email_activity.html) página.</span><span class="sxs-lookup"><span data-stu-id="4352b-209">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="4352b-210">Compruebe la carpeta de correo basura.</span><span class="sxs-lookup"><span data-stu-id="4352b-210">Check your spam folder.</span></span>
* <span data-ttu-id="4352b-211">Pruebe otro alias de correo electrónico en un proveedor de correo electrónico diferentes (Microsoft, Yahoo, Gmail, etcetera.)</span><span class="sxs-lookup"><span data-stu-id="4352b-211">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="4352b-212">Pruebe a enviar a las cuentas de correo electrónico diferente.</span><span class="sxs-lookup"><span data-stu-id="4352b-212">Try sending to different email accounts.</span></span>

<span data-ttu-id="4352b-213">**Una práctica recomendada de seguridad** es **no** use secretos de producción en desarrollo y pruebas.</span><span class="sxs-lookup"><span data-stu-id="4352b-213">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="4352b-214">Si publica la aplicación en Azure, puede establecer los secretos de SendGrid como configuración de la aplicación en el portal de Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="4352b-214">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="4352b-215">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="4352b-215">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="4352b-216">Combinar las cuentas de inicio de sesión social y local</span><span class="sxs-lookup"><span data-stu-id="4352b-216">Combine social and local login accounts</span></span>

<span data-ttu-id="4352b-217">Para completar esta sección, primero debe habilitar a un proveedor de autenticación externo.</span><span class="sxs-lookup"><span data-stu-id="4352b-217">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="4352b-218">Consulte [Facebook, Google y la autenticación de proveedor externo](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="4352b-218">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="4352b-219">Puede combinar las cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4352b-219">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="4352b-220">En la siguiente secuencia, "RickAndMSFT@gmail.com" en primer lugar se crea como un inicio de sesión local; sin embargo, puede crear la cuenta como un inicio de sesión social en primer lugar y luego agregar un inicio de sesión local.</span><span class="sxs-lookup"><span data-stu-id="4352b-220">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplicación Web: RickAndMSFT@gmail.com usuario autenticado](accconfirm/_static/rick.png)

<span data-ttu-id="4352b-222">Haga clic en el **administrar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="4352b-222">Click on the **Manage** link.</span></span> <span data-ttu-id="4352b-223">Tenga en cuenta la externa 0 (inicios de sesión sociales) asociado con esta cuenta.</span><span class="sxs-lookup"><span data-stu-id="4352b-223">Note the 0 external (social logins) associated with this account.</span></span>

![Administrar la vista](accconfirm/_static/manage.png)

<span data-ttu-id="4352b-225">Haga clic en el vínculo a otro servicio de inicio de sesión y acepte las solicitudes de aplicación.</span><span class="sxs-lookup"><span data-stu-id="4352b-225">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="4352b-226">En la siguiente imagen, Facebook es el proveedor de autenticación externos:</span><span class="sxs-lookup"><span data-stu-id="4352b-226">In the following image, Facebook is the external authentication provider:</span></span>

![Administrar la vista de inicios de sesión externos listado de Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="4352b-228">Se han combinado las dos cuentas.</span><span class="sxs-lookup"><span data-stu-id="4352b-228">The two accounts have been combined.</span></span> <span data-ttu-id="4352b-229">Es posible iniciar sesión con las cuentas.</span><span class="sxs-lookup"><span data-stu-id="4352b-229">You are able to sign in with either account.</span></span> <span data-ttu-id="4352b-230">Es posible que desee que los usuarios agreguen cuentas locales en caso de su servicio de autenticación de inicio de sesión social esté inactivo o que es más probable hayan perdido el acceso a su cuenta de redes sociales.</span><span class="sxs-lookup"><span data-stu-id="4352b-230">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="4352b-231">Habilitar la confirmación de cuenta después de un sitio tiene usuarios</span><span class="sxs-lookup"><span data-stu-id="4352b-231">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="4352b-232">Habilitar confirmación de la cuenta en un sitio con usuarios bloquea todos los usuarios existentes.</span><span class="sxs-lookup"><span data-stu-id="4352b-232">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="4352b-233">Los usuarios existentes están bloqueados porque sus cuentas no confirmadas.</span><span class="sxs-lookup"><span data-stu-id="4352b-233">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="4352b-234">Para evitar el bloqueo de usuario existente, use uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="4352b-234">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="4352b-235">Actualizar la base de datos para marcar todos los usuarios existentes, como se ha confirmado.</span><span class="sxs-lookup"><span data-stu-id="4352b-235">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="4352b-236">Confirme que los usuarios existentes.</span><span class="sxs-lookup"><span data-stu-id="4352b-236">Confirm existing users.</span></span> <span data-ttu-id="4352b-237">Por ejemplo, envío por lotes de mensajes de correo electrónico con vínculos de confirmación.</span><span class="sxs-lookup"><span data-stu-id="4352b-237">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
