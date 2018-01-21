---
title: "Autenticación en dos fases con SMS"
author: rick-anderson
description: "Muestra cómo configurar la autenticación en dos fases (2FA) con ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/2fa
ms.openlocfilehash: 0970b7e95b116ceab1d17502d2f8aee9cd821715
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="568de-103">Autenticación en dos fases con SMS</span><span class="sxs-lookup"><span data-stu-id="568de-103">Two-factor authentication with SMS</span></span>

<span data-ttu-id="568de-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [desarrolladores suizo](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="568de-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="568de-105">Este tutorial se aplica a ASP.NET Core solo 1.x.</span><span class="sxs-lookup"><span data-stu-id="568de-105">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="568de-106">Vea [generación habilitar código QR para las aplicaciones de autenticador de ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) para ASP.NET Core 2.0 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="568de-106">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="568de-107">Este tutorial muestra cómo configurar la autenticación en dos fases (2FA) con SMS.</span><span class="sxs-lookup"><span data-stu-id="568de-107">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="568de-108">Se proporcionan instrucciones para [twilio](https://www.twilio.com/) y [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), pero puede usar cualquier otro proveedor SMS.</span><span class="sxs-lookup"><span data-stu-id="568de-108">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="568de-109">Se recomienda realizar [confirmación de cuenta y contraseña de recuperación](accconfirm.md) antes de iniciar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="568de-109">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="568de-110">Ver el [ejemplo completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="568de-110">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="568de-111">[Cómo descargar](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="568de-111">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="568de-112">Cree un nuevo proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="568de-112">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="568de-113">Crear una nueva aplicación web de ASP.NET Core denominada `Web2FA` con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="568de-113">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="568de-114">Siga las instrucciones de [exigir SSL en una aplicación de ASP.NET Core](xref:security/enforcing-ssl) para configurar y requerir SSL.</span><span class="sxs-lookup"><span data-stu-id="568de-114">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="568de-115">Crear una cuenta SMS</span><span class="sxs-lookup"><span data-stu-id="568de-115">Create an SMS account</span></span>

<span data-ttu-id="568de-116">Crear una cuenta SMS, por ejemplo, de [twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="568de-116">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="568de-117">Registre las credenciales de autenticación (para twilio: accountSid y authToken para ASPSMS: clave de usuario confidenciales y la contraseña).</span><span class="sxs-lookup"><span data-stu-id="568de-117">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="568de-118">Pensar en las credenciales del proveedor de SMS</span><span class="sxs-lookup"><span data-stu-id="568de-118">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="568de-119">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="568de-119">**Twilio:**</span></span>  
<span data-ttu-id="568de-120">En la ficha Panel de su cuenta de Twilio, copie la **SID de cuenta** y **token de autenticación**.</span><span class="sxs-lookup"><span data-stu-id="568de-120">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="568de-121">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="568de-121">**ASPSMS:**</span></span>  
<span data-ttu-id="568de-122">Desde la configuración de su cuenta, vaya a **clave de usuario confidenciales** y cópielo junto con su **contraseña**.</span><span class="sxs-lookup"><span data-stu-id="568de-122">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="568de-123">Más adelante se almacenará estos valores con la herramienta Administrador de secreto en el conjunto de claves `SMSAccountIdentification` y `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="568de-123">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="568de-124">Especifica el identificador del remitente / originador</span><span class="sxs-lookup"><span data-stu-id="568de-124">Specifying SenderID / Originator</span></span>

<span data-ttu-id="568de-125">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="568de-125">**Twilio:**</span></span>  
<span data-ttu-id="568de-126">En la ficha números, copie su Twilio **número de teléfono**.</span><span class="sxs-lookup"><span data-stu-id="568de-126">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="568de-127">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="568de-127">**ASPSMS:**</span></span>  
<span data-ttu-id="568de-128">En el menú de remitentes desbloquear, desbloquear uno o más remitentes o elija un originador alfanumérico (no admitido todas las redes).</span><span class="sxs-lookup"><span data-stu-id="568de-128">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="568de-129">Más adelante se almacenará este valor con la herramienta Administrador de secreto en la clave `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="568de-129">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="568de-130">Proporcione las credenciales para el servicio SMS</span><span class="sxs-lookup"><span data-stu-id="568de-130">Provide credentials for the SMS service</span></span>

<span data-ttu-id="568de-131">Vamos a usar la [patrón opciones](xref:fundamentals/configuration/options) para tener acceso a la configuración de cuenta y clave de usuario.</span><span class="sxs-lookup"><span data-stu-id="568de-131">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="568de-132">Cree una clase para capturar la clave SMS segura.</span><span class="sxs-lookup"><span data-stu-id="568de-132">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="568de-133">En este ejemplo, el `SMSoptions` se crea una clase en el *Services/SMSoptions.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="568de-133">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="568de-134">Establecer el `SMSAccountIdentification`, `SMSAccountPassword` y `SMSAccountFrom` con el [herramienta Administrador de secreto](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="568de-134">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="568de-135">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="568de-135">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="568de-136">Agregue el paquete de NuGet del proveedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="568de-136">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="568de-137">Desde el paquete de administrador de consola (PMC) ejecutar:</span><span class="sxs-lookup"><span data-stu-id="568de-137">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="568de-138">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="568de-138">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="568de-139">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="568de-139">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="568de-140">Agregue código en el *Services/MessageServices.cs* archivo para habilitar SMS.</span><span class="sxs-lookup"><span data-stu-id="568de-140">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="568de-141">Utilice la Twilio o la sección ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="568de-141">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="568de-142">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="568de-142">**Twilio:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="568de-143">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="568de-143">**ASPSMS:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="568de-144">Configurar el inicio de usar`SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="568de-144">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="568de-145">Agregar `SMSoptions` al contenedor de servicios en la `ConfigureServices` método en el *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="568de-145">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="568de-146">Habilitar la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="568de-146">Enable two-factor authentication</span></span>

<span data-ttu-id="568de-147">Abra la *Views/Manage/Index.cshtml* archivo de vista Razor y quite el comentario de caracteres (por lo que ningún tipo de marcado es almohadilla).</span><span class="sxs-lookup"><span data-stu-id="568de-147">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="568de-148">Inicie sesión con la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="568de-148">Log in with two-factor authentication</span></span>

* <span data-ttu-id="568de-149">Ejecutar la aplicación y registrar un nuevo usuario</span><span class="sxs-lookup"><span data-stu-id="568de-149">Run the app and register a new user</span></span>

![Vista de registro de aplicación Web abierta en Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="568de-151">Puntee en el nombre de usuario, activa la `Index` métodos de acción de controlador de administrar.</span><span class="sxs-lookup"><span data-stu-id="568de-151">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="568de-152">A continuación, puntee en el número de teléfono **agregar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="568de-152">Then tap the phone number **Add** link.</span></span>

![Administrar la vista](2fa/_static/login2fa2.png)

* <span data-ttu-id="568de-154">Agregar un número de teléfono que recibirá el código de comprobación y pulse **enviar código de comprobación**.</span><span class="sxs-lookup"><span data-stu-id="568de-154">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Agregar página de número de teléfono](2fa/_static/login2fa3.png)

* <span data-ttu-id="568de-156">Obtendrá un mensaje de texto con el código de comprobación.</span><span class="sxs-lookup"><span data-stu-id="568de-156">You will get a text message with the verification code.</span></span> <span data-ttu-id="568de-157">Escríbala y pulse **enviar**</span><span class="sxs-lookup"><span data-stu-id="568de-157">Enter it and tap **Submit**</span></span>

![Compruebe la página de número de teléfono](2fa/_static/login2fa4.png)

<span data-ttu-id="568de-159">Si no recibe un mensaje de texto, consulte la página de registro de twilio.</span><span class="sxs-lookup"><span data-stu-id="568de-159">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="568de-160">La vista gestionar muestra que el número de teléfono se agregó correctamente.</span><span class="sxs-lookup"><span data-stu-id="568de-160">The Manage view shows your phone number was added successfully.</span></span>

![Administrar la vista](2fa/_static/login2fa5.png)

* <span data-ttu-id="568de-162">Pulse **habilitar** para habilitar la autenticación en dos fases.</span><span class="sxs-lookup"><span data-stu-id="568de-162">Tap **Enable** to enable two-factor authentication.</span></span>

![Administrar la vista](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="568de-164">Autenticación de dos factores de prueba</span><span class="sxs-lookup"><span data-stu-id="568de-164">Test two-factor authentication</span></span>

* <span data-ttu-id="568de-165">Cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="568de-165">Log off.</span></span>

* <span data-ttu-id="568de-166">Inicia sesión.</span><span class="sxs-lookup"><span data-stu-id="568de-166">Log in.</span></span>

* <span data-ttu-id="568de-167">La cuenta de usuario ha habilitado la autenticación en dos fases, por lo que tendrá que proporcionar el segundo factor de autenticación.</span><span class="sxs-lookup"><span data-stu-id="568de-167">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="568de-168">En este tutorial se ha habilitado la verificación por teléfono.</span><span class="sxs-lookup"><span data-stu-id="568de-168">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="568de-169">Las plantillas creadas en también le permiten configurar el correo electrónico como el segundo factor.</span><span class="sxs-lookup"><span data-stu-id="568de-169">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="568de-170">Puede configurar factores de segundo adicionales para la autenticación como códigos QR.</span><span class="sxs-lookup"><span data-stu-id="568de-170">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="568de-171">Pulse **enviar**.</span><span class="sxs-lookup"><span data-stu-id="568de-171">Tap **Submit**.</span></span>

![Enviar vista de código de comprobación](2fa/_static/login2fa7.png)

* <span data-ttu-id="568de-173">Escriba el código que se obtienen en el mensaje SMS.</span><span class="sxs-lookup"><span data-stu-id="568de-173">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="568de-174">Al hacer clic en el **recordar este explorador** casilla de verificación se excluya de la necesidad de usar 2FA para iniciar sesión cuando se usa el mismo dispositivo y el explorador.</span><span class="sxs-lookup"><span data-stu-id="568de-174">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="568de-175">Habilitar 2FA y haciendo clic en **recordar este explorador** le proporcionará protección segura 2FA de usuarios malintencionados que intenta acceder a su cuenta, siempre y cuando no tienen acceso al dispositivo.</span><span class="sxs-lookup"><span data-stu-id="568de-175">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="568de-176">Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="568de-176">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="568de-177">Estableciendo **recordar este explorador**, obtener la seguridad adicional de 2FA desde dispositivos que no use con regularidad y obtener la comodidad de no tener que pasar por 2FA en sus propios dispositivos.</span><span class="sxs-lookup"><span data-stu-id="568de-177">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Comprobar la vista](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="568de-179">Bloqueo de cuenta para protegerse contra los ataques por fuerza bruta</span><span class="sxs-lookup"><span data-stu-id="568de-179">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="568de-180">Se recomienda que usar el bloqueo de cuenta con 2FA.</span><span class="sxs-lookup"><span data-stu-id="568de-180">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="568de-181">Una vez que un usuario inicia sesión (a través de una cuenta local o sociales), se almacena cada intento fallido en 2FA y si se alcanza el número máximo de intentos (el valor predeterminado es 5), el usuario se bloquea durante cinco minutos (puede establecer el bloqueo de hora con `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="568de-181">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="568de-182">El siguiente ejemplo configura la cuenta que se bloquee durante 10 minutos tras 10 intentos fallidos.</span><span class="sxs-lookup"><span data-stu-id="568de-182">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
