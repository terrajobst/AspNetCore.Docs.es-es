---
title: Autenticación con Facebook, Google y proveedores externos en ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra cómo crear una aplicación de ASP.NET Core 2.x mediante OAuth 2.0 con proveedores de autenticación externos.
ms.author: riande
ms.custom: mvc
ms.date: 1/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: 48dd8b772234ff18158423a36ed1716102bc2f31
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396147"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="7d4fe-103">Autenticación con Facebook, Google y proveedores externos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d4fe-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="7d4fe-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7d4fe-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7d4fe-105">En este tutorial se muestra cómo crear una aplicación de ASP.NET Core 2.2 que permita a los usuarios iniciar sesión mediante OAuth 2.0 con credenciales de proveedores de autenticación externos.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="7d4fe-106">En las siguientes secciones se tratan los proveedores [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins) y [Microsoft](xref:security/authentication/microsoft-logins).</span><span class="sxs-lookup"><span data-stu-id="7d4fe-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="7d4fe-107">Hay disponibles otros proveedores en paquetes de terceros como [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) y [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="7d4fe-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Iconos de redes sociales para Facebook, Twitter, Google Plus y Windows](index/_static/social.png)

<span data-ttu-id="7d4fe-109">Permitir que los usuarios inicien sesión con sus credenciales es práctico para los usuarios y transfiere muchas de las complejidades existentes en la administración del proceso de inicio de sesión a un tercero.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="7d4fe-110">Para ver ejemplos de cómo los inicios de sesión de las redes sociales pueden controlar las conversiones del tráfico y de clientes, vea los casos prácticos de [Facebook](https://www.facebook.com/unsupportedbrowser) y [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="7d4fe-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="7d4fe-111">Crear un proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d4fe-111">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="7d4fe-112">En Visual Studio 2017, cree un proyecto en la página de inicio o a través de **Archivo** > **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-112">In Visual Studio 2017, create a new project from the Start Page, or via **File** > **New** > **Project**.</span></span>

* <span data-ttu-id="7d4fe-113">Seleccione la plantilla **Aplicación web de ASP.NET Core** disponible en la categoría **Visual C#** > **.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="7d4fe-113">Select the **ASP.NET Core Web Application** template available in the **Visual C#** > **.NET Core** category:</span></span>
* <span data-ttu-id="7d4fe-114">Seleccione **Cambiar autenticación** y establezca la autenticación en **Cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-114">Select **Change Authentication** and set authentication to **Individual User Accounts**.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="7d4fe-115">Aplicación de migraciones</span><span class="sxs-lookup"><span data-stu-id="7d4fe-115">Apply migrations</span></span>

* <span data-ttu-id="7d4fe-116">Ejecute la aplicación y seleccione el vínculo **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-116">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="7d4fe-117">Escriba el correo electrónico y la contraseña de la cuenta nueva y, luego, seleccione **Registrarse**.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-117">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="7d4fe-118">Siga estas instrucciones para aplicar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-118">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="7d4fe-119">Uso de SecretManager para almacenar los tokens asignados por los proveedores de inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="7d4fe-119">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="7d4fe-120">Los proveedores de inicio de sesión de las redes sociales asignan los tokens **Id. de aplicación** y **Secreto de la aplicación** durante el proceso de registro.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-120">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="7d4fe-121">La nomenclatura puede variar en función del proveedor.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-121">The exact token names vary by provider.</span></span> <span data-ttu-id="7d4fe-122">Estos tokens representan las credenciales que usa la aplicación para acceder a su API.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-122">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="7d4fe-123">Los tokens constituyen los "secretos" que se pueden vincular a la configuración de la aplicación con la ayuda de [Secret Manager](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="7d4fe-123">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="7d4fe-124">Secret Manager es una alternativa más segura al almacenamiento de los tokens en un archivo de configuración, como, por ejemplo, *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-124">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7d4fe-125">Secret Manager solo está pensado para fines de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-125">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="7d4fe-126">Puede almacenar y proteger sus secretos de producción y pruebas de Azure con el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="7d4fe-126">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="7d4fe-127">Siga los pasos descritos en el tema [Ubicación de almacenamiento segura de secretos de la aplicación en el desarrollo de ASP.NET Core](xref:security/app-secrets) para almacenar los tokens asignados por cada uno de los siguientes proveedores de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-127">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="7d4fe-128">Configuración de los proveedores de inicio de sesión requeridos por la aplicación</span><span class="sxs-lookup"><span data-stu-id="7d4fe-128">Setup login providers required by your application</span></span>

<span data-ttu-id="7d4fe-129">En los temas siguientes encontrará información para configurar la aplicación a fin de usar los proveedores correspondientes:</span><span class="sxs-lookup"><span data-stu-id="7d4fe-129">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="7d4fe-130">Instrucciones para [Facebook](xref:security/authentication/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="7d4fe-130">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="7d4fe-131">Instrucciones para [Twitter](xref:security/authentication/twitter-logins)</span><span class="sxs-lookup"><span data-stu-id="7d4fe-131">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="7d4fe-132">Instrucciones para [Google](xref:security/authentication/google-logins)</span><span class="sxs-lookup"><span data-stu-id="7d4fe-132">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="7d4fe-133">Instrucciones para [Microsoft](xref:security/authentication/microsoft-logins)</span><span class="sxs-lookup"><span data-stu-id="7d4fe-133">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="7d4fe-134">Instrucciones para [otros proveedores](xref:security/authentication/otherlogins)</span><span class="sxs-lookup"><span data-stu-id="7d4fe-134">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="7d4fe-135">Establecimiento opcional de contraseña</span><span class="sxs-lookup"><span data-stu-id="7d4fe-135">Optionally set password</span></span>

<span data-ttu-id="7d4fe-136">Si el registro se realiza mediante un proveedor de inicio de sesión externo, no se tiene ninguna contraseña en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-136">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="7d4fe-137">De esta forma no hace falta crear y recordar una contraseña para el sitio, aunque le hace depender del proveedor de inicio de sesión externo.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-137">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="7d4fe-138">Si el proveedor de inicio de sesión externo no está disponible, no podrá iniciar sesión en el sitio web.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-138">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="7d4fe-139">Para crear una contraseña e iniciar sesión con el correo electrónico establecido durante el proceso de inicio de sesión con proveedores externos:</span><span class="sxs-lookup"><span data-stu-id="7d4fe-139">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="7d4fe-140">Seleccione el vínculo **Hola, &lt;alias de correo electrónico&gt;** situado en la esquina superior derecha para ir a la vista **Administración**.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-140">Select the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Vista Administración de la aplicación web](index/_static/pass1a.png)

* <span data-ttu-id="7d4fe-142">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-142">Select **Create**</span></span>

![Página para establecer la contraseña](index/_static/pass2a.png)

* <span data-ttu-id="7d4fe-144">Establezca una contraseña válida. Podrá usarla para iniciar sesión con su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-144">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d4fe-145">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7d4fe-145">Next steps</span></span>

* <span data-ttu-id="7d4fe-146">En este artículo se introdujo la autenticación externa y se explicaron los requisitos previos necesarios para agregar inicios de sesión externos a la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-146">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="7d4fe-147">Páginas de referencia específicas del proveedor para configurar los inicios de sesión para los proveedores requeridos por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-147">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="7d4fe-148">Le recomendamos que conserve los datos adicionales sobre el usuario y sus tokens de acceso y actualización.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-148">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="7d4fe-149">Para obtener más información, vea <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="7d4fe-149">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
