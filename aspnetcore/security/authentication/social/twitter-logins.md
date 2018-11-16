---
title: Configuración de inicio de sesión externo de Twitter con ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Twitter en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 43a5ea59d8853d297ae2c1ec3f4b1c0c14ec80c3
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708431"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="90b75-103">Configuración de inicio de sesión externo de Twitter con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="90b75-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="90b75-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="90b75-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="90b75-105">Este tutorial muestra cómo permitir a los usuarios [inicie sesión con su cuenta de Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) con un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="90b75-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="90b75-106">Crear la aplicación en Twitter</span><span class="sxs-lookup"><span data-stu-id="90b75-106">Create the app in Twitter</span></span>

* <span data-ttu-id="90b75-107">Vaya a [ https://apps.twitter.com/ ](https://apps.twitter.com/) e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="90b75-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="90b75-108">Si no dispone de una cuenta de Twitter, use el **[Suscríbase ahora](https://twitter.com/signup)** vínculo para crear uno.</span><span class="sxs-lookup"><span data-stu-id="90b75-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="90b75-109">Después de iniciar sesión, el **Application Management** se muestra la página:</span><span class="sxs-lookup"><span data-stu-id="90b75-109">After signing in, the **Application Management** page is shown:</span></span>

  ![Administración de aplicaciones de Twitter abierto en Microsoft Edge](index/_static/TwitterAppManage.png)

* <span data-ttu-id="90b75-111">Pulse **crear nueva aplicación** y rellene la aplicación **nombre**, **descripción** públicas y **sitio Web** (Esto puede ser temporal hasta que el URI Registre el nombre de dominio):</span><span class="sxs-lookup"><span data-stu-id="90b75-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

  ![Crear una página de aplicación](index/_static/TwitterCreate.png)

* <span data-ttu-id="90b75-113">Escriba el URI de desarrollo con `/signin-twitter` anexado a la **URI de redirección de OAuth válido** campo (por ejemplo: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="90b75-113">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="90b75-114">El esquema de autenticación de Twitter configurado más adelante en este tutorial controlará automáticamente las solicitudes en `/signin-twitter` ruta para implementar el flujo de OAuth.</span><span class="sxs-lookup"><span data-stu-id="90b75-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="90b75-115">El segmento del URI `/signin-twitter` se establece como la devolución de llamada predeterminada del proveedor de autenticación de Twitter.</span><span class="sxs-lookup"><span data-stu-id="90b75-115">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="90b75-116">Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Twitter a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) clase.</span><span class="sxs-lookup"><span data-stu-id="90b75-116">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="90b75-117">Rellene el resto del formulario y pulse **crear aplicación de Twitter**.</span><span class="sxs-lookup"><span data-stu-id="90b75-117">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="90b75-118">Se muestran los detalles de la nueva aplicación:</span><span class="sxs-lookup"><span data-stu-id="90b75-118">New application details are displayed:</span></span>

  ![Pestaña Detalles de la página de aplicación](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="90b75-120">Al implementar el sitio, deberá volver a visitar la **Application Management** página y registrar un nuevo URI público.</span><span class="sxs-lookup"><span data-stu-id="90b75-120">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="90b75-121">Almacenar Twitter ConsumerKey y ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="90b75-121">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="90b75-122">Vincular los valores confidenciales como Twitter `Consumer Key` y `Consumer Secret` para la configuración de aplicación mediante el [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="90b75-122">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="90b75-123">Para los fines de este tutorial, asigne el nombre de los tokens `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="90b75-123">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="90b75-124">Estos tokens pueden encontrarse en el **claves y Tokens de acceso** ficha después de crear la nueva aplicación de Twitter:</span><span class="sxs-lookup"><span data-stu-id="90b75-124">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Pestaña de claves y Tokens de acceso](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="90b75-126">Configurar la autenticación de Twitter</span><span class="sxs-lookup"><span data-stu-id="90b75-126">Configure Twitter Authentication</span></span>

<span data-ttu-id="90b75-127">La plantilla de proyecto que se usa en este tutorial garantiza que [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) paquete ya está instalado.</span><span class="sxs-lookup"><span data-stu-id="90b75-127">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="90b75-128">Para instalar este paquete con Visual Studio 2017, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="90b75-128">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="90b75-129">Para instalar con la CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="90b75-129">To install with .NET Core CLI, execute the following in your project directory:</span></span>

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="90b75-130">Agregue el servicio de Twitter en el `ConfigureServices` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="90b75-130">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="90b75-131">Agregue el middleware de Twitter en el `Configure` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="90b75-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

<span data-ttu-id="90b75-132">Consulte la [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Twitter.</span><span class="sxs-lookup"><span data-stu-id="90b75-132">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="90b75-133">Esto puede utilizarse para solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="90b75-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="90b75-134">Inicie sesión con Twitter</span><span class="sxs-lookup"><span data-stu-id="90b75-134">Sign in with Twitter</span></span>

<span data-ttu-id="90b75-135">Ejecute la aplicación y haga clic en **inicie sesión**.</span><span class="sxs-lookup"><span data-stu-id="90b75-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="90b75-136">Aparece una opción para iniciar sesión con Twitter:</span><span class="sxs-lookup"><span data-stu-id="90b75-136">An option to sign in with Twitter appears:</span></span>

![Aplicación Web: usuario no autenticado](index/_static/DoneTwitter.png)

<span data-ttu-id="90b75-138">Al hacer clic en **Twitter** redirige a Twitter para la autenticación:</span><span class="sxs-lookup"><span data-stu-id="90b75-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Página de autenticación de Twitter](index/_static/TwitterLogin.png)

<span data-ttu-id="90b75-140">Después de escribir sus credenciales de Twitter, se le redirigirá al sitio web donde puede establecer su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="90b75-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="90b75-141">Ha iniciado sesión con sus credenciales de Twitter:</span><span class="sxs-lookup"><span data-stu-id="90b75-141">You are now logged in using your Twitter credentials:</span></span>

![Aplicación Web: usuario autenticado](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="90b75-143">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="90b75-143">Troubleshooting</span></span>

* <span data-ttu-id="90b75-144">**ASP.NET Core 2.x solo:** identidad si no está configurado mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intentando autenticarse producirá *ArgumentException: se debe proporcionar la opción 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="90b75-144">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="90b75-145">La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="90b75-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="90b75-146">Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error.</span><span class="sxs-lookup"><span data-stu-id="90b75-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="90b75-147">Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.</span><span class="sxs-lookup"><span data-stu-id="90b75-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="90b75-148">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="90b75-148">Next steps</span></span>

* <span data-ttu-id="90b75-149">Este artículo, mostramos cómo puede autenticar con Twitter.</span><span class="sxs-lookup"><span data-stu-id="90b75-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="90b75-150">Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="90b75-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="90b75-151">Una vez que publique su sitio web a la aplicación web de Azure, debe restablecer el `ConsumerSecret` en el portal para desarrolladores de Twitter.</span><span class="sxs-lookup"><span data-stu-id="90b75-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="90b75-152">Establecer el `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret` como configuración de la aplicación en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="90b75-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="90b75-153">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="90b75-153">The configuration system is set up to read keys from environment variables.</span></span>
