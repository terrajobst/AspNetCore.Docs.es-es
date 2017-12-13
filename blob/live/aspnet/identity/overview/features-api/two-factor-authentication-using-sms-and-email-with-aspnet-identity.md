---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: "Autenticación en dos fases mediante SMS y correo electrónico con ASP.NET Identity | Documentos de Microsoft"
author: HaoK
description: "Este tutorial le mostrará cómo configurar la autenticación en dos fases (2FA) mediante correo electrónico y SMS. En este artículo se escribió Rick Anderson ( @RickAndMSFT ), por..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: ecb1fc693063995a3a05a7af5db64554c9f595e2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="c6d28-104">Autenticación en dos fases mediante SMS y correo electrónico con la identidad de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c6d28-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="c6d28-105">por [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="c6d28-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="c6d28-106">Este tutorial le mostrará cómo configurar la autenticación en dos fases (2FA) mediante correo electrónico y SMS.</span><span class="sxs-lookup"><span data-stu-id="c6d28-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="c6d28-107">En este artículo se escribió Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung y Suhas Joshi.</span><span class="sxs-lookup"><span data-stu-id="c6d28-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="c6d28-108">El ejemplo de NuGet escribió principalmente Hao Kung.</span><span class="sxs-lookup"><span data-stu-id="c6d28-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="c6d28-109">En este tema se trata lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c6d28-109">This topic covers the following:</span></span>

