---
title: Configuración de inicio de sesión externo de Twitter con ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de la autenticación de usuarios de cuentas de Twitter en una aplicación ASP.NET Core existente.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: 5d0695160d90d0c5d31b8e35bc6c4cc984829333
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944218"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="0ab0a-103">Configuración de inicio de sesión externo de Twitter con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0ab0a-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="0ab0a-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0ab0a-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0ab0a-105">Este ejemplo muestra cómo permitir a los usuarios [iniciar sesión con su cuenta de Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) mediante un proyecto de ejemplo ASP.net Core 3,0 creado en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="0ab0a-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="0ab0a-106">Creación de la aplicación en Twitter</span><span class="sxs-lookup"><span data-stu-id="0ab0a-106">Create the app in Twitter</span></span>

* <span data-ttu-id="0ab0a-107">Agregue el paquete de NuGet [Microsoft. AspNetCore. Authentication. Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) al proyecto.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-107">Add the [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet package to the project.</span></span>

* <span data-ttu-id="0ab0a-108">Vaya a [ https://apps.twitter.com/ ](https://apps.twitter.com/) e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-108">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="0ab0a-109">Si aún no tiene una cuenta de Twitter, use el vínculo **[Regístrese ahora](https://twitter.com/signup)** para crear una.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-109">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="0ab0a-110">Seleccione **Crear una aplicación**.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-110">Select **Create an app**.</span></span> <span data-ttu-id="0ab0a-111">Rellene el **nombre**de la aplicación, la descripción de la **aplicación** y el URI del **sitio web** público (esto puede ser temporal hasta que registre el nombre de dominio):</span><span class="sxs-lookup"><span data-stu-id="0ab0a-111">Fill out the **App name**, **Application description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="0ab0a-112">Escriba el URI de desarrollo con `/signin-twitter` anexado en el campo **URL de devolución de llamada** (por ejemplo: `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="0ab0a-112">Enter your development URI with `/signin-twitter` appended into the **Callback URLs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="0ab0a-113">El esquema de autenticación de Twitter configurado más adelante en este ejemplo administrará automáticamente las solicitudes en `/signin-twitter` ruta para implementar el flujo de OAuth.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-113">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="0ab0a-114">El `/signin-twitter` de segmento URI se establece como la devolución de llamada predeterminada del proveedor de autenticación de Twitter.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-114">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="0ab0a-115">Puede cambiar el URI de devolución de llamada predeterminado mientras configura el middleware de autenticación de Twitter a través de la propiedad heredada [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la clase [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="0ab0a-115">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="0ab0a-116">Rellene el resto del formulario y seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-116">Fill out the rest of the form and select **Create**.</span></span> <span data-ttu-id="0ab0a-117">Se muestran los nuevos detalles de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="0ab0a-117">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="0ab0a-118">Almacenar la clave y el secreto de API de consumidor de Twitter</span><span class="sxs-lookup"><span data-stu-id="0ab0a-118">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="0ab0a-119">Ejecute los siguientes comandos para almacenar de forma segura `ClientId` y `ClientSecret` mediante el [Administrador de secretos](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="0ab0a-119">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

<span data-ttu-id="0ab0a-120">Vincule configuraciones confidenciales como Twitter `Consumer Key` y `Consumer Secret` a la configuración de la aplicación mediante el [Administrador de secretos](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="0ab0a-120">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="0ab0a-121">Para los fines de este ejemplo, asigne a los tokens el nombre `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-121">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="0ab0a-122">Estos tokens se pueden encontrar en la pestaña **claves y tokens de acceso** después de crear una nueva aplicación de Twitter:</span><span class="sxs-lookup"><span data-stu-id="0ab0a-122">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="0ab0a-123">Configuración de la autenticación de Twitter</span><span class="sxs-lookup"><span data-stu-id="0ab0a-123">Configure Twitter Authentication</span></span>

<span data-ttu-id="0ab0a-124">Agregue el servicio Twitter en el método `ConfigureServices` del archivo *Startup.CS* :</span><span class="sxs-lookup"><span data-stu-id="0ab0a-124">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="0ab0a-125">Consulte la referencia de la API de [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) para más información sobre las opciones de configuración admitidas por la autenticación de Twitter.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-125">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="0ab0a-126">Esto puede utilizarse para solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-126">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="0ab0a-127">Inicio de sesión con Twitter</span><span class="sxs-lookup"><span data-stu-id="0ab0a-127">Sign in with Twitter</span></span>

<span data-ttu-id="0ab0a-128">Ejecute la aplicación y seleccione **iniciar sesión**.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-128">Run the app and select **Log in**.</span></span> <span data-ttu-id="0ab0a-129">Aparece una opción para iniciar sesión con Twitter:</span><span class="sxs-lookup"><span data-stu-id="0ab0a-129">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="0ab0a-130">Al hacer clic en **Twitter** , se redirige a Twitter para la autenticación:</span><span class="sxs-lookup"><span data-stu-id="0ab0a-130">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="0ab0a-131">Después de escribir sus credenciales de Twitter, se le redirigirá al sitio web donde puede establecer el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-131">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="0ab0a-132">Ya ha iniciado sesión con sus credenciales de Twitter:</span><span class="sxs-lookup"><span data-stu-id="0ab0a-132">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="0ab0a-133">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="0ab0a-133">Troubleshooting</span></span>

* <span data-ttu-id="0ab0a-134">**ASP.NET Core 2.x solo:** identidad si no está configurado mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intentando autenticarse producirá *ArgumentException: se debe proporcionar la opción 'SignInScheme'* .</span><span class="sxs-lookup"><span data-stu-id="0ab0a-134">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="0ab0a-135">La plantilla de proyecto utilizada en este ejemplo garantiza que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-135">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="0ab0a-136">Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-136">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="0ab0a-137">Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-137">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ab0a-138">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="0ab0a-138">Next steps</span></span>

* <span data-ttu-id="0ab0a-139">En este artículo se ha mostrado cómo puede realizar la autenticación con Twitter.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-139">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="0ab0a-140">Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="0ab0a-140">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="0ab0a-141">Una vez que publique el sitio web en la aplicación Web de Azure, debe restablecer el `ConsumerSecret` en el portal para desarrolladores de Twitter.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-141">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="0ab0a-142">Establecer el `Authentication:Twitter:ConsumerKey` y `Authentication:Twitter:ConsumerSecret` como configuración de la aplicación en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-142">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="0ab0a-143">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="0ab0a-143">The configuration system is set up to read keys from environment variables.</span></span>
