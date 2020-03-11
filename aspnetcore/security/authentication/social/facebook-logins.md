---
title: Configuración de inicio de sesión externo de Facebook en ASP.NET Core
author: rick-anderson
description: Tutorial con ejemplos de código que muestran la integración de la autenticación de usuarios de cuentas de Facebook en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 12/02/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: 2e4cc04c6e7ff8e5f5701cc7f9ede73dbc1b4685
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654887"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="131f7-103">Configuración de inicio de sesión externo de Facebook en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="131f7-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="131f7-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="131f7-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="131f7-105">En este tutorial con ejemplos de código se muestra cómo habilitar a los usuarios para que inicien sesión con su cuenta de Facebook mediante un proyecto de ejemplo ASP.NET Core 3,0 creado en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="131f7-105">This tutorial with code examples shows how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="131f7-106">Comenzaremos por crear un identificador de aplicación de Facebook siguiendo los [pasos oficiales](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="131f7-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="131f7-107">Crear la aplicación en Facebook</span><span class="sxs-lookup"><span data-stu-id="131f7-107">Create the app in Facebook</span></span>

* <span data-ttu-id="131f7-108">Agregue el paquete NuGet [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) al proyecto.</span><span class="sxs-lookup"><span data-stu-id="131f7-108">Add the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package to the project.</span></span>

* <span data-ttu-id="131f7-109">Vaya a la página de la [aplicación de desarrolladores de Facebook](https://developers.facebook.com/apps/) e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="131f7-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="131f7-110">Si aún no tiene una cuenta de Facebook, use el vínculo **suscribirse a Facebook** en la página de inicio de sesión para crear una.</span><span class="sxs-lookup"><span data-stu-id="131f7-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>  <span data-ttu-id="131f7-111">Una vez que tenga una cuenta de Facebook, siga las instrucciones para registrarse como desarrollador de Facebook.</span><span class="sxs-lookup"><span data-stu-id="131f7-111">Once you have a Facebook account, follow the instructions to register as a Facebook Developer.</span></span>

* <span data-ttu-id="131f7-112">En el menú **mis aplicaciones** , seleccione **crear aplicación** para crear un nuevo identificador de aplicación.</span><span class="sxs-lookup"><span data-stu-id="131f7-112">From the **My Apps** menu select **Create App** to create a new App ID.</span></span>

   ![Facebook para el portal de desarrolladores abierta en Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="131f7-114">Rellene el formulario y pulse el botón **crear ID** . de aplicación.</span><span class="sxs-lookup"><span data-stu-id="131f7-114">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Crear un formulario nuevo identificador de aplicación](index/_static/FBNewAppId.png)

* <span data-ttu-id="131f7-116">En la tarjeta nueva aplicación, seleccione **Agregar un producto**.</span><span class="sxs-lookup"><span data-stu-id="131f7-116">On the new App card, select **Add a Product**.</span></span>  <span data-ttu-id="131f7-117">En la tarjeta de **Inicio de sesión de Facebook** , haga clic en **configurar** .</span><span class="sxs-lookup"><span data-stu-id="131f7-117">On the **Facebook Login** card, click **Set Up**</span></span> 

  ![Página de instalación del producto](index/_static/FBProductSetup.png)

* <span data-ttu-id="131f7-119">El Asistente de **Inicio rápido** se inicia con **elegir una plataforma** como primera página.</span><span class="sxs-lookup"><span data-stu-id="131f7-119">The **Quickstart** wizard launches with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="131f7-120">Omita el asistente por ahora; para ello, haga clic en el vínculo **configuración** de **Inicio de sesión de Facebook** en el menú de la parte inferior izquierda:</span><span class="sxs-lookup"><span data-stu-id="131f7-120">Bypass the wizard for now by clicking the **FaceBook Login** **Settings** link in the menu on the lower left:</span></span>

  ![Inicio rápido de omisión](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="131f7-122">Aparecerá la página Configuración de **OAuth de cliente** :</span><span class="sxs-lookup"><span data-stu-id="131f7-122">You are presented with the **Client OAuth Settings** page:</span></span>

  ![Página de configuración de OAuth de cliente](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="131f7-124">Escriba el URI de desarrollo con */signin-Facebook* anexado en el campo **URI de redirección de OAuth válido** (por ejemplo: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="131f7-124">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="131f7-125">La autenticación de Facebook configurada más adelante en este tutorial administrará automáticamente las solicitudes en la ruta */signin-Facebook* para implementar el flujo de OAuth.</span><span class="sxs-lookup"><span data-stu-id="131f7-125">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="131f7-126">El URI */signin-Facebook* se establece como la devolución de llamada predeterminada del proveedor de autenticación de Facebook.</span><span class="sxs-lookup"><span data-stu-id="131f7-126">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="131f7-127">Puede cambiar el URI de devolución de llamada predeterminado mientras configura el middleware de autenticación de Facebook a través de la propiedad heredada [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la clase [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) .</span><span class="sxs-lookup"><span data-stu-id="131f7-127">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="131f7-128">Haga clic en **Guardar cambios**.</span><span class="sxs-lookup"><span data-stu-id="131f7-128">Click **Save Changes**.</span></span>

* <span data-ttu-id="131f7-129">Haga clic en **configuración** > vínculo **básico** en el panel de navegación izquierdo.</span><span class="sxs-lookup"><span data-stu-id="131f7-129">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="131f7-130">En esta página, anote el `App ID` y el `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="131f7-130">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="131f7-131">En la siguiente sección, va a agregar tanto en la aplicación de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="131f7-131">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="131f7-132">Al implementar el sitio, debe volver a visitar la página de configuración de **Inicio de sesión de Facebook** y registrar un nuevo URI público.</span><span class="sxs-lookup"><span data-stu-id="131f7-132">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="131f7-133">Store Id. de aplicación de Facebook y secreto de la aplicación</span><span class="sxs-lookup"><span data-stu-id="131f7-133">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="131f7-134">Vincule configuraciones confidenciales como Facebook `App ID` y `App Secret` a la configuración de la aplicación mediante el [Administrador de secretos](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="131f7-134">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="131f7-135">Para los fines de este tutorial, asigne a los tokens el nombre `Authentication:Facebook:AppId` y `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="131f7-135">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="131f7-136">Ejecute los siguientes comandos para almacenar de forma segura `App ID` y `App Secret` mediante el administrador de secretos:</span><span class="sxs-lookup"><span data-stu-id="131f7-136">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="131f7-137">Configurar la autenticación de Facebook</span><span class="sxs-lookup"><span data-stu-id="131f7-137">Configure Facebook Authentication</span></span>

<span data-ttu-id="131f7-138">Agregue el servicio Facebook en el método `ConfigureServices` del archivo *Startup.CS* :</span><span class="sxs-lookup"><span data-stu-id="131f7-138">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="131f7-139">Consulte la referencia de la API de [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) para obtener más información sobre las opciones de configuración admitidas por la autenticación de Facebook.</span><span class="sxs-lookup"><span data-stu-id="131f7-139">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="131f7-140">Las opciones de configuración pueden utilizarse para:</span><span class="sxs-lookup"><span data-stu-id="131f7-140">Configuration options can be used to:</span></span>

* <span data-ttu-id="131f7-141">Solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="131f7-141">Request different information about the user.</span></span>
* <span data-ttu-id="131f7-142">Agregue los argumentos de cadena de consulta para personalizar la experiencia de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="131f7-142">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="131f7-143">Inicie sesión con Facebook</span><span class="sxs-lookup"><span data-stu-id="131f7-143">Sign in with Facebook</span></span>

<span data-ttu-id="131f7-144">Ejecute la aplicación y haga clic en **iniciar sesión**.</span><span class="sxs-lookup"><span data-stu-id="131f7-144">Run your application and click **Log in**.</span></span> <span data-ttu-id="131f7-145">Verá una opción para iniciar sesión con Facebook.</span><span class="sxs-lookup"><span data-stu-id="131f7-145">You see an option to sign in with Facebook.</span></span>

![Aplicación Web: usuario no autenticado](index/_static/DoneFacebook.png)

<span data-ttu-id="131f7-147">Al hacer clic en **Facebook**, se le redirigirá a Facebook para la autenticación:</span><span class="sxs-lookup"><span data-stu-id="131f7-147">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Página de autenticación de Facebook](index/_static/FBLogin.png)

<span data-ttu-id="131f7-149">Dirección pública de perfil y correo electrónico de solicitudes de autenticación de Facebook de forma predeterminada:</span><span class="sxs-lookup"><span data-stu-id="131f7-149">Facebook authentication requests public profile and email address by default:</span></span>

![Pantalla de consentimiento de página de autenticación de Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="131f7-151">Una vez que escriba sus credenciales de Facebook se redirigen a su sitio donde puede establecer su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="131f7-151">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="131f7-152">Ha iniciado sesión con sus credenciales de Facebook:</span><span class="sxs-lookup"><span data-stu-id="131f7-152">You are now logged in using your Facebook credentials:</span></span>

![Aplicación Web: usuario autenticado](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="131f7-154">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="131f7-154">Troubleshooting</span></span>

* <span data-ttu-id="131f7-155">**Solo ASP.net Core 2. x:** Si la identidad no se configura mediante una llamada a `services.AddIdentity` en `ConfigureServices`, el intento de autenticación producirá una *excepción ArgumentException: se debe proporcionar la opción ' SignInScheme '* .</span><span class="sxs-lookup"><span data-stu-id="131f7-155">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="131f7-156">La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="131f7-156">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="131f7-157">Si la base de datos del sitio no se ha creado aplicando la migración inicial, se producirá *un error en la operación de base de datos al procesar el error de solicitud* .</span><span class="sxs-lookup"><span data-stu-id="131f7-157">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="131f7-158">Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar después del error.</span><span class="sxs-lookup"><span data-stu-id="131f7-158">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="131f7-159">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="131f7-159">Next steps</span></span>

* <span data-ttu-id="131f7-160">Este artículo, mostramos cómo puede autenticar con Facebook.</span><span class="sxs-lookup"><span data-stu-id="131f7-160">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="131f7-161">Puede seguir un enfoque similar para autenticarse con otros proveedores mostrados en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="131f7-161">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="131f7-162">Una vez que publique el sitio web en la aplicación Web de Azure, debe restablecer el `AppSecret` en el portal para desarrolladores de Facebook.</span><span class="sxs-lookup"><span data-stu-id="131f7-162">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="131f7-163">Establezca la `Authentication:Facebook:AppId` y `Authentication:Facebook:AppSecret` como configuración de la aplicación en el Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="131f7-163">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="131f7-164">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="131f7-164">The configuration system is set up to read keys from environment variables.</span></span>
