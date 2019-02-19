---
title: Configuración de inicio de sesión externo de Google en ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Google en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 1/11/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 5b6bfaafba68eaf15a60b7c512a9e7406e3112ee
ms.sourcegitcommit: 6d8c362a3cb4c634e64cbb3d28069c36bc973581
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418671"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="0656b-103">Configuración de inicio de sesión externo de Google en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0656b-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="0656b-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0656b-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0656b-105">En enero de 2019 Google empezó a [apagar](https://developers.google.com/+/api-shutdown) Google + iniciar sesión y los desarrolladores deben mover a un nuevo inicio de sesión de Google en el sistema de marzo.</span><span class="sxs-lookup"><span data-stu-id="0656b-105">In January 2019 Google started to [shut down](https://developers.google.com/+/api-shutdown) Google+ sign in and developers must move to a new Google sign in system by March.</span></span> <span data-ttu-id="0656b-106">En febrero para dar cabida a los cambios, se actualizará la ASP.NET Core 2.1 y 2.2 paquetes para la autenticación de Google.</span><span class="sxs-lookup"><span data-stu-id="0656b-106">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication will be updated in February to accommodate the changes.</span></span> <span data-ttu-id="0656b-107">Para obtener más información y mitigaciones temporales para ASP.NET Core, consulte [este problema de GitHub](https://github.com/aspnet/AspNetCore/issues/6486).</span><span class="sxs-lookup"><span data-stu-id="0656b-107">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="0656b-108">En este tutorial se ha actualizado con el nuevo proceso de instalación.</span><span class="sxs-lookup"><span data-stu-id="0656b-108">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="0656b-109">Este tutorial muestra cómo habilitar usuarios iniciar sesión con su cuenta de Google con el proyecto de ASP.NET Core 2.2 creado en el [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="0656b-109">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="0656b-110">Crear un identificador de cliente y el proyecto de consola de API de Google</span><span class="sxs-lookup"><span data-stu-id="0656b-110">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="0656b-111">Vaya a [la integración de Google signo In de la aplicación web](https://developers.google.com/identity/sign-in/web/devconsole-project) y seleccione **configurar un proyecto**.</span><span class="sxs-lookup"><span data-stu-id="0656b-111">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="0656b-112">En el **configuración del cliente de OAuth** cuadro de diálogo, seleccione **servidor Web**.</span><span class="sxs-lookup"><span data-stu-id="0656b-112">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="0656b-113">En el **URI de redireccionamiento autorizado** cuadro de entrada de texto, establecer el URI de redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="0656b-113">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="0656b-114">Por ejemplo, `https://localhost:5001/signin-google`.</span><span class="sxs-lookup"><span data-stu-id="0656b-114">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="0656b-115">Guardar el **Id. de cliente** y **secreto de cliente**.</span><span class="sxs-lookup"><span data-stu-id="0656b-115">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="0656b-116">Al implementar el sitio, registrar la nueva url pública desde el **Google Console**.</span><span class="sxs-lookup"><span data-stu-id="0656b-116">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="0656b-117">Store Google ClientID y ClientSecret</span><span class="sxs-lookup"><span data-stu-id="0656b-117">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="0656b-118">Store valores confidenciales, como Google `Client ID` y `Client Secret` con el [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="0656b-118">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="0656b-119">Para los fines de este tutorial, asigne el nombre de los tokens `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="0656b-119">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

<span data-ttu-id="0656b-120">Puede administrar las credenciales de la API y el uso en el [consola de API](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="0656b-120">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="0656b-121">Configurar la autenticación de Google</span><span class="sxs-lookup"><span data-stu-id="0656b-121">Configure Google authentication</span></span>

<span data-ttu-id="0656b-122">Agregar el servicio de Google `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0656b-122">Add the Google service to `Startup.ConfigureServices`.</span></span>

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="0656b-123">Inicie sesión con Google</span><span class="sxs-lookup"><span data-stu-id="0656b-123">Sign in with Google</span></span>

* <span data-ttu-id="0656b-124">Ejecute la aplicación y haga clic en **inicie sesión**.</span><span class="sxs-lookup"><span data-stu-id="0656b-124">Run the app and click **Log in**.</span></span> <span data-ttu-id="0656b-125">Aparece una opción para iniciar sesión con Google.</span><span class="sxs-lookup"><span data-stu-id="0656b-125">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="0656b-126">Haga clic en el **Google** botón, que redirige a Google para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="0656b-126">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="0656b-127">Después de escribir sus credenciales de Google, se le redirigirá al sitio web.</span><span class="sxs-lookup"><span data-stu-id="0656b-127">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="0656b-128">Consulte la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Google.</span><span class="sxs-lookup"><span data-stu-id="0656b-128">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="0656b-129">Esto puede utilizarse para solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="0656b-129">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="0656b-130">Cambiar el URI de devolución de llamada predeterminada</span><span class="sxs-lookup"><span data-stu-id="0656b-130">Change the default callback URI</span></span>

<span data-ttu-id="0656b-131">El segmento del URI `/signin-google` se establece como la devolución de llamada predeterminada del proveedor de autenticación de Google.</span><span class="sxs-lookup"><span data-stu-id="0656b-131">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="0656b-132">Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Google a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) clase.</span><span class="sxs-lookup"><span data-stu-id="0656b-132">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="0656b-133">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="0656b-133">Troubleshooting</span></span>

* <span data-ttu-id="0656b-134">Si el inicio de sesión no funciona y no recibe algún error, cambie al modo de desarrollo para hacer más fácil de depurar el problema.</span><span class="sxs-lookup"><span data-stu-id="0656b-134">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="0656b-135">Si la identidad no está configurada mediante una llamada a `services.AddIdentity` en `ConfigureServices`, al intentar autenticar los resultados en *ArgumentException: Se debe proporcionar la opción 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="0656b-135">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="0656b-136">La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="0656b-136">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="0656b-137">Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error.</span><span class="sxs-lookup"><span data-stu-id="0656b-137">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="0656b-138">Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.</span><span class="sxs-lookup"><span data-stu-id="0656b-138">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0656b-139">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="0656b-139">Next steps</span></span>

* <span data-ttu-id="0656b-140">Este artículo, mostramos cómo puede autenticar con Google.</span><span class="sxs-lookup"><span data-stu-id="0656b-140">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="0656b-141">Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="0656b-141">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="0656b-142">Una vez que publique la aplicación en Azure, restablezca el `ClientSecret` en la consola de API de Google.</span><span class="sxs-lookup"><span data-stu-id="0656b-142">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="0656b-143">Establecer el `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret` como configuración de la aplicación en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="0656b-143">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="0656b-144">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="0656b-144">The configuration system is set up to read keys from environment variables.</span></span>
