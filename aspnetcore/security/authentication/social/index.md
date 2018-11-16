---
title: Autenticación con Facebook, Google y proveedores externos en ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra cómo crear una aplicación de ASP.NET Core 2.x mediante OAuth 2.0 con proveedores de autenticación externos.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/social/index
ms.openlocfilehash: 19074d5014a09446ceec1b89449e78760fc8e7cf
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708379"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="cc864-103">Autenticación con Facebook, Google y proveedores externos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc864-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="cc864-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cc864-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cc864-105">En este tutorial se muestra cómo crear una aplicación de ASP.NET Core 2.x que permita a los usuarios iniciar sesión mediante OAuth 2.0 con credenciales de proveedores de autenticación externos.</span><span class="sxs-lookup"><span data-stu-id="cc864-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="cc864-106">En las siguientes secciones se tratan los proveedores [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins) y [Microsoft](xref:security/authentication/microsoft-logins).</span><span class="sxs-lookup"><span data-stu-id="cc864-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="cc864-107">Hay disponibles otros proveedores en paquetes de terceros como [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) y [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="cc864-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Iconos de redes sociales para Facebook, Twitter, Google Plus y Windows](index/_static/social.png)

<span data-ttu-id="cc864-109">Permitir que los usuarios inicien sesión con sus credenciales es práctico para los usuarios y transfiere muchas de las complejidades existentes en la administración del proceso de inicio de sesión a un tercero.</span><span class="sxs-lookup"><span data-stu-id="cc864-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="cc864-110">Para ver ejemplos de cómo los inicios de sesión de las redes sociales pueden controlar las conversiones del tráfico y de clientes, vea los casos prácticos de [Facebook](https://www.facebook.com/unsupportedbrowser) y [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="cc864-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="cc864-111">Nota: Los paquetes que se presentan aquí sintetizan en gran parte la complejidad del flujo de autenticación de OAuth, pero conocer los detalles puede ser necesario a la hora de solucionar problemas.</span><span class="sxs-lookup"><span data-stu-id="cc864-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="cc864-112">Hay disponibles numerosos recursos; por ejemplo, vea [An Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) (Introducción a OAuth 2) o [Understanding OAuth2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/) (Descripción de OAuth2).</span><span class="sxs-lookup"><span data-stu-id="cc864-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="cc864-113">Algunos problemas se pueden resolver examinando el [código fuente de ASP.NET Core de los paquetes de los proveedores](https://github.com/aspnet/Security/tree/master/src).</span><span class="sxs-lookup"><span data-stu-id="cc864-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/master/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="cc864-114">Crear un proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc864-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="cc864-115">En Visual Studio 2017, cree un proyecto en la página de inicio o a través de **Archivo** > **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="cc864-115">In Visual Studio 2017, create a new project from the Start Page, or via **File** > **New** > **Project**.</span></span>

* <span data-ttu-id="cc864-116">Seleccione la plantilla **Aplicación web de ASP.NET Core** disponible en la categoría **Visual C#** > **.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="cc864-116">Select the **ASP.NET Core Web Application** template available in the **Visual C#** > **.NET Core** category:</span></span>

![Cuadro de diálogo Nuevo proyecto](index/_static/new-project.png)

* <span data-ttu-id="cc864-118">Pulse **Aplicación web** y compruebe que la opción **Autenticación** está establecida en **Cuentas de usuario individuales**:</span><span class="sxs-lookup"><span data-stu-id="cc864-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Cuadro de diálogo Nueva aplicación web](index/_static/select-project.png)

<span data-ttu-id="cc864-120">Nota: Este tutorial se aplica a la versión 2.0 del SDK de ASP.NET Core, que se puede seleccionar en la parte superior del asistente.</span><span class="sxs-lookup"><span data-stu-id="cc864-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="cc864-121">Aplicación de migraciones</span><span class="sxs-lookup"><span data-stu-id="cc864-121">Apply migrations</span></span>

* <span data-ttu-id="cc864-122">Ejecute la aplicación y seleccione el vínculo **Iniciar sesión**.</span><span class="sxs-lookup"><span data-stu-id="cc864-122">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="cc864-123">Seleccione el vínculo **Register as a new user** Registrarse como usuario nuevo).</span><span class="sxs-lookup"><span data-stu-id="cc864-123">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="cc864-124">Escriba el correo electrónico y la contraseña de la cuenta nueva y, luego, seleccione **Registrarse**.</span><span class="sxs-lookup"><span data-stu-id="cc864-124">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="cc864-125">Siga estas instrucciones para aplicar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="cc864-125">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="cc864-126">Requerir SSL</span><span class="sxs-lookup"><span data-stu-id="cc864-126">Require SSL</span></span>

<span data-ttu-id="cc864-127">OAuth 2.0 requiere el uso de SSL para la autenticación mediante el protocolo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cc864-127">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="cc864-128">Los proyectos creados con las plantillas de proyecto **Aplicación web** o **API web** con ASP.NET Core 2.1 o versiones posteriores se configuran automáticamente para habilitar SSL.</span><span class="sxs-lookup"><span data-stu-id="cc864-128">Projects created using the **Web Application** or **Web API** project templates with ASP.NET Core 2.1 or later are automatically configured to enable SSL.</span></span> <span data-ttu-id="cc864-129">La aplicación se inicia con un punto de conexión predeterminado seguro si la opción **Cuentas de usuario individuales** está seleccionada en el **cuadro de diálogo Cambiar autenticación** del asistente para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="cc864-129">The app launches with a secure default endpoint if the **Individual User Accounts** option is selected in the **Change Authentication dialog** of the project wizard.</span></span>

