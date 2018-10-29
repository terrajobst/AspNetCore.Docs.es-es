---
title: Autenticación en dos fases con SMS en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo configurar la autenticación en dos fases (2FA) con una aplicación ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
uid: security/authentication/2fa
ms.openlocfilehash: 5b0866ecf15381b040e3646eecc22374b6b0c9e2
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50205891"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="0b298-103">Autenticación en dos fases con SMS en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b298-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="0b298-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [suizo Devs](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="0b298-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

>[!WARNING]
> <span data-ttu-id="0b298-105">Dos aplicaciones de autenticador de autenticación (2FA) de factor, con una duración definida por única vez contraseña algoritmo (TOTP), son el enfoque para 2FA recomendado en el sector.</span><span class="sxs-lookup"><span data-stu-id="0b298-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="0b298-106">2FA uso TOTP es preferible a SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="0b298-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="0b298-107">Para obtener más información, consulte [generación habilitar código QR para las aplicaciones de authenticator TOTP en ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) para ASP.NET Core 2.0 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="0b298-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="0b298-108">Este tutorial muestra cómo configurar la autenticación en dos fases (2FA) mediante SMS.</span><span class="sxs-lookup"><span data-stu-id="0b298-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="0b298-109">Se proporcionan instrucciones para [twilio](https://www.twilio.com/) y [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), pero puede usar cualquier otro proveedor SMS.</span><span class="sxs-lookup"><span data-stu-id="0b298-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="0b298-110">Se recomienda realizar [confirmación de la cuenta y contraseña de recuperación](xref:security/authentication/accconfirm) antes de comenzar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="0b298-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="0b298-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="0b298-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="0b298-112">[Cómo descargar](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="0b298-112">[How to download](xref:index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="0b298-113">Crear un nuevo proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b298-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="0b298-114">Crear una nueva aplicación web de ASP.NET Core denominada `Web2FA` con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="0b298-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="0b298-115">Siga las instrucciones de [exigir SSL en una aplicación ASP.NET Core](xref:security/enforcing-ssl) para configurar y requieren SSL.</span><span class="sxs-lookup"><span data-stu-id="0b298-115">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="0b298-116">Crear una cuenta SMS</span><span class="sxs-lookup"><span data-stu-id="0b298-116">Create an SMS account</span></span>

<span data-ttu-id="0b298-117">Crear una cuenta SMS, por ejemplo, desde [twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="0b298-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="0b298-118">Registre las credenciales de autenticación (twilio: accountSid y authToken para ASPSMS: Userkey y la contraseña).</span><span class="sxs-lookup"><span data-stu-id="0b298-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="0b298-119">Averiguar las credenciales del proveedor de SMS</span><span class="sxs-lookup"><span data-stu-id="0b298-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="0b298-120">**Twilio:** en la ficha Panel de la cuenta de Twilio, copie el **SID de cuenta** y **Auth token**.</span><span class="sxs-lookup"><span data-stu-id="0b298-120">**Twilio:** From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="0b298-121">**ASPSMS:** desde la configuración de su cuenta, vaya a **Userkey** y cópielo junto con su **contraseña**.</span><span class="sxs-lookup"><span data-stu-id="0b298-121">**ASPSMS:** From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="0b298-122">Más adelante, almacenaremos estos valores con la herramienta Administrador de secretos en el conjunto de claves `SMSAccountIdentification` y `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="0b298-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="0b298-123">Especifica el Id. de remitente / originador</span><span class="sxs-lookup"><span data-stu-id="0b298-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="0b298-124">**Twilio:** desde la ficha números, copie su Twilio **número de teléfono**.</span><span class="sxs-lookup"><span data-stu-id="0b298-124">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="0b298-125">**ASPSMS:** en el menú de los originadores desbloquear, desbloquear uno o varios de los originadores o elija un originador alfanumérico (no admitido todas las redes).</span><span class="sxs-lookup"><span data-stu-id="0b298-125">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="0b298-126">Más adelante se almacenará este valor con la herramienta secret manager dentro de la clave `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="0b298-126">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="0b298-127">Proporcione las credenciales para el servicio SMS</span><span class="sxs-lookup"><span data-stu-id="0b298-127">Provide credentials for the SMS service</span></span>

<span data-ttu-id="0b298-128">Vamos a usar el [patrón de opciones](xref:fundamentals/configuration/options) para tener acceso a la configuración de cuenta y clave de usuario.</span><span class="sxs-lookup"><span data-stu-id="0b298-128">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

   * <span data-ttu-id="0b298-129">Cree una clase para capturar la clave segura de SMS.</span><span class="sxs-lookup"><span data-stu-id="0b298-129">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="0b298-130">Para este ejemplo, el `SMSoptions` se crea una clase en el *Services/SMSoptions.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="0b298-130">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="0b298-131">Establecer el `SMSAccountIdentification`, `SMSAccountPassword` y `SMSAccountFrom` con el [herramienta secret manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="0b298-131">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="0b298-132">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0b298-132">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="0b298-133">Agregue el paquete NuGet del proveedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="0b298-133">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="0b298-134">Desde el Administrador de consola paquetes (PMC) ejecute:</span><span class="sxs-lookup"><span data-stu-id="0b298-134">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="0b298-135">**Twilio:**
`Install-Package Twilio`</span><span class="sxs-lookup"><span data-stu-id="0b298-135">**Twilio:**
`Install-Package Twilio`</span></span>

<span data-ttu-id="0b298-136">**ASPSMS:**
`Install-Package ASPSMS`</span><span class="sxs-lookup"><span data-stu-id="0b298-136">**ASPSMS:**
`Install-Package ASPSMS`</span></span>


* <span data-ttu-id="0b298-137">Agregue código en el *Services/MessageServices.cs* archivo para habilitar SMS.</span><span class="sxs-lookup"><span data-stu-id="0b298-137">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="0b298-138">Utilice el Twilio o la sección ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="0b298-138">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="0b298-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span><span class="sxs-lookup"><span data-stu-id="0b298-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span></span>

<span data-ttu-id="0b298-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span><span class="sxs-lookup"><span data-stu-id="0b298-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span></span>

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="0b298-141">Configurar el inicio de usar `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="0b298-141">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="0b298-142">Agregar `SMSoptions` al contenedor de servicios en la `ConfigureServices` método en el *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0b298-142">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="0b298-143">Habilitar la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="0b298-143">Enable two-factor authentication</span></span>

<span data-ttu-id="0b298-144">Abra el *Views/Manage/Index.cshtml* archivo de vista de Razor y quite el comentario caracteres (de modo que no hay ningún marcado commnted out).</span><span class="sxs-lookup"><span data-stu-id="0b298-144">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="0b298-145">Inicie sesión con autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="0b298-145">Log in with two-factor authentication</span></span>

* <span data-ttu-id="0b298-146">Ejecute la aplicación y registrar un nuevo usuario</span><span class="sxs-lookup"><span data-stu-id="0b298-146">Run the app and register a new user</span></span>

![Vista de registro de aplicación Web abierta en Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="0b298-148">Puntee en el nombre de usuario, activa la `Index` métodos de acción de controlador de administración.</span><span class="sxs-lookup"><span data-stu-id="0b298-148">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="0b298-149">A continuación, puntee en el número de teléfono **agregar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="0b298-149">Then tap the phone number **Add** link.</span></span>

![Administrar la vista](2fa/_static/login2fa2.png)

* <span data-ttu-id="0b298-151">Agregar un número de teléfono que recibirá el código de verificación y pulse **enviar código de verificación**.</span><span class="sxs-lookup"><span data-stu-id="0b298-151">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Agregar página de número de teléfono](2fa/_static/login2fa3.png)

* <span data-ttu-id="0b298-153">Obtendrá un mensaje de texto con el código de verificación.</span><span class="sxs-lookup"><span data-stu-id="0b298-153">You will get a text message with the verification code.</span></span> <span data-ttu-id="0b298-154">Escríbala y pulse **enviar**</span><span class="sxs-lookup"><span data-stu-id="0b298-154">Enter it and tap **Submit**</span></span>

![Compruebe la página de número de teléfono](2fa/_static/login2fa4.png)

<span data-ttu-id="0b298-156">Si no recibe un mensaje de texto, consulte la página de registro de twilio.</span><span class="sxs-lookup"><span data-stu-id="0b298-156">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="0b298-157">La vista de administración se muestra que el número de teléfono se agregó correctamente.</span><span class="sxs-lookup"><span data-stu-id="0b298-157">The Manage view shows your phone number was added successfully.</span></span>

![Administrar la vista](2fa/_static/login2fa5.png)

* <span data-ttu-id="0b298-159">Pulse **habilitar** para habilitar la autenticación en dos fases.</span><span class="sxs-lookup"><span data-stu-id="0b298-159">Tap **Enable** to enable two-factor authentication.</span></span>

![Administrar la vista](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="0b298-161">Autenticación de dos fases de prueba</span><span class="sxs-lookup"><span data-stu-id="0b298-161">Test two-factor authentication</span></span>

* <span data-ttu-id="0b298-162">Cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="0b298-162">Log off.</span></span>

* <span data-ttu-id="0b298-163">Inicia sesión.</span><span class="sxs-lookup"><span data-stu-id="0b298-163">Log in.</span></span>

* <span data-ttu-id="0b298-164">La cuenta de usuario habilitó la autenticación en dos fases, por lo que debe proporcionar el segundo factor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="0b298-164">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="0b298-165">En este tutorial se ha habilitado la verificación por teléfono.</span><span class="sxs-lookup"><span data-stu-id="0b298-165">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="0b298-166">Las plantillas integradas permiten configurar el correo electrónico como segundo factor.</span><span class="sxs-lookup"><span data-stu-id="0b298-166">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="0b298-167">Puede configurar factores adicionales de segundo para la autenticación como códigos QR.</span><span class="sxs-lookup"><span data-stu-id="0b298-167">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="0b298-168">Pulse **enviar**.</span><span class="sxs-lookup"><span data-stu-id="0b298-168">Tap **Submit**.</span></span>

![Enviar la vista de código de verificación](2fa/_static/login2fa7.png)

* <span data-ttu-id="0b298-170">Escriba el código que se obtiene en el mensaje SMS.</span><span class="sxs-lookup"><span data-stu-id="0b298-170">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="0b298-171">Al hacer clic en el **recordar este explorador** casilla eximirán de la necesidad de usar 2FA para iniciar sesión cuando se usa el mismo dispositivo y el explorador.</span><span class="sxs-lookup"><span data-stu-id="0b298-171">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="0b298-172">Habilitar 2FA y haga clic en **recordar este explorador** le proporcionará 2FA fuerte protección contra usuarios malintencionados que intenta acceder a su cuenta, siempre que no tienen acceso a su dispositivo.</span><span class="sxs-lookup"><span data-stu-id="0b298-172">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="0b298-173">Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="0b298-173">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="0b298-174">Estableciendo **recordar este explorador**, obtendrá la seguridad añadida de 2FA desde dispositivos corrientemente no usamos y obtener la comodidad de no tener que pasar por 2FA en sus propios dispositivos.</span><span class="sxs-lookup"><span data-stu-id="0b298-174">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Comprobar la vista](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="0b298-176">Bloqueo de cuenta para protegerse contra los ataques por fuerza bruta</span><span class="sxs-lookup"><span data-stu-id="0b298-176">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="0b298-177">Se recomienda el bloqueo de cuenta con 2FA.</span><span class="sxs-lookup"><span data-stu-id="0b298-177">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="0b298-178">Una vez que un usuario inicia sesión a través de una cuenta local o social, se almacena cada intento incorrecto en 2FA.</span><span class="sxs-lookup"><span data-stu-id="0b298-178">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="0b298-179">Si se alcanza los intentos de acceso con error máximo, el usuario está bloqueado (valor predeterminado: bloqueo de 5 minutos después de 5 intentos de acceso).</span><span class="sxs-lookup"><span data-stu-id="0b298-179">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="0b298-180">Una autenticación correcta restablece el número de intentos de acceso erróneos y restablece el reloj.</span><span class="sxs-lookup"><span data-stu-id="0b298-180">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="0b298-181">El máximo intentos de acceso y se puede establecer el tiempo de bloqueo con [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) y [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="0b298-181">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="0b298-182">El siguiente ejemplo configura el bloqueo de cuentas durante 10 minutos tras 10 intentos de acceso:</span><span class="sxs-lookup"><span data-stu-id="0b298-182">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="0b298-183">Confirme que [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) establece `lockoutOnFailure` a `true`:</span><span class="sxs-lookup"><span data-stu-id="0b298-183">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
