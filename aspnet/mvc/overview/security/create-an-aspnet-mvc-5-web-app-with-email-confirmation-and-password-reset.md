---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, por correo electrónico de confirmación y restablecimiento de contraseña (C#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial muestra cómo compilar una aplicación web de ASP.NET MVC 5 con confirmación por correo electrónico y restablecimiento de contraseña mediante el sistema de pertenencia de ASP.NET Identity. Ca...
ms.author: aspnetcontent
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 1387c3e9c03e011b610a070aa0c273ded23b463e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823552"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="0b35a-104">Crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, por correo electrónico de confirmación y restablecimiento de contraseña (C#)</span><span class="sxs-lookup"><span data-stu-id="0b35a-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="0b35a-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0b35a-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="0b35a-106">Este tutorial muestra cómo compilar una aplicación web de ASP.NET MVC 5 con confirmación por correo electrónico y restablecimiento de contraseña mediante el sistema de pertenencia de ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="0b35a-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="0b35a-107">Puede descargar la aplicación completada [aquí](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="0b35a-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="0b35a-108">La descarga contiene aplicaciones auxiliares de depuración que le permiten probar la confirmación por correo electrónico y SMS sin tener que configurar un correo electrónico o el proveedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="0b35a-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="0b35a-109">En este tutorial se escribió por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="0b35a-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="0b35a-110">Crear una aplicación ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="0b35a-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="0b35a-111">Comience por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="0b35a-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="0b35a-112">Instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior.</span><span class="sxs-lookup"><span data-stu-id="0b35a-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="0b35a-113">Advertencia: Debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="0b35a-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="0b35a-114">Cree un nuevo proyecto Web ASP.NET y seleccione la plantilla MVC.</span><span class="sxs-lookup"><span data-stu-id="0b35a-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="0b35a-115">Formularios Web Forms también es compatible con ASP.NET Identity, por lo que podría seguir pasos similares en una aplicación de formularios web.</span><span class="sxs-lookup"><span data-stu-id="0b35a-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="0b35a-116">Deje la autenticación predeterminada como **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="0b35a-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="0b35a-117">Si desea hospedar la aplicación en Azure, deje activada la casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="0b35a-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="0b35a-118">Más adelante en el tutorial se implementará en Azure.</span><span class="sxs-lookup"><span data-stu-id="0b35a-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="0b35a-119">También puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="0b35a-119">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="0b35a-120">Establecer el [proyecto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="0b35a-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="0b35a-121">Ejecute la aplicación, haga clic en el **registrar** vincular y registrar un usuario.</span><span class="sxs-lookup"><span data-stu-id="0b35a-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="0b35a-122">En este momento, es la única validación en el correo electrónico con el [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atributo.</span><span class="sxs-lookup"><span data-stu-id="0b35a-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="0b35a-123">En el Explorador de servidores, vaya a **datos Connections\DefaultConnection\Tables\AspNetUsers**, haga clic y seleccione **Abrir definición de tabla**.</span><span class="sxs-lookup"><span data-stu-id="0b35a-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="0b35a-124">La siguiente imagen muestra la `AspNetUsers` esquema:</span><span class="sxs-lookup"><span data-stu-id="0b35a-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="0b35a-125">Haga clic con el botón derecho en el **AspNetUsers** de tabla y seleccione **mostrar datos de tabla**.</span><span class="sxs-lookup"><span data-stu-id="0b35a-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="0b35a-126">En este momento, no se ha confirmado el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="0b35a-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="0b35a-127">Haga clic en la fila y seleccione Eliminar.</span><span class="sxs-lookup"><span data-stu-id="0b35a-127">Click on the row and select delete.</span></span> <span data-ttu-id="0b35a-128">Se agregará este correo electrónico nuevo en el paso siguiente y enviar un correo electrónico de confirmación.</span><span class="sxs-lookup"><span data-stu-id="0b35a-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="0b35a-129">Confirmación por correo electrónico</span><span class="sxs-lookup"><span data-stu-id="0b35a-129">Email confirmation</span></span>

<span data-ttu-id="0b35a-130">Es una práctica recomendada para confirmar el correo electrónico de un nuevo registro de usuario para comprobar que no están suplantando persona (es decir, se hayan registrado con el correo electrónico de otra persona).</span><span class="sxs-lookup"><span data-stu-id="0b35a-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="0b35a-131">Supongamos que tenemos un foro de discusión, ¿puede evitar `"bob@example.com"` registrar como `"joe@contoso.com"`.</span><span class="sxs-lookup"><span data-stu-id="0b35a-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="0b35a-132">Sin confirmación por correo electrónico, `"joe@contoso.com"` podría obtener correo electrónico no deseado de su aplicación.</span><span class="sxs-lookup"><span data-stu-id="0b35a-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="0b35a-133">Supongamos que Bob accidentalmente registrado como `"bib@example.com"` y se había dado cuenta, no sería capaz de usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcta.</span><span class="sxs-lookup"><span data-stu-id="0b35a-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="0b35a-134">Confirmación por correo electrónico proporciona una protección limitada solo desde los bots y no proporciona protección de correo basura determinado, tienen muchos alias de correo electrónico de trabajo que pueden usar para registrar.</span><span class="sxs-lookup"><span data-stu-id="0b35a-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="0b35a-135">Por lo general desea impedir que todos los datos del sitio web de registro antes de que se han confirmado por correo electrónico, un mensaje de texto SMS u otro mecanismo de nuevos usuarios.</span><span class="sxs-lookup"><span data-stu-id="0b35a-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="0b35a-136">En las secciones siguientes, se habilitará la confirmación por correo electrónico y modifique el código para evitar que los usuarios recién registrados iniciar sesión hasta que se ha confirmado el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="0b35a-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="0b35a-137">Enlazar SendGrid</span><span class="sxs-lookup"><span data-stu-id="0b35a-137">Hook up SendGrid</span></span>

<span data-ttu-id="0b35a-138">Aunque este tutorial solo muestra cómo agregar la notificación de correo electrónico a través de [SendGrid](http://sendgrid.com/), puede enviar correo electrónico mediante SMTP y otros mecanismos (consulte [recursos adicionales](#addRes)).</span><span class="sxs-lookup"><span data-stu-id="0b35a-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="0b35a-139">En la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="0b35a-139">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="0b35a-140">Vaya a la [SendGrid de Azure página de registro](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) y registrarse para obtener una cuenta gratuita de SendGrid.</span><span class="sxs-lookup"><span data-stu-id="0b35a-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="0b35a-141">Configuración de SendGrid mediante la adición de código similar al siguiente en *App_Start/IdentityConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="0b35a-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="0b35a-142">Necesita agregar que incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0b35a-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="0b35a-143">Para simplificar este ejemplo, se almacenará la configuración de la aplicación en el *web.config* archivo:</span><span class="sxs-lookup"><span data-stu-id="0b35a-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="0b35a-144">Seguridad: nunca almacene de datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="0b35a-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="0b35a-145">La cuenta y las credenciales se almacenan en la appSetting.</span><span class="sxs-lookup"><span data-stu-id="0b35a-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="0b35a-146">En Azure, puede almacenar con seguridad estos valores en el **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** ficha en el portal de Azure.</span><span class="sxs-lookup"><span data-stu-id="0b35a-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="0b35a-147">Consulte [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="0b35a-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="0b35a-148">Habilitar la confirmación de correo electrónico en el controlador de cuentas</span><span class="sxs-lookup"><span data-stu-id="0b35a-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="0b35a-149">Compruebe el *Views\Account\ConfirmEmail.cshtml* archivo tiene una sintaxis de razor correcto.</span><span class="sxs-lookup"><span data-stu-id="0b35a-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="0b35a-150">(El @ caracteres en la primera línea es posible que falten.</span><span class="sxs-lookup"><span data-stu-id="0b35a-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="0b35a-151">)</span><span class="sxs-lookup"><span data-stu-id="0b35a-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="0b35a-152">Ejecute la aplicación y haga clic en el vínculo de registro.</span><span class="sxs-lookup"><span data-stu-id="0b35a-152">Run the app and click the Register link.</span></span> <span data-ttu-id="0b35a-153">Una vez que envíe el formulario de registro, se registran en.</span><span class="sxs-lookup"><span data-stu-id="0b35a-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="0b35a-154">Compruebe su cuenta de correo electrónico y haga clic en el vínculo para confirmar su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="0b35a-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="0b35a-155">Requerir confirmación por correo electrónico antes del inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="0b35a-155">Require email confirmation before log in</span></span>

<span data-ttu-id="0b35a-156">Actualmente una vez que un usuario completa el formulario de registro, inician sesión.</span><span class="sxs-lookup"><span data-stu-id="0b35a-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="0b35a-157">Por lo general desea confirmar su correo electrónico antes de registrarlos.</span><span class="sxs-lookup"><span data-stu-id="0b35a-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="0b35a-158">En la sección siguiente, se modificará el código para exigir que los usuarios nuevos tener un correo electrónico de confirmación antes de registrarse (autenticado).</span><span class="sxs-lookup"><span data-stu-id="0b35a-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="0b35a-159">Actualización de la `HttpPost Register` método con los cambios resaltados siguientes:</span><span class="sxs-lookup"><span data-stu-id="0b35a-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="0b35a-160">Marcando como comentario el `SignInAsync` método, el usuario no iniciará sesión en el registro.</span><span class="sxs-lookup"><span data-stu-id="0b35a-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="0b35a-161">El `TempData["ViewBagLink"] = callbackUrl;` línea se puede usar para [depurar la aplicación](#dbg) y probar el registro sin enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="0b35a-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="0b35a-162">`ViewBag.Message` se usa para mostrar las instrucciones de confirmación.</span><span class="sxs-lookup"><span data-stu-id="0b35a-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="0b35a-163">El [Descargar ejemplo](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contiene código para probar la confirmación por correo electrónico sin configurar correo electrónico y también puede utilizarse para depurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0b35a-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="0b35a-164">Crear un `Views\Shared\Info.cshtml` archivo y agregue el siguiente marcado de razor:</span><span class="sxs-lookup"><span data-stu-id="0b35a-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="0b35a-165">Agregar el [atributo Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) a la `Contact` método de acción del controlador Home.</span><span class="sxs-lookup"><span data-stu-id="0b35a-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="0b35a-166">Puede hacer clic en el **póngase en contacto con** vínculo para comprobar los usuarios anónimos no tienen acceso y los usuarios autenticados tengan acceso.</span><span class="sxs-lookup"><span data-stu-id="0b35a-166">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="0b35a-167">También debe actualizar el `HttpPost Login` método de acción:</span><span class="sxs-lookup"><span data-stu-id="0b35a-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="0b35a-168">Actualización de la *Views\Shared\Error.cshtml* vista para mostrar el mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="0b35a-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="0b35a-169">Elimine las cuentas en el **AspNetUsers** tabla que contenga el alias de correo electrónico que desea realizar pruebas con.</span><span class="sxs-lookup"><span data-stu-id="0b35a-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="0b35a-170">Ejecute la aplicación y compruebe que no puede iniciar sesión hasta que haya confirmado la dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="0b35a-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="0b35a-171">Una vez que confirme su dirección de correo electrónico, haga clic en el **póngase en contacto con** vínculo.</span><span class="sxs-lookup"><span data-stu-id="0b35a-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="0b35a-172">Recuperación/restablecimiento de contraseña</span><span class="sxs-lookup"><span data-stu-id="0b35a-172">Password recovery/reset</span></span>

<span data-ttu-id="0b35a-173">Quite los caracteres de comentario de la `HttpPost ForgotPassword` método de acción del controlador de cuentas:</span><span class="sxs-lookup"><span data-stu-id="0b35a-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="0b35a-174">Quite los caracteres de comentario de la `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) en el *Views\Account\Login.cshtml* archivo de vista razor:</span><span class="sxs-lookup"><span data-stu-id="0b35a-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="0b35a-175">El registro en la página mostrará un vínculo para restablecer la contraseña.</span><span class="sxs-lookup"><span data-stu-id="0b35a-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="0b35a-176">Vínculo de confirmación de correo electrónico de reenvío</span><span class="sxs-lookup"><span data-stu-id="0b35a-176">Resend email confirmation link</span></span>

<span data-ttu-id="0b35a-177">Una vez que un usuario crea una nueva cuenta local, reciben un correo electrónico un vínculo de confirmación deben usar antes de que puedan iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="0b35a-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="0b35a-178">Si el usuario que accidentalmente elimina el correo electrónico de confirmación o nunca llega el correo electrónico, deben volverá a enviar el vínculo de confirmación.</span><span class="sxs-lookup"><span data-stu-id="0b35a-178">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="0b35a-179">Los cambios de código siguientes muestran cómo habilitarlo.</span><span class="sxs-lookup"><span data-stu-id="0b35a-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="0b35a-180">Agregue el siguiente método auxiliar a la parte inferior de la *controllers\accountcontroller* archivo:</span><span class="sxs-lookup"><span data-stu-id="0b35a-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="0b35a-181">Actualice el método Register para usar la nueva aplicación auxiliar:</span><span class="sxs-lookup"><span data-stu-id="0b35a-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="0b35a-182">Actualice el método de inicio de sesión para volver a enviar la contraseña si no se ha confirmado la cuenta de usuario:</span><span class="sxs-lookup"><span data-stu-id="0b35a-182">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="0b35a-183">Combinar las cuentas de inicio de sesión social y local</span><span class="sxs-lookup"><span data-stu-id="0b35a-183">Combine social and local login accounts</span></span>

<span data-ttu-id="0b35a-184">Puede combinar las cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="0b35a-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="0b35a-185">En la siguiente secuencia **RickAndMSFT@gmail.com** en primer lugar se crea como un inicio de sesión local, pero puede crear la cuenta como un inicio de sesión social primero y luego agregar un inicio de sesión local.</span><span class="sxs-lookup"><span data-stu-id="0b35a-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="0b35a-186">Haga clic en el **administrar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="0b35a-186">Click on the **Manage** link.</span></span> <span data-ttu-id="0b35a-187">Tenga en cuenta la **inicios de sesión externos: 0** asociado con esta cuenta.</span><span class="sxs-lookup"><span data-stu-id="0b35a-187">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="0b35a-188">Haga clic en el vínculo a otro registro en el servicio y acepte las solicitudes de aplicación.</span><span class="sxs-lookup"><span data-stu-id="0b35a-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="0b35a-189">Se han combinado las dos cuentas, podrá iniciar sesión con las cuentas.</span><span class="sxs-lookup"><span data-stu-id="0b35a-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="0b35a-190">Es posible que desee que los usuarios agreguen cuentas locales en caso de su registro en el servicio de autenticación social esté inactivo o que es más probable hayan perdido el acceso a su cuenta de redes sociales.</span><span class="sxs-lookup"><span data-stu-id="0b35a-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="0b35a-191">En la siguiente imagen, Tom es un inicio de sesión social (que puede ver desde el **inicios de sesión externos: 1** se muestra en la página).</span><span class="sxs-lookup"><span data-stu-id="0b35a-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="0b35a-192">Al hacer clic en **elige una contraseña** le permite agregar un registro local en asociada con la misma cuenta.</span><span class="sxs-lookup"><span data-stu-id="0b35a-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="0b35a-193">Confirmación por correo electrónico con más detalle</span><span class="sxs-lookup"><span data-stu-id="0b35a-193">Email confirmation in more depth</span></span>

<span data-ttu-id="0b35a-194">Mi tutorial [confirmación de la cuenta y contraseña de recuperación con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) entra en este tema con más detalles.</span><span class="sxs-lookup"><span data-stu-id="0b35a-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="0b35a-195">Depuración de la aplicación</span><span class="sxs-lookup"><span data-stu-id="0b35a-195">Debugging the app</span></span>

<span data-ttu-id="0b35a-196">Si no recibe un correo electrónico que contiene el vínculo:</span><span class="sxs-lookup"><span data-stu-id="0b35a-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="0b35a-197">Compruebe la carpeta correo no deseado o spam.</span><span class="sxs-lookup"><span data-stu-id="0b35a-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="0b35a-198">Inicie sesión en su cuenta de SendGrid y haga clic en el [vínculo de la actividad de correo electrónico](https://sendgrid.com/logs/index).</span><span class="sxs-lookup"><span data-stu-id="0b35a-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="0b35a-199">Para probar el vínculo de comprobación sin correo electrónico, descargue el [ejemplo completado](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="0b35a-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="0b35a-200">En la página se mostrará el vínculo de confirmación y códigos de confirmación.</span><span class="sxs-lookup"><span data-stu-id="0b35a-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="0b35a-201">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0b35a-201">Additional Resources</span></span>

- [<span data-ttu-id="0b35a-202">Recursos recomendados de vínculos a ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="0b35a-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="0b35a-203">[La cuenta de confirmación y la recuperación de contraseña con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) explica con más detalle en la confirmación de contraseña de recuperación y cuenta.</span><span class="sxs-lookup"><span data-stu-id="0b35a-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="0b35a-204">[Aplicación de MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) este tutorial muestra cómo escribir una aplicación ASP.NET MVC 5 con Facebook y Google OAuth 2 la autorización.</span><span class="sxs-lookup"><span data-stu-id="0b35a-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="0b35a-205">También muestra cómo agregar datos adicionales a la base de datos de identidad.</span><span class="sxs-lookup"><span data-stu-id="0b35a-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="0b35a-206">[Implementar una aplicación ASP.NET MVC segura con pertenencia, OAuth y SQL Database en Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="0b35a-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="0b35a-207">Este tutorial agrega la implementación de Azure, cómo proteger su aplicación con los roles, cómo usar la API de pertenencia para agregar usuarios y roles y características de seguridad adicionales.</span><span class="sxs-lookup"><span data-stu-id="0b35a-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="0b35a-208">Creación de una aplicación de Google para OAuth 2 y conectarse al proyecto de la aplicación</span><span class="sxs-lookup"><span data-stu-id="0b35a-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="0b35a-209">Creación de la aplicación de Facebook y conectarse al proyecto de la aplicación</span><span class="sxs-lookup"><span data-stu-id="0b35a-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="0b35a-210">Configuración de SSL en el proyecto</span><span class="sxs-lookup"><span data-stu-id="0b35a-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
