---
title: Configuración de inicio de sesión externo de Google en ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Google en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: e12d831d2e0a5c9acae5ea41fb4187ad4ca6b0ea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082487"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="ffe1b-103">Configuración de inicio de sesión externo de Google en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ffe1b-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="ffe1b-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ffe1b-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ffe1b-105">Las [API heredadas de Google + se han cerrado a partir del 7 de marzo de 2019](https://developers.google.com/+/api-shutdown).</span><span class="sxs-lookup"><span data-stu-id="ffe1b-105">[Legacy Google+ APIs have been shut down as of March 7, 2019](https://developers.google.com/+/api-shutdown).</span></span> <span data-ttu-id="ffe1b-106">El inicio de sesión de Google + y los desarrolladores se deben trasladar a un nuevo sistema de inicio de sesión de Google.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-106">Google+ sign in and developers must move to a new Google sign in system.</span></span> <span data-ttu-id="ffe1b-107">Los paquetes ASP.NET Core 2,1 y 2,2 de la autenticación de Google se han actualizado para dar cabida a los cambios.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-107">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication have be updated to accommodate the changes.</span></span> <span data-ttu-id="ffe1b-108">Para obtener más información y mitigaciones temporales de ASP.NET Core, consulte [este problema de github](https://github.com/aspnet/AspNetCore/issues/6486).</span><span class="sxs-lookup"><span data-stu-id="ffe1b-108">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="ffe1b-109">Este tutorial se ha actualizado con el nuevo proceso de configuración.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-109">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="ffe1b-110">En este tutorial se muestra cómo permitir que los usuarios inicien sesión con su cuenta de Google mediante el proyecto ASP.NET Core 2,2 creado en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="ffe1b-110">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="ffe1b-111">Creación de un proyecto y un identificador de cliente de la consola de API de Google</span><span class="sxs-lookup"><span data-stu-id="ffe1b-111">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="ffe1b-112">Vaya a [integración de Google inicio de sesión en la aplicación web](https://developers.google.com/identity/sign-in/web/devconsole-project) y seleccione **configurar un proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-112">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="ffe1b-113">En el cuadro de diálogo **configurar el cliente de OAuth** , seleccione **servidor Web**.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-113">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="ffe1b-114">En el cuadro de entrada de texto **URI de redireccionamiento autorizados** , establezca el URI de redirección.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-114">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="ffe1b-115">Por ejemplo, `https://localhost:5001/signin-google`</span><span class="sxs-lookup"><span data-stu-id="ffe1b-115">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="ffe1b-116">Guarde el **identificador de cliente** y el **secreto de cliente**.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-116">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="ffe1b-117">Al implementar el sitio, registre la nueva dirección URL pública en la **consola de Google**.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-117">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="ffe1b-118">Store Google ClientID y ClientSecret</span><span class="sxs-lookup"><span data-stu-id="ffe1b-118">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="ffe1b-119">Almacenar configuraciones confidenciales como Google `Client ID` y `Client Secret` con el administrador de [secretos](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="ffe1b-119">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="ffe1b-120">Para los fines de este tutorial, asigne un nombre `Authentication:Google:ClientId` a los tokens y: `Authentication:Google:ClientSecret`</span><span class="sxs-lookup"><span data-stu-id="ffe1b-120">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="ffe1b-121">Puede administrar las credenciales de API y el uso en la [consola de API](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="ffe1b-121">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="ffe1b-122">Configuración de la autenticación de Google</span><span class="sxs-lookup"><span data-stu-id="ffe1b-122">Configure Google authentication</span></span>

<span data-ttu-id="ffe1b-123">Agregue el servicio de Google `Startup.ConfigureServices`a:</span><span class="sxs-lookup"><span data-stu-id="ffe1b-123">Add the Google service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="ffe1b-124">Inicie sesión con Google</span><span class="sxs-lookup"><span data-stu-id="ffe1b-124">Sign in with Google</span></span>

* <span data-ttu-id="ffe1b-125">Ejecute la aplicación y haga clic en **iniciar sesión**.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-125">Run the app and click **Log in**.</span></span> <span data-ttu-id="ffe1b-126">Aparece una opción para iniciar sesión con Google.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-126">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="ffe1b-127">Haga clic en el botón **Google** , que redirige a Google para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-127">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="ffe1b-128">Después de escribir las credenciales de Google, se le redirigirá al sitio Web.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-128">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="ffe1b-129">Consulte la <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> referencia de la API para obtener más información sobre las opciones de configuración admitidas por la autenticación de Google.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-129">See the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="ffe1b-130">Esto puede utilizarse para solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-130">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="ffe1b-131">Cambiar el URI de devolución de llamada predeterminado</span><span class="sxs-lookup"><span data-stu-id="ffe1b-131">Change the default callback URI</span></span>

<span data-ttu-id="ffe1b-132">El segmento del URI `/signin-google` se establece como la devolución de llamada predeterminada del proveedor de autenticación de Google.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-132">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="ffe1b-133">Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Google a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) clase.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-133">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ffe1b-134">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="ffe1b-134">Troubleshooting</span></span>

* <span data-ttu-id="ffe1b-135">Si el inicio de sesión no funciona y no recibe ningún error, cambie al modo de desarrollo para que el problema sea más fácil de depurar.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-135">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="ffe1b-136">Si la identidad no se configura `services.AddIdentity` mediante `ConfigureServices`una llamada a en, se intentará *autenticar los resultados en ArgumentException: Se debe proporcionar*la opción ' SignInScheme '.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-136">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="ffe1b-137">La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-137">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="ffe1b-138">Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-138">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="ffe1b-139">Seleccione **aplicar migraciones** para crear la base de datos y actualice la página para continuar después del error.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-139">Select **Apply Migrations** to create the database, and refresh the page to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ffe1b-140">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="ffe1b-140">Next steps</span></span>

* <span data-ttu-id="ffe1b-141">Este artículo, mostramos cómo puede autenticar con Google.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-141">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="ffe1b-142">Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="ffe1b-142">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="ffe1b-143">Una vez que publique la aplicación en Azure, restablezca `ClientSecret` en la consola de la API de Google.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-143">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="ffe1b-144">Establecer el `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret` como configuración de la aplicación en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-144">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="ffe1b-145">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="ffe1b-145">The configuration system is set up to read keys from environment variables.</span></span>
