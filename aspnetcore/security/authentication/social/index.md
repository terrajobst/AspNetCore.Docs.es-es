---
title: Autenticación con Facebook, Google y proveedores externos en ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra cómo crear una aplicación de ASP.NET Core 2.x mediante OAuth 2.0 con proveedores de autenticación externos.
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2019
uid: security/authentication/social/index
ms.openlocfilehash: edaf9eeaf02879b2f7816bab0eb373a7de640c05
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082506"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="2a551-103">Autenticación con Facebook, Google y proveedores externos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a551-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="2a551-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2a551-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2a551-105">En este tutorial se muestra cómo crear una aplicación de ASP.NET Core 2.2 que permita a los usuarios iniciar sesión mediante OAuth 2.0 con credenciales de proveedores de autenticación externos.</span><span class="sxs-lookup"><span data-stu-id="2a551-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="2a551-106">En las siguientes secciones se tratan los proveedores [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins) y [Microsoft](xref:security/authentication/microsoft-logins).</span><span class="sxs-lookup"><span data-stu-id="2a551-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="2a551-107">Hay disponibles otros proveedores en paquetes de terceros como [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) y [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="2a551-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Iconos de redes sociales para Facebook, Twitter, Google Plus y Windows](index/_static/social.png)

<span data-ttu-id="2a551-109">El hecho de permitir a los usuarios iniciar sesión con sus credenciales:</span><span class="sxs-lookup"><span data-stu-id="2a551-109">Enabling users to sign in with their existing credentials:</span></span>
* <span data-ttu-id="2a551-110">resulta muy práctico para ellos;</span><span class="sxs-lookup"><span data-stu-id="2a551-110">Is convenient for the users.</span></span>
* <span data-ttu-id="2a551-111">transfiere muchas de las complejidades de administrar el proceso de inicio de sesión a un tercero.</span><span class="sxs-lookup"><span data-stu-id="2a551-111">Shifts many of the complexities of managing the sign-in process onto a third party.</span></span> 

