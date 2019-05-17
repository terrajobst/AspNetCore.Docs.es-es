---
title: Autenticación con Facebook, Google y proveedores externos en ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra cómo crear una aplicación de ASP.NET Core 2.x mediante OAuth 2.0 con proveedores de autenticación externos.
ms.author: riande
ms.custom: mvc
ms.date: 4/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: e2d68ac93bdcfa2fc015e8447ea38626787cdb02
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65451046"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="44e37-103">Autenticación con Facebook, Google y proveedores externos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44e37-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="44e37-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="44e37-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="44e37-105">En este tutorial se muestra cómo crear una aplicación de ASP.NET Core 2.2 que permita a los usuarios iniciar sesión mediante OAuth 2.0 con credenciales de proveedores de autenticación externos.</span><span class="sxs-lookup"><span data-stu-id="44e37-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="44e37-106">En las siguientes secciones se tratan los proveedores [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins) y [Microsoft](xref:security/authentication/microsoft-logins).</span><span class="sxs-lookup"><span data-stu-id="44e37-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="44e37-107">Hay disponibles otros proveedores en paquetes de terceros como [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) y [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="44e37-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Iconos de redes sociales para Facebook, Twitter, Google Plus y Windows](index/_static/social.png)

<span data-ttu-id="44e37-109">El hecho de permitir a los usuarios iniciar sesión con sus credenciales:</span><span class="sxs-lookup"><span data-stu-id="44e37-109">Enabling users to sign in with their existing credentials:</span></span>
* <span data-ttu-id="44e37-110">resulta muy práctico para ellos;</span><span class="sxs-lookup"><span data-stu-id="44e37-110">Is convenient for the users.</span></span>
* <span data-ttu-id="44e37-111">transfiere muchas de las complejidades de administrar el proceso de inicio de sesión a un tercero.</span><span class="sxs-lookup"><span data-stu-id="44e37-111">Shifts many of the complexities of managing the sign-in process onto a third party.</span></span> 

