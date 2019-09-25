---
title: Autenticación en dos fases con SMS en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo configurar la autenticación en dos fases (2FA) con una aplicación ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: mvc, seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 68219579be9b7a7b25da6e348054e1ff2015cf5f
ms.sourcegitcommit: e54672f5c493258dc449fac5b98faf47eb123b28
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71248382"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="0e069-103">Autenticación en dos fases con SMS en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e069-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="0e069-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [suizo Devs](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="0e069-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

>[!WARNING]
> <span data-ttu-id="0e069-105">Dos aplicaciones de autenticador de autenticación (2FA) de factor, con una duración definida por única vez contraseña algoritmo (TOTP), son el enfoque para 2FA recomendado en el sector.</span><span class="sxs-lookup"><span data-stu-id="0e069-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="0e069-106">2FA uso TOTP es preferible a SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="0e069-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="0e069-107">Para obtener más información, consulte [generación habilitar código QR para las aplicaciones de authenticator TOTP en ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) para ASP.NET Core 2.0 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="0e069-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="0e069-108">Este tutorial muestra cómo configurar la autenticación en dos fases (2FA) mediante SMS.</span><span class="sxs-lookup"><span data-stu-id="0e069-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="0e069-109">Se proporcionan instrucciones para [twilio](https://www.twilio.com/) y [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), pero puede usar cualquier otro proveedor SMS.</span><span class="sxs-lookup"><span data-stu-id="0e069-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="0e069-110">Se recomienda realizar [confirmación de la cuenta y contraseña de recuperación](xref:security/authentication/accconfirm) antes de comenzar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="0e069-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="0e069-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="0e069-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="0e069-112">[Cómo descargar](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="0e069-112">[How to download](xref:index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="0e069-113">Crear un nuevo proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e069-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="0e069-114">Crear una nueva aplicación web de ASP.NET Core denominada `Web2FA` con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="0e069-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="0e069-115">Siga las instrucciones de <xref:security/enforcing-ssl> para configurar y requerir https.</span><span class="sxs-lookup"><span data-stu-id="0e069-115">Follow the instructions in <xref:security/enforcing-ssl> to set up and require HTTPS.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="0e069-116">Crear una cuenta SMS</span><span class="sxs-lookup"><span data-stu-id="0e069-116">Create an SMS account</span></span>

<span data-ttu-id="0e069-117">Crear una cuenta SMS, por ejemplo, desde [twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="0e069-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="0e069-118">Grabe las credenciales de autenticación (para Twilio: accountSid y authToken, para ASPSMS: Userkey y password).</span><span class="sxs-lookup"><span data-stu-id="0e069-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="0e069-119">Averiguar las credenciales del proveedor de SMS</span><span class="sxs-lookup"><span data-stu-id="0e069-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="0e069-120">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="0e069-120">**Twilio:**</span></span>

<span data-ttu-id="0e069-121">En la pestaña panel de la cuenta de Twilio, copie el SID de la **cuenta** y el **token de autenticación**.</span><span class="sxs-lookup"><span data-stu-id="0e069-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="0e069-122">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="0e069-122">**ASPSMS:**</span></span>

<span data-ttu-id="0e069-123">En la configuración de la cuenta, vaya a **Userkey** y cópiela junto con la **contraseña**.</span><span class="sxs-lookup"><span data-stu-id="0e069-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="0e069-124">Más adelante, almacenaremos estos valores con la herramienta Administrador de secretos en el conjunto de claves `SMSAccountIdentification` y `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="0e069-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="0e069-125">Especifica el Id. de remitente / originador</span><span class="sxs-lookup"><span data-stu-id="0e069-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="0e069-126">**Twilio** En la pestaña números, copie el **número de teléfono**de Twilio.</span><span class="sxs-lookup"><span data-stu-id="0e069-126">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="0e069-127">**ASPSMS:** En el menú desbloquear orígenes, desbloquee uno o más originadores o elija un originador alfanumérico (no admitido por todas las redes).</span><span class="sxs-lookup"><span data-stu-id="0e069-127">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="0e069-128">Más adelante se almacenará este valor con la herramienta secret manager dentro de la clave `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="0e069-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>

### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="0e069-129">Proporcione las credenciales para el servicio SMS</span><span class="sxs-lookup"><span data-stu-id="0e069-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="0e069-130">Vamos a usar el [patrón de opciones](xref:fundamentals/configuration/options) para tener acceso a la configuración de cuenta y clave de usuario.</span><span class="sxs-lookup"><span data-stu-id="0e069-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

* <span data-ttu-id="0e069-131">Cree una clase para capturar la clave segura de SMS.</span><span class="sxs-lookup"><span data-stu-id="0e069-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="0e069-132">Para este ejemplo, el `SMSoptions` se crea una clase en el *Services/SMSoptions.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="0e069-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="0e069-133">Establecer el `SMSAccountIdentification`, `SMSAccountPassword` y `SMSAccountFrom` con el [herramienta secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="0e069-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="0e069-134">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0e069-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* <span data-ttu-id="0e069-135">Agregue el paquete NuGet del proveedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="0e069-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="0e069-136">Desde el Administrador de consola paquetes (PMC) ejecute:</span><span class="sxs-lookup"><span data-stu-id="0e069-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="0e069-137">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="0e069-137">**Twilio:**</span></span>

`Install-Package Twilio`

<span data-ttu-id="0e069-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="0e069-138">**ASPSMS:**</span></span>

`Install-Package ASPSMS`

* <span data-ttu-id="0e069-139">Agregue código en el *Services/MessageServices.cs* archivo para habilitar SMS.</span><span class="sxs-lookup"><span data-stu-id="0e069-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="0e069-140">Utilice el Twilio o la sección ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="0e069-140">Use either the Twilio or the ASPSMS section:</span></span>

<span data-ttu-id="0e069-141">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="0e069-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="0e069-142">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="0e069-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="0e069-143">Configurar el inicio de usar `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="0e069-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="0e069-144">Agregar `SMSoptions` al contenedor de servicios en la `ConfigureServices` método en el *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0e069-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="0e069-145">Habilitar la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="0e069-145">Enable two-factor authentication</span></span>

<span data-ttu-id="0e069-146">Abra el archivo de vista de Razor *views/Manage/index. cshtml* y quite los caracteres de comentario (por lo que no hay marcado como comentario).</span><span class="sxs-lookup"><span data-stu-id="0e069-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commented out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="0e069-147">Inicie sesión con autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="0e069-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="0e069-148">Ejecute la aplicación y registrar un nuevo usuario</span><span class="sxs-lookup"><span data-stu-id="0e069-148">Run the app and register a new user</span></span>

![Vista de registro de aplicación Web abierta en Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="0e069-150">Puntee en el nombre de usuario, activa la `Index` métodos de acción de controlador de administración.</span><span class="sxs-lookup"><span data-stu-id="0e069-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="0e069-151">A continuación, puntee en el número de teléfono **agregar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="0e069-151">Then tap the phone number **Add** link.</span></span>

![Vista de administración: pulse el vínculo "Agregar"](2fa/_static/login2fa2.png)

* <span data-ttu-id="0e069-153">Agregar un número de teléfono que recibirá el código de verificación y pulse **enviar código de verificación**.</span><span class="sxs-lookup"><span data-stu-id="0e069-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Agregar página de número de teléfono](2fa/_static/login2fa3.png)

* <span data-ttu-id="0e069-155">Obtendrá un mensaje de texto con el código de verificación.</span><span class="sxs-lookup"><span data-stu-id="0e069-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="0e069-156">Escríbala y pulse **enviar**</span><span class="sxs-lookup"><span data-stu-id="0e069-156">Enter it and tap **Submit**</span></span>

![Compruebe la página de número de teléfono](2fa/_static/login2fa4.png)

<span data-ttu-id="0e069-158">Si no recibe un mensaje de texto, consulte la página de registro de twilio.</span><span class="sxs-lookup"><span data-stu-id="0e069-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="0e069-159">La vista de administración se muestra que el número de teléfono se agregó correctamente.</span><span class="sxs-lookup"><span data-stu-id="0e069-159">The Manage view shows your phone number was added successfully.</span></span>

![Administrar vista - número de teléfono se agregó correctamente](2fa/_static/login2fa5.png)

* <span data-ttu-id="0e069-161">Pulse **habilitar** para habilitar la autenticación en dos fases.</span><span class="sxs-lookup"><span data-stu-id="0e069-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Vista de administración: habilitar la autenticación en dos fases](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="0e069-163">Autenticación de dos fases de prueba</span><span class="sxs-lookup"><span data-stu-id="0e069-163">Test two-factor authentication</span></span>

* <span data-ttu-id="0e069-164">Cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="0e069-164">Log off.</span></span>

* <span data-ttu-id="0e069-165">Inicia sesión.</span><span class="sxs-lookup"><span data-stu-id="0e069-165">Log in.</span></span>

* <span data-ttu-id="0e069-166">La cuenta de usuario habilitó la autenticación en dos fases, por lo que debe proporcionar el segundo factor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="0e069-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="0e069-167">En este tutorial se ha habilitado la verificación por teléfono.</span><span class="sxs-lookup"><span data-stu-id="0e069-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="0e069-168">Las plantillas integradas permiten configurar el correo electrónico como segundo factor.</span><span class="sxs-lookup"><span data-stu-id="0e069-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="0e069-169">Puede configurar factores adicionales de segundo para la autenticación como códigos QR.</span><span class="sxs-lookup"><span data-stu-id="0e069-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="0e069-170">Pulse **enviar**.</span><span class="sxs-lookup"><span data-stu-id="0e069-170">Tap **Submit**.</span></span>

![Enviar la vista de código de verificación](2fa/_static/login2fa7.png)

* <span data-ttu-id="0e069-172">Escriba el código que se obtiene en el mensaje SMS.</span><span class="sxs-lookup"><span data-stu-id="0e069-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="0e069-173">Al hacer clic en el **recordar este explorador** casilla eximirán de la necesidad de usar 2FA para iniciar sesión cuando se usa el mismo dispositivo y el explorador.</span><span class="sxs-lookup"><span data-stu-id="0e069-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="0e069-174">Habilitar 2FA y haga clic en **recordar este explorador** le proporcionará 2FA fuerte protección contra usuarios malintencionados que intenta acceder a su cuenta, siempre que no tienen acceso a su dispositivo.</span><span class="sxs-lookup"><span data-stu-id="0e069-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="0e069-175">Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="0e069-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="0e069-176">Estableciendo **recordar este explorador**, obtendrá la seguridad añadida de 2FA desde dispositivos corrientemente no usamos y obtener la comodidad de no tener que pasar por 2FA en sus propios dispositivos.</span><span class="sxs-lookup"><span data-stu-id="0e069-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Comprobar la vista](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="0e069-178">Bloqueo de cuenta para protegerse contra los ataques por fuerza bruta</span><span class="sxs-lookup"><span data-stu-id="0e069-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="0e069-179">Se recomienda el bloqueo de cuenta con 2FA.</span><span class="sxs-lookup"><span data-stu-id="0e069-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="0e069-180">Una vez que un usuario inicia sesión a través de una cuenta local o social, se almacena cada intento incorrecto en 2FA.</span><span class="sxs-lookup"><span data-stu-id="0e069-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="0e069-181">Si se alcanza el máximo de intentos de acceso incorrectos, el usuario se bloquea (valor predeterminado: bloqueo de 5 minutos después de 5 intentos de acceso incorrectos).</span><span class="sxs-lookup"><span data-stu-id="0e069-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="0e069-182">Una autenticación correcta restablece el número de intentos de acceso erróneos y restablece el reloj.</span><span class="sxs-lookup"><span data-stu-id="0e069-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="0e069-183">El máximo intentos de acceso y se puede establecer el tiempo de bloqueo con [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) y [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="0e069-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="0e069-184">El siguiente ejemplo configura el bloqueo de cuentas durante 10 minutos tras 10 intentos de acceso:</span><span class="sxs-lookup"><span data-stu-id="0e069-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="0e069-185">Confirme que [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) establece `lockoutOnFailure` a `true`:</span><span class="sxs-lookup"><span data-stu-id="0e069-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
