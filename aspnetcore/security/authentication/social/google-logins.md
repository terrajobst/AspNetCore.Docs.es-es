---
title: Configuración de inicio de sesión externo de Google en ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra la integración de autenticación de usuario de la cuenta de Google en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/30/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 83f45143eca1be43410880bfd875a3fce1d2e9c9
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654923"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="5b4f1-103">Configuración de inicio de sesión externo de Google en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b4f1-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="5b4f1-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5b4f1-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5b4f1-105">En este tutorial se muestra cómo permitir que los usuarios inicien sesión con su cuenta de Google mediante el proyecto ASP.NET Core 3,0 creado en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="5b4f1-105">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="5b4f1-106">Creación de un proyecto y un identificador de cliente de la consola de API de Google</span><span class="sxs-lookup"><span data-stu-id="5b4f1-106">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="5b4f1-107">Instale [Microsoft. AspNetCore. Authentication. Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google).</span><span class="sxs-lookup"><span data-stu-id="5b4f1-107">Install [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google).</span></span>
* <span data-ttu-id="5b4f1-108">Vaya a [integración de Google inicio de sesión en la aplicación web](https://developers.google.com/identity/sign-in/web/devconsole-project) y seleccione **configurar un proyecto**.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-108">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="5b4f1-109">En el cuadro de diálogo **configurar el cliente de OAuth** , seleccione **servidor Web**.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-109">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="5b4f1-110">En el cuadro de entrada de texto **URI de redireccionamiento autorizados** , establezca el URI de redirección.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-110">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="5b4f1-111">Por ejemplo: `https://localhost:44312/signin-google`</span><span class="sxs-lookup"><span data-stu-id="5b4f1-111">For example, `https://localhost:44312/signin-google`</span></span>
* <span data-ttu-id="5b4f1-112">Guarde el **identificador de cliente** y el **secreto de cliente**.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-112">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="5b4f1-113">Al implementar el sitio, registre la nueva dirección URL pública en la **consola de Google**.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-113">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="5b4f1-114">Store Google ClientID y ClientSecret</span><span class="sxs-lookup"><span data-stu-id="5b4f1-114">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="5b4f1-115">Almacenar configuraciones confidenciales como el `Client ID` de Google y `Client Secret` con el [Administrador de secretos](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="5b4f1-115">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="5b4f1-116">Para los fines de este tutorial, asigne a los tokens el nombre `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="5b4f1-116">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "<client id>"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="5b4f1-117">Puede administrar las credenciales de API y el uso en la [consola de API](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="5b4f1-117">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="5b4f1-118">Configuración de la autenticación de Google</span><span class="sxs-lookup"><span data-stu-id="5b4f1-118">Configure Google authentication</span></span>

<span data-ttu-id="5b4f1-119">Agregue el servicio de Google a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5b4f1-119">Add the Google service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupGoogle3x.cs?highlight=11-19)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="5b4f1-120">Inicio de sesión con Google</span><span class="sxs-lookup"><span data-stu-id="5b4f1-120">Sign in with Google</span></span>

* <span data-ttu-id="5b4f1-121">Ejecute la aplicación y haga clic en **iniciar sesión**.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-121">Run the app and click **Log in**.</span></span> <span data-ttu-id="5b4f1-122">Aparece una opción para iniciar sesión con Google.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-122">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="5b4f1-123">Haga clic en el botón **Google** , que redirige a Google para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-123">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="5b4f1-124">Después de escribir las credenciales de Google, se le redirigirá al sitio Web.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-124">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="5b4f1-125">Consulte la referencia de la API de <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> para obtener más información sobre las opciones de configuración admitidas por la autenticación de Google.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-125">See the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="5b4f1-126">Esto puede utilizarse para solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-126">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="5b4f1-127">Cambiar el URI de devolución de llamada predeterminado</span><span class="sxs-lookup"><span data-stu-id="5b4f1-127">Change the default callback URI</span></span>

<span data-ttu-id="5b4f1-128">El `/signin-google` de segmento URI se establece como la devolución de llamada predeterminada del proveedor de autenticación de Google.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-128">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="5b4f1-129">Puede cambiar el URI de devolución de llamada predeterminado mientras configura el middleware de autenticación de Google a través de la propiedad heredada [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la clase [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) .</span><span class="sxs-lookup"><span data-stu-id="5b4f1-129">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="5b4f1-130">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="5b4f1-130">Troubleshooting</span></span>

* <span data-ttu-id="5b4f1-131">Si el inicio de sesión no funciona y no recibe ningún error, cambie al modo de desarrollo para que el problema sea más fácil de depurar.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-131">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="5b4f1-132">Si la identidad no se configura mediante una llamada a `services.AddIdentity` en `ConfigureServices`, se intentará autenticar los resultados en *ArgumentException: se debe proporcionar la opción ' SignInScheme '* .</span><span class="sxs-lookup"><span data-stu-id="5b4f1-132">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="5b4f1-133">La plantilla de proyecto que se usa en este tutorial, se garantiza que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-133">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="5b4f1-134">Si la base de datos del sitio no se ha creado aplicando la migración inicial, se producirá *un error en la operación de base de datos al procesar el error de solicitud* .</span><span class="sxs-lookup"><span data-stu-id="5b4f1-134">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="5b4f1-135">Seleccione **aplicar migraciones** para crear la base de datos y actualice la página para continuar después del error.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-135">Select **Apply Migrations** to create the database, and refresh the page to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b4f1-136">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="5b4f1-136">Next steps</span></span>

* <span data-ttu-id="5b4f1-137">Este artículo, mostramos cómo puede autenticar con Google.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-137">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="5b4f1-138">Puede seguir un enfoque similar para autenticarse con otros proveedores mostrados en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="5b4f1-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="5b4f1-139">Una vez que publique la aplicación en Azure, restablezca el `ClientSecret` en la consola de la API de Google.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-139">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="5b4f1-140">Establezca la `Authentication:Google:ClientId` y `Authentication:Google:ClientSecret` como configuración de la aplicación en el Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-140">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="5b4f1-141">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="5b4f1-141">The configuration system is set up to read keys from environment variables.</span></span>
