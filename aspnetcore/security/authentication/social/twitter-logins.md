---
title: "Programa de instalación de inicio de sesión externo de Twitter"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 11/1/2016
ms.topic: article
ms.assetid: E5931607-31C0-4B20-B416-85E3550F5EA8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/twitter-logins
ms.openlocfilehash: 800f98285859a54198b76411aea000384de05cd3
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2017
---
# <a name="configuring-twitter-authentication"></a><span data-ttu-id="4cfe4-103">Configurar la autenticación de Twitter</span><span class="sxs-lookup"><span data-stu-id="4cfe4-103">Configuring Twitter authentication</span></span>

<a name=security-authentication-twitter-logins></a>

<span data-ttu-id="4cfe4-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4cfe4-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4cfe4-105">Este tutorial muestra cómo permitir a los usuarios a [iniciar sesión con su cuenta de Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) usando un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](index.md).</span><span class="sxs-lookup"><span data-stu-id="4cfe4-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="4cfe4-106">Crear la aplicación en Twitter</span><span class="sxs-lookup"><span data-stu-id="4cfe4-106">Create the app in Twitter</span></span>

* <span data-ttu-id="4cfe4-107">Vaya a [https://apps.twitter.com/](https://apps.twitter.com/) e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="4cfe4-108">Si ya no tiene una cuenta de Twitter, use la  **[Regístrese ahora](https://twitter.com/signup)**  vínculo para crear uno.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="4cfe4-109">Después de iniciar sesión, el **Application Management** se muestra la página:</span><span class="sxs-lookup"><span data-stu-id="4cfe4-109">After signing in, the **Application Management** page is shown:</span></span>

![Administración de aplicaciones de Twitter abierta en Microsoft Edge](index/_static/TwitterAppManage.png)

* <span data-ttu-id="4cfe4-111">Pulse **crear una aplicación nueva** y rellene la aplicación **nombre**, **descripción** públicas y **sitio Web** (Esto puede ser temporal hasta que el URI Registre el nombre de dominio):</span><span class="sxs-lookup"><span data-stu-id="4cfe4-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![Crear una página de aplicación](index/_static/TwitterCreate.png)

* <span data-ttu-id="4cfe4-113">Escriba el URI de desarrollo con */signin-twitter* anexan a la **válido URI de redireccionamiento de OAuth** campo (por ejemplo: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="4cfe4-113">Enter your development URI with */signin-twitter* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="4cfe4-114">El esquema de autenticación de Twitter configurado más adelante en este tutorial controlará automáticamente las solicitudes en */signin-twitter* ruta para implementar el flujo de OAuth.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at */signin-twitter* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="4cfe4-115">Rellene el resto del formulario y pulse **crear su aplicación de Twitter**.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-115">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="4cfe4-116">Se muestran los detalles de la nueva aplicación:</span><span class="sxs-lookup"><span data-stu-id="4cfe4-116">New application details are displayed:</span></span>

![Pestaña Detalles de la página de aplicación](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="4cfe4-118">Al implementar el sitio que necesite volver a visitar el **Application Management** página y registrar un nuevo URI público.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-118">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="4cfe4-119">Almacenar Twitter ConsumerKey y ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="4cfe4-119">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="4cfe4-120">Vincular valores confidenciales como Twitter `Consumer Key` y `Consumer Secret` a su configuración de aplicación con el [secreto Manager](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="4cfe4-120">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="4cfe4-121">Para los fines de este tutorial, nombre de los tokens `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-121">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="4cfe4-122">Estos tokens se pueden encontrar en el **claves y Tokens de acceso** ficha después de crear la nueva aplicación de Twitter:</span><span class="sxs-lookup"><span data-stu-id="4cfe4-122">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Pestaña de claves y los Tokens de acceso](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="4cfe4-124">Configurar la autenticación de Twitter</span><span class="sxs-lookup"><span data-stu-id="4cfe4-124">Configure Twitter Authentication</span></span>

<span data-ttu-id="4cfe4-125">La plantilla de proyecto que se usan en este tutorial asegura de que [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) paquete ya está instalado.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-125">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="4cfe4-126">Para instalar este paquete con 2017 de Visual Studio, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-126">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="4cfe4-127">Para instalar con CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="4cfe4-127">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4cfe4-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4cfe4-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4cfe4-129">Agregue el servicio de Twitter en el `ConfigureServices` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="4cfe4-129">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

<span data-ttu-id="4cfe4-130">El `AddAuthentication` método solo debe llamarse una vez cuando se agrega varios proveedores de autenticación.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-130">The `AddAuthentication` method should only be called once when adding multiple authentication providers.</span></span> <span data-ttu-id="4cfe4-131">Las llamadas posteriores a la existe la posibilidad de reemplazar cualquiera configurado previamente [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) propiedades.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-131">Subsequent calls to it have the potential of overriding any previously configured [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) properties.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4cfe4-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4cfe4-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4cfe4-133">Agregar el middleware de Twitter en el `Configure` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="4cfe4-133">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

<span data-ttu-id="4cfe4-134">Consulte la [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) referencia de API para obtener más información sobre las opciones de configuración compatible con autenticación de Twitter.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-134">See the [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="4cfe4-135">Esto se puede usar para solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-135">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="4cfe4-136">Inicie sesión con Twitter</span><span class="sxs-lookup"><span data-stu-id="4cfe4-136">Sign in with Twitter</span></span>

<span data-ttu-id="4cfe4-137">Ejecute la aplicación y haga clic en **sesión**.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-137">Run your application and click **Log in**.</span></span> <span data-ttu-id="4cfe4-138">Aparece una opción para iniciar sesión con Twitter:</span><span class="sxs-lookup"><span data-stu-id="4cfe4-138">An option to sign in with Twitter appears:</span></span>

![Aplicación Web: usuario no autenticado](index/_static/DoneTwitter.png)

<span data-ttu-id="4cfe4-140">Al hacer clic en **Twitter** redirige a Twitter para la autenticación:</span><span class="sxs-lookup"><span data-stu-id="4cfe4-140">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Página de autenticación de Twitter](index/_static/TwitterLogin.png)

<span data-ttu-id="4cfe4-142">Después de escribir sus credenciales de Twitter, se le redirigirá al sitio web donde puede establecer el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-142">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="4cfe4-143">Ahora que haya iniciado sesión con sus credenciales de Twitter:</span><span class="sxs-lookup"><span data-stu-id="4cfe4-143">You are now logged in using your Twitter credentials:</span></span>

![Aplicación Web: usuario autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="4cfe4-145">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="4cfe4-145">Troubleshooting</span></span>

* <span data-ttu-id="4cfe4-146">**ASP.NET Core solo 2.x:** identidad si no se ha configurado mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intenta autenticar se producirá en *ArgumentException: se debe proporcionar la opción 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-146">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="4cfe4-147">La plantilla de proyecto que se usan en este tutorial se asegura de que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-147">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="4cfe4-148">Si la base de datos de sitio no se ha creado mediante la aplicación de la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-148">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="4cfe4-149">Pulse **migraciones aplicar** para crear la base de datos y actualizar para continuar después del error.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-149">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4cfe4-150">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="4cfe4-150">Next steps</span></span>

* <span data-ttu-id="4cfe4-151">En este artículo se ha explicado cómo puede autenticar con Twitter.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-151">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="4cfe4-152">Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](index.md).</span><span class="sxs-lookup"><span data-stu-id="4cfe4-152">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="4cfe4-153">Una vez que se publica un sitio web a la aplicación web de Azure, debe restablecer la `ConsumerSecret` en el portal para desarrolladores de Twitter.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-153">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="4cfe4-154">Establecer el `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret` como configuración de la aplicación en el portal de Azure.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-154">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="4cfe4-155">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="4cfe4-155">The configuration system is set up to read keys from environment variables.</span></span>
