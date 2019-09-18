---
title: Configuración de inicio de sesión externo de cuenta de Microsoft con ASP.NET Core
author: rick-anderson
description: En este ejemplo se muestra la integración de cuenta de Microsoft la autenticación de usuario en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 91ace293fd16cd180b3d5c183c637af6db1d08c3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082341"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="0cd70-103">Configuración de inicio de sesión externo de cuenta de Microsoft con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0cd70-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="0cd70-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0cd70-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0cd70-105">Este ejemplo muestra cómo permitir a los usuarios iniciar sesión con sus cuenta de Microsoft mediante el proyecto ASP.NET Core 2,2 creado en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="0cd70-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="0cd70-106">Creación de la aplicación en el portal para desarrolladores de Microsoft</span><span class="sxs-lookup"><span data-stu-id="0cd70-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="0cd70-107">Vaya a la página [de registros de aplicaciones de Azure portal](https://go.microsoft.com/fwlink/?linkid=2083908) y cree o inicie sesión en un cuenta de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="0cd70-107">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="0cd70-108">Si no tiene un cuenta de Microsoft, seleccione **crear uno**.</span><span class="sxs-lookup"><span data-stu-id="0cd70-108">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="0cd70-109">Después de iniciar sesión, se le redirigirá a la página **registros de aplicaciones** :</span><span class="sxs-lookup"><span data-stu-id="0cd70-109">After signing in you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="0cd70-110">Seleccionar **nuevo registro**</span><span class="sxs-lookup"><span data-stu-id="0cd70-110">Select **New registration**</span></span>
* <span data-ttu-id="0cd70-111">Escriba un **nombre**.</span><span class="sxs-lookup"><span data-stu-id="0cd70-111">Enter a **Name**.</span></span>
* <span data-ttu-id="0cd70-112">Seleccione una opción para los **tipos de cuenta compatibles**.</span><span class="sxs-lookup"><span data-stu-id="0cd70-112">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="0cd70-113">En **URI de redirección**, escriba la dirección URL `/signin-microsoft` de desarrollo con anexado.</span><span class="sxs-lookup"><span data-stu-id="0cd70-113">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="0cd70-114">Por ejemplo, `https://localhost:44389/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="0cd70-114">For example, `https://localhost:44389/signin-microsoft`.</span></span> <span data-ttu-id="0cd70-115">El esquema de autenticación de Microsoft configurado más adelante en este ejemplo administrará `/signin-microsoft` automáticamente las solicitudes en la ruta para implementar el flujo de OAuth.</span><span class="sxs-lookup"><span data-stu-id="0cd70-115">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="0cd70-116">Seleccionar **registro**</span><span class="sxs-lookup"><span data-stu-id="0cd70-116">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="0cd70-117">Crear secreto de cliente</span><span class="sxs-lookup"><span data-stu-id="0cd70-117">Create client secret</span></span>

* <span data-ttu-id="0cd70-118">En el panel izquierdo, seleccione **certificados & secretos**.</span><span class="sxs-lookup"><span data-stu-id="0cd70-118">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="0cd70-119">En **secretos de cliente**, seleccione **nuevo secreto de cliente** .</span><span class="sxs-lookup"><span data-stu-id="0cd70-119">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="0cd70-120">Agregue una descripción para el secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="0cd70-120">Add a description for the client secret.</span></span>
  * <span data-ttu-id="0cd70-121">Seleccione el botón **Agregar** .</span><span class="sxs-lookup"><span data-stu-id="0cd70-121">Select the **Add** button.</span></span>

