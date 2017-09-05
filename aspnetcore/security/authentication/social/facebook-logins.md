---
title: "Programa de instalación de inicio de sesión externo de Facebook en ASP.NET Core"
author: rick-anderson
description: "Programa de instalación de inicio de sesión externo de Facebook en ASP.NET Core"
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 8/1/2017
ms.topic: article
ms.assetid: 8c65179b-688c-4af1-8f5e-1862920cda95
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/facebook-logins
ms.openlocfilehash: 9554d66712f93df6d2c50503b60162757986e707
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2017
---
# <a name="configuring-facebook-authentication"></a><span data-ttu-id="7cc2b-104">Configurar la autenticación de Facebook</span><span class="sxs-lookup"><span data-stu-id="7cc2b-104">Configuring Facebook authentication</span></span>

<a name=security-authentication-facebook-logins></a>

<span data-ttu-id="7cc2b-105">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7cc2b-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7cc2b-106">Este tutorial muestra cómo permitir a los usuarios iniciar sesión con su cuenta de Facebook mediante un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](index.md).</span><span class="sxs-lookup"><span data-stu-id="7cc2b-106">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="7cc2b-107">Comenzamos creando un Facebook App ID siguiendo el [pasos oficiales](https://developers.facebook.com/docs/apps/register).</span><span class="sxs-lookup"><span data-stu-id="7cc2b-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com/docs/apps/register).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="7cc2b-108">Crear la aplicación de Facebook</span><span class="sxs-lookup"><span data-stu-id="7cc2b-108">Create the app in Facebook</span></span>

