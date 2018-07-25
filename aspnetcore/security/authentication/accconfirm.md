---
title: Confirmación de la cuenta y la recuperación de contraseñas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo compilar una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.
ms.author: riande
ms.date: 7/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 84eb3580107572f66f0c3b565b8e76ba401c0ddb
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219412"
---
::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="863a2-103">Consulte [este archivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para el núcleo de ASP.NET 1.1 y versión 2.1.</span><span class="sxs-lookup"><span data-stu-id="863a2-103">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="863a2-104">Confirmación de la cuenta y la recuperación de contraseñas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="863a2-104">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="863a2-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="863a2-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="863a2-106">Este tutorial muestra cómo crear una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="863a2-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="863a2-107">Este tutorial es **no** un tema de principio.</span><span class="sxs-lookup"><span data-stu-id="863a2-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="863a2-108">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="863a2-108">You should be familiar with:</span></span>

* [<span data-ttu-id="863a2-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="863a2-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="863a2-110">Autenticación</span><span class="sxs-lookup"><span data-stu-id="863a2-110">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="863a2-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="863a2-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="863a2-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="863a2-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="863a2-113">Crear una aplicación web y aplicar la técnica scaffolding identidad</span><span class="sxs-lookup"><span data-stu-id="863a2-113">Create a web  app and scaffold Identity</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="863a2-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="863a2-114">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="863a2-115">En Visual Studio, cree un nuevo **aplicación Web** proyecto denominado **WebPWrecover**.</span><span class="sxs-lookup"><span data-stu-id="863a2-115">In Visual Studio, create a new **Web Application** project named **WebPWrecover**.</span></span>
* <span data-ttu-id="863a2-116">Seleccione **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="863a2-116">Select **ASP.NET Core 2.1**.</span></span>
* <span data-ttu-id="863a2-117">Mantenga el valor predeterminado **autenticación** establecido en **sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="863a2-117">Keep the default **Authentication** set to **No Authentication**.</span></span> <span data-ttu-id="863a2-118">La autenticación se agrega en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="863a2-118">Authentication is added in the next step.</span></span>

<span data-ttu-id="863a2-119">En el paso siguiente:</span><span class="sxs-lookup"><span data-stu-id="863a2-119">In the next step:</span></span>

* <span data-ttu-id="863a2-120">Establece la página de diseño en *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="863a2-120">Set the layout page to *~/Pages/Shared/_Layout.cshtml*</span></span>
* <span data-ttu-id="863a2-121">Seleccione *cuenta/Register*</span><span class="sxs-lookup"><span data-stu-id="863a2-121">Select *Account/Register*</span></span>
* <span data-ttu-id="863a2-122">Cree un nuevo **clase de contexto de datos**</span><span class="sxs-lookup"><span data-stu-id="863a2-122">Create a new **Data context class**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="863a2-123">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="863a2-123">.NET Core CLI</span></span>](#tab/netcore-cli)

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

<span data-ttu-id="863a2-124">Ejecute `dotnet aspnet-codegenerator identity --help` para obtener ayuda acerca de la herramienta de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="863a2-124">Run `dotnet aspnet-codegenerator identity --help` to get help on the scaffolding tool.</span></span>

------

<span data-ttu-id="863a2-125">Siga las instrucciones de [Habilitar autenticación](xref:security/authentication/scaffold-identity#useauthentication):</span><span class="sxs-lookup"><span data-stu-id="863a2-125">Follow the instructions in [Enable authentication](xref:security/authentication/scaffold-identity#useauthentication):</span></span>

* <span data-ttu-id="863a2-126">Agregar `app.UseAuthentication();` a `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="863a2-126">Add `app.UseAuthentication();` to `Startup.Configure`</span></span>
* <span data-ttu-id="863a2-127">Agregar `<partial name="_LoginPartial" />` para el archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="863a2-127">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="863a2-128">Nuevo registro de usuario de prueba</span><span class="sxs-lookup"><span data-stu-id="863a2-128">Test new user registration</span></span>

<span data-ttu-id="863a2-129">Ejecute la aplicación, seleccione la **registrar** vincular y registrar un usuario.</span><span class="sxs-lookup"><span data-stu-id="863a2-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="863a2-130">En este momento, es la única validación en el correo electrónico con el [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo.</span><span class="sxs-lookup"><span data-stu-id="863a2-130">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="863a2-131">Después de enviar el registro, se registran en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="863a2-131">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="863a2-132">Más adelante en el tutorial, se actualiza el código para que los nuevos usuarios no pueden iniciar sesión hasta que se valida su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="863a2-132">Later in the tutorial, the code is updated so new users can't log in until their email is validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="863a2-133">Vista de la base de datos de identidad</span><span class="sxs-lookup"><span data-stu-id="863a2-133">View the Identity database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="863a2-134">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="863a2-134">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="863a2-135">Desde el **vista** menú, seleccione **Explorador de objetos de SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="863a2-135">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="863a2-136">Vaya a **MSSQLLocalDB (13 de SQL Server) (localdb)**.</span><span class="sxs-lookup"><span data-stu-id="863a2-136">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="863a2-137">Haga doble clic en **dbo. AspNetUsers** > **ver datos**:</span><span class="sxs-lookup"><span data-stu-id="863a2-137">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menú contextual de la tabla AspNetUsers en el Explorador de objetos de SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="863a2-139">Tenga en cuenta la tabla `EmailConfirmed` campo es `False`.</span><span class="sxs-lookup"><span data-stu-id="863a2-139">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="863a2-140">Es posible que desee volver a usar este correo electrónico en el paso siguiente cuando la aplicación envía un correo electrónico de confirmación.</span><span class="sxs-lookup"><span data-stu-id="863a2-140">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="863a2-141">Haga doble clic en la fila y seleccione **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="863a2-141">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="863a2-142">Eliminar el alias de correo electrónico resulta más fácil en los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="863a2-142">Deleting the email alias makes it easier in the following steps.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="863a2-143">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="863a2-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="863a2-144">Consulte [trabajar con SQLite en un proyecto de ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) para obtener instrucciones sobre cómo ver la base de datos de SQLite.</span><span class="sxs-lookup"><span data-stu-id="863a2-144">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

------

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="863a2-145">Requerir confirmación por correo electrónico</span><span class="sxs-lookup"><span data-stu-id="863a2-145">Require email confirmation</span></span>

<span data-ttu-id="863a2-146">Es una práctica recomendada para confirmar el correo electrónico de un nuevo registro de usuario.</span><span class="sxs-lookup"><span data-stu-id="863a2-146">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="863a2-147">Enviar por correo electrónico de confirmación le ayuda a comprobar que no están suplantando persona (es decir, se hayan registrado con el correo electrónico de otra persona).</span><span class="sxs-lookup"><span data-stu-id="863a2-147">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="863a2-148">Supongamos que tuviera un foro de discusión y desea evitar "yli@example.com"al registrar como"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="863a2-148">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="863a2-149">Sin confirmación por correo electrónico, "nolivetto@contoso.com" podría recibir el correo no deseado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="863a2-149">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="863a2-150">Supongamos que el usuario registrado accidentalmente como "ylo@example.com" y no se vio el error ortográfico de "yli".</span><span class="sxs-lookup"><span data-stu-id="863a2-150">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="863a2-151">No podrán usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto.</span><span class="sxs-lookup"><span data-stu-id="863a2-151">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="863a2-152">Confirmación por correo electrónico proporciona una protección limitada de bots.</span><span class="sxs-lookup"><span data-stu-id="863a2-152">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="863a2-153">Confirmación por correo electrónico no proporciona protección contra usuarios malintencionados con varias cuentas de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="863a2-153">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="863a2-154">Por lo general desea impedir que todos los datos del sitio web de registro antes de que tengan un correo electrónico confirmado nuevos usuarios.</span><span class="sxs-lookup"><span data-stu-id="863a2-154">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="863a2-155">Actualización *Areas/Identity/IdentityHostingStartup.cs* para requerir un correo electrónico de confirmación:</span><span class="sxs-lookup"><span data-stu-id="863a2-155">Update *Areas/Identity/IdentityHostingStartup.cs*  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

<span data-ttu-id="863a2-156">`config.SignIn.RequireConfirmedEmail = true;` impide que los usuarios registrados iniciar sesión hasta que se confirme su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="863a2-156">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="863a2-157">Configurar el proveedor de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="863a2-157">Configure email provider</span></span>

<span data-ttu-id="863a2-158">En este tutorial, [SendGrid](https://sendgrid.com) se usa para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="863a2-158">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="863a2-159">Necesita una cuenta de SendGrid y una clave para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="863a2-159">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="863a2-160">Puede usar otros proveedores de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="863a2-160">You can use other email providers.</span></span> <span data-ttu-id="863a2-161">ASP.NET Core 2.x incluye `System.Net.Mail`, lo que permite enviar correo electrónico desde la aplicación.</span><span class="sxs-lookup"><span data-stu-id="863a2-161">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="863a2-162">Se recomienda que usar SendGrid u otro servicio de correo electrónico para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="863a2-162">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="863a2-163">SMTP es difícil proteger y configurado correctamente.</span><span class="sxs-lookup"><span data-stu-id="863a2-163">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="863a2-164">El [patrón de opciones](xref:fundamentals/configuration/options) se usa para acceder a la configuración de cuenta y clave de usuario.</span><span class="sxs-lookup"><span data-stu-id="863a2-164">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="863a2-165">Para obtener más información, consulte [configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="863a2-165">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="863a2-166">Cree una clase para recuperar la clave de protección del correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="863a2-166">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="863a2-167">Para este ejemplo, crear *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="863a2-167">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="863a2-168">Configurar secretos de usuario de SendGrid</span><span class="sxs-lookup"><span data-stu-id="863a2-168">Configure SendGrid user secrets</span></span>

<span data-ttu-id="863a2-169">Agregar un único `<UserSecretsId>` valor para el `<PropertyGroup>` elemento del archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="863a2-169">Add a unique `<UserSecretsId>` value to the `<PropertyGroup>` element of the project file:</span></span>

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

<span data-ttu-id="863a2-170">Establecer el `SendGridUser` y `SendGridKey` con el [herramienta secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="863a2-170">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="863a2-171">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="863a2-171">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="863a2-172">En Windows, Secret Manager almacena los pares de claves/valor en un *secrets.json* de archivos en el `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="863a2-172">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="863a2-173">El contenido de la *secrets.json* archivo no se cifran.</span><span class="sxs-lookup"><span data-stu-id="863a2-173">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="863a2-174">El *secrets.json* archivo se muestra a continuación (el `SendGridKey` se ha quitado el valor.)</span><span class="sxs-lookup"><span data-stu-id="863a2-174">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="install-sendgrid"></a><span data-ttu-id="863a2-175">Instalar SendGrid</span><span class="sxs-lookup"><span data-stu-id="863a2-175">Install SendGrid</span></span>

<span data-ttu-id="863a2-176">Este tutorial muestra cómo agregar notificaciones de correo electrónico a través de [SendGrid](https://sendgrid.com/), pero puede enviar correo electrónico mediante SMTP y otros mecanismos.</span><span class="sxs-lookup"><span data-stu-id="863a2-176">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="863a2-177">Instalar el `SendGrid` paquete NuGet:</span><span class="sxs-lookup"><span data-stu-id="863a2-177">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="863a2-178">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="863a2-178">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="863a2-179">Desde la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="863a2-179">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="863a2-180">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="863a2-180">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="863a2-181">En la consola, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="863a2-181">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="863a2-182">Consulte [comience gratis con SendGrid](https://sendgrid.com/free/) para registrar una cuenta gratuita de SendGrid.</span><span class="sxs-lookup"><span data-stu-id="863a2-182">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="863a2-183">Implementar IEmailSender</span><span class="sxs-lookup"><span data-stu-id="863a2-183">Implement IEmailSender</span></span>

<span data-ttu-id="863a2-184">Para implementar `IEmailSender`, crear *Services/EmailSender.cs* con código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="863a2-184">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="863a2-185">Configurar el inicio para admitir correo electrónico</span><span class="sxs-lookup"><span data-stu-id="863a2-185">Configure startup to support email</span></span>

<span data-ttu-id="863a2-186">Agregue el código siguiente a la `ConfigureServices` método en el *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="863a2-186">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="863a2-187">Agregar `EmailSender` como un servicio de singleton.</span><span class="sxs-lookup"><span data-stu-id="863a2-187">Add `EmailSender` as a singleton service.</span></span>
* <span data-ttu-id="863a2-188">Registrar el `AuthMessageSenderOptions` instancia de configuración.</span><span class="sxs-lookup"><span data-stu-id="863a2-188">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="863a2-189">Habilitar la recuperación de confirmación y la contraseña de cuenta</span><span class="sxs-lookup"><span data-stu-id="863a2-189">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="863a2-190">La plantilla tiene el código para la recuperación de confirmación y la contraseña de cuenta.</span><span class="sxs-lookup"><span data-stu-id="863a2-190">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="863a2-191">Buscar el `OnPostAsync` método *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="863a2-191">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="863a2-192">Impedir que los usuarios recién registrados que se ha iniciado sesión automáticamente al marcar como comentario la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="863a2-192">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="863a2-193">El método completo se muestra con la línea cambiada resaltada:</span><span class="sxs-lookup"><span data-stu-id="863a2-193">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="863a2-194">Registrar, confirmar correo electrónico y restablecimiento de contraseña</span><span class="sxs-lookup"><span data-stu-id="863a2-194">Register, confirm email, and reset password</span></span>

<span data-ttu-id="863a2-195">Ejecutar la aplicación web y probar el flujo de recuperación de contraseña y confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="863a2-195">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="863a2-196">Ejecute la aplicación y registrar un nuevo usuario</span><span class="sxs-lookup"><span data-stu-id="863a2-196">Run the app and register a new user</span></span>

  ![Vista de cuenta registrar la aplicación Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="863a2-198">Compruebe su correo electrónico para el vínculo de confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="863a2-198">Check your email for the account confirmation link.</span></span> <span data-ttu-id="863a2-199">Consulte [depurar correo electrónico](#debug) si no recibe el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="863a2-199">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="863a2-200">Haga clic en el vínculo para confirmar su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="863a2-200">Click the link to confirm your email.</span></span>
* <span data-ttu-id="863a2-201">Inicie sesión con su correo electrónico y contraseña.</span><span class="sxs-lookup"><span data-stu-id="863a2-201">Log in with your email and password.</span></span>
* <span data-ttu-id="863a2-202">Cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="863a2-202">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="863a2-203">Ver la página de administración</span><span class="sxs-lookup"><span data-stu-id="863a2-203">View the manage page</span></span>

<span data-ttu-id="863a2-204">Seleccione el nombre de usuario en el explorador: ![ventana del explorador con el nombre de usuario](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="863a2-204">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="863a2-205">Es posible que deba expandir la barra de navegación para ver el nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="863a2-205">You might need to expand the navbar to see user name.</span></span>

![barra de navegación](accconfirm/_static/x.png)

<span data-ttu-id="863a2-207">Se muestra la página de administración con el **perfil** pestaña seleccionada.</span><span class="sxs-lookup"><span data-stu-id="863a2-207">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="863a2-208">El **correo electrónico** muestra una casilla de verificación que indica el correo electrónico se ha confirmado.</span><span class="sxs-lookup"><span data-stu-id="863a2-208">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="863a2-209">Restablecimiento de contraseña de la prueba</span><span class="sxs-lookup"><span data-stu-id="863a2-209">Test password reset</span></span>

* <span data-ttu-id="863a2-210">Si ha iniciado sesión, seleccione **Logout**.</span><span class="sxs-lookup"><span data-stu-id="863a2-210">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="863a2-211">Seleccione el **iniciarla** vínculo y seleccione el **¿olvidó su contraseña?** vínculo.</span><span class="sxs-lookup"><span data-stu-id="863a2-211">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="863a2-212">Escriba el correo electrónico usado para registrar la cuenta.</span><span class="sxs-lookup"><span data-stu-id="863a2-212">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="863a2-213">Se envía un correo electrónico con un vínculo para restablecer su contraseña.</span><span class="sxs-lookup"><span data-stu-id="863a2-213">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="863a2-214">Compruebe su correo electrónico y haga clic en el vínculo para restablecer la contraseña.</span><span class="sxs-lookup"><span data-stu-id="863a2-214">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="863a2-215">Después de que se ha restablecido correctamente su contraseña, puede iniciar sesión con su correo electrónico y la nueva contraseña.</span><span class="sxs-lookup"><span data-stu-id="863a2-215">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="863a2-216">Depurar el correo electrónico</span><span class="sxs-lookup"><span data-stu-id="863a2-216">Debug email</span></span>

<span data-ttu-id="863a2-217">Si no se puede obtener el trabajo de correo electrónico:</span><span class="sxs-lookup"><span data-stu-id="863a2-217">If you can't get email working:</span></span>

* <span data-ttu-id="863a2-218">Establecer un punto de interrupción en `EmailSender.Execute` comprobar `SendGridClient.SendEmailAsync` se llama.</span><span class="sxs-lookup"><span data-stu-id="863a2-218">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="863a2-219">Crear un [aplicación de consola para enviar correo electrónico](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) mediante código similar a `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="863a2-219">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="863a2-220">Revise el [actividad de correo electrónico](https://sendgrid.com/docs/User_Guide/email_activity.html) página.</span><span class="sxs-lookup"><span data-stu-id="863a2-220">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="863a2-221">Compruebe la carpeta de correo basura.</span><span class="sxs-lookup"><span data-stu-id="863a2-221">Check your spam folder.</span></span>
* <span data-ttu-id="863a2-222">Pruebe otro alias de correo electrónico en un proveedor de correo electrónico diferentes (Microsoft, Yahoo, Gmail, etcetera.)</span><span class="sxs-lookup"><span data-stu-id="863a2-222">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="863a2-223">Pruebe a enviar a las cuentas de correo electrónico diferente.</span><span class="sxs-lookup"><span data-stu-id="863a2-223">Try sending to different email accounts.</span></span>

<span data-ttu-id="863a2-224">**Una práctica recomendada de seguridad** es **no** use secretos de producción en desarrollo y pruebas.</span><span class="sxs-lookup"><span data-stu-id="863a2-224">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="863a2-225">Si publica la aplicación en Azure, puede establecer los secretos de SendGrid como configuración de la aplicación en el portal de Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="863a2-225">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="863a2-226">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="863a2-226">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="863a2-227">Combinar las cuentas de inicio de sesión social y local</span><span class="sxs-lookup"><span data-stu-id="863a2-227">Combine social and local login accounts</span></span>

<span data-ttu-id="863a2-228">Para completar esta sección, primero debe habilitar a un proveedor de autenticación externo.</span><span class="sxs-lookup"><span data-stu-id="863a2-228">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="863a2-229">Consulte [Facebook, Google y la autenticación de proveedor externo](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="863a2-229">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="863a2-230">Puede combinar las cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="863a2-230">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="863a2-231">En la siguiente secuencia, "RickAndMSFT@gmail.com" en primer lugar se crea como un inicio de sesión local; sin embargo, puede crear la cuenta como un inicio de sesión social en primer lugar y luego agregar un inicio de sesión local.</span><span class="sxs-lookup"><span data-stu-id="863a2-231">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplicación Web: RickAndMSFT@gmail.com usuario autenticado](accconfirm/_static/rick.png)

<span data-ttu-id="863a2-233">Haga clic en el **administrar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="863a2-233">Click on the **Manage** link.</span></span> <span data-ttu-id="863a2-234">Tenga en cuenta la externa 0 (inicios de sesión sociales) asociado con esta cuenta.</span><span class="sxs-lookup"><span data-stu-id="863a2-234">Note the 0 external (social logins) associated with this account.</span></span>

![Administrar la vista](accconfirm/_static/manage.png)

<span data-ttu-id="863a2-236">Haga clic en el vínculo a otro servicio de inicio de sesión y acepte las solicitudes de aplicación.</span><span class="sxs-lookup"><span data-stu-id="863a2-236">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="863a2-237">En la siguiente imagen, Facebook es el proveedor de autenticación externos:</span><span class="sxs-lookup"><span data-stu-id="863a2-237">In the following image, Facebook is the external authentication provider:</span></span>

![Administrar la vista de inicios de sesión externos listado de Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="863a2-239">Se han combinado las dos cuentas.</span><span class="sxs-lookup"><span data-stu-id="863a2-239">The two accounts have been combined.</span></span> <span data-ttu-id="863a2-240">Es posible iniciar sesión con las cuentas.</span><span class="sxs-lookup"><span data-stu-id="863a2-240">You are able to log on with either account.</span></span> <span data-ttu-id="863a2-241">Es posible que desee que los usuarios agreguen cuentas locales en caso de su servicio de autenticación de inicio de sesión social esté inactivo o que es más probable hayan perdido el acceso a su cuenta de redes sociales.</span><span class="sxs-lookup"><span data-stu-id="863a2-241">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="863a2-242">Habilitar la confirmación de cuenta después de un sitio tiene usuarios</span><span class="sxs-lookup"><span data-stu-id="863a2-242">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="863a2-243">Habilitar confirmación de la cuenta en un sitio con usuarios bloquea todos los usuarios existentes.</span><span class="sxs-lookup"><span data-stu-id="863a2-243">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="863a2-244">Los usuarios existentes están bloqueados porque sus cuentas no confirmadas.</span><span class="sxs-lookup"><span data-stu-id="863a2-244">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="863a2-245">Para evitar el bloqueo de usuario existente, use uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="863a2-245">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="863a2-246">Actualizar la base de datos para marcar todos los usuarios existentes, como se ha confirmado.</span><span class="sxs-lookup"><span data-stu-id="863a2-246">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="863a2-247">Confirme que los usuarios existentes.</span><span class="sxs-lookup"><span data-stu-id="863a2-247">Confirm exiting users.</span></span> <span data-ttu-id="863a2-248">Por ejemplo, envío por lotes de mensajes de correo electrónico con vínculos de confirmación.</span><span class="sxs-lookup"><span data-stu-id="863a2-248">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
