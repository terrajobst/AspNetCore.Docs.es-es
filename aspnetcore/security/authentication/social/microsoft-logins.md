---
title: Programa de instalación de Microsoft Account inicio de sesión externo con ASP.NET Core
author: rick-anderson
description: Este tutorial muestra la integración de autenticación de usuario de cuenta de Microsoft en una aplicación existente de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/microsoft-logins
ms.openlocfilehash: aabbbe66aee8c8b93140bcc4181b432017cec1d7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="f83bb-103">Programa de instalación de Microsoft Account inicio de sesión externo con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f83bb-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="f83bb-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f83bb-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f83bb-105">Este tutorial muestra cómo permitir a los usuarios iniciar sesión con su cuenta de Microsoft mediante un proyecto de ASP.NET Core 2.0 de ejemplo creado en el [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="f83bb-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="f83bb-106">Crear la aplicación en el Portal para desarrolladores de Microsoft</span><span class="sxs-lookup"><span data-stu-id="f83bb-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="f83bb-107">Vaya a [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) y crear o iniciar sesión en una cuenta de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="f83bb-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![Inicie sesión en el cuadro de diálogo](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="f83bb-109">Si ya no tiene una cuenta de Microsoft, pulse  **[crear uno.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="f83bb-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="f83bb-110">Después de iniciar sesión se le redirigirá a **mis aplicaciones** página:</span><span class="sxs-lookup"><span data-stu-id="f83bb-110">After signing in you are redirected to **My applications** page:</span></span>

![Portal para desarrolladores de Microsoft abierta en Microsoft Edge](index/_static/MicrosoftDev.png)

* <span data-ttu-id="f83bb-112">Pulse **agregar una aplicación** en la esquina superior derecha de las esquinas y escriba su **nombre de la aplicación** y **correo electrónico de contacto**:</span><span class="sxs-lookup"><span data-stu-id="f83bb-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Cuadro de diálogo nuevo registro de la aplicación](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="f83bb-114">Para los fines de este tutorial, desactive el **el programa de instalación interactiva** casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="f83bb-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="f83bb-115">Pulse **crear** para continuar la **registro** página.</span><span class="sxs-lookup"><span data-stu-id="f83bb-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="f83bb-116">Proporcionar un **nombre** y anote el valor de la **Id. de aplicación**, que se utiliza como `ClientId` más adelante en el tutorial:</span><span class="sxs-lookup"><span data-stu-id="f83bb-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Página de registro](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="f83bb-118">Pulse **Agregar plataforma** en el **plataformas** sección y seleccione el **Web** plataforma:</span><span class="sxs-lookup"><span data-stu-id="f83bb-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Agregar cuadro de diálogo de plataforma](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="f83bb-120">En el nuevo **Web** plataforma sección, escriba la dirección URL de desarrollo con */signin-microsoft* anexan a la **redirigir direcciones URL** campo (por ejemplo: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="f83bb-120">In the new **Web** platform section, enter your development URL with */signin-microsoft* appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="f83bb-121">El esquema de autenticación de Microsoft configurado más adelante en este tutorial controlará automáticamente las solicitudes en */signin-microsoft* ruta para implementar el flujo de OAuth:</span><span class="sxs-lookup"><span data-stu-id="f83bb-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at */signin-microsoft* route to implement the OAuth flow:</span></span>

![Sección de plataforma Web](index/_static/MicrosoftRedirectUri.png)

* <span data-ttu-id="f83bb-123">Pulse **agregar dirección URL** para asegurarse de que se agregó la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="f83bb-123">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="f83bb-124">Rellene cualquier configuración de la aplicación si es necesario y pulse **guardar** en la parte inferior de la página para guardar los cambios en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f83bb-124">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="f83bb-125">Al implementar el sitio que necesite volver a visitar el **registro** página y establezca una nueva dirección URL pública.</span><span class="sxs-lookup"><span data-stu-id="f83bb-125">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="f83bb-126">Almacenar identificador de la aplicación de Microsoft y la contraseña</span><span class="sxs-lookup"><span data-stu-id="f83bb-126">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="f83bb-127">Tenga en cuenta el `Application Id` muestra en el **registro** página.</span><span class="sxs-lookup"><span data-stu-id="f83bb-127">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="f83bb-128">Pulse **generar nueva contraseña** en el **aplicación secretos** sección.</span><span class="sxs-lookup"><span data-stu-id="f83bb-128">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="f83bb-129">Se muestra un cuadro en el que puede copiar la contraseña de aplicación:</span><span class="sxs-lookup"><span data-stu-id="f83bb-129">This displays a box where you can copy the application password:</span></span>

![Cuadro de diálogo nueva contraseña generada](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="f83bb-131">Vincular valores confidenciales como Microsoft `Application ID` y `Password` a su configuración de aplicación con el [secreto Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="f83bb-131">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="f83bb-132">Para los fines de este tutorial, nombre de los tokens `Authentication:Microsoft:ApplicationId` y `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="f83bb-132">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="f83bb-133">Configurar la autenticación de cuenta de Microsoft</span><span class="sxs-lookup"><span data-stu-id="f83bb-133">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="f83bb-134">La plantilla de proyecto que se usan en este tutorial asegura de que [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) paquete ya está instalado.</span><span class="sxs-lookup"><span data-stu-id="f83bb-134">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="f83bb-135">Para instalar este paquete con 2017 de Visual Studio, haga doble clic en el proyecto y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f83bb-135">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="f83bb-136">Para instalar con CLI de .NET Core, ejecute lo siguiente en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="f83bb-136">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f83bb-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f83bb-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="f83bb-138">Agregue el servicio de Microsoft Account en el `ConfigureServices` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="f83bb-138">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f83bb-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f83bb-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="f83bb-140">Agregar el middleware de Microsoft Account en el `Configure` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="f83bb-140">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

* * *
<span data-ttu-id="f83bb-141">Aunque la terminología utilizada en el Portal para desarrolladores de Microsoft nombres estos tokens `ApplicationId` y `Password`, se halle expuestos como `ClientId` y `ClientSecret` a la API de configuración.</span><span class="sxs-lookup"><span data-stu-id="f83bb-141">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="f83bb-142">Consulte la [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con autenticación de Microsoft Account.</span><span class="sxs-lookup"><span data-stu-id="f83bb-142">See the [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="f83bb-143">Esto se puede usar para solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="f83bb-143">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="f83bb-144">Inicie sesión con la cuenta de Microsoft</span><span class="sxs-lookup"><span data-stu-id="f83bb-144">Sign in with Microsoft Account</span></span>

<span data-ttu-id="f83bb-145">Ejecute la aplicación y haga clic en **sesión**.</span><span class="sxs-lookup"><span data-stu-id="f83bb-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="f83bb-146">Aparece una opción para iniciar sesión con Microsoft:</span><span class="sxs-lookup"><span data-stu-id="f83bb-146">An option to sign in with Microsoft appears:</span></span>

![Registro de aplicación en la página Web: usuario no autenticado](index/_static/DoneMicrosoft.png)

<span data-ttu-id="f83bb-148">Al hacer clic en Microsoft, se le redirigirá a Microsoft para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="f83bb-148">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="f83bb-149">Después de iniciar sesión con su Account de Microsoft (si no lo ha hecho) se le pedirá para permitir que la aplicación acceder a su información:</span><span class="sxs-lookup"><span data-stu-id="f83bb-149">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Cuadro de diálogo de autenticación de Microsoft](index/_static/MicrosoftLogin.png)

<span data-ttu-id="f83bb-151">Pulse **Sí** y se le redirigirá al sitio web donde puede establecer el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="f83bb-151">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="f83bb-152">Ahora que haya iniciado sesión con sus credenciales de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="f83bb-152">You are now logged in using your Microsoft credentials:</span></span>

![Aplicación Web: usuario autenticado](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="f83bb-154">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="f83bb-154">Troubleshooting</span></span>

* <span data-ttu-id="f83bb-155">Si el proveedor de Microsoft Account le redirige a una página de error de inicio de sesión, tenga en cuenta el error título y descripción de la cadena parámetros de consulta justo después del `#` (hashtag) en el Uri.</span><span class="sxs-lookup"><span data-stu-id="f83bb-155">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="f83bb-156">Aunque parezca que el mensaje de error indica un problema con la autenticación de Microsoft, la causa más común es la aplicación Uri no coincide con ninguno de los **URI de redireccionamiento** especificado para la **Web** plataforma .</span><span class="sxs-lookup"><span data-stu-id="f83bb-156">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="f83bb-157">**ASP.NET Core solo 2.x:** identidad si no está configurado mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intenta autenticar se producirá en *ArgumentException: se debe proporcionar la opción 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="f83bb-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="f83bb-158">La plantilla de proyecto que se usan en este tutorial se asegura de que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="f83bb-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="f83bb-159">Si la base de datos de sitio no se ha creado mediante la aplicación de la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error.</span><span class="sxs-lookup"><span data-stu-id="f83bb-159">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="f83bb-160">Pulse **migraciones aplicar** para crear la base de datos y actualizar para continuar después del error.</span><span class="sxs-lookup"><span data-stu-id="f83bb-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f83bb-161">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="f83bb-161">Next steps</span></span>

* <span data-ttu-id="f83bb-162">En este artículo se ha explicado cómo puede autenticar con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f83bb-162">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="f83bb-163">Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="f83bb-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="f83bb-164">Una vez que se publica un sitio web a la aplicación web de Azure, debe crear un nuevo `Password` en el Portal para desarrolladores de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f83bb-164">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="f83bb-165">Establecer el `Authentication:Microsoft:ApplicationId` y `Authentication:Microsoft:Password` como configuración de la aplicación en el portal de Azure.</span><span class="sxs-lookup"><span data-stu-id="f83bb-165">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="f83bb-166">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="f83bb-166">The configuration system is set up to read keys from environment variables.</span></span>
