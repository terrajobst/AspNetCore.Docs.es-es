---
title: Confirmación de la cuenta y la recuperación de contraseñas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo compilar una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.
ms.author: riande
ms.date: 7/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 0dc9907f9f54c8a0daf2e05a3769897e5145935f
ms.sourcegitcommit: e418cb9cddeb3de06fa0cb4fdb5529da03ff6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "54444147"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="bf1cd-103">Confirmación de la cuenta y la recuperación de contraseñas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf1cd-103">Account confirmation and password recovery in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="bf1cd-104">Consulte [este archivo PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) para el núcleo de ASP.NET 1.1 y versión 2.1.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-104">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bf1cd-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="bf1cd-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="bf1cd-106">Este tutorial muestra cómo crear una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="bf1cd-107">Este tutorial es **no** un tema de principio.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="bf1cd-108">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-108">You should be familiar with:</span></span>

* [<span data-ttu-id="bf1cd-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf1cd-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="bf1cd-110">Autenticación</span><span class="sxs-lookup"><span data-stu-id="bf1cd-110">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="bf1cd-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="bf1cd-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="bf1cd-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="bf1cd-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="bf1cd-113">Crear una aplicación web y aplicar la técnica scaffolding identidad</span><span class="sxs-lookup"><span data-stu-id="bf1cd-113">Create a web  app and scaffold Identity</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bf1cd-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bf1cd-114">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="bf1cd-115">En Visual Studio, cree un nuevo **aplicación Web** proyecto denominado **WebPWrecover**.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-115">In Visual Studio, create a new **Web Application** project named **WebPWrecover**.</span></span>
* <span data-ttu-id="bf1cd-116">Seleccione **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-116">Select **ASP.NET Core 2.1**.</span></span>
* <span data-ttu-id="bf1cd-117">Mantenga el valor predeterminado **autenticación** establecido en **sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-117">Keep the default **Authentication** set to **No Authentication**.</span></span> <span data-ttu-id="bf1cd-118">La autenticación se agrega en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-118">Authentication is added in the next step.</span></span>

<span data-ttu-id="bf1cd-119">En el paso siguiente:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-119">In the next step:</span></span>

* <span data-ttu-id="bf1cd-120">Establece la página de diseño en *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bf1cd-120">Set the layout page to *~/Pages/Shared/_Layout.cshtml*</span></span>
* <span data-ttu-id="bf1cd-121">Seleccione *cuenta/Register*</span><span class="sxs-lookup"><span data-stu-id="bf1cd-121">Select *Account/Register*</span></span>
* <span data-ttu-id="bf1cd-122">Cree un nuevo **clase de contexto de datos**</span><span class="sxs-lookup"><span data-stu-id="bf1cd-122">Create a new **Data context class**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bf1cd-123">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="bf1cd-123">.NET Core CLI</span></span>](#tab/netcore-cli)

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

<span data-ttu-id="bf1cd-124">Ejecute `dotnet aspnet-codegenerator identity --help` para obtener ayuda acerca de la herramienta de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-124">Run `dotnet aspnet-codegenerator identity --help` to get help on the scaffolding tool.</span></span>

------

<span data-ttu-id="bf1cd-125">Siga las instrucciones de [Habilitar autenticación](xref:security/authentication/scaffold-identity#useauthentication):</span><span class="sxs-lookup"><span data-stu-id="bf1cd-125">Follow the instructions in [Enable authentication](xref:security/authentication/scaffold-identity#useauthentication):</span></span>

* <span data-ttu-id="bf1cd-126">Agregar `app.UseAuthentication();` a `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="bf1cd-126">Add `app.UseAuthentication();` to `Startup.Configure`</span></span>
* <span data-ttu-id="bf1cd-127">Agregar `<partial name="_LoginPartial" />` para el archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-127">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="bf1cd-128">Nuevo registro de usuario de prueba</span><span class="sxs-lookup"><span data-stu-id="bf1cd-128">Test new user registration</span></span>

<span data-ttu-id="bf1cd-129">Ejecute la aplicación, seleccione la **registrar** vincular y registrar un usuario.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="bf1cd-130">En este momento, es la única validación en el correo electrónico con el [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-130">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="bf1cd-131">Después de enviar el registro, se registran en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-131">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="bf1cd-132">Más adelante en el tutorial, se actualiza el código para que los nuevos usuarios no pueden iniciar sesión hasta que se valida su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-132">Later in the tutorial, the code is updated so new users can't log in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="bf1cd-133">Tenga en cuenta la tabla `EmailConfirmed` campo es `False`.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-133">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="bf1cd-134">Es posible que desee volver a usar este correo electrónico en el paso siguiente cuando la aplicación envía un correo electrónico de confirmación.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-134">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="bf1cd-135">Haga doble clic en la fila y seleccione **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-135">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="bf1cd-136">Eliminar el alias de correo electrónico resulta más fácil en los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-136">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="bf1cd-137">Requerir confirmación por correo electrónico</span><span class="sxs-lookup"><span data-stu-id="bf1cd-137">Require email confirmation</span></span>

<span data-ttu-id="bf1cd-138">Es una práctica recomendada para confirmar el correo electrónico de un nuevo registro de usuario.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-138">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="bf1cd-139">Enviar por correo electrónico de confirmación le ayuda a comprobar que no están suplantando persona (es decir, se hayan registrado con el correo electrónico de otra persona).</span><span class="sxs-lookup"><span data-stu-id="bf1cd-139">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="bf1cd-140">Supongamos que tuviera un foro de discusión y desea evitar "yli@example.com"al registrar como"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="bf1cd-140">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="bf1cd-141">Sin confirmación por correo electrónico, "nolivetto@contoso.com" podría recibir el correo no deseado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-141">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="bf1cd-142">Supongamos que el usuario registrado accidentalmente como "ylo@example.com" y no se vio el error ortográfico de "yli".</span><span class="sxs-lookup"><span data-stu-id="bf1cd-142">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="bf1cd-143">No podrán usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-143">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="bf1cd-144">Confirmación por correo electrónico proporciona una protección limitada de bots.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-144">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="bf1cd-145">Confirmación por correo electrónico no proporciona protección contra usuarios malintencionados con varias cuentas de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-145">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="bf1cd-146">Por lo general desea impedir que todos los datos del sitio web de registro antes de que tengan un correo electrónico confirmado nuevos usuarios.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-146">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="bf1cd-147">Actualización *Areas/Identity/IdentityHostingStartup.cs* para requerir un correo electrónico de confirmación:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-147">Update *Areas/Identity/IdentityHostingStartup.cs*  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

<span data-ttu-id="bf1cd-148">`config.SignIn.RequireConfirmedEmail = true;` impide que los usuarios registrados iniciar sesión hasta que se confirme su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-148">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="bf1cd-149">Configurar el proveedor de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="bf1cd-149">Configure email provider</span></span>

<span data-ttu-id="bf1cd-150">En este tutorial, [SendGrid](https://sendgrid.com) se usa para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-150">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="bf1cd-151">Necesita una cuenta de SendGrid y una clave para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-151">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="bf1cd-152">Puede usar otros proveedores de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-152">You can use other email providers.</span></span> <span data-ttu-id="bf1cd-153">ASP.NET Core 2.x incluye `System.Net.Mail`, lo que permite enviar correo electrónico desde la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-153">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="bf1cd-154">Se recomienda que usar SendGrid u otro servicio de correo electrónico para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-154">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="bf1cd-155">SMTP es difícil proteger y configurado correctamente.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-155">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="bf1cd-156">Cree una clase para recuperar la clave de protección del correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-156">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="bf1cd-157">Para este ejemplo, crear *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-157">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="bf1cd-158">Configurar secretos de usuario de SendGrid</span><span class="sxs-lookup"><span data-stu-id="bf1cd-158">Configure SendGrid user secrets</span></span>

<span data-ttu-id="bf1cd-159">Agregar un único `<UserSecretsId>` valor para el `<PropertyGroup>` elemento del archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-159">Add a unique `<UserSecretsId>` value to the `<PropertyGroup>` element of the project file:</span></span>

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

<span data-ttu-id="bf1cd-160">Establecer el `SendGridUser` y `SendGridKey` con el [herramienta secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="bf1cd-160">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="bf1cd-161">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-161">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="bf1cd-162">En Windows, Secret Manager almacena los pares de claves/valor en un *secrets.json* de archivos en el `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-162">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="bf1cd-163">El contenido de la *secrets.json* archivo no se cifran.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-163">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="bf1cd-164">El *secrets.json* archivo se muestra a continuación (el `SendGridKey` se ha quitado el valor.)</span><span class="sxs-lookup"><span data-stu-id="bf1cd-164">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```
 
<span data-ttu-id="bf1cd-165">Para obtener más información, consulte el [patrón de opciones](xref:fundamentals/configuration/options) y [configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="bf1cd-165">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="bf1cd-166">Instalar SendGrid</span><span class="sxs-lookup"><span data-stu-id="bf1cd-166">Install SendGrid</span></span>

<span data-ttu-id="bf1cd-167">Este tutorial muestra cómo agregar notificaciones de correo electrónico a través de [SendGrid](https://sendgrid.com/), pero puede enviar correo electrónico mediante SMTP y otros mecanismos.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-167">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="bf1cd-168">Instalar el `SendGrid` paquete NuGet:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-168">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bf1cd-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bf1cd-169">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="bf1cd-170">Desde la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-170">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bf1cd-171">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="bf1cd-171">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bf1cd-172">En la consola, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-172">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="bf1cd-173">Consulte [comience gratis con SendGrid](https://sendgrid.com/free/) para registrar una cuenta gratuita de SendGrid.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-173">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="bf1cd-174">Implementar IEmailSender</span><span class="sxs-lookup"><span data-stu-id="bf1cd-174">Implement IEmailSender</span></span>

<span data-ttu-id="bf1cd-175">Para implementar `IEmailSender`, crear *Services/EmailSender.cs* con código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-175">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="bf1cd-176">Configurar el inicio para admitir correo electrónico</span><span class="sxs-lookup"><span data-stu-id="bf1cd-176">Configure startup to support email</span></span>

<span data-ttu-id="bf1cd-177">Agregue el código siguiente a la `ConfigureServices` método en el *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-177">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="bf1cd-178">Agregar `EmailSender` como un servicio de singleton.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-178">Add `EmailSender` as a singleton service.</span></span>
* <span data-ttu-id="bf1cd-179">Registrar el `AuthMessageSenderOptions` instancia de configuración.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-179">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="bf1cd-180">Habilitar la recuperación de confirmación y la contraseña de cuenta</span><span class="sxs-lookup"><span data-stu-id="bf1cd-180">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="bf1cd-181">La plantilla tiene el código para la recuperación de confirmación y la contraseña de cuenta.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-181">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="bf1cd-182">Buscar el `OnPostAsync` método *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-182">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="bf1cd-183">Impedir que los usuarios recién registrados que se ha iniciado sesión automáticamente al marcar como comentario la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-183">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="bf1cd-184">El método completo se muestra con la línea cambiada resaltada:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-184">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="bf1cd-185">Registrar, confirmar correo electrónico y restablecimiento de contraseña</span><span class="sxs-lookup"><span data-stu-id="bf1cd-185">Register, confirm email, and reset password</span></span>

<span data-ttu-id="bf1cd-186">Ejecutar la aplicación web y probar el flujo de recuperación de contraseña y confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-186">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="bf1cd-187">Ejecute la aplicación y registrar un nuevo usuario</span><span class="sxs-lookup"><span data-stu-id="bf1cd-187">Run the app and register a new user</span></span>

  ![Vista de cuenta registrar la aplicación Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="bf1cd-189">Compruebe su correo electrónico para el vínculo de confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-189">Check your email for the account confirmation link.</span></span> <span data-ttu-id="bf1cd-190">Consulte [depurar correo electrónico](#debug) si no recibe el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-190">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="bf1cd-191">Haga clic en el vínculo para confirmar su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-191">Click the link to confirm your email.</span></span>
* <span data-ttu-id="bf1cd-192">Inicie sesión con su correo electrónico y contraseña.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-192">Log in with your email and password.</span></span>
* <span data-ttu-id="bf1cd-193">Cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-193">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="bf1cd-194">Ver la página de administración</span><span class="sxs-lookup"><span data-stu-id="bf1cd-194">View the manage page</span></span>

<span data-ttu-id="bf1cd-195">Seleccione el nombre de usuario en el explorador: ![ventana del explorador con el nombre de usuario](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="bf1cd-195">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="bf1cd-196">Es posible que deba expandir la barra de navegación para ver el nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-196">You might need to expand the navbar to see user name.</span></span>

![navbar](accconfirm/_static/x.png)

<span data-ttu-id="bf1cd-198">Se muestra la página de administración con el **perfil** pestaña seleccionada.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-198">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="bf1cd-199">El **correo electrónico** muestra una casilla de verificación que indica el correo electrónico se ha confirmado.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-199">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="bf1cd-200">Restablecimiento de contraseña de la prueba</span><span class="sxs-lookup"><span data-stu-id="bf1cd-200">Test password reset</span></span>

* <span data-ttu-id="bf1cd-201">Si ha iniciado sesión, seleccione **Logout**.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-201">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="bf1cd-202">Seleccione el **iniciarla** vínculo y seleccione el **¿olvidó su contraseña?** vínculo.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-202">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="bf1cd-203">Escriba el correo electrónico usado para registrar la cuenta.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-203">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="bf1cd-204">Se envía un correo electrónico con un vínculo para restablecer su contraseña.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-204">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="bf1cd-205">Compruebe su correo electrónico y haga clic en el vínculo para restablecer la contraseña.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-205">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="bf1cd-206">Después de que se ha restablecido correctamente su contraseña, puede iniciar sesión con su correo electrónico y la nueva contraseña.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-206">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="bf1cd-207">Depurar el correo electrónico</span><span class="sxs-lookup"><span data-stu-id="bf1cd-207">Debug email</span></span>

<span data-ttu-id="bf1cd-208">Si no se puede obtener el trabajo de correo electrónico:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-208">If you can't get email working:</span></span>

* <span data-ttu-id="bf1cd-209">Establecer un punto de interrupción en `EmailSender.Execute` comprobar `SendGridClient.SendEmailAsync` se llama.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-209">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="bf1cd-210">Crear un [aplicación de consola para enviar correo electrónico](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) mediante código similar a `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-210">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="bf1cd-211">Revise el [actividad de correo electrónico](https://sendgrid.com/docs/User_Guide/email_activity.html) página.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-211">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="bf1cd-212">Compruebe la carpeta de correo basura.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-212">Check your spam folder.</span></span>
* <span data-ttu-id="bf1cd-213">Pruebe otro alias de correo electrónico en un proveedor de correo electrónico diferentes (Microsoft, Yahoo, Gmail, etcetera.)</span><span class="sxs-lookup"><span data-stu-id="bf1cd-213">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="bf1cd-214">Pruebe a enviar a las cuentas de correo electrónico diferente.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-214">Try sending to different email accounts.</span></span>

<span data-ttu-id="bf1cd-215">**Una práctica recomendada de seguridad** es **no** use secretos de producción en desarrollo y pruebas.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-215">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="bf1cd-216">Si publica la aplicación en Azure, puede establecer los secretos de SendGrid como configuración de la aplicación en el portal de Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-216">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="bf1cd-217">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-217">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="bf1cd-218">Combinar las cuentas de inicio de sesión social y local</span><span class="sxs-lookup"><span data-stu-id="bf1cd-218">Combine social and local login accounts</span></span>

<span data-ttu-id="bf1cd-219">Para completar esta sección, primero debe habilitar a un proveedor de autenticación externo.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-219">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="bf1cd-220">Consulte [Facebook, Google y la autenticación de proveedor externo](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="bf1cd-220">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="bf1cd-221">Puede combinar las cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-221">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="bf1cd-222">En la siguiente secuencia, "RickAndMSFT@gmail.com" en primer lugar se crea como un inicio de sesión local; sin embargo, puede crear la cuenta como un inicio de sesión social en primer lugar y luego agregar un inicio de sesión local.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-222">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplicación Web: RickAndMSFT@gmail.com usuario autenticado](accconfirm/_static/rick.png)

<span data-ttu-id="bf1cd-224">Haga clic en el **administrar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-224">Click on the **Manage** link.</span></span> <span data-ttu-id="bf1cd-225">Tenga en cuenta la externa 0 (inicios de sesión sociales) asociado con esta cuenta.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-225">Note the 0 external (social logins) associated with this account.</span></span>

![Administrar la vista](accconfirm/_static/manage.png)

<span data-ttu-id="bf1cd-227">Haga clic en el vínculo a otro servicio de inicio de sesión y acepte las solicitudes de aplicación.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-227">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="bf1cd-228">En la siguiente imagen, Facebook es el proveedor de autenticación externos:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-228">In the following image, Facebook is the external authentication provider:</span></span>

![Administrar la vista de inicios de sesión externos listado de Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="bf1cd-230">Se han combinado las dos cuentas.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-230">The two accounts have been combined.</span></span> <span data-ttu-id="bf1cd-231">Es posible iniciar sesión con las cuentas.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-231">You are able to log on with either account.</span></span> <span data-ttu-id="bf1cd-232">Es posible que desee que los usuarios agreguen cuentas locales en caso de su servicio de autenticación de inicio de sesión social esté inactivo o que es más probable hayan perdido el acceso a su cuenta de redes sociales.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-232">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="bf1cd-233">Habilitar la confirmación de cuenta después de un sitio tiene usuarios</span><span class="sxs-lookup"><span data-stu-id="bf1cd-233">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="bf1cd-234">Habilitar confirmación de la cuenta en un sitio con usuarios bloquea todos los usuarios existentes.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-234">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="bf1cd-235">Los usuarios existentes están bloqueados porque sus cuentas no confirmadas.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-235">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="bf1cd-236">Para evitar el bloqueo de usuario existente, use uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="bf1cd-236">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="bf1cd-237">Actualizar la base de datos para marcar todos los usuarios existentes, como se ha confirmado.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-237">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="bf1cd-238">Confirme que los usuarios existentes.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-238">Confirm existing users.</span></span> <span data-ttu-id="bf1cd-239">Por ejemplo, envío por lotes de mensajes de correo electrónico con vínculos de confirmación.</span><span class="sxs-lookup"><span data-stu-id="bf1cd-239">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
