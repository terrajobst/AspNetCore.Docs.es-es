---
title: Autenticación en dos fases con SMS en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo configurar la autenticación en dos fases (2FA) con una aplicación ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: mvc, seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 424d21e446de02b39daa7685205a00931361b326
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652727"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="e2ad2-103">Autenticación en dos fases con SMS en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2ad2-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="e2ad2-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Suiza-desarrolladores](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="e2ad2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

>[!WARNING]
> <span data-ttu-id="e2ad2-105">Dos aplicaciones de autenticador de autenticación (2FA) de factor, con una duración definida por única vez contraseña algoritmo (TOTP), son el enfoque para 2FA recomendado en el sector.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="e2ad2-106">2FA uso TOTP es preferible a SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="e2ad2-107">Para obtener más información, consulte [habilitación de la generación de código QR para aplicaciones TOTP Authenticator en ASP.net Core](xref:security/authentication/identity-enable-qrcodes) para ASP.net Core 2,0 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="e2ad2-108">Este tutorial muestra cómo configurar la autenticación en dos fases (2FA) mediante SMS.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="e2ad2-109">Se proporcionan instrucciones para [Twilio](https://www.twilio.com/) y [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), pero puede usar cualquier otro proveedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="e2ad2-110">Se recomienda completar la [confirmación de la cuenta y la recuperación de la contraseña antes de](xref:security/authentication/accconfirm) iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="e2ad2-111">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="e2ad2-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="e2ad2-112">[Cómo descargar](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="e2ad2-112">[How to download](xref:index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="e2ad2-113">Crear un nuevo proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2ad2-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="e2ad2-114">Cree una nueva aplicación Web de ASP.NET Core denominada `Web2FA` con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="e2ad2-115">Siga las instrucciones de <xref:security/enforcing-ssl> para configurar y requerir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-115">Follow the instructions in <xref:security/enforcing-ssl> to set up and require HTTPS.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="e2ad2-116">Crear una cuenta SMS</span><span class="sxs-lookup"><span data-stu-id="e2ad2-116">Create an SMS account</span></span>

<span data-ttu-id="e2ad2-117">Cree una cuenta de SMS, por ejemplo, de [Twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="e2ad2-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="e2ad2-118">Registre las credenciales de autenticación (twilio: accountSid y authToken para ASPSMS: Userkey y la contraseña).</span><span class="sxs-lookup"><span data-stu-id="e2ad2-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="e2ad2-119">Averiguar las credenciales del proveedor de SMS</span><span class="sxs-lookup"><span data-stu-id="e2ad2-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="e2ad2-120">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="e2ad2-120">**Twilio:**</span></span>

<span data-ttu-id="e2ad2-121">En la pestaña panel de la cuenta de Twilio, copie el SID de la **cuenta** y el **token de autenticación**.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="e2ad2-122">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="e2ad2-122">**ASPSMS:**</span></span>

<span data-ttu-id="e2ad2-123">En la configuración de la cuenta, vaya a **Userkey** y cópiela junto con la **contraseña**.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="e2ad2-124">Más adelante almacenaremos estos valores en con la herramienta de administrador de secretos dentro de las claves `SMSAccountIdentification` y `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="e2ad2-125">Especifica el Id. de remitente / originador</span><span class="sxs-lookup"><span data-stu-id="e2ad2-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="e2ad2-126">**Twilio:** En la pestaña números, copie el **número de teléfono**de Twilio.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-126">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="e2ad2-127">**ASPSMS:** En el menú desbloquear orígenes, desbloquee uno o más originadores o elija un originador alfanumérico (no admitido por todas las redes).</span><span class="sxs-lookup"><span data-stu-id="e2ad2-127">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="e2ad2-128">Más adelante se almacenará este valor con la herramienta de administración de secretos en el `SMSAccountFrom`de claves.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>

### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="e2ad2-129">Proporcione las credenciales para el servicio SMS</span><span class="sxs-lookup"><span data-stu-id="e2ad2-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="e2ad2-130">Usaremos el [patrón de opciones](xref:fundamentals/configuration/options) para tener acceso a la configuración de clave y cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

* <span data-ttu-id="e2ad2-131">Cree una clase para capturar la clave segura de SMS.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="e2ad2-132">En este ejemplo, la clase `SMSoptions` se crea en el archivo *Services/SMSoptions. CS* .</span><span class="sxs-lookup"><span data-stu-id="e2ad2-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="e2ad2-133">Establezca el `SMSAccountIdentification`, `SMSAccountPassword` y `SMSAccountFrom` con la [herramienta de administración de secretos](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="e2ad2-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="e2ad2-134">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e2ad2-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* <span data-ttu-id="e2ad2-135">Agregue el paquete NuGet del proveedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="e2ad2-136">Desde el Administrador de consola paquetes (PMC) ejecute:</span><span class="sxs-lookup"><span data-stu-id="e2ad2-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="e2ad2-137">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="e2ad2-137">**Twilio:**</span></span>

`Install-Package Twilio`

<span data-ttu-id="e2ad2-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="e2ad2-138">**ASPSMS:**</span></span>

`Install-Package ASPSMS`

* <span data-ttu-id="e2ad2-139">Agregue código en el archivo *Services/MessageServices. CS* para habilitar SMS.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="e2ad2-140">Utilice el Twilio o la sección ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="e2ad2-140">Use either the Twilio or the ASPSMS section:</span></span>

<span data-ttu-id="e2ad2-141">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="e2ad2-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="e2ad2-142">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="e2ad2-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="e2ad2-143">Configurar el inicio para usar `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="e2ad2-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="e2ad2-144">Agregue `SMSoptions` al contenedor de servicios en el método `ConfigureServices` en *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="e2ad2-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="e2ad2-145">Habilitar la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="e2ad2-145">Enable two-factor authentication</span></span>

<span data-ttu-id="e2ad2-146">Abra el archivo de vista de Razor *views/Manage/index. cshtml* y quite los caracteres de comentario (por lo que no hay marcado como comentario).</span><span class="sxs-lookup"><span data-stu-id="e2ad2-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commented out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="e2ad2-147">Inicie sesión con autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="e2ad2-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="e2ad2-148">Ejecute la aplicación y registrar un nuevo usuario</span><span class="sxs-lookup"><span data-stu-id="e2ad2-148">Run the app and register a new user</span></span>

![Vista de registro de aplicación Web abierta en Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="e2ad2-150">Puntee en el nombre de usuario, que activa el método de acción `Index` del controlador de administración.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="e2ad2-151">A continuación, puntee en el vínculo número de teléfono para **Agregar** .</span><span class="sxs-lookup"><span data-stu-id="e2ad2-151">Then tap the phone number **Add** link.</span></span>

![Vista de administración: pulse el vínculo "Agregar"](2fa/_static/login2fa2.png)

* <span data-ttu-id="e2ad2-153">Agregue un número de teléfono que recibirá el código de verificación y pulse **Enviar código de verificación**.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Agregar página de número de teléfono](2fa/_static/login2fa3.png)

* <span data-ttu-id="e2ad2-155">Obtendrá un mensaje de texto con el código de verificación.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="e2ad2-156">Escríbalo y pulse **Enviar** .</span><span class="sxs-lookup"><span data-stu-id="e2ad2-156">Enter it and tap **Submit**</span></span>

![Compruebe la página de número de teléfono](2fa/_static/login2fa4.png)

<span data-ttu-id="e2ad2-158">Si no recibe un mensaje de texto, consulte la página de registro de twilio.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="e2ad2-159">La vista de administración se muestra que el número de teléfono se agregó correctamente.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-159">The Manage view shows your phone number was added successfully.</span></span>

![Administrar vista - número de teléfono se agregó correctamente](2fa/_static/login2fa5.png)

* <span data-ttu-id="e2ad2-161">Pulse **Habilitar** para habilitar la autenticación en dos fases.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Vista de administración: habilitar la autenticación en dos fases](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="e2ad2-163">Autenticación de dos fases de prueba</span><span class="sxs-lookup"><span data-stu-id="e2ad2-163">Test two-factor authentication</span></span>

* <span data-ttu-id="e2ad2-164">Cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-164">Log off.</span></span>

* <span data-ttu-id="e2ad2-165">Inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-165">Log in.</span></span>

* <span data-ttu-id="e2ad2-166">La cuenta de usuario habilitó la autenticación en dos fases, por lo que debe proporcionar el segundo factor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="e2ad2-167">En este tutorial se ha habilitado la verificación por teléfono.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="e2ad2-168">Las plantillas integradas permiten configurar el correo electrónico como segundo factor.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="e2ad2-169">Puede configurar factores adicionales de segundo para la autenticación como códigos QR.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="e2ad2-170">Pulse **submit (enviar**).</span><span class="sxs-lookup"><span data-stu-id="e2ad2-170">Tap **Submit**.</span></span>

![Enviar la vista de código de verificación](2fa/_static/login2fa7.png)

* <span data-ttu-id="e2ad2-172">Escriba el código que se obtiene en el mensaje SMS.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="e2ad2-173">Al hacer clic en la casilla **recordar este explorador** , no es necesario usar 2FA para iniciar sesión cuando se usa el mismo dispositivo y explorador.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="e2ad2-174">Al habilitar 2FA y hacer clic en **recordar, este explorador** le proporcionará protección de 2FA segura a los usuarios malintencionados que intentan acceder a su cuenta, siempre y cuando no tengan acceso a su dispositivo.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="e2ad2-175">Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="e2ad2-176">Al establecer la opción **recordar este explorador**, obtendrá la seguridad agregada de 2FA de los dispositivos que no use con regularidad y que le resulte más cómodo no tener que ir a través de 2FA en sus propios dispositivos.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Comprobar la vista](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="e2ad2-178">Bloqueo de cuenta para protegerse contra los ataques por fuerza bruta</span><span class="sxs-lookup"><span data-stu-id="e2ad2-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="e2ad2-179">Se recomienda el bloqueo de cuenta con 2FA.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="e2ad2-180">Una vez que un usuario inicia sesión a través de una cuenta local o social, se almacena cada intento incorrecto en 2FA.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="e2ad2-181">Si se alcanza los intentos de acceso con error máximo, el usuario está bloqueado (valor predeterminado: bloqueo de 5 minutos después de 5 intentos de acceso).</span><span class="sxs-lookup"><span data-stu-id="e2ad2-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="e2ad2-182">Una autenticación correcta restablece el número de intentos de acceso erróneos y restablece el reloj.</span><span class="sxs-lookup"><span data-stu-id="e2ad2-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="e2ad2-183">El tiempo máximo de bloqueo y los intentos de acceso incorrectos se pueden establecer con [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) y [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="e2ad2-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="e2ad2-184">El siguiente ejemplo configura el bloqueo de cuentas durante 10 minutos tras 10 intentos de acceso:</span><span class="sxs-lookup"><span data-stu-id="e2ad2-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="e2ad2-185">Confirme que [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) establece `lockoutOnFailure` en `true`:</span><span class="sxs-lookup"><span data-stu-id="e2ad2-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
