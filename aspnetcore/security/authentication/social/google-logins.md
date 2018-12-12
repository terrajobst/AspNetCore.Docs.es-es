---
title: Configuración de inicio de sesión externo de Google en ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Google en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: 372504eb4f6fea412b5b160e0d5e9251dafe0d56
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284499"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="2e8d0-103">Configuración de inicio de sesión externo de Google en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e8d0-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="2e8d0-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2e8d0-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2e8d0-105">Este tutorial muestra cómo permitir que los usuarios iniciar sesión con su cuenta de Google + mediante un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="2e8d0-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="2e8d0-106">Comenzamos siguiendo el [pasos oficiales](https://developers.google.com/identity/sign-in/web/devconsole-project) para crear una nueva aplicación en la consola de API de Google.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="2e8d0-107">Creación de la aplicación en la consola de API de Google</span><span class="sxs-lookup"><span data-stu-id="2e8d0-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="2e8d0-108">Vaya a [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="2e8d0-109">Si no dispone de una cuenta de Google, use **más opciones** > **[crear cuenta](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  vínculo para crear uno:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Consola de API de Google](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="2e8d0-111">Se le redirigirá a **biblioteca API Manager** página:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-111">You are redirected to **API Manager Library** page:</span></span>

![En la página de la biblioteca del Administrador de la API de inicio](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="2e8d0-113">Pulse **crear** y escriba su **nombre del proyecto**:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-113">Tap **Create** and enter your **Project name**:</span></span>

![Cuadro de diálogo Nuevo proyecto](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="2e8d0-115">Después de aceptar el cuadro de diálogo, se le redirigirá a la página de la biblioteca que le permite elegir las características de la nueva aplicación.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="2e8d0-116">Buscar **Google + API** en la lista y haga clic en el vínculo al agregar la característica de API:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Busque "API de Google +" en la página de la biblioteca del Administrador de API](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="2e8d0-118">Se muestra la página de la API recién agregada.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="2e8d0-119">Pulse **habilitar** para agregar el inicio de sesión de Google + en función a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![En la página Administrador de API de Google + API de inicio](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="2e8d0-121">Después de habilitar la API, pulse **crear credenciales** para configurar los secretos:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![Crear el botón de credenciales en la página Administrador de API de Google + API](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="2e8d0-123">Elija:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-123">Choose:</span></span>
  * <span data-ttu-id="2e8d0-124">**Google + API**</span><span class="sxs-lookup"><span data-stu-id="2e8d0-124">**Google+ API**</span></span>
  * <span data-ttu-id="2e8d0-125">**Servidor Web (por ejemplo, node.js, Tomcat)**, y</span><span class="sxs-lookup"><span data-stu-id="2e8d0-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
  * <span data-ttu-id="2e8d0-126">**Datos de usuario**:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-126">**User data**:</span></span>

![Página de credenciales de administrador de API: Descubra de qué tipo de credenciales necesita el panel](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="2e8d0-128">Pulse **las credenciales que es necesario?** que le lleva al segundo paso de configuración de la aplicación, **crear un identificador de cliente de OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![Página de credenciales de administrador de API: Crear un identificador de cliente de OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="2e8d0-130">Dado que vamos a crear un proyecto de Google + con una sola característica (inicio de sesión), podemos escribir el mismo **nombre** para el identificador de cliente de OAuth 2.0 que se usan para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="2e8d0-131">Escriba el URI de desarrollo con `/signin-google` anexado a la **URI de redireccionamiento autorizado** campo (por ejemplo: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="2e8d0-131">Enter your development URI with `/signin-google` appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="2e8d0-132">La autenticación de Google configurada más adelante en este tutorial controlará automáticamente las solicitudes en `/signin-google` ruta para implementar el flujo de OAuth.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-132">The Google authentication configured later in this tutorial will automatically handle requests at `/signin-google` route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="2e8d0-133">El segmento del URI `/signin-google` se establece como la devolución de llamada predeterminada del proveedor de autenticación de Google.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-133">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="2e8d0-134">Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Google a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) clase.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-134">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

* <span data-ttu-id="2e8d0-135">Presione la tecla TAB para agregar la **URI de redireccionamiento autorizado** entrada.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-135">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="2e8d0-136">Pulse **crear Id. de cliente**, que le lleva al tercer paso, **configurar la pantalla de consentimiento de OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-136">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![Página de credenciales de administrador de API: Configurar la pantalla de consentimiento de OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="2e8d0-138">Escriba su orientados al público **dirección de correo electrónico** y **nombre de producto** que se muestra para la aplicación cuando Google + pide al usuario que inicie sesión en.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-138">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="2e8d0-139">Opciones adicionales están disponibles en **más opciones de personalización**.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-139">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="2e8d0-140">Pulse **continuar** para continuar con el último paso, **descargar credenciales**:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-140">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![Página de credenciales de administrador de API: Descargar credenciales](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="2e8d0-142">Pulse **descargar** para guardar un archivo JSON con los secretos de aplicación, y **realiza** para completar la creación de la nueva aplicación.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-142">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="2e8d0-143">Al implementar el sitio, deberá volver a visitar la **Google Console** y registrar una nueva dirección url pública.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-143">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="2e8d0-144">Store Google ClientID y ClientSecret</span><span class="sxs-lookup"><span data-stu-id="2e8d0-144">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="2e8d0-145">Vincular configuración confidencial, como Google `Client ID` y `Client Secret` para la configuración de aplicación mediante el [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="2e8d0-145">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="2e8d0-146">Para los fines de este tutorial, asigne el nombre de los tokens `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-146">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="2e8d0-147">Los valores para estos tokens pueden encontrarse en el archivo JSON descargado en el paso anterior en `web.client_id` y `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-147">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="2e8d0-148">Configurar la autenticación de Google</span><span class="sxs-lookup"><span data-stu-id="2e8d0-148">Configure Google Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2e8d0-149">Agregue el servicio de Google en el `ConfigureServices` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-149">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="2e8d0-150">La plantilla de proyecto que se usa en este tutorial garantiza que [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) está instalado el paquete.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-150">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

* <span data-ttu-id="2e8d0-151">Para instalar este paquete con Visual Studio 2017, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-151">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="2e8d0-152">Para instalar con la CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-152">To install with .NET Core CLI, execute the following in your project directory:</span></span>

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="2e8d0-153">Agregue el middleware de Google en el `Configure` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-153">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

<span data-ttu-id="2e8d0-154">Consulte la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Google.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-154">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="2e8d0-155">Esto puede utilizarse para solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-155">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="2e8d0-156">Inicie sesión con Google</span><span class="sxs-lookup"><span data-stu-id="2e8d0-156">Sign in with Google</span></span>

<span data-ttu-id="2e8d0-157">Ejecute la aplicación y haga clic en **inicie sesión**.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-157">Run your application and click **Log in**.</span></span> <span data-ttu-id="2e8d0-158">Aparece una opción para iniciar sesión con Google:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-158">An option to sign in with Google appears:</span></span>

![Aplicación Web que se ejecuta en Microsoft Edge: Usuario no autenticado](index/_static/DoneGoogle.png)

<span data-ttu-id="2e8d0-160">Al hacer clic en Google, se le redirigirá a Google para la autenticación:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-160">When you click on Google, you are redirected to Google for authentication:</span></span>

![Cuadro de diálogo de autenticación de Google](index/_static/GoogleLogin.png)

<span data-ttu-id="2e8d0-162">Después de escribir sus credenciales de Google, a continuación, se le redirigirá al sitio web donde puede establecer su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-162">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="2e8d0-163">Ha iniciado sesión con sus credenciales de Google:</span><span class="sxs-lookup"><span data-stu-id="2e8d0-163">You are now logged in using your Google credentials:</span></span>

![Aplicación Web que se ejecuta en Microsoft Edge: Usuario autenticado](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="2e8d0-165">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="2e8d0-165">Troubleshooting</span></span>

* <span data-ttu-id="2e8d0-166">Si recibe un `403 (Forbidden)` página de error desde su propia aplicación al que se ejecute en modo de desarrollo (o interrumpir el depurador con el mismo error), asegúrese de que **Google + API** se ha habilitado en el **biblioteca del Administrador de la API** siguiendo los pasos enumerados [anteriores en esta página](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="2e8d0-166">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="2e8d0-167">Si no funciona el inicio de sesión y no recibe algún error, cambie al modo de desarrollo para hacer más fácil de depurar el problema.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-167">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="2e8d0-168">**ASP.NET Core 2.x solo:** Si la identidad no está configurada mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intentando autenticarse producirá *ArgumentException: Se debe proporcionar la opción 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-168">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="2e8d0-169">La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-169">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="2e8d0-170">Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-170">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="2e8d0-171">Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-171">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e8d0-172">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="2e8d0-172">Next steps</span></span>

* <span data-ttu-id="2e8d0-173">Este artículo, mostramos cómo puede autenticar con Google.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-173">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="2e8d0-174">Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="2e8d0-174">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="2e8d0-175">Una vez que publique su sitio web a la aplicación web de Azure, debe restablecer el `ClientSecret` en la consola de API de Google.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-175">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="2e8d0-176">Establecer el `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret` como configuración de la aplicación en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-176">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="2e8d0-177">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="2e8d0-177">The configuration system is set up to read keys from environment variables.</span></span>