- [<span data-ttu-id="c6d28-110">Generar el ejemplo de identidad</span><span class="sxs-lookup"><span data-stu-id="c6d28-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="c6d28-111">Configurar SMS para la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="c6d28-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="c6d28-112">Habilitar la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="c6d28-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="c6d28-113">Cómo registrar un proveedor de autenticación de dos factores</span><span class="sxs-lookup"><span data-stu-id="c6d28-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="c6d28-114">Combinar las cuentas de inicio de sesión locales y redes sociales</span><span class="sxs-lookup"><span data-stu-id="c6d28-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="c6d28-115">Bloqueo de la cuenta de ataques por fuerza bruta</span><span class="sxs-lookup"><span data-stu-id="c6d28-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="c6d28-116">Generar el ejemplo de identidad</span><span class="sxs-lookup"><span data-stu-id="c6d28-116">Building the Identity sample</span></span>

<span data-ttu-id="c6d28-117">En esta sección, deberá usar NuGet para descargar un ejemplo con que vamos a trabajar.</span><span class="sxs-lookup"><span data-stu-id="c6d28-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="c6d28-118">Empiece por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="c6d28-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="c6d28-119">Instalar Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) o superior.</span><span class="sxs-lookup"><span data-stu-id="c6d28-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="c6d28-120">Advertencia: Debe instalar Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="c6d28-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="c6d28-121">Crear un nuevo ***vacía*** proyecto Web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c6d28-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="c6d28-122">En la consola de administrador de paquetes, escriba lo siguiente los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="c6d28-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
 <span data-ttu-id="c6d28-123">En este tutorial, usaremos [SendGrid](http://sendgrid.com/) para enviar correo electrónico y [Twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) de texto sms.</span><span class="sxs-lookup"><span data-stu-id="c6d28-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="c6d28-124">El `Identity.Samples` paquete instala el código que se va a trabajar.</span><span class="sxs-lookup"><span data-stu-id="c6d28-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="c6d28-125">Establecer el [proyecto para usar SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="c6d28-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="c6d28-126">*Opcional*: siga las instrucciones que aparecen en mi [tutorial de confirmación de correo electrónico](account-confirmation-and-password-recovery-with-aspnet-identity.md) enlazar SendGrid y, a continuación, ejecutar la aplicación y registrar una cuenta de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="c6d28-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="c6d28-127">* Opcional: * eliminar el código de confirmación del vínculo de correo electrónico de demostración de la muestra (el `ViewBag.Link` código en el controlador de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="c6d28-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="c6d28-128">Consulte la `DisplayEmail` y `ForgotPasswordConfirmation` métodos de acción y vistas de razor).</span><span class="sxs-lookup"><span data-stu-id="c6d28-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="c6d28-129">* Opcional: * quitar el `ViewBag.Status` código desde los controladores de administración y la cuenta y de la *Views\Account\VerifyCode.cshtml* y *Views\Manage\VerifyPhoneNumber.cshtml* vistas razor.</span><span class="sxs-lookup"><span data-stu-id="c6d28-129">*Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml* and *Views\Manage\VerifyPhoneNumber.cshtml* razor views.</span></span> <span data-ttu-id="c6d28-130">Como alternativa, puede mantener la `ViewBag.Status` presentación para probar cómo funciona esta aplicación localmente sin necesidad de enlazar y enviar correos electrónicos y mensajes SMS.</span><span class="sxs-lookup"><span data-stu-id="c6d28-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="c6d28-131">Advertencia: Si cambia cualquiera de las opciones de seguridad en este ejemplo, aplicaciones de ensayo deberá someterse a una auditoría de seguridad que se llame explícitamente a los cambios realizados.</span><span class="sxs-lookup"><span data-stu-id="c6d28-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="c6d28-132">Configurar SMS para la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="c6d28-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="c6d28-133">Este tutorial proporciona instrucciones para el uso de Twilio o ASPSMS, pero puede usar cualquier otro proveedor SMS.</span><span class="sxs-lookup"><span data-stu-id="c6d28-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="c6d28-134">**Crear una cuenta de usuario con un proveedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="c6d28-134">**Creating a User Account with an SMS provider**</span></span>  
  
 <span data-ttu-id="c6d28-135">Crear un [Twilio](https://www.twilio.com/try-twilio) o un [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) cuenta.</span><span class="sxs-lookup"><span data-stu-id="c6d28-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="c6d28-136">**Instalación de paquetes adicionales o agregar referencias de servicio**</span><span class="sxs-lookup"><span data-stu-id="c6d28-136">**Installing additional packages or adding service references**</span></span>  
  
 <span data-ttu-id="c6d28-137">Twilio:</span><span class="sxs-lookup"><span data-stu-id="c6d28-137">Twilio:</span></span>  
 <span data-ttu-id="c6d28-138">En la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="c6d28-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
 <span data-ttu-id="c6d28-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="c6d28-139">ASPSMS:</span></span>  
 <span data-ttu-id="c6d28-140">Es necesario agregar la siguiente referencia de servicio:</span><span class="sxs-lookup"><span data-stu-id="c6d28-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
 <span data-ttu-id="c6d28-141">Dirección:</span><span class="sxs-lookup"><span data-stu-id="c6d28-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 <span data-ttu-id="c6d28-142">Espacio de nombres:</span><span class="sxs-lookup"><span data-stu-id="c6d28-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="c6d28-143">**Pensar en las credenciales de usuario del proveedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="c6d28-143">**Figuring out SMS Provider User credentials**</span></span>  
  
 <span data-ttu-id="c6d28-144">Twilio:</span><span class="sxs-lookup"><span data-stu-id="c6d28-144">Twilio:</span></span>  
 <span data-ttu-id="c6d28-145">Desde el **panel** ficha de la cuenta de Twilio, copia el **SID de cuenta** y **token de autenticación**.</span><span class="sxs-lookup"><span data-stu-id="c6d28-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
 <span data-ttu-id="c6d28-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="c6d28-146">ASPSMS:</span></span>  
 <span data-ttu-id="c6d28-147">Desde la configuración de su cuenta, vaya a **clave de usuario confidenciales** y copie junto con su propio definido **contraseña**.</span><span class="sxs-lookup"><span data-stu-id="c6d28-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
 <span data-ttu-id="c6d28-148">Más adelante almacenaremos estos valores en las variables `SMSAccountIdentification` y `SMSAccountPassword` .</span><span class="sxs-lookup"><span data-stu-id="c6d28-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="c6d28-149">**Especifica el identificador del remitente / originador**</span><span class="sxs-lookup"><span data-stu-id="c6d28-149">**Specifying SenderID / Originator**</span></span>  
  
 <span data-ttu-id="c6d28-150">Twilio:</span><span class="sxs-lookup"><span data-stu-id="c6d28-150">Twilio:</span></span>  
 <span data-ttu-id="c6d28-151">Desde el **números** ficha, copie el número de teléfono Twilio.</span><span class="sxs-lookup"><span data-stu-id="c6d28-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
 <span data-ttu-id="c6d28-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="c6d28-152">ASPSMS:</span></span>  
 <span data-ttu-id="c6d28-153">En el **desbloquear remitentes** menú, desbloquear uno o más remitentes o elija un originador alfanumérico (no admite todas las redes).</span><span class="sxs-lookup"><span data-stu-id="c6d28-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
 <span data-ttu-id="c6d28-154">Se almacenará más adelante este valor en la variable `SMSAccountFrom` .</span><span class="sxs-lookup"><span data-stu-id="c6d28-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="c6d28-155">**Transferir las credenciales del proveedor SMS en la aplicación**</span><span class="sxs-lookup"><span data-stu-id="c6d28-155">**Transferring SMS provider credentials into app**</span></span>  
  
 <span data-ttu-id="c6d28-156">Disponer de las credenciales y el número de teléfono del remitente a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="c6d28-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="c6d28-157">Seguridad - nunca almacenar los datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="c6d28-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="c6d28-158">La cuenta y las credenciales se agregan al código anterior para simplificar el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="c6d28-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="c6d28-159">Consulte de Jon Atten [MVC de ASP.NET: mantener privados configuración fuera del Control de código fuente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span><span class="sxs-lookup"><span data-stu-id="c6d28-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="c6d28-160">**Implementación de transferencia de datos al proveedor SMS**</span><span class="sxs-lookup"><span data-stu-id="c6d28-160">**Implementation of data transfer to SMS provider**</span></span>  
  
 <span data-ttu-id="c6d28-161">Configurar la `SmsService` clase en el *aplicación\_Start\IdentityConfig.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="c6d28-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
 <span data-ttu-id="c6d28-162">En función del proveedor SMS usado activar cualquiera el **Twilio** o **ASPSMS** sección:</span><span class="sxs-lookup"><span data-stu-id="c6d28-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="c6d28-163">Ejecutar la aplicación e inicie sesión con la cuenta que se registraron anteriormente.</span><span class="sxs-lookup"><span data-stu-id="c6d28-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="c6d28-164">Haga clic en el identificador de usuario, que activa el `Index` método de acción en `Manage` controlador.</span><span class="sxs-lookup"><span data-stu-id="c6d28-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="c6d28-165">Haga clic en Agregar.</span><span class="sxs-lookup"><span data-stu-id="c6d28-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="c6d28-166">En unos segundos, obtendrá un mensaje de texto con el código de comprobación.</span><span class="sxs-lookup"><span data-stu-id="c6d28-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="c6d28-167">Escriba y presione **enviar**.</span><span class="sxs-lookup"><span data-stu-id="c6d28-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="c6d28-168">La vista gestionar muestra que el número de teléfono se ha agregado.</span><span class="sxs-lookup"><span data-stu-id="c6d28-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="c6d28-169">Examine el código</span><span class="sxs-lookup"><span data-stu-id="c6d28-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="c6d28-170">El `Index` método de acción en `Manage` controlador establece el mensaje de estado en función de la acción anterior y se proporcionan vínculos para cambiar la contraseña local o agregar una cuenta local.</span><span class="sxs-lookup"><span data-stu-id="c6d28-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="c6d28-171">El `Index` método también muestra el estado o la 2FA número, inicios de sesión externos, 2FA habilitado por teléfono y recuerde 2FA método para este explorador (se explica más adelante).</span><span class="sxs-lookup"><span data-stu-id="c6d28-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="c6d28-172">Al hacer clic en el identificador del usuario (correo electrónico) en la barra de título no pasa un mensaje.</span><span class="sxs-lookup"><span data-stu-id="c6d28-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="c6d28-173">Al hacer clic en el **número de teléfono: quitar** enlace pasadas `Message=RemovePhoneSuccess` como una cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="c6d28-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="c6d28-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="c6d28-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="c6d28-175">El `AddPhoneNumber` método de acción muestra un cuadro de diálogo para especificar un número de teléfono que puede recibir mensajes SMS.</span><span class="sxs-lookup"><span data-stu-id="c6d28-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="c6d28-176">Al hacer clic en el **enviar código de comprobación** botón devuelve el número de teléfono para HTTP POST `AddPhoneNumber` método de acción.</span><span class="sxs-lookup"><span data-stu-id="c6d28-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="c6d28-177">El `GenerateChangePhoneNumberTokenAsync` método genera el token de seguridad que se establecerá en el mensaje SMS.</span><span class="sxs-lookup"><span data-stu-id="c6d28-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="c6d28-178">Si se ha configurado el servicio SMS, el token se envía como la cadena &quot;es el código de seguridad &lt;token&gt;&quot;.</span><span class="sxs-lookup"><span data-stu-id="c6d28-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="c6d28-179">El `SmsService.SendAsync` método al que se llama de forma asincrónica, la aplicación se redirigirá a la `VerifyPhoneNumber` método de acción (que muestra el siguiente cuadro de diálogo), donde puede escribir el código de comprobación.</span><span class="sxs-lookup"><span data-stu-id="c6d28-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="c6d28-180">Una vez que escriba el código y haga clic en enviar, se registra el código para HTTP POST `VerifyPhoneNumber` método de acción.</span><span class="sxs-lookup"><span data-stu-id="c6d28-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="c6d28-181">El `ChangePhoneNumberAsync` método comprueba el código de seguridad expuesto.</span><span class="sxs-lookup"><span data-stu-id="c6d28-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="c6d28-182">Si el código es correcto, el número de teléfono se agrega a la `PhoneNumber` campo de la `AspNetUsers` tabla.</span><span class="sxs-lookup"><span data-stu-id="c6d28-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="c6d28-183">Si esa llamada se realiza correctamente, el `SignInAsync` se llama al método:</span><span class="sxs-lookup"><span data-stu-id="c6d28-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="c6d28-184">El `isPersistent` parámetro establece si la sesión de autenticación se conserva entre varias solicitudes.</span><span class="sxs-lookup"><span data-stu-id="c6d28-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="c6d28-185">Cuando se cambia el perfil de seguridad, un nuevo sello de seguridad se generan y almacenan en la `SecurityStamp` campo de la *AspNetUsers* tabla.</span><span class="sxs-lookup"><span data-stu-id="c6d28-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="c6d28-186">Tenga en cuenta que el `SecurityStamp` campo es diferente de la cookie de seguridad.</span><span class="sxs-lookup"><span data-stu-id="c6d28-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="c6d28-187">La cookie de seguridad no se almacena en la `AspNetUsers` tabla (o cualquier otra estructura en la base de datos de identidad en).</span><span class="sxs-lookup"><span data-stu-id="c6d28-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="c6d28-188">El token de cookie de seguridad es autofirmado mediante [DPAPI](https://msdn.microsoft.com/en-us/library/system.security.cryptography.protecteddata.aspx) y se crea con el `UserId, SecurityStamp` e información de tiempo de expiración.</span><span class="sxs-lookup"><span data-stu-id="c6d28-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/en-us/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="c6d28-189">El middleware de cookies, comprueba la cookie en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="c6d28-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="c6d28-190">El `SecurityStampValidator` método en el `Startup` clase llega a la base de datos y comprueba el sello de seguridad de forma periódica, según se haya especificado con el `validateInterval`.</span><span class="sxs-lookup"><span data-stu-id="c6d28-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="c6d28-191">Esto sólo ocurre cada 30 minutos (en nuestro ejemplo) a menos que cambie el perfil de seguridad.</span><span class="sxs-lookup"><span data-stu-id="c6d28-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="c6d28-192">El intervalo de 30 minutos se ha elegido para reducir los viajes a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c6d28-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="c6d28-193">El `SignInAsync` método debe llamarse cuando se realiza cualquier cambio en el perfil de seguridad.</span><span class="sxs-lookup"><span data-stu-id="c6d28-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="c6d28-194">Cuando se cambia el perfil de seguridad, la base de datos es de actualizaciones el `SecurityStamp` campo y sin llamar a la `SignInAsync` se mantiene la sesión en el método *solo* hasta la próxima vez que la canalización OWIN llega a la base de datos (el `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="c6d28-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="c6d28-195">Puede probar esto cambiando la `SignInAsync` método para devolver resultados inmediatamente y establecer la cookie `validateInterval` propiedad de 30 minutos en 5 segundos:</span><span class="sxs-lookup"><span data-stu-id="c6d28-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="c6d28-196">Con los cambios de código anterior, puede cambiar el perfil de seguridad (por ejemplo, al cambiar el estado de **dos fases habilitado**) y se registra la en 5 segundos cuando la `SecurityStampValidator.OnValidateIdentity` método se produce un error.</span><span class="sxs-lookup"><span data-stu-id="c6d28-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="c6d28-197">Quite la línea de retorno en la `SignInAsync` (método), cambiar otro perfil de seguridad y no se registrarán. El `SignInAsync` método genera una nueva cookie de seguridad.</span><span class="sxs-lookup"><span data-stu-id="c6d28-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="c6d28-198">Habilitar la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="c6d28-198">Enable two-factor authentication</span></span>

<span data-ttu-id="c6d28-199">En la aplicación de ejemplo, debe usar la interfaz de usuario para habilitar la autenticación en dos fases (2FA).</span><span class="sxs-lookup"><span data-stu-id="c6d28-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="c6d28-200">Para habilitar 2FA, haga clic en el identificador de usuario (alias de correo electrónico) en la barra de navegación.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="c6d28-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="c6d28-201">Haga clic en Habilitar 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="c6d28-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="c6d28-202">Cierre sesión y, a continuación, volver a iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="c6d28-202">Log out, then log back in.</span></span> <span data-ttu-id="c6d28-203">Si ha habilitado el correo electrónico (consulte mi [tutorial anterior](account-confirmation-and-password-recovery-with-aspnet-identity.md)), puede seleccionar el SMS o correo electrónico para 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="c6d28-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="c6d28-204">Se muestra la página de códigos comprobar donde puede escribir el código (de SMS o correo electrónico).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="c6d28-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="c6d28-205">Al hacer clic en el **recordar este explorador** casilla de verificación se excluya de la necesidad de usar 2FA para iniciar sesión con ese equipo y el explorador.</span><span class="sxs-lookup"><span data-stu-id="c6d28-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="c6d28-206">Habilitar 2FA y haciendo clic en el **recordar este explorador** le proporcionará protección segura 2FA de usuarios malintencionados que intenta acceder a su cuenta, siempre y cuando no tienen acceso a su equipo.</span><span class="sxs-lookup"><span data-stu-id="c6d28-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="c6d28-207">Puede hacerlo en cualquier equipo privado que se usan con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="c6d28-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="c6d28-208">Estableciendo **recordar este explorador**, obtendrá la seguridad añadida de 2FA de equipos que no use con regularidad y obtener la comodidad de no tener que pasar por 2FA en sus propios equipos.</span><span class="sxs-lookup"><span data-stu-id="c6d28-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="c6d28-209">Cómo registrar un proveedor de autenticación de dos factores</span><span class="sxs-lookup"><span data-stu-id="c6d28-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="c6d28-210">Cuando se crea un nuevo proyecto MVC, el *IdentityConfig.cs* archivo contiene el código siguiente para registrar un proveedor de autenticación de dos factores:</span><span class="sxs-lookup"><span data-stu-id="c6d28-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="c6d28-211">Agregar un número de teléfono para 2FA</span><span class="sxs-lookup"><span data-stu-id="c6d28-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="c6d28-212">El `AddPhoneNumber` método de acción en el `Manage` controlador genera un token de seguridad y la envía para el teléfono número que se ha proporcionado.</span><span class="sxs-lookup"><span data-stu-id="c6d28-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="c6d28-213">Después de enviar el token, redirige a la `VerifyPhoneNumber` método de acción, donde puede escribir el código para registrar SMS para 2FA.</span><span class="sxs-lookup"><span data-stu-id="c6d28-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="c6d28-214">No se utiliza SMS 2FA hasta que haya comprobado el número de teléfono.</span><span class="sxs-lookup"><span data-stu-id="c6d28-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="c6d28-215">Habilitar 2FA</span><span class="sxs-lookup"><span data-stu-id="c6d28-215">Enabling 2FA</span></span>

<span data-ttu-id="c6d28-216">El `EnableTFA` método de acción permite 2FA:</span><span class="sxs-lookup"><span data-stu-id="c6d28-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="c6d28-217">Tenga en cuenta el `SignInAsync` debe llamarse porque habilitar 2FA es un cambio en el perfil de seguridad.</span><span class="sxs-lookup"><span data-stu-id="c6d28-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="c6d28-218">Cuando 2FA está habilitada, el usuario deberá usar 2FA para iniciar sesión, utilizando los métodos de 2FA se hayan registrado (SMS y correo electrónico en el ejemplo).</span><span class="sxs-lookup"><span data-stu-id="c6d28-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="c6d28-219">Puede agregar más proveedores 2FA como generadores de código QR o puede escribir su propiedad (vea [usando Google Authenticator con ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span><span class="sxs-lookup"><span data-stu-id="c6d28-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="c6d28-220">Los códigos de 2FA se generan mediante [algoritmo de contraseña de un solo uso basado en tiempo](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) y códigos son válidos durante seis minutos.</span><span class="sxs-lookup"><span data-stu-id="c6d28-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="c6d28-221">Si tiene más de seis minutos para escribir el código, obtendrá un mensaje de error de código no válido.</span><span class="sxs-lookup"><span data-stu-id="c6d28-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="c6d28-222">Combinar las cuentas de inicio de sesión locales y redes sociales</span><span class="sxs-lookup"><span data-stu-id="c6d28-222">Combine social and local login accounts</span></span>

<span data-ttu-id="c6d28-223">Puede combinar cuentas locales y redes sociales, haga clic en el vínculo de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="c6d28-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="c6d28-224">En la siguiente secuencia &quot; RickAndMSFT@gmail.com &quot; en primer lugar se crea como un inicio de sesión local, pero puede crear la cuenta como un inicio de sesión social primero y luego agregar un inicio de sesión local.</span><span class="sxs-lookup"><span data-stu-id="c6d28-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="c6d28-225">Haga clic en el **administrar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="c6d28-225">Click on the **Manage** link.</span></span> <span data-ttu-id="c6d28-226">Tenga en cuenta el externo 0 (inicios de sesión sociales) asociados con esta cuenta.</span><span class="sxs-lookup"><span data-stu-id="c6d28-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="c6d28-227">Haga clic en el vínculo en otro registro de servicio y Aceptar las solicitudes de aplicación.</span><span class="sxs-lookup"><span data-stu-id="c6d28-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="c6d28-228">Se han combinado las dos cuentas, podrá iniciar sesión con cualquiera de estas cuentas.</span><span class="sxs-lookup"><span data-stu-id="c6d28-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="c6d28-229">Puede que los usuarios agreguen cuentas locales en caso de que su registro sociales en servicio de autenticación está inactivo o, más probable es que ha perdido el acceso a su cuenta sociales.</span><span class="sxs-lookup"><span data-stu-id="c6d28-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="c6d28-230">En la siguiente imagen, Tom es un inicio de sesión social (que puede ver desde el **inicios de sesión externos: 1** se muestra en la página).</span><span class="sxs-lookup"><span data-stu-id="c6d28-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="c6d28-231">Al hacer clic en **elegir una contraseña** le permite agregar un registro local en asociados con la misma cuenta.</span><span class="sxs-lookup"><span data-stu-id="c6d28-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="c6d28-232">Bloqueo de la cuenta de ataques por fuerza bruta</span><span class="sxs-lookup"><span data-stu-id="c6d28-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="c6d28-233">Puede proteger las cuentas en la aplicación frente a ataques de diccionario habilitando el bloqueo de usuario.</span><span class="sxs-lookup"><span data-stu-id="c6d28-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="c6d28-234">El siguiente código en el `ApplicationUserManager Create` método configura de bloqueo:</span><span class="sxs-lookup"><span data-stu-id="c6d28-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="c6d28-235">El código anterior permite el bloqueo para la autenticación en dos fases solo.</span><span class="sxs-lookup"><span data-stu-id="c6d28-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="c6d28-236">Si bien puede permitir bloqueo de inicios de sesión cambiando `shouldLockout` en true en la `Login` método de controlador de la cuenta, se recomienda no habilitar de bloqueo para inicios de sesión ya que hace que la cuenta sea susceptible a [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) inicio de sesión los ataques.</span><span class="sxs-lookup"><span data-stu-id="c6d28-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="c6d28-237">En el código de ejemplo, el bloqueo está deshabilitado para la cuenta de administrador que se crea en el `ApplicationDbInitializer Seed` método:</span><span class="sxs-lookup"><span data-stu-id="c6d28-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="c6d28-238">Obligar al usuario a tiene una cuenta de correo electrónico validado</span><span class="sxs-lookup"><span data-stu-id="c6d28-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="c6d28-239">El código siguiente requiere que un usuario tiene una cuenta de correo electrónico validada antes de poder iniciar sesión:</span><span class="sxs-lookup"><span data-stu-id="c6d28-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="c6d28-240">¿Cómo SignInManager busca 2FA requisito</span><span class="sxs-lookup"><span data-stu-id="c6d28-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="c6d28-241">El inicio de sesión local tanto el registro sociales de comprobación para ver si 2FA está habilitado.</span><span class="sxs-lookup"><span data-stu-id="c6d28-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="c6d28-242">Si está habilitado 2FA, la `SignInManager` devuelve el método de inicio de sesión `SignInStatus.RequiresVerification`, y el usuario será redirigido a la `SendCode` método de acción, donde tendrán que escribir el código para completar el inicio de sesión en secuencia.</span><span class="sxs-lookup"><span data-stu-id="c6d28-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="c6d28-243">Si el usuario tiene RememberMe se establece en la cookie local del usuario, el `SignInManager` devolverá `SignInStatus.Success` y no tendrá que pasar por 2FA.</span><span class="sxs-lookup"><span data-stu-id="c6d28-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="c6d28-244">El código siguiente muestra el `SendCode` método de acción.</span><span class="sxs-lookup"><span data-stu-id="c6d28-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="c6d28-245">A [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) se crea con todos los métodos de 2FA habilitados para el usuario.</span><span class="sxs-lookup"><span data-stu-id="c6d28-245">A [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="c6d28-246">El [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) se pasa a la [DropDownListFor](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.aspx) aplicación auxiliar, lo que permite al usuario seleccionar el enfoque de 2FA (normalmente el correo electrónico y SMS).</span><span class="sxs-lookup"><span data-stu-id="c6d28-246">The [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="c6d28-247">Una vez que el usuario envía el enfoque 2FA, la `HTTP POST SendCode` se llama el método de acción, el `SignInManager` envía el código de 2FA y el usuario se redirige a la `VerifyCode` método de acción en el que pueden especificar el código para completar el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="c6d28-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="c6d28-248">Bloqueo de 2FA</span><span class="sxs-lookup"><span data-stu-id="c6d28-248">2FA Lockout</span></span>

<span data-ttu-id="c6d28-249">Aunque puede establecer el bloqueo de cuenta en los errores de intento de contraseña de inicio de sesión, este enfoque hace susceptible a su inicio de sesión [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) bloqueos.</span><span class="sxs-lookup"><span data-stu-id="c6d28-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="c6d28-250">Se recomienda que usar el bloqueo de cuenta con 2FA.</span><span class="sxs-lookup"><span data-stu-id="c6d28-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="c6d28-251">Cuando el `ApplicationUserManager` está creado, el código de ejemplo establece bloqueo 2FA y `MaxFailedAccessAttemptsBeforeLockout` a cinco.</span><span class="sxs-lookup"><span data-stu-id="c6d28-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="c6d28-252">Una vez que un usuario inicia sesión (a través de cuenta local o sociales), se almacena cada intento fallido en 2FA y si se alcanza el número máximo de intentos, el usuario se bloquea durante cinco minutos (puede establecer el bloqueo de hora con `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="c6d28-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="c6d28-253">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c6d28-253">Additional Resources</span></span>

- <span data-ttu-id="c6d28-254">[ASP.NET Identity recomienda recursos](../getting-started/aspnet-identity-recommended-resources.md) por lo que vincula la lista completa de identidad blogs, vídeos, tutoriales y muy bien.</span><span class="sxs-lookup"><span data-stu-id="c6d28-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="c6d28-255">[Aplicación de MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) también muestra cómo agregar información de perfil a la tabla de usuarios.</span><span class="sxs-lookup"><span data-stu-id="c6d28-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="c6d28-256">[ASP.NET MVC e identidad 2.0: comprende los fundamentos](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) por John Atten.</span><span class="sxs-lookup"><span data-stu-id="c6d28-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="c6d28-257">Confirmación de cuenta y contraseña de recuperación con la identidad de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c6d28-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="c6d28-258">Introducción a la identidad de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c6d28-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="c6d28-259">[Anuncio de RTM de ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) por Pranav Rastogi.</span><span class="sxs-lookup"><span data-stu-id="c6d28-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="c6d28-260">[Identidad de ASP.NET 2.0: Configurar la validación de la cuenta y la autorización de dos fases](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) por John Atten.</span><span class="sxs-lookup"><span data-stu-id="c6d28-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>
