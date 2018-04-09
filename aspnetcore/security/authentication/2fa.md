---
title: Autenticación en dos fases con SMS en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo configurar la autenticación en dos fases (2FA) con una aplicación de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 1c4acc4e4be593051d30793b7f73ad90ce727283
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="07014-103">Autenticación en dos fases con SMS en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07014-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="07014-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [desarrolladores suizo](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="07014-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="07014-105">Este tutorial se aplica a ASP.NET Core solo 1.x.</span><span class="sxs-lookup"><span data-stu-id="07014-105">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="07014-106">Vea [generación habilitar código QR para las aplicaciones de autenticador de ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) para ASP.NET Core 2.0 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="07014-106">See [Enable QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="07014-107">Este tutorial muestra cómo configurar la autenticación en dos fases (2FA) con SMS.</span><span class="sxs-lookup"><span data-stu-id="07014-107">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="07014-108">Se proporcionan instrucciones para [twilio](https://www.twilio.com/) y [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), pero puede usar cualquier otro proveedor SMS.</span><span class="sxs-lookup"><span data-stu-id="07014-108">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="07014-109">Se recomienda realizar [confirmación de cuenta y contraseña de recuperación](xref:security/authentication/accconfirm) antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="07014-109">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="07014-110">Ver el [ejemplo completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="07014-110">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="07014-111">[Cómo descargar](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="07014-111">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="07014-112">Cree un nuevo proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07014-112">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="07014-113">Crear una nueva aplicación web de ASP.NET Core denominada `Web2FA` con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="07014-113">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="07014-114">Siga las instrucciones de [exigir SSL en una aplicación de ASP.NET Core](xref:security/enforcing-ssl) para configurar y requerir SSL.</span><span class="sxs-lookup"><span data-stu-id="07014-114">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="07014-115">Crear una cuenta SMS</span><span class="sxs-lookup"><span data-stu-id="07014-115">Create an SMS account</span></span>

<span data-ttu-id="07014-116">Crear una cuenta SMS, por ejemplo, de [twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="07014-116">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="07014-117">Registre las credenciales de autenticación (para twilio: accountSid y authToken para ASPSMS: clave de usuario confidenciales y la contraseña).</span><span class="sxs-lookup"><span data-stu-id="07014-117">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="07014-118">Pensar en las credenciales del proveedor de SMS</span><span class="sxs-lookup"><span data-stu-id="07014-118">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="07014-119">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="07014-119">**Twilio:**</span></span>  
<span data-ttu-id="07014-120">En la ficha Panel de su cuenta de Twilio, copie la **SID de cuenta** y **token de autenticación**.</span><span class="sxs-lookup"><span data-stu-id="07014-120">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="07014-121">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="07014-121">**ASPSMS:**</span></span>  
<span data-ttu-id="07014-122">Desde la configuración de su cuenta, vaya a **clave de usuario confidenciales** y cópielo junto con su **contraseña**.</span><span class="sxs-lookup"><span data-stu-id="07014-122">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="07014-123">Más adelante se almacenará estos valores con la herramienta Administrador de secreto en el conjunto de claves `SMSAccountIdentification` y `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="07014-123">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="07014-124">Especifica el identificador del remitente / originador</span><span class="sxs-lookup"><span data-stu-id="07014-124">Specifying SenderID / Originator</span></span>

<span data-ttu-id="07014-125">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="07014-125">**Twilio:**</span></span>  
<span data-ttu-id="07014-126">En la ficha números, copie su Twilio **número de teléfono**.</span><span class="sxs-lookup"><span data-stu-id="07014-126">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="07014-127">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="07014-127">**ASPSMS:**</span></span>  
<span data-ttu-id="07014-128">En el menú de remitentes desbloquear, desbloquear uno o más remitentes o elija un originador alfanumérico (no admitido todas las redes).</span><span class="sxs-lookup"><span data-stu-id="07014-128">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="07014-129">Más adelante se almacenará este valor con la herramienta Administrador de secreto en la clave `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="07014-129">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="07014-130">Proporcione las credenciales para el servicio SMS</span><span class="sxs-lookup"><span data-stu-id="07014-130">Provide credentials for the SMS service</span></span>

<span data-ttu-id="07014-131">Vamos a usar la [patrón opciones](xref:fundamentals/configuration/options) para tener acceso a la configuración de cuenta y clave de usuario.</span><span class="sxs-lookup"><span data-stu-id="07014-131">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="07014-132">Cree una clase para capturar la clave SMS segura.</span><span class="sxs-lookup"><span data-stu-id="07014-132">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="07014-133">En este ejemplo, el `SMSoptions` se crea una clase en el *Services/SMSoptions.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="07014-133">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="07014-134">Establecer el `SMSAccountIdentification`, `SMSAccountPassword` y `SMSAccountFrom` con el [herramienta Administrador de secreto](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="07014-134">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="07014-135">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="07014-135">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="07014-136">Agregue el paquete de NuGet del proveedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="07014-136">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="07014-137">Desde el paquete de administrador de consola (PMC) ejecutar:</span><span class="sxs-lookup"><span data-stu-id="07014-137">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="07014-138">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="07014-138">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="07014-139">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="07014-139">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="07014-140">Agregue código en el *Services/MessageServices.cs* archivo para habilitar SMS.</span><span class="sxs-lookup"><span data-stu-id="07014-140">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="07014-141">Utilice la Twilio o la sección ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="07014-141">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="07014-142">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="07014-142">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="07014-143">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="07014-143">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="07014-144">Configurar el inicio de usar `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="07014-144">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="07014-145">Agregar `SMSoptions` al contenedor de servicios en la `ConfigureServices` método en el *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="07014-145">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="07014-146">Habilitar la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="07014-146">Enable two-factor authentication</span></span>

<span data-ttu-id="07014-147">Abra la *Views/Manage/Index.cshtml* archivo de vista Razor y quite el comentario de caracteres (por lo que ningún tipo de marcado es almohadilla).</span><span class="sxs-lookup"><span data-stu-id="07014-147">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="07014-148">Inicie sesión con la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="07014-148">Log in with two-factor authentication</span></span>

* <span data-ttu-id="07014-149">Ejecutar la aplicación y registrar un nuevo usuario</span><span class="sxs-lookup"><span data-stu-id="07014-149">Run the app and register a new user</span></span>

![Vista de registro de aplicación Web abierta en Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="07014-151">Puntee en el nombre de usuario, activa la `Index` métodos de acción de controlador de administrar.</span><span class="sxs-lookup"><span data-stu-id="07014-151">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="07014-152">A continuación, puntee en el número de teléfono **agregar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="07014-152">Then tap the phone number **Add** link.</span></span>

![Administrar la vista](2fa/_static/login2fa2.png)

* <span data-ttu-id="07014-154">Agregar un número de teléfono que recibirá el código de comprobación y pulse **enviar código de comprobación**.</span><span class="sxs-lookup"><span data-stu-id="07014-154">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Agregar página de número de teléfono](2fa/_static/login2fa3.png)

* <span data-ttu-id="07014-156">Obtendrá un mensaje de texto con el código de comprobación.</span><span class="sxs-lookup"><span data-stu-id="07014-156">You will get a text message with the verification code.</span></span> <span data-ttu-id="07014-157">Escríbala y pulse **enviar**</span><span class="sxs-lookup"><span data-stu-id="07014-157">Enter it and tap **Submit**</span></span>

![Compruebe la página de número de teléfono](2fa/_static/login2fa4.png)

<span data-ttu-id="07014-159">Si no recibe un mensaje de texto, consulte la página de registro de twilio.</span><span class="sxs-lookup"><span data-stu-id="07014-159">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="07014-160">La vista gestionar muestra que el número de teléfono se agregó correctamente.</span><span class="sxs-lookup"><span data-stu-id="07014-160">The Manage view shows your phone number was added successfully.</span></span>

![Administrar la vista](2fa/_static/login2fa5.png)

* <span data-ttu-id="07014-162">Pulse **habilitar** para habilitar la autenticación en dos fases.</span><span class="sxs-lookup"><span data-stu-id="07014-162">Tap **Enable** to enable two-factor authentication.</span></span>

![Administrar la vista](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="07014-164">Autenticación de dos factores de prueba</span><span class="sxs-lookup"><span data-stu-id="07014-164">Test two-factor authentication</span></span>

* <span data-ttu-id="07014-165">Cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="07014-165">Log off.</span></span>

* <span data-ttu-id="07014-166">Inicia sesión.</span><span class="sxs-lookup"><span data-stu-id="07014-166">Log in.</span></span>

* <span data-ttu-id="07014-167">La cuenta de usuario ha habilitado la autenticación en dos fases, por lo que tendrá que proporcionar el segundo factor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="07014-167">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="07014-168">En este tutorial se ha habilitado la verificación por teléfono.</span><span class="sxs-lookup"><span data-stu-id="07014-168">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="07014-169">Las plantillas creadas en también le permiten configurar el correo electrónico como el segundo factor.</span><span class="sxs-lookup"><span data-stu-id="07014-169">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="07014-170">Puede configurar factores de segundo adicionales para la autenticación como códigos QR.</span><span class="sxs-lookup"><span data-stu-id="07014-170">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="07014-171">Pulse **enviar**.</span><span class="sxs-lookup"><span data-stu-id="07014-171">Tap **Submit**.</span></span>

![Enviar vista de código de comprobación](2fa/_static/login2fa7.png)

* <span data-ttu-id="07014-173">Escriba el código que se obtienen en el mensaje SMS.</span><span class="sxs-lookup"><span data-stu-id="07014-173">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="07014-174">Al hacer clic en el **recordar este explorador** casilla de verificación se excluya de la necesidad de usar 2FA para iniciar sesión cuando se usa el mismo dispositivo y el explorador.</span><span class="sxs-lookup"><span data-stu-id="07014-174">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="07014-175">Habilitar 2FA y haciendo clic en **recordar este explorador** le proporcionará protección segura 2FA de usuarios malintencionados que intenta acceder a su cuenta, siempre y cuando no tienen acceso al dispositivo.</span><span class="sxs-lookup"><span data-stu-id="07014-175">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="07014-176">Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="07014-176">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="07014-177">Estableciendo **recordar este explorador**, obtener la seguridad adicional de 2FA desde dispositivos que no use con regularidad y obtener la comodidad de no tener que pasar por 2FA en sus propios dispositivos.</span><span class="sxs-lookup"><span data-stu-id="07014-177">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Comprobar la vista](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="07014-179">Bloqueo de cuenta para protegerse contra los ataques por fuerza bruta</span><span class="sxs-lookup"><span data-stu-id="07014-179">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="07014-180">Se recomienda el bloqueo de cuenta con 2FA.</span><span class="sxs-lookup"><span data-stu-id="07014-180">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="07014-181">Una vez que un usuario inicia sesión a través de una cuenta local o sociales, se almacena cada intento fallido en 2FA.</span><span class="sxs-lookup"><span data-stu-id="07014-181">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="07014-182">Si se alcanza los intentos de acceso erróneos máximo, el usuario está bloqueado (valor predeterminado: bloqueo de 5 minutos después de 5 intentos de acceso).</span><span class="sxs-lookup"><span data-stu-id="07014-182">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="07014-183">Una autenticación correcta restablece el número de intentos de acceso erróneos y restablece el reloj.</span><span class="sxs-lookup"><span data-stu-id="07014-183">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="07014-184">El valor máximo intentos de acceso y se puede establecer el tiempo de bloqueo con [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) y [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="07014-184">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="07014-185">El siguiente ejemplo configura el bloqueo de cuenta de 10 minutos tras 10 intentos de acceso:</span><span class="sxs-lookup"><span data-stu-id="07014-185">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="07014-186">Confirme que [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) establece `lockoutOnFailure` a `true`:</span><span class="sxs-lookup"><span data-stu-id="07014-186">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
