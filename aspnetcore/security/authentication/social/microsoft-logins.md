---
title: Configuración de inicio de sesión externo Account de Microsoft con ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Microsoft en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 1733d049d6752c24d7749b5b5ae2a4b866492358
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2019
ms.locfileid: "58751007"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="b4415-103">Configuración de inicio de sesión externo Account de Microsoft con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b4415-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="b4415-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b4415-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b4415-105">Este tutorial muestra cómo permitir que los usuarios iniciar sesión con su cuenta de Microsoft mediante un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b4415-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="b4415-106">Crear la aplicación en el Portal para desarrolladores de Microsoft</span><span class="sxs-lookup"><span data-stu-id="b4415-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="b4415-107">Vaya a [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) y crear o iniciar sesión en una cuenta de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="b4415-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![Inicie sesión en el cuadro de diálogo](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="b4415-109">Si aún no tiene una cuenta de Microsoft, pulse  **[crear uno!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="b4415-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="b4415-110">Después de iniciar sesión se le redirigirá a **mis aplicaciones** página:</span><span class="sxs-lookup"><span data-stu-id="b4415-110">After signing in you are redirected to **My applications** page:</span></span>

![Portal para desarrolladores de Microsoft abierto en Microsoft Edge](index/_static/MicrosoftDev.png)

* <span data-ttu-id="b4415-112">Pulse **agregar una aplicación** en la parte superior derecha esquina y escriba su **nombre de la aplicación** y **correo electrónico de contacto**:</span><span class="sxs-lookup"><span data-stu-id="b4415-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Cuadro de diálogo nuevo registro de la aplicación](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="b4415-114">Para los fines de este tutorial, desactive la **configuración paso a paso** casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="b4415-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="b4415-115">Pulse **crear** para continuar la **registro** página.</span><span class="sxs-lookup"><span data-stu-id="b4415-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="b4415-116">Proporcionar un **nombre** y anote el valor de la **Id. de aplicación**, que usan como `ClientId` más adelante en el tutorial:</span><span class="sxs-lookup"><span data-stu-id="b4415-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Página de registro](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="b4415-118">Pulse **Agregar plataforma** en el **plataformas** sección y seleccione el **Web** plataforma:</span><span class="sxs-lookup"><span data-stu-id="b4415-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Agregar cuadro de diálogo plataforma](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="b4415-120">En el nuevo **Web** plataforma sección, escriba la dirección URL de desarrollo con `/signin-microsoft` anexado a la **URL de redirección** campo (por ejemplo: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="b4415-120">In the new **Web** platform section, enter your development URL with `/signin-microsoft` appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="b4415-121">El esquema de autenticación de Microsoft configurado más adelante en este tutorial controlará automáticamente las solicitudes en `/signin-microsoft` ruta para implementar el flujo de OAuth:</span><span class="sxs-lookup"><span data-stu-id="b4415-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow:</span></span>

![Sección de plataforma Web](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> <span data-ttu-id="b4415-123">El segmento del URI `/signin-microsoft` se establece como la devolución de llamada predeterminada del proveedor de autenticación de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b4415-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="b4415-124">Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Microsoft a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) clase.</span><span class="sxs-lookup"><span data-stu-id="b4415-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

* <span data-ttu-id="b4415-125">Pulse **agregar dirección URL** para asegurarse de que se ha agregado la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="b4415-125">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="b4415-126">Complete otras configuraciones de aplicaciones si es necesario y pulse **guardar** en la parte inferior de la página para guardar los cambios en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b4415-126">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="b4415-127">Al implementar el sitio, deberá volver a visitar la **registro** página y establecer una nueva dirección URL pública.</span><span class="sxs-lookup"><span data-stu-id="b4415-127">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="b4415-128">Store Id. de aplicación de Microsoft y la contraseña</span><span class="sxs-lookup"><span data-stu-id="b4415-128">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="b4415-129">Tenga en cuenta la `Application Id` muestra en el **registro** página.</span><span class="sxs-lookup"><span data-stu-id="b4415-129">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="b4415-130">Pulse **generar nueva contraseña** en el **secretos de aplicación** sección.</span><span class="sxs-lookup"><span data-stu-id="b4415-130">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="b4415-131">Esto muestra un cuadro donde puede copiar la contraseña de aplicación:</span><span class="sxs-lookup"><span data-stu-id="b4415-131">This displays a box where you can copy the application password:</span></span>

![Cuadro de diálogo nueva contraseña generada](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="b4415-133">Vincular configuración confidencial, como Microsoft `Application ID` y `Password` para la configuración de aplicación mediante el [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="b4415-133">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="b4415-134">Para los fines de este tutorial, asigne el nombre de los tokens `Authentication:Microsoft:ApplicationId` y `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="b4415-134">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="b4415-135">Configurar la autenticación de la cuenta de Microsoft</span><span class="sxs-lookup"><span data-stu-id="b4415-135">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="b4415-136">La plantilla de proyecto que se usa en este tutorial garantiza que [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) paquete ya está instalado.</span><span class="sxs-lookup"><span data-stu-id="b4415-136">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="b4415-137">Para instalar este paquete con Visual Studio 2017, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b4415-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="b4415-138">Para instalar con la CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="b4415-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b4415-139">Agregue el servicio de Microsoft Account en el `ConfigureServices` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="b4415-139">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b4415-140">Agregue el middleware de Microsoft Account en el `Configure` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="b4415-140">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

<span data-ttu-id="b4415-141">Aunque la terminología usada en el Portal para desarrolladores de Microsoft los nombres de estos tokens `ApplicationId` y `Password`, estas se exponen como `ClientId` y `ClientSecret` a la API de configuración.</span><span class="sxs-lookup"><span data-stu-id="b4415-141">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="b4415-142">Consulte la [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Microsoft Account.</span><span class="sxs-lookup"><span data-stu-id="b4415-142">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="b4415-143">Esto puede utilizarse para solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="b4415-143">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="b4415-144">Inicie sesión con la cuenta de Microsoft</span><span class="sxs-lookup"><span data-stu-id="b4415-144">Sign in with Microsoft Account</span></span>

<span data-ttu-id="b4415-145">Ejecute la aplicación y haga clic en **inicie sesión**.</span><span class="sxs-lookup"><span data-stu-id="b4415-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="b4415-146">Aparece una opción para iniciar sesión en Microsoft:</span><span class="sxs-lookup"><span data-stu-id="b4415-146">An option to sign in with Microsoft appears:</span></span>

![Aplicación Web de registro en la página: Usuario no autenticado](index/_static/DoneMicrosoft.png)

<span data-ttu-id="b4415-148">Al hacer clic en Microsoft, se le redirigirá a Microsoft para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="b4415-148">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="b4415-149">Después de iniciar sesión con su Account de Microsoft (si aún no ha iniciado sesión) se le indicará que permiten que la aplicación acceso a tu información:</span><span class="sxs-lookup"><span data-stu-id="b4415-149">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Cuadro de diálogo de autenticación de Microsoft](index/_static/MicrosoftLogin.png)

<span data-ttu-id="b4415-151">Pulse **Sí** y se le redirigirá al sitio web donde puede establecer su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="b4415-151">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="b4415-152">Ha iniciado sesión con sus credenciales de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="b4415-152">You are now logged in using your Microsoft credentials:</span></span>

![Aplicación Web: Usuario autenticado](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="b4415-154">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="b4415-154">Troubleshooting</span></span>

* <span data-ttu-id="b4415-155">Si el proveedor de Microsoft Account le redirige a una página de error de inicio de sesión, tenga en cuenta el error título y descripción cadena parámetros de consulta justo después de la `#` (hashtag) en el Uri.</span><span class="sxs-lookup"><span data-stu-id="b4415-155">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="b4415-156">Aunque parezca que el mensaje de error indica un problema con la autenticación de Microsoft, la causa más común es la aplicación del Uri no coincide con cualquiera de los **URI de redirección** especificado para el **Web** plataforma .</span><span class="sxs-lookup"><span data-stu-id="b4415-156">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="b4415-157">**ASP.NET Core 2.x solo:** Si la identidad no está configurada mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intentando autenticarse producirá *ArgumentException: Se debe proporcionar la opción 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="b4415-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="b4415-158">La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="b4415-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="b4415-159">Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error.</span><span class="sxs-lookup"><span data-stu-id="b4415-159">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="b4415-160">Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.</span><span class="sxs-lookup"><span data-stu-id="b4415-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4415-161">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="b4415-161">Next steps</span></span>

* <span data-ttu-id="b4415-162">Este artículo, mostramos cómo puede autenticar con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b4415-162">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="b4415-163">Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b4415-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="b4415-164">Una vez que publique su sitio web a la aplicación web de Azure, debe crear un nuevo `Password` en el Portal para desarrolladores de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b4415-164">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="b4415-165">Establecer el `Authentication:Microsoft:ApplicationId` y `Authentication:Microsoft:Password` como configuración de la aplicación en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="b4415-165">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="b4415-166">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="b4415-166">The configuration system is set up to read keys from environment variables.</span></span>
