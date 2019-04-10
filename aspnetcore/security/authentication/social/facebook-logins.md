---
title: Configuración de inicio de sesión externo de Facebook en ASP.NET Core
author: rick-anderson
description: Tutorial con ejemplos de código que muestra la integración de autenticación de usuario de la cuenta de Facebook en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 03/04/2019
uid: security/authentication/facebook-logins
ms.openlocfilehash: 84b891f6cee71a26726d49a9d42ae45007d2b429
ms.sourcegitcommit: 258a97159da206f9009f23fdf6f8fa32f178e50b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59425041"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="cf87c-103">Configuración de inicio de sesión externo de Facebook en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cf87c-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="cf87c-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cf87c-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cf87c-105">Este tutorial con ejemplos de código muestra cómo permitir que los usuarios iniciar sesión con su cuenta de Facebook con un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="cf87c-105">This tutorial with code examples shows how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="cf87c-106">Empezamos creando un App identificador Facebook siguiendo el [pasos oficiales](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="cf87c-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="cf87c-107">Crear la aplicación en Facebook</span><span class="sxs-lookup"><span data-stu-id="cf87c-107">Create the app in Facebook</span></span>

* <span data-ttu-id="cf87c-108">Navegue hasta la [app de desarrolladores de Facebook](https://developers.facebook.com/apps/) página e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="cf87c-108">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="cf87c-109">Si no dispone de una cuenta de Facebook, use el **suscribirse a Facebook** vínculo en la página de inicio de sesión para crear uno.</span><span class="sxs-lookup"><span data-stu-id="cf87c-109">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="cf87c-110">Pulse el **agregar una nueva aplicación** botón en la esquina superior derecha para crear un nuevo identificador de aplicación.</span><span class="sxs-lookup"><span data-stu-id="cf87c-110">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook para el portal de desarrolladores abierta en Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="cf87c-112">Rellene el formulario y pulse el **crear Id. de aplicación** botón.</span><span class="sxs-lookup"><span data-stu-id="cf87c-112">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Crear un formulario nuevo identificador de aplicación](index/_static/FBNewAppId.png)

* <span data-ttu-id="cf87c-114">En el **seleccionar un producto** página, haga clic en **Set Up** en el **inicio de sesión de Facebook** tarjeta.</span><span class="sxs-lookup"><span data-stu-id="cf87c-114">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

  ![Página de instalación del producto](index/_static/FBProductSetup.png)

* <span data-ttu-id="cf87c-116">El **Quickstart** se iniciará con **elija una plataforma** como la primera página.</span><span class="sxs-lookup"><span data-stu-id="cf87c-116">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="cf87c-117">Omitir el asistente por ahora, haga clic en el **configuración** vínculo en el menú de la izquierda:</span><span class="sxs-lookup"><span data-stu-id="cf87c-117">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

  ![Inicio rápido de omisión](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="cf87c-119">Se le presentará la **configuración de cliente OAuth** página:</span><span class="sxs-lookup"><span data-stu-id="cf87c-119">You are presented with the **Client OAuth Settings** page:</span></span>

  ![Página de configuración de OAuth de cliente](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="cf87c-121">Escriba el URI de desarrollo con */signin-facebook* anexado a la **URI de redirección de OAuth válido** campo (por ejemplo: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="cf87c-121">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="cf87c-122">La autenticación de Facebook configurada más adelante en este tutorial controlará automáticamente las solicitudes en */signin-facebook* ruta para implementar el flujo de OAuth.</span><span class="sxs-lookup"><span data-stu-id="cf87c-122">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="cf87c-123">El URI */signin-facebook* se establece como la devolución de llamada predeterminada del proveedor de autenticación de Facebook.</span><span class="sxs-lookup"><span data-stu-id="cf87c-123">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="cf87c-124">Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Facebook a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) clase.</span><span class="sxs-lookup"><span data-stu-id="cf87c-124">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="cf87c-125">Haga clic en **guardar cambios**.</span><span class="sxs-lookup"><span data-stu-id="cf87c-125">Click **Save Changes**.</span></span>

* <span data-ttu-id="cf87c-126">Haga clic en **configuración** > **básica** vínculo en el panel de navegación izquierdo.</span><span class="sxs-lookup"><span data-stu-id="cf87c-126">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="cf87c-127">En esta página, tome nota de su `App ID` y su `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="cf87c-127">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="cf87c-128">En la siguiente sección, va a agregar tanto en la aplicación de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="cf87c-128">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="cf87c-129">Al implementar el sitio tiene que volver a visitar la **inicio de sesión de Facebook** página de la instalación y registrar un nuevo URI público.</span><span class="sxs-lookup"><span data-stu-id="cf87c-129">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="cf87c-130">Store Id. de aplicación de Facebook y secreto de la aplicación</span><span class="sxs-lookup"><span data-stu-id="cf87c-130">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="cf87c-131">Vincular configuración confidencial, como Facebook `App ID` y `App Secret` para la configuración de aplicación mediante el [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="cf87c-131">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="cf87c-132">Para los fines de este tutorial, asigne el nombre de los tokens `Authentication:Facebook:AppId` y `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="cf87c-132">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="cf87c-133">Ejecute los comandos siguientes para almacenar de forma segura `App ID` y `App Secret` con Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="cf87c-133">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="cf87c-134">Configurar la autenticación de Facebook</span><span class="sxs-lookup"><span data-stu-id="cf87c-134">Configure Facebook Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="cf87c-135">Agregue el servicio de Facebook en la `ConfigureServices` método en el *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="cf87c-135">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="cf87c-136">Instalar el [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) paquete.</span><span class="sxs-lookup"><span data-stu-id="cf87c-136">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="cf87c-137">Para instalar este paquete con Visual Studio 2017, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="cf87c-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="cf87c-138">Para instalar con la CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="cf87c-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="cf87c-139">Agregue el middleware de Facebook en la `Configure` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="cf87c-139">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

<span data-ttu-id="cf87c-140">Consulte la [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Facebook.</span><span class="sxs-lookup"><span data-stu-id="cf87c-140">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="cf87c-141">Las opciones de configuración pueden utilizarse para:</span><span class="sxs-lookup"><span data-stu-id="cf87c-141">Configuration options can be used to:</span></span>

* <span data-ttu-id="cf87c-142">Solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="cf87c-142">Request different information about the user.</span></span>
* <span data-ttu-id="cf87c-143">Agregue los argumentos de cadena de consulta para personalizar la experiencia de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="cf87c-143">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="cf87c-144">Inicie sesión con Facebook</span><span class="sxs-lookup"><span data-stu-id="cf87c-144">Sign in with Facebook</span></span>

<span data-ttu-id="cf87c-145">Ejecute la aplicación y haga clic en **inicie sesión**.</span><span class="sxs-lookup"><span data-stu-id="cf87c-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="cf87c-146">Verá una opción para iniciar sesión con Facebook.</span><span class="sxs-lookup"><span data-stu-id="cf87c-146">You see an option to sign in with Facebook.</span></span>

![Aplicación Web: Usuario no autenticado](index/_static/DoneFacebook.png)

<span data-ttu-id="cf87c-148">Al hacer clic en **Facebook**, se le redirigirá a Facebook para la autenticación:</span><span class="sxs-lookup"><span data-stu-id="cf87c-148">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Página de autenticación de Facebook](index/_static/FBLogin.png)

<span data-ttu-id="cf87c-150">Dirección pública de perfil y correo electrónico de solicitudes de autenticación de Facebook de forma predeterminada:</span><span class="sxs-lookup"><span data-stu-id="cf87c-150">Facebook authentication requests public profile and email address by default:</span></span>

![Pantalla de consentimiento de página de autenticación de Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="cf87c-152">Una vez que escriba sus credenciales de Facebook se redirigen a su sitio donde puede establecer su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="cf87c-152">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="cf87c-153">Ha iniciado sesión con sus credenciales de Facebook:</span><span class="sxs-lookup"><span data-stu-id="cf87c-153">You are now logged in using your Facebook credentials:</span></span>

![Aplicación Web: Usuario autenticado](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="cf87c-155">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="cf87c-155">Troubleshooting</span></span>

* <span data-ttu-id="cf87c-156">**ASP.NET Core 2.x solo:** Si la identidad no está configurada mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intentando autenticarse producirá *ArgumentException: Se debe proporcionar la opción 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="cf87c-156">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="cf87c-157">La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="cf87c-157">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="cf87c-158">Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error.</span><span class="sxs-lookup"><span data-stu-id="cf87c-158">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="cf87c-159">Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.</span><span class="sxs-lookup"><span data-stu-id="cf87c-159">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf87c-160">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="cf87c-160">Next steps</span></span>

* <span data-ttu-id="cf87c-161">Agregar el [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) paquete NuGet al proyecto para escenarios avanzados de autenticación de Facebook.</span><span class="sxs-lookup"><span data-stu-id="cf87c-161">Add the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package to your project for advanced Facebook authentication scenarios.</span></span> <span data-ttu-id="cf87c-162">Este paquete no es necesario integrar la funcionalidad de inicio de sesión externo de Facebook con su aplicación.</span><span class="sxs-lookup"><span data-stu-id="cf87c-162">This package isn't required to integrate Facebook external login functionality with your app.</span></span> 

* <span data-ttu-id="cf87c-163">Este artículo, mostramos cómo puede autenticar con Facebook.</span><span class="sxs-lookup"><span data-stu-id="cf87c-163">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="cf87c-164">Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="cf87c-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="cf87c-165">Una vez que publique su sitio web a la aplicación web de Azure, debe restablecer el `AppSecret` en el portal para desarrolladores de Facebook.</span><span class="sxs-lookup"><span data-stu-id="cf87c-165">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="cf87c-166">Establecer el `Authentication:Facebook:AppId` y `Authentication:Facebook:AppSecret` como configuración de la aplicación en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="cf87c-166">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="cf87c-167">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="cf87c-167">The configuration system is set up to read keys from environment variables.</span></span>
