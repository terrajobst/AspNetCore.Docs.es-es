---
title: Protección de un ASP.NET Core Blazor aplicación independiente webassembly con cuentas de Microsoft
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: 6883af3486256e7c6905626d8da09e8ae0c4a896
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083654"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a><span data-ttu-id="05be9-102">Protección de un ASP.NET Core Blazor aplicación independiente webassembly con cuentas de Microsoft</span><span class="sxs-lookup"><span data-stu-id="05be9-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Microsoft Accounts</span></span>

<span data-ttu-id="05be9-103">Por [Javier Calvarro Nelson](https://github.com/javiercn) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="05be9-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="05be9-104">Para crear una aplicación independiente Blazor webassembly que usa [cuentas de Microsoft con Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) para la autenticación:</span><span class="sxs-lookup"><span data-stu-id="05be9-104">To create a Blazor WebAssembly standalone app that uses [Microsoft Accounts with Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) for authentication:</span></span>

1. [<span data-ttu-id="05be9-105">Creación de un inquilino de AAD y una aplicación Web</span><span class="sxs-lookup"><span data-stu-id="05be9-105">Create an AAD tenant and web application</span></span>](/azure/active-directory/develop/v2-overview)

   <span data-ttu-id="05be9-106">Registre una aplicación de AAD en el área de **registros de aplicaciones** de > de **Azure Active Directory** del Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="05be9-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

   <span data-ttu-id="05be9-107">1 \.</span><span class="sxs-lookup"><span data-stu-id="05be9-107">1\.</span></span> <span data-ttu-id="05be9-108">Proporcione un **nombre** para la aplicación (por ejemplo, **Blazor cliente AAD**).</span><span class="sxs-lookup"><span data-stu-id="05be9-108">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span><br>
   <span data-ttu-id="05be9-109">2 \.</span><span class="sxs-lookup"><span data-stu-id="05be9-109">2\.</span></span> <span data-ttu-id="05be9-110">En **tipos de cuenta compatibles**, seleccione **cuentas en cualquier directorio de la organización**.</span><span class="sxs-lookup"><span data-stu-id="05be9-110">In **Supported account types**, select **Accounts in any organizational directory**.</span></span><br>
   <span data-ttu-id="05be9-111">3 \.</span><span class="sxs-lookup"><span data-stu-id="05be9-111">3\.</span></span> <span data-ttu-id="05be9-112">Deje la lista desplegable **URI de redirección** establecida en **Web**y proporcione un URI de redireccionamiento de `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="05be9-112">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span><br>
   <span data-ttu-id="05be9-113">4 \.</span><span class="sxs-lookup"><span data-stu-id="05be9-113">4\.</span></span> <span data-ttu-id="05be9-114">Deshabilite la casilla **permisos** > **conceder permisos de administrador a OpenID y offline_access** .</span><span class="sxs-lookup"><span data-stu-id="05be9-114">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span><br>
   <span data-ttu-id="05be9-115">5 \.</span><span class="sxs-lookup"><span data-stu-id="05be9-115">5\.</span></span> <span data-ttu-id="05be9-116">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="05be9-116">Select **Register**.</span></span>

   <span data-ttu-id="05be9-117">En **autenticación** > **configuraciones de plataforma** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="05be9-117">In **Authentication** > **Platform configurations** > **Web**:</span></span>

   <span data-ttu-id="05be9-118">1 \.</span><span class="sxs-lookup"><span data-stu-id="05be9-118">1\.</span></span> <span data-ttu-id="05be9-119">Confirme que el **URI de redirección** de `https://localhost:5001/authentication/login-callback` está presente.</span><span class="sxs-lookup"><span data-stu-id="05be9-119">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span><br>
   <span data-ttu-id="05be9-120">2 \.</span><span class="sxs-lookup"><span data-stu-id="05be9-120">2\.</span></span> <span data-ttu-id="05be9-121">En **concesión implícita**, active las casillas de verificación de **tokens de acceso** y **tokens de identificador**.</span><span class="sxs-lookup"><span data-stu-id="05be9-121">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span><br>
   <span data-ttu-id="05be9-122">3 \.</span><span class="sxs-lookup"><span data-stu-id="05be9-122">3\.</span></span> <span data-ttu-id="05be9-123">Los valores predeterminados restantes de la aplicación son aceptables para esta experiencia.</span><span class="sxs-lookup"><span data-stu-id="05be9-123">The remaining defaults for the app are acceptable for this experience.</span></span><br>
   <span data-ttu-id="05be9-124">4 \.</span><span class="sxs-lookup"><span data-stu-id="05be9-124">4\.</span></span> <span data-ttu-id="05be9-125">Seleccione el botón **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="05be9-125">Select the **Save** button.</span></span>

   <span data-ttu-id="05be9-126">Registre el identificador de aplicación (ID. de cliente) (por ejemplo, `11111111-1111-1111-1111-111111111111`).</span><span class="sxs-lookup"><span data-stu-id="05be9-126">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

1. <span data-ttu-id="05be9-127">Reemplace los marcadores de posición en el siguiente comando por la información registrada anteriormente y ejecute el comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="05be9-127">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   <span data-ttu-id="05be9-128">Para especificar la ubicación de salida, que crea una carpeta de proyecto si no existe, incluya la opción output en el comando con una ruta de acceso (por ejemplo, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="05be9-128">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="05be9-129">El nombre de la carpeta también se convierte en parte del nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="05be9-129">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="05be9-130">Después de crear la aplicación, debe poder:</span><span class="sxs-lookup"><span data-stu-id="05be9-130">After creating the app, you should be able to:</span></span>

* <span data-ttu-id="05be9-131">Inicie sesión en la aplicación con una cuenta de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="05be9-131">Log into the app using a Microsoft Account.</span></span>
* <span data-ttu-id="05be9-132">Solicite tokens de acceso para las API de Microsoft con el mismo enfoque que para las aplicaciones de Blazor independientes, siempre que haya configurado la aplicación correctamente.</span><span class="sxs-lookup"><span data-stu-id="05be9-132">Request access tokens for Microsoft APIs using the same approach as for standalone Blazor apps provided that you have configured the app correctly.</span></span> <span data-ttu-id="05be9-133">Para obtener más información, consulte [Inicio rápido: configurar una aplicación para exponer API Web](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span><span class="sxs-lookup"><span data-stu-id="05be9-133">For more information, see [Quickstart: Configure an application to expose web APIs](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span></span>

## <a name="authentication-package"></a><span data-ttu-id="05be9-134">Paquete de autenticación</span><span class="sxs-lookup"><span data-stu-id="05be9-134">Authentication package</span></span>

<span data-ttu-id="05be9-135">Cuando se crea una aplicación para usar cuentas profesionales o educativas (`SingleOrg`), la aplicación recibe automáticamente una referencia de paquete para la [biblioteca de autenticación de Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="05be9-135">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="05be9-136">El paquete proporciona un conjunto de primitivas que ayudan a la aplicación a autenticar a los usuarios y obtener tokens para llamar a las API protegidas.</span><span class="sxs-lookup"><span data-stu-id="05be9-136">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="05be9-137">Si agrega la autenticación a una aplicación, agregue manualmente el paquete al archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="05be9-137">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="05be9-138">Reemplace `{VERSION}` en la referencia de paquete anterior por la versión del paquete de `Microsoft.AspNetCore.Blazor.Templates` que se muestra en el artículo <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="05be9-138">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="05be9-139">El paquete de `Microsoft.Authentication.WebAssembly.Msal` agrega de manera transitiva el paquete de `Microsoft.AspNetCore.Components.WebAssembly.Authentication` a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="05be9-139">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="05be9-140">Compatibilidad con el servicio de autenticación</span><span class="sxs-lookup"><span data-stu-id="05be9-140">Authentication service support</span></span>

<span data-ttu-id="05be9-141">La compatibilidad con la autenticación de usuarios se registra en el contenedor de servicios con el método de extensión `AddMsalAuthentication` proporcionado por el paquete de `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="05be9-141">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="05be9-142">Este método configura todos los servicios necesarios para que la aplicación interactúe con el proveedor de identidades (IP).</span><span class="sxs-lookup"><span data-stu-id="05be9-142">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="05be9-143">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="05be9-143">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="05be9-144">El método `AddMsalAuthentication` acepta una devolución de llamada para configurar los parámetros necesarios para autenticar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="05be9-144">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="05be9-145">Los valores necesarios para configurar la aplicación se pueden obtener a partir de la configuración de las cuentas de Microsoft cuando se registra la aplicación.</span><span class="sxs-lookup"><span data-stu-id="05be9-145">The values required for configuring the app can be obtained from the Microsoft Accounts configuration when you register the app.</span></span>

## <a name="index-page"></a><span data-ttu-id="05be9-146">Página de índice</span><span class="sxs-lookup"><span data-stu-id="05be9-146">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a><span data-ttu-id="05be9-147">Componente de aplicación</span><span class="sxs-lookup"><span data-stu-id="05be9-147">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="05be9-148">Componente RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="05be9-148">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="05be9-149">Componente LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="05be9-149">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="05be9-150">Componente de autenticación</span><span class="sxs-lookup"><span data-stu-id="05be9-150">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="05be9-151">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="05be9-151">Additional resources</span></span>

* [<span data-ttu-id="05be9-152">Inicio rápido: registro de una aplicación con la plataforma de Microsoft Identity</span><span class="sxs-lookup"><span data-stu-id="05be9-152">Quickstart: Register an application with the Microsoft identity platform</span></span>](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [<span data-ttu-id="05be9-153">Inicio rápido: configurar una aplicación para exponer las API Web</span><span class="sxs-lookup"><span data-stu-id="05be9-153">Quickstart: Configure an application to expose web APIs</span></span>](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
