---
title: "Habilitación de la autenticación con Facebook, Google y otros proveedores externos"
author: rick-anderson
description: 
keywords: "ASP.NET Core, autenticación, social, proveedores de autenticación, google, facebook, twitter, cuenta de microsoft"
ms.author: riande
manager: wpickett
ms.date: 11/1/2016
ms.topic: article
ms.assetid: eda7ee17-f38c-462e-8d1d-63f459901cf3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: ee6f08fe5d5dcf2883b5404f176d1f3c5ce2cd5b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a><span data-ttu-id="9732e-103">Habilitación de la autenticación con Facebook, Google y otros proveedores externos</span><span class="sxs-lookup"><span data-stu-id="9732e-103">Enabling authentication using Facebook, Google and other external providers</span></span>

<a name=security-authentication-social-logins></a>

<span data-ttu-id="9732e-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9732e-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9732e-105">En este tutorial se muestra cómo crear una aplicación de ASP.NET Core 2.x que permita a los usuarios iniciar sesión mediante OAuth 2.0 con credenciales de proveedores de autenticación externos.</span><span class="sxs-lookup"><span data-stu-id="9732e-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="9732e-106">En las siguientes secciones se tratan los proveedores [Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md) y [Microsoft](microsoft-logins.md).</span><span class="sxs-lookup"><span data-stu-id="9732e-106">[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), and [Microsoft](microsoft-logins.md) providers are covered in the following sections.</span></span> <span data-ttu-id="9732e-107">Hay disponibles otros proveedores en paquetes de terceros como [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) y [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="9732e-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Iconos de redes sociales para Facebook, Twitter, Google Plus y Windows](index/_static/social.png)

<span data-ttu-id="9732e-109">Permitir que los usuarios inicien sesión con sus credenciales es práctico para los usuarios y transfiere muchas de las complejidades existentes en la administración del proceso de inicio de sesión a un tercero.</span><span class="sxs-lookup"><span data-stu-id="9732e-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="9732e-110">Para ver ejemplos de cómo los inicios de sesión de las redes sociales pueden controlar las conversiones del tráfico y de clientes, vea los casos prácticos de [Facebook](https://developers.facebook.com/case-studies) y [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="9732e-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://developers.facebook.com/case-studies) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="9732e-111">Nota: Los paquetes que se presentan aquí sintetizan en gran parte la complejidad del flujo de autenticación de OAuth, pero conocer los detalles puede ser necesario a la hora de solucionar problemas.</span><span class="sxs-lookup"><span data-stu-id="9732e-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="9732e-112">Hay disponibles numerosos recursos; por ejemplo, vea [An Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) (Introducción a OAuth 2) o [Understanding OAuth2](http://www.bubblecode.net/en/2016/01/22/understanding-oauth2/) (Descripción de OAuth2).</span><span class="sxs-lookup"><span data-stu-id="9732e-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/en/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="9732e-113">Algunos problemas se pueden resolver examinando el [código fuente de ASP.NET Core de los paquetes de los proveedores](https://github.com/aspnet/Security/tree/dev/src).</span><span class="sxs-lookup"><span data-stu-id="9732e-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="9732e-114">Crear un proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9732e-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="9732e-115">En Visual Studio 2017, cree un proyecto en la página de inicio o a través de **Archivo > Nuevo > Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="9732e-115">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="9732e-116">Seleccione la plantilla **Aplicación web de ASP.NET Core** disponible en la categoría **Visual C# > .NET Core**:</span><span class="sxs-lookup"><span data-stu-id="9732e-116">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![Cuadro de diálogo Nuevo proyecto](index/_static/new-project.png)

* <span data-ttu-id="9732e-118">Pulse **Aplicación web** y compruebe que la opción **Autenticación** está establecida en **Cuentas de usuario individuales**:</span><span class="sxs-lookup"><span data-stu-id="9732e-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Cuadro de diálogo Nueva aplicación web](index/_static/select-project.png)

<span data-ttu-id="9732e-120">Nota: Este tutorial se aplica a la versión 2.0 del SDK de ASP.NET Core, que se puede seleccionar en la parte superior del asistente.</span><span class="sxs-lookup"><span data-stu-id="9732e-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="9732e-121">Requerir SSL</span><span class="sxs-lookup"><span data-stu-id="9732e-121">Require SSL</span></span>

<span data-ttu-id="9732e-122">OAuth 2.0 requiere el uso de SSL para la autenticación mediante el protocolo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9732e-122">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="9732e-123">Nota: Los proyectos creados con plantillas de proyecto de **aplicación web** o **API Web** de ASP.NET Core 2.x se configuran automáticamente para habilitar SSL e iniciarse con una dirección URL https si la opción **Cuentas de usuario individuales** estaba seleccionada en el cuadro de diálogo **Cambiar autenticación** del asistente de proyectos, como se muestra arriba.</span><span class="sxs-lookup"><span data-stu-id="9732e-123">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="9732e-124">Aprenda a habilitar SSL manualmente siguiendo los pasos descritos en el tema [Setting up HTTPS for development in ASP.NET Core](xref:security/https) (Configurar HTTPS para el desarrollo en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="9732e-124">Learn how to enable SSL manually by following the steps in [Setting up HTTPS for development in ASP.NET Core](xref:security/https) topic.</span></span>

