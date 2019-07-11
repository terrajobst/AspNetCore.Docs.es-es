---
title: Twitter inicio de sesión de instalación externo con ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Twitter en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: d816ed27898639b0af6896a51ac035d5526c5d29
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814071"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="789d2-103">Twitter inicio de sesión de instalación externo con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="789d2-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="789d2-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="789d2-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="789d2-105">Este ejemplo muestra cómo habilitar usuarios para [inicie sesión con su cuenta de Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) usando un proyecto de ASP.NET Core 2.2 de ejemplo creado en el [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="789d2-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="789d2-106">Crear la aplicación en Twitter</span><span class="sxs-lookup"><span data-stu-id="789d2-106">Create the app in Twitter</span></span>

* <span data-ttu-id="789d2-107">Vaya a [ https://apps.twitter.com/ ](https://apps.twitter.com/) e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="789d2-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="789d2-108">Si no dispone de una cuenta de Twitter, use el **[Suscríbase ahora](https://twitter.com/signup)** vínculo para crear uno.</span><span class="sxs-lookup"><span data-stu-id="789d2-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="789d2-109">Pulse **crear nueva aplicación** y rellene la aplicación **nombre**, **descripción** públicas y **sitio Web** (Esto puede ser temporal hasta que el URI Registre el nombre de dominio):</span><span class="sxs-lookup"><span data-stu-id="789d2-109">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="789d2-110">Escriba el URI de desarrollo con `/signin-twitter` anexado a la **URI de redirección de OAuth válido** campo (por ejemplo: `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="789d2-110">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="789d2-111">El esquema de autenticación de Twitter configurado más adelante en este ejemplo controlará automáticamente las solicitudes en `/signin-twitter` ruta para implementar el flujo de OAuth.</span><span class="sxs-lookup"><span data-stu-id="789d2-111">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="789d2-112">El segmento del URI `/signin-twitter` se establece como la devolución de llamada predeterminada del proveedor de autenticación de Twitter.</span><span class="sxs-lookup"><span data-stu-id="789d2-112">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="789d2-113">Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Twitter a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) clase.</span><span class="sxs-lookup"><span data-stu-id="789d2-113">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="789d2-114">Rellene el resto del formulario y pulse **crear aplicación de Twitter**.</span><span class="sxs-lookup"><span data-stu-id="789d2-114">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="789d2-115">Se muestran los detalles de la nueva aplicación:</span><span class="sxs-lookup"><span data-stu-id="789d2-115">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="789d2-116">Almacenar la clave de API de consumidor de Twitter y el secreto</span><span class="sxs-lookup"><span data-stu-id="789d2-116">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="789d2-117">Ejecute los comandos siguientes para almacenar de forma segura `ClientId` y `ClientSecret` mediante [Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="789d2-117">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerAPISecret <Secret>
```

<span data-ttu-id="789d2-118">Vincular los valores confidenciales como Twitter `Consumer Key` y `Consumer Secret` para la configuración de aplicación mediante el [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="789d2-118">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="789d2-119">Para los fines de este ejemplo, asigne el nombre de los tokens `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="789d2-119">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="789d2-120">Estos tokens pueden encontrarse en el **claves y Tokens de acceso** ficha después de crear una nueva aplicación de Twitter:</span><span class="sxs-lookup"><span data-stu-id="789d2-120">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="789d2-121">Configurar la autenticación de Twitter</span><span class="sxs-lookup"><span data-stu-id="789d2-121">Configure Twitter Authentication</span></span>

<span data-ttu-id="789d2-122">Agregue el servicio de Twitter en el `ConfigureServices` método *Startup.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="789d2-122">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="789d2-123">Consulte la [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Twitter.</span><span class="sxs-lookup"><span data-stu-id="789d2-123">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="789d2-124">Esto puede utilizarse para solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="789d2-124">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="789d2-125">Inicie sesión con Twitter</span><span class="sxs-lookup"><span data-stu-id="789d2-125">Sign in with Twitter</span></span>

<span data-ttu-id="789d2-126">Ejecute la aplicación y seleccione **inicie sesión**.</span><span class="sxs-lookup"><span data-stu-id="789d2-126">Run the app and select **Log in**.</span></span> <span data-ttu-id="789d2-127">Aparece una opción para iniciar sesión con Twitter:</span><span class="sxs-lookup"><span data-stu-id="789d2-127">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="789d2-128">Al hacer clic en **Twitter** redirige a Twitter para la autenticación:</span><span class="sxs-lookup"><span data-stu-id="789d2-128">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="789d2-129">Después de escribir sus credenciales de Twitter, se le redirigirá al sitio web donde puede establecer su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="789d2-129">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="789d2-130">Ha iniciado sesión con sus credenciales de Twitter:</span><span class="sxs-lookup"><span data-stu-id="789d2-130">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="789d2-131">solución de problemas</span><span class="sxs-lookup"><span data-stu-id="789d2-131">Troubleshooting</span></span>

* <span data-ttu-id="789d2-132">**ASP.NET Core 2.x solo:** Si la identidad no está configurada mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intentando autenticarse producirá *ArgumentException: Se debe proporcionar la opción 'SignInScheme'* .</span><span class="sxs-lookup"><span data-stu-id="789d2-132">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="789d2-133">La plantilla de proyecto utilizada en este ejemplo garantiza que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="789d2-133">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="789d2-134">Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error.</span><span class="sxs-lookup"><span data-stu-id="789d2-134">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="789d2-135">Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.</span><span class="sxs-lookup"><span data-stu-id="789d2-135">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="789d2-136">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="789d2-136">Next steps</span></span>

* <span data-ttu-id="789d2-137">Este artículo, mostramos cómo puede autenticar con Twitter.</span><span class="sxs-lookup"><span data-stu-id="789d2-137">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="789d2-138">Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="789d2-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="789d2-139">Una vez que publique su sitio web a la aplicación web de Azure, debe restablecer el `ConsumerSecret` en el portal para desarrolladores de Twitter.</span><span class="sxs-lookup"><span data-stu-id="789d2-139">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="789d2-140">Establecer el `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret` como configuración de la aplicación en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="789d2-140">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="789d2-141">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="789d2-141">The configuration system is set up to read keys from environment variables.</span></span>