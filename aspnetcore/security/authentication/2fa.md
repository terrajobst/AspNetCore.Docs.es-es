---
title: Autenticación en dos fases con SMS en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo configurar la autenticación en dos fases (2FA) con una aplicación de ASP.NET Core.
manager: wpickett
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 20f00c2307e140d81e716304c96a143340d934d0
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2018
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="66a9b-103">Autenticación en dos fases con SMS en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66a9b-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="66a9b-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [desarrolladores suizo](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="66a9b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="66a9b-105">Vea [generación habilitar código QR para las aplicaciones de autenticador de ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) para ASP.NET Core 2.0 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="66a9b-105">See [Enable QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="66a9b-106">Este tutorial muestra cómo configurar la autenticación en dos fases (2FA) con SMS.</span><span class="sxs-lookup"><span data-stu-id="66a9b-106">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="66a9b-107">Se proporcionan instrucciones para [twilio](https://www.twilio.com/) y [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), pero puede usar cualquier otro proveedor SMS.</span><span class="sxs-lookup"><span data-stu-id="66a9b-107">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="66a9b-108">Se recomienda realizar [confirmación de cuenta y contraseña de recuperación](xref:security/authentication/accconfirm) antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="66a9b-108">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="66a9b-109">Ver el [ejemplo completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="66a9b-109">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="66a9b-110">[Cómo descargar](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="66a9b-110">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="66a9b-111">Cree un nuevo proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66a9b-111">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="66a9b-112">Crear una nueva aplicación web de ASP.NET Core denominada `Web2FA` con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="66a9b-112">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="66a9b-113">Siga las instrucciones de [exigir SSL en una aplicación de ASP.NET Core](xref:security/enforcing-ssl) para configurar y requerir SSL.</span><span class="sxs-lookup"><span data-stu-id="66a9b-113">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="66a9b-114">Crear una cuenta SMS</span><span class="sxs-lookup"><span data-stu-id="66a9b-114">Create an SMS account</span></span>

<span data-ttu-id="66a9b-115">Crear una cuenta SMS, por ejemplo, de [twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="66a9b-115">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="66a9b-116">Registre las credenciales de autenticación (para twilio: accountSid y authToken para ASPSMS: clave de usuario confidenciales y la contraseña).</span><span class="sxs-lookup"><span data-stu-id="66a9b-116">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="66a9b-117">Pensar en las credenciales del proveedor de SMS</span><span class="sxs-lookup"><span data-stu-id="66a9b-117">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="66a9b-118">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="66a9b-118">**Twilio:**</span></span>  
<span data-ttu-id="66a9b-119">En la ficha Panel de su cuenta de Twilio, copie la **SID de cuenta** y **token de autenticación**.</span><span class="sxs-lookup"><span data-stu-id="66a9b-119">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="66a9b-120">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="66a9b-120">**ASPSMS:**</span></span>  
<span data-ttu-id="66a9b-121">Desde la configuración de su cuenta, vaya a **clave de usuario confidenciales** y cópielo junto con su **contraseña**.</span><span class="sxs-lookup"><span data-stu-id="66a9b-121">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="66a9b-122">Más adelante se almacenará estos valores con la herramienta Administrador de secreto en el conjunto de claves `SMSAccountIdentification` y `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="66a9b-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="66a9b-123">Especifica el identificador del remitente / originador</span><span class="sxs-lookup"><span data-stu-id="66a9b-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="66a9b-124">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="66a9b-124">**Twilio:**</span></span>  
<span data-ttu-id="66a9b-125">En la ficha números, copie su Twilio **número de teléfono**.</span><span class="sxs-lookup"><span data-stu-id="66a9b-125">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="66a9b-126">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="66a9b-126">**ASPSMS:**</span></span>  
<span data-ttu-id="66a9b-127">En el menú de remitentes desbloquear, desbloquear uno o más remitentes o elija un originador alfanumérico (no admitido todas las redes).</span><span class="sxs-lookup"><span data-stu-id="66a9b-127">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="66a9b-128">Más adelante se almacenará este valor con la herramienta Administrador de secreto en la clave `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="66a9b-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="66a9b-129">Proporcione las credenciales para el servicio SMS</span><span class="sxs-lookup"><span data-stu-id="66a9b-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="66a9b-130">Vamos a usar la [patrón opciones](xref:fundamentals/configuration/options) para tener acceso a la configuración de cuenta y clave de usuario.</span><span class="sxs-lookup"><span data-stu-id="66a9b-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="66a9b-131">Cree una clase para capturar la clave SMS segura.</span><span class="sxs-lookup"><span data-stu-id="66a9b-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="66a9b-132">En este ejemplo, el `SMSoptions` se crea una clase en el *Services/SMSoptions.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="66a9b-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="66a9b-133">Establecer el `SMSAccountIdentification`, `SMSAccountPassword` y `SMSAccountFrom` con el [herramienta Administrador de secreto](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="66a9b-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="66a9b-134">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="66a9b-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="66a9b-135">Agregue el paquete de NuGet del proveedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="66a9b-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="66a9b-136">Desde el paquete de administrador de consola (PMC) ejecutar:</span><span class="sxs-lookup"><span data-stu-id="66a9b-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="66a9b-137">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="66a9b-137">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="66a9b-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="66a9b-138">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="66a9b-139">Agregue código en el *Services/MessageServices.cs* archivo para habilitar SMS.</span><span class="sxs-lookup"><span data-stu-id="66a9b-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="66a9b-140">Utilice la Twilio o la sección ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="66a9b-140">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="66a9b-141">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="66a9b-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="66a9b-142">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="66a9b-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="66a9b-143">Configurar el inicio de usar `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="66a9b-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="66a9b-144">Agregar `SMSoptions` al contenedor de servicios en la `ConfigureServices` método en el *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="66a9b-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="66a9b-145">Habilitar la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="66a9b-145">Enable two-factor authentication</span></span>

<span data-ttu-id="66a9b-146">Abra la *Views/Manage/Index.cshtml* archivo de vista Razor y quite el comentario de caracteres (por lo que ningún tipo de marcado es almohadilla).</span><span class="sxs-lookup"><span data-stu-id="66a9b-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="66a9b-147">Inicie sesión con la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="66a9b-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="66a9b-148">Ejecutar la aplicación y registrar un nuevo usuario</span><span class="sxs-lookup"><span data-stu-id="66a9b-148">Run the app and register a new user</span></span>

![Vista de registro de aplicación Web abierta en Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="66a9b-150">Puntee en el nombre de usuario, activa la `Index` métodos de acción de controlador de administrar.</span><span class="sxs-lookup"><span data-stu-id="66a9b-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="66a9b-151">A continuación, puntee en el número de teléfono **agregar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="66a9b-151">Then tap the phone number **Add** link.</span></span>

![Administrar la vista](2fa/_static/login2fa2.png)

* <span data-ttu-id="66a9b-153">Agregar un número de teléfono que recibirá el código de comprobación y pulse **enviar código de comprobación**.</span><span class="sxs-lookup"><span data-stu-id="66a9b-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Agregar página de número de teléfono](2fa/_static/login2fa3.png)

* <span data-ttu-id="66a9b-155">Obtendrá un mensaje de texto con el código de comprobación.</span><span class="sxs-lookup"><span data-stu-id="66a9b-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="66a9b-156">Escríbala y pulse **enviar**</span><span class="sxs-lookup"><span data-stu-id="66a9b-156">Enter it and tap **Submit**</span></span>

![Compruebe la página de número de teléfono](2fa/_static/login2fa4.png)

<span data-ttu-id="66a9b-158">Si no recibe un mensaje de texto, consulte la página de registro de twilio.</span><span class="sxs-lookup"><span data-stu-id="66a9b-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="66a9b-159">La vista gestionar muestra que el número de teléfono se agregó correctamente.</span><span class="sxs-lookup"><span data-stu-id="66a9b-159">The Manage view shows your phone number was added successfully.</span></span>

![Administrar la vista](2fa/_static/login2fa5.png)

* <span data-ttu-id="66a9b-161">Pulse **habilitar** para habilitar la autenticación en dos fases.</span><span class="sxs-lookup"><span data-stu-id="66a9b-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Administrar la vista](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="66a9b-163">Autenticación de dos factores de prueba</span><span class="sxs-lookup"><span data-stu-id="66a9b-163">Test two-factor authentication</span></span>

* <span data-ttu-id="66a9b-164">Cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="66a9b-164">Log off.</span></span>

* <span data-ttu-id="66a9b-165">Inicia sesión.</span><span class="sxs-lookup"><span data-stu-id="66a9b-165">Log in.</span></span>

* <span data-ttu-id="66a9b-166">La cuenta de usuario ha habilitado la autenticación en dos fases, por lo que tendrá que proporcionar el segundo factor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="66a9b-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="66a9b-167">En este tutorial se ha habilitado la verificación por teléfono.</span><span class="sxs-lookup"><span data-stu-id="66a9b-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="66a9b-168">Las plantillas creadas en también le permiten configurar el correo electrónico como el segundo factor.</span><span class="sxs-lookup"><span data-stu-id="66a9b-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="66a9b-169">Puede configurar factores de segundo adicionales para la autenticación como códigos QR.</span><span class="sxs-lookup"><span data-stu-id="66a9b-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="66a9b-170">Pulse **enviar**.</span><span class="sxs-lookup"><span data-stu-id="66a9b-170">Tap **Submit**.</span></span>

![Enviar vista de código de comprobación](2fa/_static/login2fa7.png)

* <span data-ttu-id="66a9b-172">Escriba el código que se obtienen en el mensaje SMS.</span><span class="sxs-lookup"><span data-stu-id="66a9b-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="66a9b-173">Al hacer clic en el **recordar este explorador** casilla de verificación se excluya de la necesidad de usar 2FA para iniciar sesión cuando se usa el mismo dispositivo y el explorador.</span><span class="sxs-lookup"><span data-stu-id="66a9b-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="66a9b-174">Habilitar 2FA y haciendo clic en **recordar este explorador** le proporcionará protección segura 2FA de usuarios malintencionados que intenta acceder a su cuenta, siempre y cuando no tienen acceso al dispositivo.</span><span class="sxs-lookup"><span data-stu-id="66a9b-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="66a9b-175">Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="66a9b-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="66a9b-176">Estableciendo **recordar este explorador**, obtener la seguridad adicional de 2FA desde dispositivos que no use con regularidad y obtener la comodidad de no tener que pasar por 2FA en sus propios dispositivos.</span><span class="sxs-lookup"><span data-stu-id="66a9b-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Comprobar la vista](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="66a9b-178">Bloqueo de cuenta para protegerse contra los ataques por fuerza bruta</span><span class="sxs-lookup"><span data-stu-id="66a9b-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="66a9b-179">Se recomienda el bloqueo de cuenta con 2FA.</span><span class="sxs-lookup"><span data-stu-id="66a9b-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="66a9b-180">Una vez que un usuario inicia sesión a través de una cuenta local o sociales, se almacena cada intento fallido en 2FA.</span><span class="sxs-lookup"><span data-stu-id="66a9b-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="66a9b-181">Si se alcanza los intentos de acceso erróneos máximo, el usuario está bloqueado (valor predeterminado: bloqueo de 5 minutos después de 5 intentos de acceso).</span><span class="sxs-lookup"><span data-stu-id="66a9b-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="66a9b-182">Una autenticación correcta restablece el número de intentos de acceso erróneos y restablece el reloj.</span><span class="sxs-lookup"><span data-stu-id="66a9b-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="66a9b-183">El valor máximo intentos de acceso y se puede establecer el tiempo de bloqueo con [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) y [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="66a9b-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="66a9b-184">El siguiente ejemplo configura el bloqueo de cuenta de 10 minutos tras 10 intentos de acceso:</span><span class="sxs-lookup"><span data-stu-id="66a9b-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="66a9b-185">Confirme que [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) establece `lockoutOnFailure` a `true`:</span><span class="sxs-lookup"><span data-stu-id="66a9b-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
