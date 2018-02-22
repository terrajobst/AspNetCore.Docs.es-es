---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: "Crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, enviar por correo electrónico de confirmación y restablecimiento de contraseña (C#) | Documentos de Microsoft"
author: Rick-Anderson
description: "Este tutorial muestra cómo compilar una aplicación web de ASP.NET MVC 5 con confirmación por correo electrónico y con el sistema de pertenencia de ASP.NET Identity de restablecimiento de contraseña. Ca..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: d55b34135d5bab98ab8de31cc4b12dcc272cbc0a
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2018
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="e36c3-104">Crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, enviar por correo electrónico de confirmación y restablecimiento de contraseña (C#)</span><span class="sxs-lookup"><span data-stu-id="e36c3-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="e36c3-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="e36c3-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="e36c3-106">Este tutorial muestra cómo compilar una aplicación web de ASP.NET MVC 5 con confirmación por correo electrónico y con el sistema de pertenencia de ASP.NET Identity de restablecimiento de contraseña.</span><span class="sxs-lookup"><span data-stu-id="e36c3-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="e36c3-107">Puede descargar la aplicación completa [aquí](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="e36c3-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="e36c3-108">La descarga contiene aplicaciones auxiliares de depuración que permiten probar confirmación por correo electrónico y SMS sin necesidad de preparar un correo electrónico o el proveedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="e36c3-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="e36c3-109">Este tutorial se escribió por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="e36c3-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="e36c3-110">Crear una aplicación de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e36c3-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="e36c3-111">Empiece por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="e36c3-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="e36c3-112">Instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior.</span><span class="sxs-lookup"><span data-stu-id="e36c3-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="e36c3-113">Advertencia: Debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="e36c3-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="e36c3-114">Cree un nuevo proyecto Web de ASP.NET y seleccione la plantilla MVC.</span><span class="sxs-lookup"><span data-stu-id="e36c3-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="e36c3-115">Formularios Web Forms también admite la identidad de ASP.NET, por lo que podría seguir pasos similares en una aplicación de formularios web.</span><span class="sxs-lookup"><span data-stu-id="e36c3-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="e36c3-116">Deje la autenticación predeterminada como **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="e36c3-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="e36c3-117">Si desea hospedar la aplicación en Azure, deje activada la casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="e36c3-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="e36c3-118">Más adelante en el tutorial se implementará en Azure.</span><span class="sxs-lookup"><span data-stu-id="e36c3-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="e36c3-119">También puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="e36c3-119">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="e36c3-120">Establecer el [proyecto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="e36c3-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="e36c3-121">Ejecutar la aplicación, haga clic en el **registrar** vincular y registrar un usuario.</span><span class="sxs-lookup"><span data-stu-id="e36c3-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="e36c3-122">En este punto, es la única validación en el correo electrónico con el [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atributo.</span><span class="sxs-lookup"><span data-stu-id="e36c3-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="e36c3-123">En el Explorador de servidores, vaya a **datos Connections\DefaultConnection\Tables\AspNetUsers**, haga clic en y seleccione **Abrir definición de tabla**.</span><span class="sxs-lookup"><span data-stu-id="e36c3-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="e36c3-124">La siguiente imagen muestra la `AspNetUsers` esquema:</span><span class="sxs-lookup"><span data-stu-id="e36c3-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="e36c3-125">Haga clic con el botón secundario en el **AspNetUsers** de tabla y seleccione **mostrar datos de tabla**.</span><span class="sxs-lookup"><span data-stu-id="e36c3-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="e36c3-126">En este momento, no se ha confirmado el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="e36c3-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="e36c3-127">Haga clic en la fila y seleccione Eliminar.</span><span class="sxs-lookup"><span data-stu-id="e36c3-127">Click on the row and select delete.</span></span> <span data-ttu-id="e36c3-128">Podrá agregar este correo electrónico nuevo en el paso siguiente y enviar un correo electrónico de confirmación.</span><span class="sxs-lookup"><span data-stu-id="e36c3-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="e36c3-129">Confirmación por correo electrónico</span><span class="sxs-lookup"><span data-stu-id="e36c3-129">Email confirmation</span></span>

<span data-ttu-id="e36c3-130">Es una práctica recomendada para confirmar el correo electrónico de un nuevo registro de usuario para comprobar que no utiliza la suplantación otra persona (es decir, no ha registrado con el correo electrónico de otra persona).</span><span class="sxs-lookup"><span data-stu-id="e36c3-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="e36c3-131">Supongamos que tenemos un foro de discusión, puede que desee evitar `"bob@example.com"` al registrar como `"joe@contoso.com"`.</span><span class="sxs-lookup"><span data-stu-id="e36c3-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="e36c3-132">Sin confirmación por correo electrónico, `"joe@contoso.com"` pudo obtener el correo electrónico no deseado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e36c3-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="e36c3-133">Suponga que Roberto accidentalmente registrado como `"bib@example.com"` y no vio, no podrá usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto.</span><span class="sxs-lookup"><span data-stu-id="e36c3-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="e36c3-134">Confirmación por correo electrónico proporciona sólo una protección limitada de robots y no proporciona protección de correo basura determinado, tienen muchos alias de correo electrónico de trabajo que puede usar para registrar.</span><span class="sxs-lookup"><span data-stu-id="e36c3-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="e36c3-135">En general conveniente evitar que los nuevos usuarios se registren todos los datos del sitio web antes de que se han confirmado por correo electrónico, un mensaje de texto SMS u otro mecanismo.</span><span class="sxs-lookup"><span data-stu-id="e36c3-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="e36c3-136">En las secciones siguientes, se habilitará la confirmación por correo electrónico y modificar el código para evitar que los usuarios recién registrados el registro hasta que se ha confirmado el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="e36c3-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="e36c3-137">Enlazar SendGrid</span><span class="sxs-lookup"><span data-stu-id="e36c3-137">Hook up SendGrid</span></span>

<span data-ttu-id="e36c3-138">Aunque este tutorial solo muestra cómo agregar la notificación de correo electrónico a través de [SendGrid](http://sendgrid.com/), puede enviar correo electrónico mediante SMTP y otros mecanismos (vea [recursos adicionales](#addRes)).</span><span class="sxs-lookup"><span data-stu-id="e36c3-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="e36c3-139">En la consola de administrador de paquetes, escriba lo siguiente el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="e36c3-139">In the Package Manager Console, enter the following the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="e36c3-140">Vaya a la [página de registro Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) y registrar una cuenta gratuita de SendGrid.</span><span class="sxs-lookup"><span data-stu-id="e36c3-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="e36c3-141">Configurar SendGrid agregando código similar al siguiente en *App_Start/IdentityConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="e36c3-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="e36c3-142">Debe agregar que incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e36c3-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="e36c3-143">Para simplificar este ejemplo, se podrá almacenar la configuración de la aplicación en el *web.config* archivo:</span><span class="sxs-lookup"><span data-stu-id="e36c3-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="e36c3-144">Seguridad - nunca almacenar los datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="e36c3-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="e36c3-145">La cuenta y las credenciales se almacenan en el appSetting.</span><span class="sxs-lookup"><span data-stu-id="e36c3-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="e36c3-146">En Azure, puede almacenar con seguridad estos valores en el  **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  ficha en el portal de Azure.</span><span class="sxs-lookup"><span data-stu-id="e36c3-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="e36c3-147">Vea [las prácticas recomendadas para implementar las contraseñas y otros datos confidenciales en ASP.NET y Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="e36c3-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="e36c3-148">Habilitar la confirmación de correo electrónico en el controlador de cuenta</span><span class="sxs-lookup"><span data-stu-id="e36c3-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="e36c3-149">Compruebe el *Views\Account\ConfirmEmail.cshtml* archivo tiene la sintaxis razor correcto.</span><span class="sxs-lookup"><span data-stu-id="e36c3-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="e36c3-150">(El carácter en la primera @ línea es posible que falte.</span><span class="sxs-lookup"><span data-stu-id="e36c3-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="e36c3-151">)</span><span class="sxs-lookup"><span data-stu-id="e36c3-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="e36c3-152">Ejecute la aplicación y haga clic en el vínculo de registro.</span><span class="sxs-lookup"><span data-stu-id="e36c3-152">Run the app and click the Register link.</span></span> <span data-ttu-id="e36c3-153">Una vez que se envía el formulario de registro, que haya iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="e36c3-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="e36c3-154">Compruebe su cuenta de correo electrónico y haga clic en el vínculo para confirmar su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="e36c3-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="e36c3-155">Requerir confirmación por correo electrónico antes de inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="e36c3-155">Require email confirmation before log in</span></span>

<span data-ttu-id="e36c3-156">Actualmente una vez que un usuario complete el formulario de registro, se registran en.</span><span class="sxs-lookup"><span data-stu-id="e36c3-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="e36c3-157">Por lo general desea confirmar su correo electrónico antes de registrarlos.</span><span class="sxs-lookup"><span data-stu-id="e36c3-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="e36c3-158">En la sección siguiente, se modificará el código para requerir que los usuarios nuevos para que tenga un correo electrónico confirmado antes de que se registran en (autenticado).</span><span class="sxs-lookup"><span data-stu-id="e36c3-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="e36c3-159">Actualización de la `HttpPost Register` método con los siguientes cambios resaltados:</span><span class="sxs-lookup"><span data-stu-id="e36c3-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="e36c3-160">Marcando como comentario el `SignInAsync` método, el usuario no se firmará mediante el registro.</span><span class="sxs-lookup"><span data-stu-id="e36c3-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="e36c3-161">El `TempData["ViewBagLink"] = callbackUrl;` línea se puede usar para [depurar la aplicación](#dbg) y probar el registro sin enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="e36c3-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="e36c3-162">`ViewBag.Message` se usa para mostrar las instrucciones de confirmación.</span><span class="sxs-lookup"><span data-stu-id="e36c3-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="e36c3-163">El [Descargar ejemplo](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contiene código para probar la confirmación por correo electrónico sin tener que configurar el correo electrónico y también puede utilizarse para depurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e36c3-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="e36c3-164">Crear un `Views\Shared\Info.cshtml` de archivos y agregue el siguiente marcado de razor:</span><span class="sxs-lookup"><span data-stu-id="e36c3-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="e36c3-165">Agregar el [atributo Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) a la `Contact` método de acción del controlador Home.</span><span class="sxs-lookup"><span data-stu-id="e36c3-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="e36c3-166">Puede usar haga clic en el **póngase en contacto con** vínculo para comprobar que los usuarios anónimos no tienen acceso y los usuarios autenticados tengan acceso.</span><span class="sxs-lookup"><span data-stu-id="e36c3-166">You can use click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="e36c3-167">También debe actualizar el `HttpPost Login` método de acción:</span><span class="sxs-lookup"><span data-stu-id="e36c3-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="e36c3-168">Actualización de la *Views\Shared\Error.cshtml* vista para mostrar el mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="e36c3-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="e36c3-169">Elimine las cuentas en el **AspNetUsers** tabla que contenga el alias de correo electrónico que se va a probar.</span><span class="sxs-lookup"><span data-stu-id="e36c3-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="e36c3-170">Ejecutar la aplicación y compruebe que no puede iniciar sesión hasta que haya confirmado la dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="e36c3-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="e36c3-171">Una vez que confirme la dirección de correo electrónico, haga clic en el **póngase en contacto con** vínculo.</span><span class="sxs-lookup"><span data-stu-id="e36c3-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="e36c3-172">Recuperación/restablecimiento de contraseña</span><span class="sxs-lookup"><span data-stu-id="e36c3-172">Password recovery/reset</span></span>

<span data-ttu-id="e36c3-173">Quite los caracteres de comentario de la `HttpPost ForgotPassword` método de acción en el controlador de cuenta:</span><span class="sxs-lookup"><span data-stu-id="e36c3-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="e36c3-174">Quite los caracteres de comentario de la `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) en el *Views\Account\Login.cshtml* archivo de vista razor:</span><span class="sxs-lookup"><span data-stu-id="e36c3-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="e36c3-175">Ahora, la página de inicio de sesión mostrará un vínculo para restablecer la contraseña.</span><span class="sxs-lookup"><span data-stu-id="e36c3-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="e36c3-176">Vínculo de confirmación de correo electrónico de reenvío</span><span class="sxs-lookup"><span data-stu-id="e36c3-176">Resend email confirmation link</span></span>

<span data-ttu-id="e36c3-177">Una vez que un usuario crea una nueva cuenta local, reciben un correo electrónico un vínculo de confirmación que son necesarios para usar antes de que puedan iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="e36c3-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="e36c3-178">Si el usuario accidentalmente elimina el correo electrónico de confirmación o nunca llega el correo electrónico, necesitará el vínculo de confirmación que se envíen de nuevo.</span><span class="sxs-lookup"><span data-stu-id="e36c3-178">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="e36c3-179">Los cambios de código siguientes muestran cómo habilitarlo.</span><span class="sxs-lookup"><span data-stu-id="e36c3-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="e36c3-180">Agregue el siguiente método auxiliar a la parte inferior de la *Controllers\AccountController.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="e36c3-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="e36c3-181">El método Register para utilizar la nueva aplicación auxiliar de actualización:</span><span class="sxs-lookup"><span data-stu-id="e36c3-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="e36c3-182">Actualizar el método de inicio de sesión para enviar la contraseña cuando si no se ha confirmado la cuenta de usuario:</span><span class="sxs-lookup"><span data-stu-id="e36c3-182">Update the Login method to resend the password when if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="e36c3-183">Combinar las cuentas de inicio de sesión locales y redes sociales</span><span class="sxs-lookup"><span data-stu-id="e36c3-183">Combine social and local login accounts</span></span>

<span data-ttu-id="e36c3-184">Puede combinar cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="e36c3-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="e36c3-185">En la siguiente secuencia  **RickAndMSFT@gmail.com**  en primer lugar se crea como un inicio de sesión local, pero puede crear la cuenta como un inicio de sesión social primero y luego agregar un inicio de sesión local.</span><span class="sxs-lookup"><span data-stu-id="e36c3-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="e36c3-186">Haga clic en el **administrar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="e36c3-186">Click on the **Manage** link.</span></span> <span data-ttu-id="e36c3-187">Tenga en cuenta el externo 0 (inicios de sesión sociales) asociados con esta cuenta.</span><span class="sxs-lookup"><span data-stu-id="e36c3-187">Note the 0 external (social logins) associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="e36c3-188">Haga clic en el vínculo en otro registro de servicio y Aceptar las solicitudes de aplicación.</span><span class="sxs-lookup"><span data-stu-id="e36c3-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="e36c3-189">Se han combinado las dos cuentas, podrá iniciar sesión con cualquiera de estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="e36c3-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="e36c3-190">Puede que los usuarios agreguen cuentas locales en caso de que su registro sociales en servicio de autenticación está inactivo o, más probable es que ha perdido el acceso a su cuenta sociales.</span><span class="sxs-lookup"><span data-stu-id="e36c3-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="e36c3-191">En la siguiente imagen, Tom es un inicio de sesión social (que puede ver desde el **inicios de sesión externos: 1** se muestra en la página).</span><span class="sxs-lookup"><span data-stu-id="e36c3-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="e36c3-192">Al hacer clic en **elegir una contraseña** le permite agregar un registro local en asociados con la misma cuenta.</span><span class="sxs-lookup"><span data-stu-id="e36c3-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="e36c3-193">Confirmación por correo electrónico de forma más minuciosa</span><span class="sxs-lookup"><span data-stu-id="e36c3-193">Email confirmation in more depth</span></span>

<span data-ttu-id="e36c3-194">Mi tutorial [confirmación de cuenta y contraseña de recuperación con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) entra en este tema con más detalles.</span><span class="sxs-lookup"><span data-stu-id="e36c3-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="e36c3-195">Para depurar la aplicación</span><span class="sxs-lookup"><span data-stu-id="e36c3-195">Debugging the app</span></span>

<span data-ttu-id="e36c3-196">Si no recibe un correo electrónico que contiene el vínculo:</span><span class="sxs-lookup"><span data-stu-id="e36c3-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="e36c3-197">Compruebe la carpeta correo no deseado o spam.</span><span class="sxs-lookup"><span data-stu-id="e36c3-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="e36c3-198">Inicie sesión en la cuenta de SendGrid y haga clic en el [vínculo de actividad de correo electrónico](https://sendgrid.com/logs/index).</span><span class="sxs-lookup"><span data-stu-id="e36c3-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="e36c3-199">Para probar el vínculo de comprobación sin correo electrónico, descargue el [ejemplo completo](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="e36c3-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="e36c3-200">Se mostrará el vínculo de confirmación y códigos de confirmación en la página.</span><span class="sxs-lookup"><span data-stu-id="e36c3-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="e36c3-201">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e36c3-201">Additional Resources</span></span>

- [<span data-ttu-id="e36c3-202">Vínculos a la identidad de ASP.NET de los recursos recomendados</span><span class="sxs-lookup"><span data-stu-id="e36c3-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="e36c3-203">[La cuenta de confirmación y recuperación de contraseña con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) explica con más detalle en la confirmación de contraseña de cuenta y la recuperación.</span><span class="sxs-lookup"><span data-stu-id="e36c3-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="e36c3-204">[Aplicación de MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) este tutorial muestra cómo escribir una aplicación de ASP.NET MVC 5 con autorización de Facebook y Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="e36c3-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="e36c3-205">También muestra cómo agregar datos adicionales a la base de datos de identidad.</span><span class="sxs-lookup"><span data-stu-id="e36c3-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="e36c3-206">[Implementar una aplicación de MVC de ASP.NET segura con pertenencia, OAuth y base de datos SQL en Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="e36c3-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="e36c3-207">Este tutorial agrega la implementación de Azure, cómo proteger su aplicación con roles, cómo usar la API de pertenencia para agregar usuarios y roles y características de seguridad adicionales.</span><span class="sxs-lookup"><span data-stu-id="e36c3-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="e36c3-208">Crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto</span><span class="sxs-lookup"><span data-stu-id="e36c3-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="e36c3-209">Crear la aplicación de Facebook y conectar la aplicación al proyecto</span><span class="sxs-lookup"><span data-stu-id="e36c3-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="e36c3-210">Cómo configurar SSL en el proyecto</span><span class="sxs-lookup"><span data-stu-id="e36c3-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