<span data-ttu-id="44e37-112">Para ver ejemplos de cómo los inicios de sesión de las redes sociales pueden controlar las conversiones del tráfico y de clientes, vea los casos prácticos de [Facebook](https://www.facebook.com/unsupportedbrowser) y [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="44e37-112">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="44e37-113">Crear un proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44e37-113">Create a New ASP.NET Core Project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="44e37-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="44e37-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="44e37-115">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="44e37-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="44e37-116">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="44e37-116">Create a new ASP.NET Core Web Application.</span></span>
* <span data-ttu-id="44e37-117">Seleccione **ASP.NET Core 2.2** en la lista desplegable y, luego, **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="44e37-117">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>
* <span data-ttu-id="44e37-118">Seleccione **Cambiar autenticación** y establezca la autenticación en **Cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="44e37-118">Select **Change Authentication** and set authentication to **Individual User Accounts**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="44e37-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="44e37-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="44e37-120">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="44e37-120">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="44e37-121">Cambie los directorios (`cd`) a una carpeta que contenga el proyecto.</span><span class="sxs-lookup"><span data-stu-id="44e37-121">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="44e37-122">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="44e37-122">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o WebApp1 -au Individual -uld
  code -r WebApp1
  ```

  * <span data-ttu-id="44e37-123">El comando `dotnet new` crea un nuevo proyecto de Razor Pages en la carpeta *WebApp1*.</span><span class="sxs-lookup"><span data-stu-id="44e37-123">The `dotnet new` command creates a new Razor Pages project in the *WebApp1* folder.</span></span>
  * <span data-ttu-id="44e37-124">`-uld` usa LocalDB en lugar de SQLite.</span><span class="sxs-lookup"><span data-stu-id="44e37-124">`-uld` uses LocalDB instead of SQLite.</span></span> <span data-ttu-id="44e37-125">Omita `-uld` para usar SQLite.</span><span class="sxs-lookup"><span data-stu-id="44e37-125">Omit `-uld` to use SQLite.</span></span>
  * <span data-ttu-id="44e37-126">`-au Individual` crea el código para la autenticación individual.</span><span class="sxs-lookup"><span data-stu-id="44e37-126">`-au Individual` creates the code for Individual authentication.</span></span>
  * <span data-ttu-id="44e37-127">El comando `code` abre la carpeta *WebApp1* en una nueva instancia de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="44e37-127">The `code` command opens the *WebApp1* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="44e37-128">Se muestra un cuadro de diálogo con el texto **Required assets to build and debug are missing from 'WebApp1'. Add them?** (Faltan los activos necesarios para compilar y depurar en "RazorPagesMovie". ¿Desea agregarlos?).</span><span class="sxs-lookup"><span data-stu-id="44e37-128">A dialog box appears with **Required assets to build and debug are missing from 'WebApp1'. Add them?**</span></span>

* <span data-ttu-id="44e37-129">Seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="44e37-129">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="44e37-130">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="44e37-130">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="44e37-131">Desde un terminal, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="44e37-131">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o WebApp1 -au Individual
```

<span data-ttu-id="44e37-132">Los comandos anteriores utilizan la [CLI de .NET Core](/dotnet/core/tools/dotnet) para crear un proyecto de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="44e37-132">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="44e37-133">Abrir el proyecto</span><span class="sxs-lookup"><span data-stu-id="44e37-133">Open the project</span></span>

<span data-ttu-id="44e37-134">En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *WebApp1.csproj*.</span><span class="sxs-lookup"><span data-stu-id="44e37-134">From Visual Studio, select **File > Open**, and then select the *WebApp1.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="apply-migrations"></a><span data-ttu-id="44e37-135">Aplicación de migraciones</span><span class="sxs-lookup"><span data-stu-id="44e37-135">Apply migrations</span></span>

* <span data-ttu-id="44e37-136">Ejecute la aplicación y seleccione el vínculo **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="44e37-136">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="44e37-137">Escriba el correo electrónico y la contraseña de la cuenta nueva y, luego, seleccione **Registrarse**.</span><span class="sxs-lookup"><span data-stu-id="44e37-137">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="44e37-138">Siga estas instrucciones para aplicar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="44e37-138">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="44e37-139">Uso de SecretManager para almacenar los tokens asignados por los proveedores de inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="44e37-139">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="44e37-140">Los proveedores de inicio de sesión de las redes sociales asignan los tokens **Id. de aplicación** y **Secreto de la aplicación** durante el proceso de registro.</span><span class="sxs-lookup"><span data-stu-id="44e37-140">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="44e37-141">La nomenclatura puede variar en función del proveedor.</span><span class="sxs-lookup"><span data-stu-id="44e37-141">The exact token names vary by provider.</span></span> <span data-ttu-id="44e37-142">Estos tokens representan las credenciales que usa la aplicación para acceder a su API.</span><span class="sxs-lookup"><span data-stu-id="44e37-142">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="44e37-143">Los tokens constituyen los "secretos" que se pueden vincular a la configuración de la aplicación con la ayuda de [Secret Manager](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="44e37-143">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="44e37-144">Secret Manager es una alternativa más segura al almacenamiento de los tokens en un archivo de configuración, como, por ejemplo, *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="44e37-144">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="44e37-145">Secret Manager solo está pensado para fines de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="44e37-145">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="44e37-146">Puede almacenar y proteger sus secretos de producción y pruebas de Azure con el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="44e37-146">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="44e37-147">Siga los pasos descritos en el tema [Ubicación de almacenamiento segura de secretos de la aplicación en el desarrollo de ASP.NET Core](xref:security/app-secrets) para almacenar los tokens asignados por cada uno de los siguientes proveedores de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="44e37-147">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="44e37-148">Configuración de los proveedores de inicio de sesión requeridos por la aplicación</span><span class="sxs-lookup"><span data-stu-id="44e37-148">Setup login providers required by your application</span></span>

<span data-ttu-id="44e37-149">En los temas siguientes encontrará información para configurar la aplicación a fin de usar los proveedores correspondientes:</span><span class="sxs-lookup"><span data-stu-id="44e37-149">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="44e37-150">Instrucciones para [Facebook](xref:security/authentication/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="44e37-150">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="44e37-151">Instrucciones para [Twitter](xref:security/authentication/twitter-logins)</span><span class="sxs-lookup"><span data-stu-id="44e37-151">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="44e37-152">Instrucciones para [Google](xref:security/authentication/google-logins)</span><span class="sxs-lookup"><span data-stu-id="44e37-152">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="44e37-153">Instrucciones para [Microsoft](xref:security/authentication/microsoft-logins)</span><span class="sxs-lookup"><span data-stu-id="44e37-153">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="44e37-154">Instrucciones para [otros proveedores](xref:security/authentication/otherlogins)</span><span class="sxs-lookup"><span data-stu-id="44e37-154">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="44e37-155">Establecimiento opcional de contraseña</span><span class="sxs-lookup"><span data-stu-id="44e37-155">Optionally set password</span></span>

<span data-ttu-id="44e37-156">Si el registro se realiza mediante un proveedor de inicio de sesión externo, no se tiene ninguna contraseña en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="44e37-156">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="44e37-157">De esta forma no hace falta crear y recordar una contraseña para el sitio, aunque le hace depender del proveedor de inicio de sesión externo.</span><span class="sxs-lookup"><span data-stu-id="44e37-157">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="44e37-158">Si el proveedor de inicio de sesión externo no está disponible, no podrá iniciar sesión en el sitio web.</span><span class="sxs-lookup"><span data-stu-id="44e37-158">If the external login provider is unavailable, you won't be able to sign in to the web site.</span></span>

<span data-ttu-id="44e37-159">Para crear una contraseña e iniciar sesión con el correo electrónico establecido durante el proceso de inicio de sesión con proveedores externos:</span><span class="sxs-lookup"><span data-stu-id="44e37-159">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="44e37-160">Seleccione el vínculo **Hola, &lt;alias de correo electrónico&gt;** situado en la esquina superior derecha para ir a la vista **Administración**.</span><span class="sxs-lookup"><span data-stu-id="44e37-160">Select the **Hello &lt;email alias&gt;** link at the top-right corner to navigate to the **Manage** view.</span></span>

![Vista Administración de la aplicación web](index/_static/pass1a.png)

* <span data-ttu-id="44e37-162">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="44e37-162">Select **Create**</span></span>

![Página para establecer la contraseña](index/_static/pass2a.png)

* <span data-ttu-id="44e37-164">Establezca una contraseña válida. Podrá usarla para iniciar sesión con su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="44e37-164">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="44e37-165">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="44e37-165">Next steps</span></span>

* <span data-ttu-id="44e37-166">En este artículo se introdujo la autenticación externa y se explicaron los requisitos previos necesarios para agregar inicios de sesión externos a la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="44e37-166">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="44e37-167">Páginas de referencia específicas del proveedor para configurar los inicios de sesión para los proveedores requeridos por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="44e37-167">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="44e37-168">Le recomendamos que conserve los datos adicionales sobre el usuario y sus tokens de acceso y actualización.</span><span class="sxs-lookup"><span data-stu-id="44e37-168">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="44e37-169">Para obtener más información, vea <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="44e37-169">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