<span data-ttu-id="2a551-112">Para ver ejemplos de cómo los inicios de sesión de las redes sociales pueden controlar las conversiones del tráfico y de clientes, vea los casos prácticos de [Facebook](https://www.facebook.com/unsupportedbrowser) y [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="2a551-112">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="2a551-113">Crear un proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a551-113">Create a New ASP.NET Core Project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2a551-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2a551-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2a551-115">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="2a551-115">Create a new project.</span></span>
* <span data-ttu-id="2a551-116">Seleccione **Aplicación web de ASP.NET Core** y **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="2a551-116">Select **ASP.NET Core Web Application** and **Next**.</span></span>
* <span data-ttu-id="2a551-117">Proporcione un **Nombre del proyecto** y confirme o cambie la **Ubicación**.</span><span class="sxs-lookup"><span data-stu-id="2a551-117">Provide a **Project name** and confirm or change the **Location**.</span></span> <span data-ttu-id="2a551-118">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="2a551-118">Select **Create**.</span></span>
* <span data-ttu-id="2a551-119">Seleccione **ASP.NET Core 2.2** en la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="2a551-119">Select **ASP.NET Core 2.2** in the drop down.</span></span> <span data-ttu-id="2a551-120">Seleccione **Aplicación web** en la lista de plantillas.</span><span class="sxs-lookup"><span data-stu-id="2a551-120">Select **Web Application** in the template list.</span></span>
* <span data-ttu-id="2a551-121">En **Autenticación**, seleccione **Cambiar** y establezca la autenticación en **Cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="2a551-121">Under **Authentication**, select **Change** and set the authentication to **Individual User Accounts**.</span></span> <span data-ttu-id="2a551-122">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="2a551-122">Select **OK**.</span></span>
* <span data-ttu-id="2a551-123">En la ventana **Crear una aplicación web ASP.NET Core**, seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="2a551-123">In the **Create a new ASP.NET Core Web Application** window, select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2a551-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2a551-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2a551-125">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="2a551-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="2a551-126">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="2a551-126">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="2a551-127">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="2a551-127">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o WebApp1 -au Individual -uld
  code -r WebApp1
  ```

  * <span data-ttu-id="2a551-128">El comando `dotnet new` crea un nuevo proyecto de Razor Pages en la carpeta *WebApp1*.</span><span class="sxs-lookup"><span data-stu-id="2a551-128">The `dotnet new` command creates a new Razor Pages project in the *WebApp1* folder.</span></span>
  * <span data-ttu-id="2a551-129">`-uld` usa LocalDB en lugar de SQLite.</span><span class="sxs-lookup"><span data-stu-id="2a551-129">`-uld` uses LocalDB instead of SQLite.</span></span> <span data-ttu-id="2a551-130">Omita `-uld` para usar SQLite.</span><span class="sxs-lookup"><span data-stu-id="2a551-130">Omit `-uld` to use SQLite.</span></span>
  * <span data-ttu-id="2a551-131">`-au Individual` crea el código para la autenticación individual.</span><span class="sxs-lookup"><span data-stu-id="2a551-131">`-au Individual` creates the code for Individual authentication.</span></span>
  * <span data-ttu-id="2a551-132">El comando `code` abre la carpeta *WebApp1* en una nueva instancia de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2a551-132">The `code` command opens the *WebApp1* folder in a new instance of Visual Studio Code.</span></span>

* <span data-ttu-id="2a551-133">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'WebApp1'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="2a551-133">A dialog box appears with **Required assets to build and debug are missing from 'WebApp1'. Add them?**</span></span> <span data-ttu-id="2a551-134">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="2a551-134">Select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2a551-135">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="2a551-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2a551-136">Seleccione **Archivo** > **Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="2a551-136">Select **File** > **New Solution**.</span></span>
* <span data-ttu-id="2a551-137">Seleccione **.NET Core** > **Aplicación** en la barra lateral.</span><span class="sxs-lookup"><span data-stu-id="2a551-137">Select **.NET Core** > **App** in the sidebar.</span></span> <span data-ttu-id="2a551-138">Seleccione la plantilla **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="2a551-138">Select the **Web Application** template.</span></span> <span data-ttu-id="2a551-139">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="2a551-139">Select **Next**.</span></span>
* <span data-ttu-id="2a551-140">Establezca la **Plataforma de destino** del menú desplegable en **.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="2a551-140">Set the **Target Framework** drop down to **.NET Core 2.2**.</span></span> <span data-ttu-id="2a551-141">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="2a551-141">Select **Next**.</span></span>
* <span data-ttu-id="2a551-142">Proporcione un **Nombre del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="2a551-142">Provide a **Project Name**.</span></span> <span data-ttu-id="2a551-143">Confirme o cambie la **Ubicación**.</span><span class="sxs-lookup"><span data-stu-id="2a551-143">Confirm or change the **Location**.</span></span> <span data-ttu-id="2a551-144">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="2a551-144">Select **Create**.</span></span>

---

## <a name="apply-migrations"></a><span data-ttu-id="2a551-145">Aplicación de migraciones</span><span class="sxs-lookup"><span data-stu-id="2a551-145">Apply migrations</span></span>

* <span data-ttu-id="2a551-146">Ejecute la aplicación y seleccione el vínculo **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="2a551-146">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="2a551-147">Escriba el correo electrónico y la contraseña de la cuenta nueva y, luego, seleccione **Registrarse**.</span><span class="sxs-lookup"><span data-stu-id="2a551-147">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="2a551-148">Siga estas instrucciones para aplicar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="2a551-148">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="2a551-149">Uso de SecretManager para almacenar los tokens asignados por los proveedores de inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="2a551-149">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="2a551-150">Los proveedores de inicio de sesión de las redes sociales asignan los tokens **Id. de aplicación** y **Secreto de la aplicación** durante el proceso de registro.</span><span class="sxs-lookup"><span data-stu-id="2a551-150">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="2a551-151">La nomenclatura puede variar en función del proveedor.</span><span class="sxs-lookup"><span data-stu-id="2a551-151">The exact token names vary by provider.</span></span> <span data-ttu-id="2a551-152">Estos tokens representan las credenciales que usa la aplicación para acceder a su API.</span><span class="sxs-lookup"><span data-stu-id="2a551-152">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="2a551-153">Los tokens constituyen los "secretos" que se pueden vincular a la configuración de la aplicación con la ayuda de [Secret Manager](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="2a551-153">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="2a551-154">Secret Manager es una alternativa más segura al almacenamiento de los tokens en un archivo de configuración, como, por ejemplo, *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2a551-154">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2a551-155">Secret Manager solo está pensado para fines de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="2a551-155">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="2a551-156">Puede almacenar y proteger sus secretos de producción y pruebas de Azure con el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="2a551-156">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="2a551-157">Siga los pasos descritos en el tema [Ubicación de almacenamiento segura de secretos de la aplicación en el desarrollo de ASP.NET Core](xref:security/app-secrets) para almacenar los tokens asignados por cada uno de los siguientes proveedores de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="2a551-157">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="2a551-158">Configuración de los proveedores de inicio de sesión requeridos por la aplicación</span><span class="sxs-lookup"><span data-stu-id="2a551-158">Setup login providers required by your application</span></span>

<span data-ttu-id="2a551-159">En los temas siguientes encontrará información para configurar la aplicación a fin de usar los proveedores correspondientes:</span><span class="sxs-lookup"><span data-stu-id="2a551-159">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="2a551-160">Instrucciones para [Facebook](xref:security/authentication/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="2a551-160">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="2a551-161">Instrucciones para [Twitter](xref:security/authentication/twitter-logins)</span><span class="sxs-lookup"><span data-stu-id="2a551-161">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="2a551-162">Instrucciones para [Google](xref:security/authentication/google-logins)</span><span class="sxs-lookup"><span data-stu-id="2a551-162">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="2a551-163">Instrucciones para [Microsoft](xref:security/authentication/microsoft-logins)</span><span class="sxs-lookup"><span data-stu-id="2a551-163">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="2a551-164">Instrucciones para [otros proveedores](xref:security/authentication/otherlogins)</span><span class="sxs-lookup"><span data-stu-id="2a551-164">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="2a551-165">Establecimiento opcional de contraseña</span><span class="sxs-lookup"><span data-stu-id="2a551-165">Optionally set password</span></span>

<span data-ttu-id="2a551-166">Si el registro se realiza mediante un proveedor de inicio de sesión externo, no se tiene ninguna contraseña en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2a551-166">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="2a551-167">De esta forma no hace falta crear y recordar una contraseña para el sitio, aunque le hace depender del proveedor de inicio de sesión externo.</span><span class="sxs-lookup"><span data-stu-id="2a551-167">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="2a551-168">Si el proveedor de inicio de sesión externo no está disponible, no podrá iniciar sesión en el sitio web.</span><span class="sxs-lookup"><span data-stu-id="2a551-168">If the external login provider is unavailable, you won't be able to sign in to the web site.</span></span>

<span data-ttu-id="2a551-169">Para crear una contraseña e iniciar sesión con el correo electrónico establecido durante el proceso de inicio de sesión con proveedores externos:</span><span class="sxs-lookup"><span data-stu-id="2a551-169">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="2a551-170">Seleccione el vínculo **Hola, &lt;alias de correo electrónico&gt;** situado en la esquina superior derecha para ir a la vista **Administración**.</span><span class="sxs-lookup"><span data-stu-id="2a551-170">Select the **Hello &lt;email alias&gt;** link at the top-right corner to navigate to the **Manage** view.</span></span>

![Vista Administración de la aplicación web](index/_static/pass1a.png)

* <span data-ttu-id="2a551-172">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="2a551-172">Select **Create**</span></span>

![Página para establecer la contraseña](index/_static/pass2a.png)

* <span data-ttu-id="2a551-174">Establezca una contraseña válida. Podrá usarla para iniciar sesión con su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="2a551-174">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a551-175">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="2a551-175">Next steps</span></span>

* <span data-ttu-id="2a551-176">En este artículo se introdujo la autenticación externa y se explicaron los requisitos previos necesarios para agregar inicios de sesión externos a la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a551-176">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="2a551-177">Páginas de referencia específicas del proveedor para configurar los inicios de sesión para los proveedores requeridos por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2a551-177">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="2a551-178">Le recomendamos que conserve los datos adicionales sobre el usuario y sus tokens de acceso y actualización.</span><span class="sxs-lookup"><span data-stu-id="2a551-178">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="2a551-179">Para más información, consulte <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="2a551-179">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
