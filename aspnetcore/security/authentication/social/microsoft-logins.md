---
title: Configuración de inicio de sesión externo Account de Microsoft con ASP.NET Core
author: rick-anderson
description: En este ejemplo se muestra la integración de autenticación de usuario de la cuenta de Microsoft en una aplicación de ASP.NET Core existente.
ms.author: riande
ms.custom: mvc
ms.date: 5/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 16ec2d5f2bccc59958b884869ef42af9cfa13df0
ms.sourcegitcommit: 06a455d63ff7d6b571ca832e8117f4ac9d646baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67316589"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="1c464-103">Configuración de inicio de sesión externo Account de Microsoft con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c464-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="1c464-104">Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1c464-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1c464-105">Este ejemplo muestra cómo habilitar usuarios iniciar sesión con su cuenta de Microsoft con el proyecto de ASP.NET Core 2.2 creado en el [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="1c464-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="1c464-106">Crear la aplicación en el Portal para desarrolladores de Microsoft</span><span class="sxs-lookup"><span data-stu-id="1c464-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="1c464-107">Navegue hasta la [Azure portal: registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) página y crear o iniciar sesión en una cuenta de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="1c464-107">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="1c464-108">Si no tienes una cuenta de Microsoft, seleccione **crearla**.</span><span class="sxs-lookup"><span data-stu-id="1c464-108">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="1c464-109">Después de iniciar sesión se le redirigirá a la **registros de aplicaciones** página:</span><span class="sxs-lookup"><span data-stu-id="1c464-109">After signing in you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="1c464-110">Seleccione **nuevo registro**</span><span class="sxs-lookup"><span data-stu-id="1c464-110">Select **New registration**</span></span>
* <span data-ttu-id="1c464-111">Escriba un **nombre**.</span><span class="sxs-lookup"><span data-stu-id="1c464-111">Enter a **Name**.</span></span>
* <span data-ttu-id="1c464-112">Seleccione una opción para **admite tipos de cuenta**.</span><span class="sxs-lookup"><span data-stu-id="1c464-112">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="1c464-113">En **URI de redireccionamiento**, escriba la dirección URL de desarrollo con `/signin-microsoft` anexado.</span><span class="sxs-lookup"><span data-stu-id="1c464-113">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="1c464-114">Por ejemplo: `https://localhost:44389/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="1c464-114">For example, `https://localhost:44389/signin-microsoft`.</span></span> <span data-ttu-id="1c464-115">El esquema de autenticación de Microsoft configurado más adelante en este ejemplo controlará automáticamente las solicitudes en `/signin-microsoft` ruta para implementar el flujo de OAuth.</span><span class="sxs-lookup"><span data-stu-id="1c464-115">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="1c464-116">Seleccione **registrar**</span><span class="sxs-lookup"><span data-stu-id="1c464-116">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="1c464-117">Crear el secreto de cliente</span><span class="sxs-lookup"><span data-stu-id="1c464-117">Create client secret</span></span>

* <span data-ttu-id="1c464-118">En el panel izquierdo, seleccione **certificados y secretos**.</span><span class="sxs-lookup"><span data-stu-id="1c464-118">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="1c464-119">En **los secretos de cliente**, seleccione **nuevo secreto de cliente**</span><span class="sxs-lookup"><span data-stu-id="1c464-119">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="1c464-120">Agregue una descripción para el secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="1c464-120">Add a description for the client secret.</span></span>
  * <span data-ttu-id="1c464-121">Seleccione el **agregar** botón.</span><span class="sxs-lookup"><span data-stu-id="1c464-121">Select the **Add** button.</span></span>

* <span data-ttu-id="1c464-122">En **los secretos de cliente**, copie el valor del secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="1c464-122">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="1c464-123">El segmento del URI `/signin-microsoft` se establece como la devolución de llamada predeterminada del proveedor de autenticación de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1c464-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="1c464-124">Puede cambiar el URI de devolución de forma predeterminada al configurar el middleware de autenticación de Microsoft a través de los heredados [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propiedad de la [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) clase.</span><span class="sxs-lookup"><span data-stu-id="1c464-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="1c464-125">El secreto de cliente y el identificador de cliente de Microsoft Store</span><span class="sxs-lookup"><span data-stu-id="1c464-125">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="1c464-126">Ejecute los comandos siguientes para almacenar de forma segura `ClientId` y `ClientSecret` mediante [Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="1c464-126">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```console
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="1c464-127">Vincular configuración confidencial, como Microsoft `ClientId` y `ClientSecret` para la configuración de aplicación mediante el [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="1c464-127">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="1c464-128">Para los fines de este ejemplo, asigne el nombre de los tokens `Authentication:Microsoft:ClientId` y `Authentication:Microsoft:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="1c464-128">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="1c464-129">Configurar la autenticación de la cuenta de Microsoft</span><span class="sxs-lookup"><span data-stu-id="1c464-129">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="1c464-130">Agregar el servicio de Microsoft Account `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1c464-130">Add the Microsoft Account service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="1c464-131">Consulte la [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) referencia de API para obtener más información sobre las opciones de configuración compatible con la autenticación de Microsoft Account.</span><span class="sxs-lookup"><span data-stu-id="1c464-131">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="1c464-132">Esto puede utilizarse para solicitar información diferente sobre el usuario.</span><span class="sxs-lookup"><span data-stu-id="1c464-132">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="1c464-133">Inicie sesión con la cuenta de Microsoft</span><span class="sxs-lookup"><span data-stu-id="1c464-133">Sign in with Microsoft Account</span></span>

<span data-ttu-id="1c464-134">Ejecute el y haga clic en **inicie sesión**.</span><span class="sxs-lookup"><span data-stu-id="1c464-134">Run the and click **Log in**.</span></span> <span data-ttu-id="1c464-135">Aparece una opción para iniciar sesión en Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1c464-135">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="1c464-136">Al hacer clic en Microsoft, se le redirigirá a Microsoft para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="1c464-136">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="1c464-137">Después de iniciar sesión con su Account de Microsoft (si aún no ha iniciado sesión) se le indicará que permiten que la aplicación acceso a tu información:</span><span class="sxs-lookup"><span data-stu-id="1c464-137">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="1c464-138">Pulse **Sí** y se le redirigirá al sitio web donde puede establecer su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="1c464-138">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="1c464-139">Ha iniciado sesión con sus credenciales de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="1c464-139">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="1c464-140">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="1c464-140">Troubleshooting</span></span>

* <span data-ttu-id="1c464-141">Si el proveedor de Microsoft Account le redirige a una página de error de inicio de sesión, tenga en cuenta el error título y descripción cadena parámetros de consulta justo después de la `#` (hashtag) en el Uri.</span><span class="sxs-lookup"><span data-stu-id="1c464-141">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="1c464-142">Aunque parezca que el mensaje de error indica un problema con la autenticación de Microsoft, la causa más común es la aplicación del Uri no coincide con cualquiera de los **URI de redirección** especificado para el **Web** plataforma .</span><span class="sxs-lookup"><span data-stu-id="1c464-142">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="1c464-143">Si la identidad no está configurada mediante una llamada a `services.AddIdentity` en `ConfigureServices`, intentando autenticarse producirá *ArgumentException: Se debe proporcionar la opción 'SignInScheme'* .</span><span class="sxs-lookup"><span data-stu-id="1c464-143">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="1c464-144">La plantilla de proyecto utilizada en este ejemplo garantiza que esto se realiza.</span><span class="sxs-lookup"><span data-stu-id="1c464-144">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="1c464-145">Si la base de datos de sitio no se ha creado aplicando a la migración inicial, obtendrá *error en una operación de base de datos al procesar la solicitud* error.</span><span class="sxs-lookup"><span data-stu-id="1c464-145">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="1c464-146">Pulse **aplicar migraciones** para crear la base de datos y actualizar para continuar más allá del error.</span><span class="sxs-lookup"><span data-stu-id="1c464-146">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1c464-147">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="1c464-147">Next steps</span></span>

* <span data-ttu-id="1c464-148">Este artículo, mostramos cómo puede autenticar con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1c464-148">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="1c464-149">Puede seguir un enfoque similar para autenticar con otros proveedores que se enumeran en la [página anterior](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="1c464-149">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="1c464-150">Una vez que publique su sitio web a la aplicación web de Azure, cree a un nuevo cliente secretos en el Portal para desarrolladores de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1c464-150">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="1c464-151">Establecer el `Authentication:Microsoft:ClientId` y `Authentication:Microsoft:ClientSecret` como configuración de la aplicación en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="1c464-151">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="1c464-152">El sistema de configuración está configurado para leer las claves de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="1c464-152">The configuration system is set up to read keys from environment variables.</span></span>