*  <span data-ttu-id="7cc2b-109">Navegue hasta la [Facebook para los desarrolladores](https://developers.facebook.com/apps) página e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-109">Navigate to the [Facebook for Developers](https://developers.facebook.com/apps) page and sign in.</span></span> <span data-ttu-id="7cc2b-110">Si ya no tiene una cuenta de Facebook, use la **registrarse para Facebook** vínculo en la página de inicio de sesión para crear uno.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="7cc2b-111">Pulse la **crear aplicación** botón en la esquina superior derecha para crear un nuevo identificador de aplicación.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-111">Tap the **Create App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook para portal de desarrolladores de abrir con Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="7cc2b-113">Rellene el formulario y pulse el **crear Id. de aplicación** botón.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-113">Fill out the form and tap the **Create App ID** button.</span></span>

   ![Crear un formulario nuevo Id. de aplicación](index/_static/FBNewAppId.png)

* <span data-ttu-id="7cc2b-115">Cuando aparezca **seleccionar un producto** símbolo del sistema, haga clic en **Set Up** en el **inicio de sesión de Facebook** tarjeta.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-115">When presented with **Select a product** prompt, Click **Set Up** on the **Facebook Login** card.</span></span>

   ![Página de instalación del producto](index/_static/FBProductSetup.png)

* <span data-ttu-id="7cc2b-117">El **inicio rápido** asistente se iniciará con **elegir una plataforma** como la primera página.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="7cc2b-118">Omitir el asistente por ahora, haga clic en el **configuración** vínculo en el menú de la izquierda:</span><span class="sxs-lookup"><span data-stu-id="7cc2b-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Inicio rápido de Skip](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="7cc2b-120">Se le presentará la **configuración de cliente OAuth** página:</span><span class="sxs-lookup"><span data-stu-id="7cc2b-120">You are presented with the **Client OAuth Settings** page:</span></span>

![Página de configuración de OAuth de cliente](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="7cc2b-122">Escriba el URI de desarrollo con */signin-facebook* anexan a la **válido URI de redireccionamiento de OAuth** campo (por ejemplo: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="7cc2b-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="7cc2b-123">La autenticación de Facebook configurada más adelante en este tutorial controlará automáticamente las solicitudes en */signin-facebook* ruta para implementar el flujo de OAuth.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="7cc2b-124">Haga clic en **guardar cambios**.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-124">Click **Save Changes**.</span></span>

* <span data-ttu-id="7cc2b-125">Haga clic en el **panel** vínculo en el panel de navegación izquierdo.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-125">Click the **Dashboard** link in the left navigation.</span></span> 

    <span data-ttu-id="7cc2b-126">En esta página, tome nota de su `App ID` y su `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-126">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="7cc2b-127">Tanto en la aplicación de ASP.NET Core va a agregar en la sección siguiente:</span><span class="sxs-lookup"><span data-stu-id="7cc2b-127">You will add both into your ASP.NET Core application in the next section:</span></span>

   ![Panel para desarrolladores de Facebook](index/_static/FBDashboard.png)

* <span data-ttu-id="7cc2b-129">Al implementar el sitio debe volver a visitar el **inicio de sesión de Facebook** página de la instalación y registrar un nuevo URI público.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-129">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="7cc2b-130">Almacenar identificador de la aplicación de Facebook y secreto de la aplicación</span><span class="sxs-lookup"><span data-stu-id="7cc2b-130">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="7cc2b-131">Vincular valores confidenciales como Facebook `App ID` y `App Secret` a su configuración de aplicación con el [secreto Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7cc2b-131">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="7cc2b-132">Para los fines de este tutorial, nombre de los tokens `Authentication:Facebook:AppId` y `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-132">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

## <a name="configure-facebook-authentication"></a><span data-ttu-id="7cc2b-133">Configurar la autenticación de Facebook</span><span class="sxs-lookup"><span data-stu-id="7cc2b-133">Configure Facebook Authentication</span></span>

<span data-ttu-id="7cc2b-134">La plantilla de proyecto que se usan en este tutorial asegura de que [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) paquete ya está instalado.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-134">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package is already installed.</span></span>

* <span data-ttu-id="7cc2b-135">Para instalar este paquete con 2017 de Visual Studio, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-135">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="7cc2b-136">Para instalar con CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="7cc2b-136">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7cc2b-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7cc2b-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7cc2b-138">Agregue el servicio de Facebook en la `ConfigureServices` método en el *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="7cc2b-138">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

<span data-ttu-id="7cc2b-139">El `AddAuthentication` método solo debe llamarse una vez cuando se agrega varios proveedores de autenticación.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-139">The `AddAuthentication` method should only be called once when adding multiple authentication providers.</span></span> <span data-ttu-id="7cc2b-140">Las llamadas posteriores a la existe la posibilidad de reemplazar cualquiera configurado previamente [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) propiedades.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-140">Subsequent calls to it have the potential of overriding any previously configured [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) properties.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7cc2b-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7cc2b-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7cc2b-142">Agregar el middleware de Facebook en la `Configure` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="7cc2b-142">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="7cc2b-143">Consulte la [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Facebook.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-143">See the [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="7cc2b-144">Opciones de configuración pueden utilizarse para:</span><span class="sxs-lookup"><span data-stu-id="7cc2b-144">Configuration options can be used to:</span></span>

* <span data-ttu-id="7cc2b-145">Solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-145">Request different information about the user.</span></span>
* <span data-ttu-id="7cc2b-146">Agregue los argumentos de cadena de consulta para personalizar la experiencia de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-146">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="7cc2b-147">Inicie sesión con Facebook</span><span class="sxs-lookup"><span data-stu-id="7cc2b-147">Sign in with Facebook</span></span>

<span data-ttu-id="7cc2b-148">Ejecute la aplicación y haga clic en **sesión**.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-148">Run your application and click **Log in**.</span></span> <span data-ttu-id="7cc2b-149">Verá una opción para iniciar sesión con Facebook.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-149">You see an option to sign in with Facebook.</span></span>

![Aplicación Web: usuario no autenticado](index/_static/DoneFacebook.png)

<span data-ttu-id="7cc2b-151">Al hacer clic en **Facebook**, se le redirigirá a Facebook para la autenticación:</span><span class="sxs-lookup"><span data-stu-id="7cc2b-151">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Página de autenticación de Facebook](index/_static/FBLogin.png)

<span data-ttu-id="7cc2b-153">Las solicitudes de autenticación de Facebook dirección pública de perfil y el correo electrónico de forma predeterminada:</span><span class="sxs-lookup"><span data-stu-id="7cc2b-153">Facebook authentication requests public profile and email address by default:</span></span>

![Página de autenticación de Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="7cc2b-155">Una vez que escriba sus credenciales de Facebook que se le redirigirá al sitio donde puede establecer el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-155">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="7cc2b-156">Ahora que haya iniciado sesión con sus credenciales de Facebook:</span><span class="sxs-lookup"><span data-stu-id="7cc2b-156">You are now logged in using your Facebook credentials:</span></span>

![Aplicación Web: usuario autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="7cc2b-158">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="7cc2b-158">Troubleshooting</span></span>

* <span data-ttu-id="7cc2b-159">**ASP.NET Core solo 2.x:** identidad si no se ha configurado mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intenta autenticar se producirá en *ArgumentException: se debe proporcionar la opción 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-159">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="7cc2b-160">La plantilla de proyecto que se usan en este tutorial se asegura de que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-160">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="7cc2b-161">Si la base de datos de sitio no se ha creado mediante la aplicación de la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-161">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="7cc2b-162">Pulse **migraciones aplicar** para crear la base de datos y actualizar para continuar después del error.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-162">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7cc2b-163">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7cc2b-163">Next steps</span></span>

* <span data-ttu-id="7cc2b-164">En este artículo se ha explicado cómo puede autenticar con Facebook.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-164">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="7cc2b-165">Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](index.md).</span><span class="sxs-lookup"><span data-stu-id="7cc2b-165">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="7cc2b-166">Una vez que se publica un sitio web a la aplicación web de Azure, debe restablecer la `AppSecret` en el portal para desarrolladores de Facebook.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-166">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="7cc2b-167">Establecer el `Authentication:Facebook:AppId` y `Authentication:Facebook:AppSecret` como configuración de la aplicación en el portal de Azure.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-167">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="7cc2b-168">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="7cc2b-168">The configuration system is set up to read keys from environment variables.</span></span>