* <span data-ttu-id="0cd70-122">En **secretos de cliente**, copie el valor del secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="0cd70-122">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="0cd70-123">El segmento `/signin-microsoft` URI se establece como la devolución de llamada predeterminada del proveedor de autenticación de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0cd70-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="0cd70-124">Puede cambiar el URI de devolución de llamada predeterminado mientras configura el middleware de autenticación de Microsoft a través de la propiedad [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) heredada de la clase [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) .</span><span class="sxs-lookup"><span data-stu-id="0cd70-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="0cd70-125">Almacenar el ID. de cliente y el secreto de cliente de Microsoft</span><span class="sxs-lookup"><span data-stu-id="0cd70-125">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="0cd70-126">Ejecute los siguientes comandos para almacenar `ClientId` y `ClientSecret` usar el administrador de [secretos](xref:security/app-secrets)de forma segura:</span><span class="sxs-lookup"><span data-stu-id="0cd70-126">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="0cd70-127">Vincule configuraciones confidenciales como `ClientId` Microsoft `ClientSecret` y a la configuración de la aplicación mediante el [Administrador de secretos](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="0cd70-127">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="0cd70-128">Para los fines de este ejemplo, asigne un nombre `Authentication:Microsoft:ClientId` a los tokens y. `Authentication:Microsoft:ClientSecret`</span><span class="sxs-lookup"><span data-stu-id="0cd70-128">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="0cd70-129">Configurar la autenticación de la cuenta Microsoft</span><span class="sxs-lookup"><span data-stu-id="0cd70-129">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="0cd70-130">Agregue el servicio de la cuenta `Startup.ConfigureServices`de Microsoft a:</span><span class="sxs-lookup"><span data-stu-id="0cd70-130">Add the Microsoft Account service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="0cd70-131">Consulte la referencia de la API de [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) para obtener más información sobre las opciones de configuración admitidas por la autenticación de la cuenta de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0cd70-131">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="0cd70-132">Esto puede utilizarse para solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="0cd70-132">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="0cd70-133">Iniciar sesión con Microsoft cuenta</span><span class="sxs-lookup"><span data-stu-id="0cd70-133">Sign in with Microsoft Account</span></span>

<span data-ttu-id="0cd70-134">Ejecute el y haga clic **en iniciar sesión**.</span><span class="sxs-lookup"><span data-stu-id="0cd70-134">Run the and click **Log in**.</span></span> <span data-ttu-id="0cd70-135">Aparece una opción para iniciar sesión con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0cd70-135">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="0cd70-136">Al hacer clic en Microsoft, se le redirigirá a Microsoft para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="0cd70-136">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="0cd70-137">Después de iniciar sesión con su cuenta de Microsoft (si aún no ha iniciado sesión), se le pedirá que permita que la aplicación acceda a su información:</span><span class="sxs-lookup"><span data-stu-id="0cd70-137">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="0cd70-138">Puntee en **sí** y se le redirigirá de nuevo al sitio web donde puede establecer el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="0cd70-138">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="0cd70-139">Ya ha iniciado sesión con sus credenciales de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="0cd70-139">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="0cd70-140">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="0cd70-140">Troubleshooting</span></span>

* <span data-ttu-id="0cd70-141">Si el proveedor de la cuenta de Microsoft le redirige a una página de error de inicio de sesión, tenga en cuenta los parámetros de cadena `#` de consulta título y descripción del error directamente después de (hashtag) en el URI.</span><span class="sxs-lookup"><span data-stu-id="0cd70-141">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="0cd70-142">Aunque el mensaje de error parece indicar un problema con la autenticación de Microsoft, la causa más común es que el URI de la aplicación no coincida con ninguno de los **URI de redirección** especificados para la plataforma **Web** .</span><span class="sxs-lookup"><span data-stu-id="0cd70-142">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="0cd70-143">Si la identidad no se configura `services.AddIdentity` mediante `ConfigureServices`una llamada a en, si se intenta realizar *la autenticación, se producirá una excepción ArgumentException: Se debe proporcionar*la opción ' SignInScheme '.</span><span class="sxs-lookup"><span data-stu-id="0cd70-143">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="0cd70-144">La plantilla de proyecto utilizada en este ejemplo garantiza que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="0cd70-144">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="0cd70-145">Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error.</span><span class="sxs-lookup"><span data-stu-id="0cd70-145">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="0cd70-146">Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.</span><span class="sxs-lookup"><span data-stu-id="0cd70-146">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0cd70-147">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="0cd70-147">Next steps</span></span>

* <span data-ttu-id="0cd70-148">En este artículo se ha mostrado cómo puede autenticarse con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0cd70-148">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="0cd70-149">Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="0cd70-149">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="0cd70-150">Una vez que publique el sitio web en la aplicación Web de Azure, cree un nuevo secreto de cliente en el portal para desarrolladores de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0cd70-150">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="0cd70-151">Establecer el `Authentication:Microsoft:ClientId` y `Authentication:Microsoft:ClientSecret` como configuración de la aplicación en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="0cd70-151">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="0cd70-152">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="0cd70-152">The configuration system is set up to read keys from environment variables.</span></span>