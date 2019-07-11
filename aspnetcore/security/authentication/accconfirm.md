---
title: Confirmación de la cuenta y la recuperación de contraseñas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo compilar una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.
ms.author: riande
ms.date: 03/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 802ba446af04df6a35ac73187ad693b8ec80c654
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814848"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="f7b0c-103">Confirmación de la cuenta y la recuperación de contraseñas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7b0c-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="f7b0c-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), y [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="f7b0c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="f7b0c-105">Este tutorial muestra cómo crear una aplicación de ASP.NET Core con el restablecimiento de confirmación y la contraseña de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-105">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="f7b0c-106">Este tutorial es **no** un tema de principio.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="f7b0c-107">Debe estar familiarizado con:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-107">You should be familiar with:</span></span>

* [<span data-ttu-id="f7b0c-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7b0c-108">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="f7b0c-109">Autenticación</span><span class="sxs-lookup"><span data-stu-id="f7b0c-109">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="f7b0c-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f7b0c-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="f7b0c-111">Consulte [este archivo PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) para la versión 1.1 de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-111">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 version.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="f7b0c-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f7b0c-112">Prerequisites</span></span>

[<span data-ttu-id="f7b0c-113">SDK de .NET core 3.0 o posterior</span><span class="sxs-lookup"><span data-stu-id="f7b0c-113">.NET Core 3.0 SDK or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## <a name="create-and-test-a-web-app-with-authentication"></a><span data-ttu-id="f7b0c-114">Crear y probar una aplicación web con autenticación</span><span class="sxs-lookup"><span data-stu-id="f7b0c-114">Create and test a web app with authentication</span></span>

<span data-ttu-id="f7b0c-115">Ejecute los comandos siguientes para crear una aplicación web con la autenticación.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-115">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet run
```

<span data-ttu-id="f7b0c-116">Ejecute la aplicación, seleccione la **registrar** vincular y registrar un usuario.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-116">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="f7b0c-117">Una vez registrado, se le redirigirá a la que `/Identity/Account/RegisterConfirmation` página que contiene un vínculo para simular la confirmación por correo electrónico:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-117">Once registered, you are redirected to the to `/Identity/Account/RegisterConfirmation` page which contains a link to simulate email confirmation:</span></span>

* <span data-ttu-id="f7b0c-118">Seleccione el `Click here to confirm your account` vínculo.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-118">Select the `Click here to confirm your account` link.</span></span>
* <span data-ttu-id="f7b0c-119">Seleccione el **inicio de sesión** vínculo e inicie sesión con las mismas credenciales.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-119">Select the **Login** link and sign-in with the same credentials.</span></span>
* <span data-ttu-id="f7b0c-120">Seleccione el `Hello YourEmail@provider.com!` vínculo, que se le redirigirá a la `/Identity/Account/Manage/PersonalData` página.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-120">Select the `Hello YourEmail@provider.com!` link, which redirects you to the `/Identity/Account/Manage/PersonalData` page.</span></span>
* <span data-ttu-id="f7b0c-121">Seleccione el **datos personales** pestaña a la izquierda y, a continuación, seleccione **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-121">Select the **Personal data** tab on the left, and then select **Delete**.</span></span>

### <a name="configure-an-email-provider"></a><span data-ttu-id="f7b0c-122">Configurar un proveedor de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="f7b0c-122">Configure an email provider</span></span>

<span data-ttu-id="f7b0c-123">En este tutorial, [SendGrid](https://sendgrid.com) se usa para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-123">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="f7b0c-124">Necesita una cuenta de SendGrid y una clave para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-124">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="f7b0c-125">Puede usar otros proveedores de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-125">You can use other email providers.</span></span> <span data-ttu-id="f7b0c-126">Se recomienda que usar SendGrid u otro servicio de correo electrónico para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-126">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="f7b0c-127">SMTP es difícil proteger y configurado correctamente.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-127">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="f7b0c-128">Cree una clase para recuperar la clave de protección del correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-128">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="f7b0c-129">Para este ejemplo, crear *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-129">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="f7b0c-130">Configurar secretos de usuario de SendGrid</span><span class="sxs-lookup"><span data-stu-id="f7b0c-130">Configure SendGrid user secrets</span></span>

<span data-ttu-id="f7b0c-131">Establecer el `SendGridUser` y `SendGridKey` con el [herramienta secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="f7b0c-131">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="f7b0c-132">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-132">For example:</span></span>

```console
dotnet user-secrets set SendGridUser RickAndMSFT
dotnet user-secrets set SendGridKey <key>

Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="f7b0c-133">En Windows, Secret Manager almacena los pares de claves/valor en un *secrets.json* de archivos en el `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-133">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="f7b0c-134">El contenido de la *secrets.json* archivo no se cifran.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-134">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="f7b0c-135">El marcado siguiente se muestra el *secrets.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-135">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="f7b0c-136">El `SendGridKey` se ha quitado el valor.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-136">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="f7b0c-137">Para obtener más información, consulte el [patrón de opciones](xref:fundamentals/configuration/options) y [configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="f7b0c-137">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="f7b0c-138">Instalar SendGrid</span><span class="sxs-lookup"><span data-stu-id="f7b0c-138">Install SendGrid</span></span>

<span data-ttu-id="f7b0c-139">Este tutorial muestra cómo agregar notificaciones de correo electrónico a través de [SendGrid](https://sendgrid.com/), pero puede enviar correo electrónico mediante SMTP y otros mecanismos.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-139">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="f7b0c-140">Instalar el `SendGrid` paquete NuGet:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-140">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f7b0c-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7b0c-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f7b0c-142">Desde la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-142">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f7b0c-143">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f7b0c-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f7b0c-144">En la consola, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-144">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

---

<span data-ttu-id="f7b0c-145">Consulte [comience gratis con SendGrid](https://sendgrid.com/free/) para registrar una cuenta gratuita de SendGrid.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-145">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="f7b0c-146">Implementar IEmailSender</span><span class="sxs-lookup"><span data-stu-id="f7b0c-146">Implement IEmailSender</span></span>

<span data-ttu-id="f7b0c-147">Para implementar `IEmailSender`, crear *Services/EmailSender.cs* con código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-147">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="f7b0c-148">Configurar el inicio para admitir correo electrónico</span><span class="sxs-lookup"><span data-stu-id="f7b0c-148">Configure startup to support email</span></span>

<span data-ttu-id="f7b0c-149">Agregue el código siguiente a la `ConfigureServices` método en el *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-149">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="f7b0c-150">Agregar `EmailSender` como un servicio transitorio.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-150">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="f7b0c-151">Registrar el `AuthMessageSenderOptions` instancia de configuración.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-151">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Startup.cs?name=snippet1&highlight=11-15)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="f7b0c-152">Registrar, confirmar correo electrónico y restablecimiento de contraseña</span><span class="sxs-lookup"><span data-stu-id="f7b0c-152">Register, confirm email, and reset password</span></span>

<span data-ttu-id="f7b0c-153">Ejecutar la aplicación web y probar el flujo de recuperación de contraseña y confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-153">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="f7b0c-154">Ejecute la aplicación y registrar un nuevo usuario</span><span class="sxs-lookup"><span data-stu-id="f7b0c-154">Run the app and register a new user</span></span>
* <span data-ttu-id="f7b0c-155">Compruebe su correo electrónico para el vínculo de confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-155">Check your email for the account confirmation link.</span></span> <span data-ttu-id="f7b0c-156">Consulte [depurar correo electrónico](#debug) si no recibe el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-156">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="f7b0c-157">Haga clic en el vínculo para confirmar su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-157">Click the link to confirm your email.</span></span>
* <span data-ttu-id="f7b0c-158">Inicie sesión con su correo electrónico y contraseña.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-158">Sign in with your email and password.</span></span>
* <span data-ttu-id="f7b0c-159">Cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-159">Sign out.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="f7b0c-160">Restablecimiento de contraseña de la prueba</span><span class="sxs-lookup"><span data-stu-id="f7b0c-160">Test password reset</span></span>

* <span data-ttu-id="f7b0c-161">Si ha iniciado sesión, seleccione **Logout**.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-161">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="f7b0c-162">Seleccione el **iniciarla** vínculo y seleccione el **¿olvidó su contraseña?** vínculo.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-162">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="f7b0c-163">Escriba el correo electrónico usado para registrar la cuenta.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-163">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="f7b0c-164">Se envía un correo electrónico con un vínculo para restablecer su contraseña.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-164">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="f7b0c-165">Compruebe su correo electrónico y haga clic en el vínculo para restablecer la contraseña.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-165">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="f7b0c-166">Una vez que se ha restablecido correctamente su contraseña, puede iniciar sesión con su correo electrónico y la contraseña nueva.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-166">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="f7b0c-167">Cambiar el tiempo de espera de correo electrónico y la actividad</span><span class="sxs-lookup"><span data-stu-id="f7b0c-167">Change email and activity timeout</span></span>

<span data-ttu-id="f7b0c-168">El tiempo de espera de inactividad predeterminado es 14 días.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-168">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="f7b0c-169">El código siguiente establece el tiempo de espera de inactividad en 5 días:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-169">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="f7b0c-170">Cambiar todas las duraciones de token protección de datos</span><span class="sxs-lookup"><span data-stu-id="f7b0c-170">Change all data protection token lifespans</span></span>

<span data-ttu-id="f7b0c-171">El código siguiente cambia el tiempo de espera de los tokens de protección de datos todas a 3 horas:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-171">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAllTokens.cs?name=snippet1&highlight=11-12)]

<span data-ttu-id="f7b0c-172">Integrado en tokens de identidad de usuario (consulte [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) tiene un [tiempo de espera de un día](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="f7b0c-172">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="f7b0c-173">Cambiar la duración del token de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="f7b0c-173">Change the email token lifespan</span></span>

<span data-ttu-id="f7b0c-174">La duración del token predeterminado de [los tokens de identidad de usuario](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) es [un día](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="f7b0c-174">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="f7b0c-175">En esta sección se muestra cómo cambiar la duración del token de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-175">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="f7b0c-176">Agregar un personalizado [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) y <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-176">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="f7b0c-177">Agregue el proveedor personalizado para el contenedor de servicios:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-177">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupEmail.cs?name=snippet1&highlight=10-16)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="f7b0c-178">Reenviar correo electrónico de confirmación</span><span class="sxs-lookup"><span data-stu-id="f7b0c-178">Resend email confirmation</span></span>

<span data-ttu-id="f7b0c-179">Consulte [este problema de GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="f7b0c-179">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="f7b0c-180">Depurar el correo electrónico</span><span class="sxs-lookup"><span data-stu-id="f7b0c-180">Debug email</span></span>

<span data-ttu-id="f7b0c-181">Si no se puede obtener el trabajo de correo electrónico:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-181">If you can't get email working:</span></span>

* <span data-ttu-id="f7b0c-182">Establecer un punto de interrupción en `EmailSender.Execute` comprobar `SendGridClient.SendEmailAsync` se llama.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-182">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="f7b0c-183">Crear un [aplicación de consola para enviar correo electrónico](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) mediante código similar a `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-183">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="f7b0c-184">Revise el [actividad de correo electrónico](https://sendgrid.com/docs/User_Guide/email_activity.html) página.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-184">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="f7b0c-185">Compruebe la carpeta de correo basura.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-185">Check your spam folder.</span></span>
* <span data-ttu-id="f7b0c-186">Pruebe otro alias de correo electrónico en un proveedor de correo electrónico diferentes (Microsoft, Yahoo, Gmail, etcetera.)</span><span class="sxs-lookup"><span data-stu-id="f7b0c-186">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="f7b0c-187">Pruebe a enviar a las cuentas de correo electrónico diferente.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-187">Try sending to different email accounts.</span></span>

<span data-ttu-id="f7b0c-188">**Una práctica recomendada de seguridad** es **no** use secretos de producción en desarrollo y pruebas.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-188">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="f7b0c-189">Si publica la aplicación en Azure, establecer los secretos de SendGrid como configuración de la aplicación en el portal de Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-189">If you publish the app to Azure, set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="f7b0c-190">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-190">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="f7b0c-191">Combinar las cuentas de inicio de sesión social y local</span><span class="sxs-lookup"><span data-stu-id="f7b0c-191">Combine social and local login accounts</span></span>

<span data-ttu-id="f7b0c-192">Para completar esta sección, primero debe habilitar a un proveedor de autenticación externo.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-192">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="f7b0c-193">Consulte [Facebook, Google y la autenticación de proveedor externo](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="f7b0c-193">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="f7b0c-194">Puede combinar las cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-194">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="f7b0c-195">En la siguiente secuencia, "RickAndMSFT@gmail.com" en primer lugar se crea como un inicio de sesión local; sin embargo, puede crear la cuenta como un inicio de sesión social en primer lugar y luego agregar un inicio de sesión local.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-195">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplicación Web: RickAndMSFT@gmail.com usuario autenticado](accconfirm/_static/rick.png)

<span data-ttu-id="f7b0c-197">Haga clic en el **administrar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-197">Click on the **Manage** link.</span></span> <span data-ttu-id="f7b0c-198">Tenga en cuenta la externa 0 (inicios de sesión sociales) asociado con esta cuenta.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-198">Note the 0 external (social logins) associated with this account.</span></span>

![Administrar la vista](accconfirm/_static/manage.png)

<span data-ttu-id="f7b0c-200">Haga clic en el vínculo a otro servicio de inicio de sesión y acepte las solicitudes de aplicación.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-200">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="f7b0c-201">En la siguiente imagen, Facebook es el proveedor de autenticación externos:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-201">In the following image, Facebook is the external authentication provider:</span></span>

![Administrar la vista de inicios de sesión externos listado de Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="f7b0c-203">Se han combinado las dos cuentas.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-203">The two accounts have been combined.</span></span> <span data-ttu-id="f7b0c-204">Es posible iniciar sesión con las cuentas.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-204">You are able to sign in with either account.</span></span> <span data-ttu-id="f7b0c-205">Es posible que desee que los usuarios agreguen cuentas locales en caso de su servicio de autenticación de inicio de sesión social esté inactivo o que es más probable hayan perdido el acceso a su cuenta de redes sociales.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-205">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="f7b0c-206">Habilitar la confirmación de cuenta después de un sitio tiene usuarios</span><span class="sxs-lookup"><span data-stu-id="f7b0c-206">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="f7b0c-207">Habilitar confirmación de la cuenta en un sitio con usuarios bloquea todos los usuarios existentes.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-207">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="f7b0c-208">Los usuarios existentes están bloqueados porque sus cuentas no confirmadas.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-208">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="f7b0c-209">Para evitar el bloqueo de usuario existente, use uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-209">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="f7b0c-210">Actualizar la base de datos para marcar todos los usuarios existentes, como se ha confirmado.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-210">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="f7b0c-211">Confirme que los usuarios existentes.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-211">Confirm existing users.</span></span> <span data-ttu-id="f7b0c-212">Por ejemplo, envío por lotes de mensajes de correo electrónico con vínculos de confirmación.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-212">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0 < aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="f7b0c-213">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f7b0c-213">Prerequisites</span></span>

[<span data-ttu-id="f7b0c-214">SDK de .NET core 2.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="f7b0c-214">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="f7b0c-215">Crear una aplicación web y aplicar la técnica scaffolding identidad</span><span class="sxs-lookup"><span data-stu-id="f7b0c-215">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="f7b0c-216">Ejecute los comandos siguientes para crear una aplicación web con la autenticación.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-216">Run the following commands to create a web app with authentication.</span></span>

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="f7b0c-217">Nuevo registro de usuario de prueba</span><span class="sxs-lookup"><span data-stu-id="f7b0c-217">Test new user registration</span></span>

<span data-ttu-id="f7b0c-218">Ejecute la aplicación, seleccione la **registrar** vincular y registrar un usuario.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-218">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="f7b0c-219">En este momento, es la única validación en el correo electrónico con el [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) atributo.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-219">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="f7b0c-220">Después de enviar el registro, se registran en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-220">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="f7b0c-221">Más adelante en el tutorial, se actualiza el código para que los nuevos usuarios no pueden iniciar sesión hasta que se valida su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-221">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="f7b0c-222">Tenga en cuenta la tabla `EmailConfirmed` campo es `False`.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-222">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="f7b0c-223">Es posible que desee volver a usar este correo electrónico en el paso siguiente cuando la aplicación envía un correo electrónico de confirmación.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-223">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="f7b0c-224">Haga doble clic en la fila y seleccione **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-224">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="f7b0c-225">Eliminar el alias de correo electrónico resulta más fácil en los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-225">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="f7b0c-226">Requerir confirmación por correo electrónico</span><span class="sxs-lookup"><span data-stu-id="f7b0c-226">Require email confirmation</span></span>

<span data-ttu-id="f7b0c-227">Es una práctica recomendada para confirmar el correo electrónico de un nuevo registro de usuario.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-227">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="f7b0c-228">Enviar por correo electrónico de confirmación le ayuda a comprobar que no están suplantando persona (es decir, se hayan registrado con el correo electrónico de otra persona).</span><span class="sxs-lookup"><span data-stu-id="f7b0c-228">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="f7b0c-229">Supongamos que tuviera un foro de discusión y desea evitar "yli@example.com"al registrar como"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="f7b0c-229">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="f7b0c-230">Sin confirmación por correo electrónico, "nolivetto@contoso.com" podría recibir el correo no deseado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-230">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="f7b0c-231">Supongamos que el usuario registrado accidentalmente como "ylo@example.com" y no se vio el error ortográfico de "yli".</span><span class="sxs-lookup"><span data-stu-id="f7b0c-231">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="f7b0c-232">No podrán usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-232">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="f7b0c-233">Confirmación por correo electrónico proporciona una protección limitada de bots.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-233">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="f7b0c-234">Confirmación por correo electrónico no proporciona protección contra usuarios malintencionados con varias cuentas de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-234">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="f7b0c-235">Por lo general desea impedir que todos los datos del sitio web de registro antes de que tengan un correo electrónico confirmado nuevos usuarios.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-235">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="f7b0c-236">Actualización `Startup.ConfigureServices` para requerir un correo electrónico de confirmación:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-236">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="f7b0c-237">`config.SignIn.RequireConfirmedEmail = true;` impide que los usuarios registrados iniciar sesión hasta que se confirme su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-237">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="f7b0c-238">Configurar el proveedor de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="f7b0c-238">Configure email provider</span></span>

<span data-ttu-id="f7b0c-239">En este tutorial, [SendGrid](https://sendgrid.com) se usa para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-239">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="f7b0c-240">Necesita una cuenta de SendGrid y una clave para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-240">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="f7b0c-241">Puede usar otros proveedores de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-241">You can use other email providers.</span></span> <span data-ttu-id="f7b0c-242">ASP.NET Core 2.x incluye `System.Net.Mail`, lo que permite enviar correo electrónico desde la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-242">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="f7b0c-243">Se recomienda que usar SendGrid u otro servicio de correo electrónico para enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-243">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="f7b0c-244">SMTP es difícil proteger y configurado correctamente.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-244">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="f7b0c-245">Cree una clase para recuperar la clave de protección del correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-245">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="f7b0c-246">Para este ejemplo, crear *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-246">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="f7b0c-247">Configurar secretos de usuario de SendGrid</span><span class="sxs-lookup"><span data-stu-id="f7b0c-247">Configure SendGrid user secrets</span></span>

<span data-ttu-id="f7b0c-248">Establecer el `SendGridUser` y `SendGridKey` con el [herramienta secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="f7b0c-248">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="f7b0c-249">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-249">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="f7b0c-250">En Windows, Secret Manager almacena los pares de claves/valor en un *secrets.json* de archivos en el `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-250">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="f7b0c-251">El contenido de la *secrets.json* archivo no se cifran.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-251">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="f7b0c-252">El marcado siguiente se muestra el *secrets.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-252">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="f7b0c-253">El `SendGridKey` se ha quitado el valor.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-253">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="f7b0c-254">Para obtener más información, consulte el [patrón de opciones](xref:fundamentals/configuration/options) y [configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="f7b0c-254">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="f7b0c-255">Instalar SendGrid</span><span class="sxs-lookup"><span data-stu-id="f7b0c-255">Install SendGrid</span></span>

<span data-ttu-id="f7b0c-256">Este tutorial muestra cómo agregar notificaciones de correo electrónico a través de [SendGrid](https://sendgrid.com/), pero puede enviar correo electrónico mediante SMTP y otros mecanismos.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-256">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="f7b0c-257">Instalar el `SendGrid` paquete NuGet:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-257">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f7b0c-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7b0c-258">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f7b0c-259">Desde la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-259">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f7b0c-260">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f7b0c-260">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f7b0c-261">En la consola, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-261">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

---

<span data-ttu-id="f7b0c-262">Consulte [comience gratis con SendGrid](https://sendgrid.com/free/) para registrar una cuenta gratuita de SendGrid.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-262">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="f7b0c-263">Implementar IEmailSender</span><span class="sxs-lookup"><span data-stu-id="f7b0c-263">Implement IEmailSender</span></span>

<span data-ttu-id="f7b0c-264">Para implementar `IEmailSender`, crear *Services/EmailSender.cs* con código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-264">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="f7b0c-265">Configurar el inicio para admitir correo electrónico</span><span class="sxs-lookup"><span data-stu-id="f7b0c-265">Configure startup to support email</span></span>

<span data-ttu-id="f7b0c-266">Agregue el código siguiente a la `ConfigureServices` método en el *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-266">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="f7b0c-267">Agregar `EmailSender` como un servicio transitorio.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-267">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="f7b0c-268">Registrar el `AuthMessageSenderOptions` instancia de configuración.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-268">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="f7b0c-269">Habilitar la recuperación de confirmación y la contraseña de cuenta</span><span class="sxs-lookup"><span data-stu-id="f7b0c-269">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="f7b0c-270">La plantilla tiene el código para la recuperación de confirmación y la contraseña de cuenta.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-270">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="f7b0c-271">Buscar el `OnPostAsync` método *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-271">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="f7b0c-272">Impedir que los usuarios recién registrados que se va a iniciar sesión automáticamente en marcando como comentario la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-272">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="f7b0c-273">El método completo se muestra con la línea cambiada resaltada:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-273">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="f7b0c-274">Registrar, confirmar correo electrónico y restablecimiento de contraseña</span><span class="sxs-lookup"><span data-stu-id="f7b0c-274">Register, confirm email, and reset password</span></span>

<span data-ttu-id="f7b0c-275">Ejecutar la aplicación web y probar el flujo de recuperación de contraseña y confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-275">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="f7b0c-276">Ejecute la aplicación y registrar un nuevo usuario</span><span class="sxs-lookup"><span data-stu-id="f7b0c-276">Run the app and register a new user</span></span>
* <span data-ttu-id="f7b0c-277">Compruebe su correo electrónico para el vínculo de confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-277">Check your email for the account confirmation link.</span></span> <span data-ttu-id="f7b0c-278">Consulte [depurar correo electrónico](#debug) si no recibe el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-278">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="f7b0c-279">Haga clic en el vínculo para confirmar su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-279">Click the link to confirm your email.</span></span>
* <span data-ttu-id="f7b0c-280">Inicie sesión con su correo electrónico y contraseña.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-280">Sign in with your email and password.</span></span>
* <span data-ttu-id="f7b0c-281">Cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-281">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="f7b0c-282">Ver la página de administración</span><span class="sxs-lookup"><span data-stu-id="f7b0c-282">View the manage page</span></span>

<span data-ttu-id="f7b0c-283">Seleccione el nombre de usuario en el explorador: ![ventana del explorador con el nombre de usuario](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="f7b0c-283">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="f7b0c-284">Se muestra la página de administración con el **perfil** pestaña seleccionada.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-284">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="f7b0c-285">El **correo electrónico** muestra una casilla de verificación que indica el correo electrónico se ha confirmado.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-285">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="f7b0c-286">Restablecimiento de contraseña de la prueba</span><span class="sxs-lookup"><span data-stu-id="f7b0c-286">Test password reset</span></span>

* <span data-ttu-id="f7b0c-287">Si ha iniciado sesión, seleccione **Logout**.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-287">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="f7b0c-288">Seleccione el **iniciarla** vínculo y seleccione el **¿olvidó su contraseña?** vínculo.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-288">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="f7b0c-289">Escriba el correo electrónico usado para registrar la cuenta.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-289">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="f7b0c-290">Se envía un correo electrónico con un vínculo para restablecer su contraseña.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-290">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="f7b0c-291">Compruebe su correo electrónico y haga clic en el vínculo para restablecer la contraseña.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-291">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="f7b0c-292">Una vez que se ha restablecido correctamente su contraseña, puede iniciar sesión con su correo electrónico y la contraseña nueva.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-292">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="f7b0c-293">Cambiar el tiempo de espera de correo electrónico y la actividad</span><span class="sxs-lookup"><span data-stu-id="f7b0c-293">Change email and activity timeout</span></span>

<span data-ttu-id="f7b0c-294">El tiempo de espera de inactividad predeterminado es 14 días.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-294">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="f7b0c-295">El código siguiente establece el tiempo de espera de inactividad en 5 días:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-295">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="f7b0c-296">Cambiar todas las duraciones de token protección de datos</span><span class="sxs-lookup"><span data-stu-id="f7b0c-296">Change all data protection token lifespans</span></span>

<span data-ttu-id="f7b0c-297">El código siguiente cambia el tiempo de espera de los tokens de protección de datos todas a 3 horas:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-297">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="f7b0c-298">Integrado en tokens de identidad de usuario (consulte [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) tiene un [tiempo de espera de un día](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="f7b0c-298">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="f7b0c-299">Cambiar la duración del token de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="f7b0c-299">Change the email token lifespan</span></span>

<span data-ttu-id="f7b0c-300">La duración del token predeterminado de [los tokens de identidad de usuario](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) es [un día](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="f7b0c-300">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="f7b0c-301">En esta sección se muestra cómo cambiar la duración del token de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-301">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="f7b0c-302">Agregar un personalizado [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) y <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-302">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="f7b0c-303">Agregue el proveedor personalizado para el contenedor de servicios:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-303">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="f7b0c-304">Reenviar correo electrónico de confirmación</span><span class="sxs-lookup"><span data-stu-id="f7b0c-304">Resend email confirmation</span></span>

<span data-ttu-id="f7b0c-305">Consulte [este problema de GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="f7b0c-305">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="f7b0c-306">Depurar el correo electrónico</span><span class="sxs-lookup"><span data-stu-id="f7b0c-306">Debug email</span></span>

<span data-ttu-id="f7b0c-307">Si no se puede obtener el trabajo de correo electrónico:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-307">If you can't get email working:</span></span>

* <span data-ttu-id="f7b0c-308">Establecer un punto de interrupción en `EmailSender.Execute` comprobar `SendGridClient.SendEmailAsync` se llama.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-308">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="f7b0c-309">Crear un [aplicación de consola para enviar correo electrónico](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) mediante código similar a `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-309">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="f7b0c-310">Revise el [actividad de correo electrónico](https://sendgrid.com/docs/User_Guide/email_activity.html) página.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-310">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="f7b0c-311">Compruebe la carpeta de correo basura.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-311">Check your spam folder.</span></span>
* <span data-ttu-id="f7b0c-312">Pruebe otro alias de correo electrónico en un proveedor de correo electrónico diferentes (Microsoft, Yahoo, Gmail, etcetera.)</span><span class="sxs-lookup"><span data-stu-id="f7b0c-312">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="f7b0c-313">Pruebe a enviar a las cuentas de correo electrónico diferente.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-313">Try sending to different email accounts.</span></span>

<span data-ttu-id="f7b0c-314">**Una práctica recomendada de seguridad** es **no** use secretos de producción en desarrollo y pruebas.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-314">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="f7b0c-315">Si publica la aplicación en Azure, puede establecer los secretos de SendGrid como configuración de la aplicación en el portal de Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-315">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="f7b0c-316">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-316">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="f7b0c-317">Combinar las cuentas de inicio de sesión social y local</span><span class="sxs-lookup"><span data-stu-id="f7b0c-317">Combine social and local login accounts</span></span>

<span data-ttu-id="f7b0c-318">Para completar esta sección, primero debe habilitar a un proveedor de autenticación externo.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-318">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="f7b0c-319">Consulte [Facebook, Google y la autenticación de proveedor externo](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="f7b0c-319">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="f7b0c-320">Puede combinar las cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-320">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="f7b0c-321">En la siguiente secuencia, "RickAndMSFT@gmail.com" en primer lugar se crea como un inicio de sesión local; sin embargo, puede crear la cuenta como un inicio de sesión social en primer lugar y luego agregar un inicio de sesión local.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-321">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Aplicación Web: RickAndMSFT@gmail.com usuario autenticado](accconfirm/_static/rick.png)

<span data-ttu-id="f7b0c-323">Haga clic en el **administrar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-323">Click on the **Manage** link.</span></span> <span data-ttu-id="f7b0c-324">Tenga en cuenta la externa 0 (inicios de sesión sociales) asociado con esta cuenta.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-324">Note the 0 external (social logins) associated with this account.</span></span>

![Administrar la vista](accconfirm/_static/manage.png)

<span data-ttu-id="f7b0c-326">Haga clic en el vínculo a otro servicio de inicio de sesión y acepte las solicitudes de aplicación.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-326">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="f7b0c-327">En la siguiente imagen, Facebook es el proveedor de autenticación externos:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-327">In the following image, Facebook is the external authentication provider:</span></span>

![Administrar la vista de inicios de sesión externos listado de Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="f7b0c-329">Se han combinado las dos cuentas.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-329">The two accounts have been combined.</span></span> <span data-ttu-id="f7b0c-330">Es posible iniciar sesión con las cuentas.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-330">You are able to sign in with either account.</span></span> <span data-ttu-id="f7b0c-331">Es posible que desee que los usuarios agreguen cuentas locales en caso de su servicio de autenticación de inicio de sesión social esté inactivo o que es más probable hayan perdido el acceso a su cuenta de redes sociales.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-331">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="f7b0c-332">Habilitar la confirmación de cuenta después de un sitio tiene usuarios</span><span class="sxs-lookup"><span data-stu-id="f7b0c-332">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="f7b0c-333">Habilitar confirmación de la cuenta en un sitio con usuarios bloquea todos los usuarios existentes.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-333">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="f7b0c-334">Los usuarios existentes están bloqueados porque sus cuentas no confirmadas.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-334">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="f7b0c-335">Para evitar el bloqueo de usuario existente, use uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="f7b0c-335">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="f7b0c-336">Actualizar la base de datos para marcar todos los usuarios existentes, como se ha confirmado.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-336">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="f7b0c-337">Confirme que los usuarios existentes.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-337">Confirm existing users.</span></span> <span data-ttu-id="f7b0c-338">Por ejemplo, envío por lotes de mensajes de correo electrónico con vínculos de confirmación.</span><span class="sxs-lookup"><span data-stu-id="f7b0c-338">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
