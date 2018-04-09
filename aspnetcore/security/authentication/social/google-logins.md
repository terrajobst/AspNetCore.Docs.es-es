---
title: Programa de instalación de inicio de sesión externo de Google en ASP.NET Core
author: rick-anderson
description: Este tutorial muestra la integración de autenticación de usuario de cuenta de Google en una aplicación existente de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/google-logins
ms.openlocfilehash: ab49eb1c45d69ff918b25190d7b94a105ff13972
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="50d39-103">Programa de instalación de inicio de sesión externo de Google en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="50d39-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="50d39-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="50d39-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="50d39-105">Este tutorial muestra cómo permitir a los usuarios iniciar sesión con su cuenta de Google + usando un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="50d39-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="50d39-106">Comenzamos siguiendo el [pasos oficiales](https://developers.google.com/identity/sign-in/web/devconsole-project) para crear una nueva aplicación de consola de API de Google.</span><span class="sxs-lookup"><span data-stu-id="50d39-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="50d39-107">Crear la aplicación de consola de API de Google</span><span class="sxs-lookup"><span data-stu-id="50d39-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="50d39-108">Vaya a [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="50d39-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="50d39-109">Si ya no tiene una cuenta de Google, use **más opciones** > **[crear cuenta](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  vínculo para crear una:</span><span class="sxs-lookup"><span data-stu-id="50d39-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Consola de API de Google](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="50d39-111">Se le redirigirá a **biblioteca API Manager** página:</span><span class="sxs-lookup"><span data-stu-id="50d39-111">You are redirected to **API Manager Library** page:</span></span>

![Página de la biblioteca de API de administrador](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="50d39-113">Pulse **crear** y escriba su **nombre del proyecto**:</span><span class="sxs-lookup"><span data-stu-id="50d39-113">Tap **Create** and enter your **Project name**:</span></span>

![Cuadro de diálogo Nuevo proyecto](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="50d39-115">Después de aceptar el cuadro de diálogo, se le redirigirá a la página de la biblioteca que le permite elegir las características de la aplicación nuevo.</span><span class="sxs-lookup"><span data-stu-id="50d39-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="50d39-116">Buscar **API de Google +** en la lista y haga clic en el vínculo para agregar la característica de API:</span><span class="sxs-lookup"><span data-stu-id="50d39-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Página de la biblioteca de API de administrador](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="50d39-118">Se muestra la página de la API recién agregada.</span><span class="sxs-lookup"><span data-stu-id="50d39-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="50d39-119">Pulse **habilitar** para agregar inicio de sesión de Google + en la característica a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="50d39-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![Página de Google + API del Administrador de API](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="50d39-121">Después de habilitar la API, pulse **crear credenciales** para configurar los secretos:</span><span class="sxs-lookup"><span data-stu-id="50d39-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![Página de Google + API del Administrador de API](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="50d39-123">Elija:</span><span class="sxs-lookup"><span data-stu-id="50d39-123">Choose:</span></span>
   * <span data-ttu-id="50d39-124">**API de Google +**</span><span class="sxs-lookup"><span data-stu-id="50d39-124">**Google+ API**</span></span>
   * <span data-ttu-id="50d39-125">**Servidor Web (por ejemplo, node.js, Tomcat)**, y</span><span class="sxs-lookup"><span data-stu-id="50d39-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
   * <span data-ttu-id="50d39-126">**Datos de usuario**:</span><span class="sxs-lookup"><span data-stu-id="50d39-126">**User data**:</span></span>

![Página credenciales de administrador de APIs: Obtenga más información sobre qué tipo de credenciales necesita el panel](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="50d39-128">Pulse **las credenciales que es necesario?** lo que irá al segundo paso de configuración de la aplicación, **crear un identificador de cliente de OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="50d39-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![Página credenciales de administrador de APIs: crear un identificador de cliente de OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="50d39-130">Dado que vamos a crear un proyecto de Google + con una sola característica (inicio de sesión), podemos escribir el mismo **nombre** para el identificador de cliente de OAuth 2.0 que la que se utilizará para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="50d39-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="50d39-131">Escriba el URI de desarrollo con */signin-google* anexan a la **URI de redireccionamiento autorizados** campo (por ejemplo: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="50d39-131">Enter your development URI with */signin-google* appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="50d39-132">La autenticación de Google configurada más adelante en este tutorial controlará automáticamente las solicitudes en */signin-google* ruta para implementar el flujo de OAuth.</span><span class="sxs-lookup"><span data-stu-id="50d39-132">The Google authentication configured later in this tutorial will automatically handle requests at */signin-google* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="50d39-133">Presione la tecla TAB para agregar el **URI de redireccionamiento autorizados** entrada.</span><span class="sxs-lookup"><span data-stu-id="50d39-133">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="50d39-134">Pulse **crear ID de cliente**, lo que irá con el tercer paso: **configurar la pantalla de consentimiento de OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="50d39-134">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![Página credenciales de administrador de APIs: configurar la pantalla de consentimiento de OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="50d39-136">Escriba el acceso público **dirección de correo electrónico** y **nombre de producto** se muestra para la aplicación cuando Google + pide al usuario que inicie sesión en.</span><span class="sxs-lookup"><span data-stu-id="50d39-136">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="50d39-137">Hay opciones adicionales disponibles en **más opciones de personalización**.</span><span class="sxs-lookup"><span data-stu-id="50d39-137">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="50d39-138">Pulse **continuar** para continuar con el último paso, **descargar credenciales**:</span><span class="sxs-lookup"><span data-stu-id="50d39-138">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![Página credenciales de administrador de APIs: descargar credenciales](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="50d39-140">Pulse **descargar** para guardar un archivo JSON con secretos de aplicación, y **realiza** para completar la creación de la nueva aplicación.</span><span class="sxs-lookup"><span data-stu-id="50d39-140">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="50d39-141">Al implementar el sitio que necesite volver a visitar el **consola de Google** y registrar una nueva dirección url pública.</span><span class="sxs-lookup"><span data-stu-id="50d39-141">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="50d39-142">Almacén Google ClientID y ClientSecret</span><span class="sxs-lookup"><span data-stu-id="50d39-142">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="50d39-143">Vincular valores confidenciales como Google `Client ID` y `Client Secret` a su configuración de aplicación con el [secreto Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="50d39-143">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="50d39-144">Para los fines de este tutorial, nombre de los tokens `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="50d39-144">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="50d39-145">Los valores para estos tokens se pueden encontrar en el archivo JSON que descargó en el paso anterior en `web.client_id` y `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="50d39-145">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="50d39-146">Configurar la autenticación de Google</span><span class="sxs-lookup"><span data-stu-id="50d39-146">Configure Google Authentication</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="50d39-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="50d39-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="50d39-148">Agregue el servicio de Google en el `ConfigureServices` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="50d39-148">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="50d39-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="50d39-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="50d39-150">La plantilla de proyecto que se usan en este tutorial asegura de que [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) paquete está instalado.</span><span class="sxs-lookup"><span data-stu-id="50d39-150">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

 * <span data-ttu-id="50d39-151">Para instalar este paquete con 2017 de Visual Studio, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="50d39-151">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
 * <span data-ttu-id="50d39-152">Para instalar con CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="50d39-152">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="50d39-153">Agregar el middleware de Google en el `Configure` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="50d39-153">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

* * *
<span data-ttu-id="50d39-154">Consulte la [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con autenticación de Google.</span><span class="sxs-lookup"><span data-stu-id="50d39-154">See the [GoogleOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="50d39-155">Esto se puede usar para solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="50d39-155">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="50d39-156">Inicie sesión con Google</span><span class="sxs-lookup"><span data-stu-id="50d39-156">Sign in with Google</span></span>

<span data-ttu-id="50d39-157">Ejecute la aplicación y haga clic en **sesión**.</span><span class="sxs-lookup"><span data-stu-id="50d39-157">Run your application and click **Log in**.</span></span> <span data-ttu-id="50d39-158">Aparece una opción para iniciar sesión con Google:</span><span class="sxs-lookup"><span data-stu-id="50d39-158">An option to sign in with Google appears:</span></span>

![Aplicación Web que se ejecuta en Microsoft Edge: usuario no autenticado](index/_static/DoneGoogle.png)

<span data-ttu-id="50d39-160">Al hacer clic en Google, se le redirigirá a Google para la autenticación:</span><span class="sxs-lookup"><span data-stu-id="50d39-160">When you click on Google, you are redirected to Google for authentication:</span></span>

![Cuadro de diálogo de autenticación de Google](index/_static/GoogleLogin.png)

<span data-ttu-id="50d39-162">Después de escribir sus credenciales de Google, a continuación, se le redirigirá al sitio web donde puede establecer el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="50d39-162">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="50d39-163">Ahora que haya iniciado sesión con sus credenciales de Google:</span><span class="sxs-lookup"><span data-stu-id="50d39-163">You are now logged in using your Google credentials:</span></span>

![Aplicación Web que se ejecuta en Microsoft Edge: usuario autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="50d39-165">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="50d39-165">Troubleshooting</span></span>

* <span data-ttu-id="50d39-166">Si recibe un `403 (Forbidden)` página de error de la propia aplicación cuando se ejecuta en modo de desarrollo (o interrupción en el depurador con el mismo error), asegúrese de que **API de Google +** se ha habilitado en el **biblioteca del Administrador de la API** siguiendo los pasos enumerados [anteriores en esta página](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="50d39-166">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="50d39-167">Si el inicio de sesión no funciona y no reciben los errores, cambie al modo de desarrollo para que sea más fácil depurar el problema.</span><span class="sxs-lookup"><span data-stu-id="50d39-167">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="50d39-168">**ASP.NET Core solo 2.x:** identidad si no está configurado mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intenta autenticar se producirá en *ArgumentException: se debe proporcionar la opción 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="50d39-168">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="50d39-169">La plantilla de proyecto que se usan en este tutorial se asegura de que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="50d39-169">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="50d39-170">Si la base de datos de sitio no se ha creado mediante la aplicación de la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error.</span><span class="sxs-lookup"><span data-stu-id="50d39-170">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="50d39-171">Pulse **migraciones aplicar** para crear la base de datos y actualizar para continuar después del error.</span><span class="sxs-lookup"><span data-stu-id="50d39-171">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="50d39-172">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="50d39-172">Next steps</span></span>

* <span data-ttu-id="50d39-173">En este artículo se ha explicado cómo puede autenticar con Google.</span><span class="sxs-lookup"><span data-stu-id="50d39-173">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="50d39-174">Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="50d39-174">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="50d39-175">Una vez que se publica un sitio web a la aplicación web de Azure, debe restablecer la `ClientSecret` en la consola de API de Google.</span><span class="sxs-lookup"><span data-stu-id="50d39-175">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="50d39-176">Establecer el `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret` como configuración de la aplicación en el portal de Azure.</span><span class="sxs-lookup"><span data-stu-id="50d39-176">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="50d39-177">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="50d39-177">The configuration system is set up to read keys from environment variables.</span></span>