* <span data-ttu-id="9732e-125">Luego, requiera SSL en el sitio siguiendo los pasos descritos en el tema [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) (Exigir SSL en una aplicación de ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="9732e-125">Then, require SSL on your site by following the steps in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="9732e-126">Uso de SecretManager para almacenar los tokens asignados por los proveedores de inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="9732e-126">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="9732e-127">Los proveedores de inicio de sesión de las redes sociales asignan tokens de **Id. de aplicación** y de **secreto de aplicación** durante el proceso de registro (la nomenclatura exacta varía según el proveedor).</span><span class="sxs-lookup"><span data-stu-id="9732e-127">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process (exact naming varies by provider).</span></span>

<span data-ttu-id="9732e-128">Estos valores son el *nombre de usuario* y la *contraseña* que usa la aplicación para obtener acceso a la API. Conforman los "secretos" que se pueden vincular a la configuración de la aplicación con la ayuda del **Administrador de secretos**, en vez de almacenarlos directamente en archivos de configuración o de codificarlos de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="9732e-128">These values are effectively the *user name* and *password* your application uses to access their API, and constitute the "secrets" that can be linked to your application configuration with the help of **Secret Manager** instead of storing them in configuration files directly or hard-coding them.</span></span>

<span data-ttu-id="9732e-129">Siga los pasos descritos en el tema [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets) (Almacenamiento seguro de secretos de aplicación durante el desarrollo en ASP.NET Core) para poder almacenar los tokens asignados por cada uno de los siguientes proveedores de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="9732e-129">Follow the steps in [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets) topic so that you can store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="9732e-130">Configuración de los proveedores de inicio de sesión requeridos por la aplicación</span><span class="sxs-lookup"><span data-stu-id="9732e-130">Setup login providers required by your application</span></span>

<span data-ttu-id="9732e-131">En los temas siguientes encontrará información para configurar la aplicación a fin de usar los proveedores correspondientes:</span><span class="sxs-lookup"><span data-stu-id="9732e-131">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="9732e-132">Instrucciones para [Facebook](facebook-logins.md)</span><span class="sxs-lookup"><span data-stu-id="9732e-132">[Facebook](facebook-logins.md) instructions</span></span>
* <span data-ttu-id="9732e-133">Instrucciones para [Twitter](twitter-logins.md)</span><span class="sxs-lookup"><span data-stu-id="9732e-133">[Twitter](twitter-logins.md) instructions</span></span>
* <span data-ttu-id="9732e-134">Instrucciones para [Google](google-logins.md)</span><span class="sxs-lookup"><span data-stu-id="9732e-134">[Google](google-logins.md) instructions</span></span>
* <span data-ttu-id="9732e-135">Instrucciones para [Microsoft](microsoft-logins.md)</span><span class="sxs-lookup"><span data-stu-id="9732e-135">[Microsoft](microsoft-logins.md) instructions</span></span>
* <span data-ttu-id="9732e-136">Instrucciones para [otros proveedores](other-logins.md)</span><span class="sxs-lookup"><span data-stu-id="9732e-136">[Other provider](other-logins.md) instructions</span></span>

## <a name="optionally-set-password"></a><span data-ttu-id="9732e-137">Establecimiento opcional de contraseña</span><span class="sxs-lookup"><span data-stu-id="9732e-137">Optionally set password</span></span>

<span data-ttu-id="9732e-138">Si el registro se realiza mediante un proveedor de inicio de sesión externo, no se tiene ninguna contraseña en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9732e-138">When you register with an external login provider, you do not have a password registered with the app.</span></span> <span data-ttu-id="9732e-139">De esta forma no hace falta crear y recordar una contraseña para el sitio, aunque le hace depender del proveedor de inicio de sesión externo.</span><span class="sxs-lookup"><span data-stu-id="9732e-139">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="9732e-140">Si el proveedor de inicio de sesión externo no está disponible, no podrá iniciar sesión en el sitio web.</span><span class="sxs-lookup"><span data-stu-id="9732e-140">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="9732e-141">Para crear una contraseña e iniciar sesión con el correo electrónico establecido durante el proceso de inicio de sesión con proveedores externos:</span><span class="sxs-lookup"><span data-stu-id="9732e-141">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="9732e-142">Pulse el vínculo **Hola <email alias>** situado en la esquina superior derecha para ir a la vista **Administración**.</span><span class="sxs-lookup"><span data-stu-id="9732e-142">Tap the **Hello <email alias>** link at the top right corner to navigate to the **Manage** view.</span></span>

![Vista Administración de la aplicación web](index/_static/pass1a.png)

* <span data-ttu-id="9732e-144">Pulse **Crear**.</span><span class="sxs-lookup"><span data-stu-id="9732e-144">Tap **Create**</span></span>

![Página para establecer la contraseña](index/_static/pass2a.png)

* <span data-ttu-id="9732e-146">Establezca una contraseña válida. Podrá usarla para iniciar sesión con su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="9732e-146">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9732e-147">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="9732e-147">Next steps</span></span>

* <span data-ttu-id="9732e-148">En este artículo se introdujo la autenticación externa y se explicaron los requisitos previos necesarios para agregar inicios de sesión externos a la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9732e-148">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="9732e-149">Páginas de referencia específicas del proveedor para configurar los inicios de sesión para los proveedores requeridos por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9732e-149">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