<span data-ttu-id="cc864-130">Para obtener más información, vea <xref:security/enforcing-ssl>.</span><span class="sxs-lookup"><span data-stu-id="cc864-130">For more information, see <xref:security/enforcing-ssl>.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="cc864-131">Uso de SecretManager para almacenar los tokens asignados por los proveedores de inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="cc864-131">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="cc864-132">Los proveedores de inicio de sesión de las redes sociales asignan los tokens **Id. de aplicación** y **Secreto de la aplicación** durante el proceso de registro.</span><span class="sxs-lookup"><span data-stu-id="cc864-132">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="cc864-133">La nomenclatura puede variar en función del proveedor.</span><span class="sxs-lookup"><span data-stu-id="cc864-133">The exact token names vary by provider.</span></span> <span data-ttu-id="cc864-134">Estos tokens representan las credenciales que usa la aplicación para acceder a su API.</span><span class="sxs-lookup"><span data-stu-id="cc864-134">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="cc864-135">Los tokens constituyen los "secretos" que se pueden vincular a la configuración de la aplicación con la ayuda de [Secret Manager](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="cc864-135">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="cc864-136">Secret Manager es una alternativa más segura al almacenamiento de los tokens en un archivo de configuración, como, por ejemplo, *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="cc864-136">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cc864-137">Secret Manager solo está pensado para fines de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="cc864-137">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="cc864-138">Puede almacenar y proteger sus secretos de producción y pruebas de Azure con el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="cc864-138">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="cc864-139">Siga los pasos descritos en el tema [Ubicación de almacenamiento segura de secretos de la aplicación en el desarrollo de ASP.NET Core](xref:security/app-secrets) para almacenar los tokens asignados por cada uno de los siguientes proveedores de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="cc864-139">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="cc864-140">Configuración de los proveedores de inicio de sesión requeridos por la aplicación</span><span class="sxs-lookup"><span data-stu-id="cc864-140">Setup login providers required by your application</span></span>

<span data-ttu-id="cc864-141">En los temas siguientes encontrará información para configurar la aplicación a fin de usar los proveedores correspondientes:</span><span class="sxs-lookup"><span data-stu-id="cc864-141">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="cc864-142">Instrucciones para [Facebook](xref:security/authentication/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="cc864-142">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="cc864-143">Instrucciones para [Twitter](xref:security/authentication/twitter-logins)</span><span class="sxs-lookup"><span data-stu-id="cc864-143">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="cc864-144">Instrucciones para [Google](xref:security/authentication/google-logins)</span><span class="sxs-lookup"><span data-stu-id="cc864-144">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="cc864-145">Instrucciones para [Microsoft](xref:security/authentication/microsoft-logins)</span><span class="sxs-lookup"><span data-stu-id="cc864-145">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="cc864-146">Instrucciones para [otros proveedores](xref:security/authentication/otherlogins)</span><span class="sxs-lookup"><span data-stu-id="cc864-146">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="cc864-147">Establecimiento opcional de contraseña</span><span class="sxs-lookup"><span data-stu-id="cc864-147">Optionally set password</span></span>

<span data-ttu-id="cc864-148">Si el registro se realiza mediante un proveedor de inicio de sesión externo, no se tiene ninguna contraseña en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cc864-148">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="cc864-149">De esta forma no hace falta crear y recordar una contraseña para el sitio, aunque le hace depender del proveedor de inicio de sesión externo.</span><span class="sxs-lookup"><span data-stu-id="cc864-149">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="cc864-150">Si el proveedor de inicio de sesión externo no está disponible, no podrá iniciar sesión en el sitio web.</span><span class="sxs-lookup"><span data-stu-id="cc864-150">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="cc864-151">Para crear una contraseña e iniciar sesión con el correo electrónico establecido durante el proceso de inicio de sesión con proveedores externos:</span><span class="sxs-lookup"><span data-stu-id="cc864-151">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="cc864-152">Pulse el vínculo **Hola, &lt;alias de correo electrónico&gt;** situado en la esquina superior derecha para ir a la vista **Administración**.</span><span class="sxs-lookup"><span data-stu-id="cc864-152">Tap the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Vista Administración de la aplicación web](index/_static/pass1a.png)

* <span data-ttu-id="cc864-154">Pulse **Crear**.</span><span class="sxs-lookup"><span data-stu-id="cc864-154">Tap **Create**</span></span>

![Página para establecer la contraseña](index/_static/pass2a.png)

* <span data-ttu-id="cc864-156">Establezca una contraseña válida. Podrá usarla para iniciar sesión con su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="cc864-156">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cc864-157">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="cc864-157">Next steps</span></span>

* <span data-ttu-id="cc864-158">En este artículo se introdujo la autenticación externa y se explicaron los requisitos previos necesarios para agregar inicios de sesión externos a la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc864-158">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="cc864-159">Páginas de referencia específicas del proveedor para configurar los inicios de sesión para los proveedores requeridos por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cc864-159">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="cc864-160">Le recomendamos que conserve los datos adicionales sobre el usuario y sus tokens de acceso y actualización.</span><span class="sxs-lookup"><span data-stu-id="cc864-160">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="cc864-161">Para obtener más información, vea <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="cc864-161">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
