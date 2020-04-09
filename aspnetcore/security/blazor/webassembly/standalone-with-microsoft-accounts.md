---
title: Proteja una Blazor aplicación independiente de ASP.NET Core WebAssembly con cuentas De Microsoft
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: 8c409651b3338c2baeae497bef43b994823a20f9
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977085"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a><span data-ttu-id="68802-102">Proteja una Blazor aplicación independiente de ASP.NET Core WebAssembly con cuentas De Microsoft</span><span class="sxs-lookup"><span data-stu-id="68802-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Microsoft Accounts</span></span>

<span data-ttu-id="68802-103">Por [Javier Calvarro Nelson](https://github.com/javiercn) y Luke [Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="68802-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="68802-104">Para crear Blazor una aplicación independiente de WebAssembly que use [cuentas de Microsoft con Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) para la autenticación:</span><span class="sxs-lookup"><span data-stu-id="68802-104">To create a Blazor WebAssembly standalone app that uses [Microsoft Accounts with Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) for authentication:</span></span>

1. [<span data-ttu-id="68802-105">Crear un inquilino de AAD y una aplicación web</span><span class="sxs-lookup"><span data-stu-id="68802-105">Create an AAD tenant and web application</span></span>](/azure/active-directory/develop/v2-overview)

   <span data-ttu-id="68802-106">Registre una aplicación de AAD en el área**registros** de aplicaciones de **Azure Active Directory** > de Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="68802-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

   <span data-ttu-id="68802-107">1\.</span><span class="sxs-lookup"><span data-stu-id="68802-107">1\.</span></span> <span data-ttu-id="68802-108">Proporcione un **nombre** para la aplicación (por ejemplo, \*\* Blazor Client AAD\*\*).</span><span class="sxs-lookup"><span data-stu-id="68802-108">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span><br>
   <span data-ttu-id="68802-109">2\.</span><span class="sxs-lookup"><span data-stu-id="68802-109">2\.</span></span> <span data-ttu-id="68802-110">En **Tipos de cuenta scompatibles**, seleccione **Cuentas en cualquier directorio de organización**.</span><span class="sxs-lookup"><span data-stu-id="68802-110">In **Supported account types**, select **Accounts in any organizational directory**.</span></span><br>
   <span data-ttu-id="68802-111">3\.</span><span class="sxs-lookup"><span data-stu-id="68802-111">3\.</span></span> <span data-ttu-id="68802-112">Deje el descenso desplegable URI de **redirección** establecido `https://localhost:5001/authentication/login-callback`en **Web**y proporcione un URI de redireccionamiento de .</span><span class="sxs-lookup"><span data-stu-id="68802-112">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span><br>
   <span data-ttu-id="68802-113">4\.</span><span class="sxs-lookup"><span data-stu-id="68802-113">4\.</span></span> <span data-ttu-id="68802-114">Deshabilite la casilla de verificación **Permisos** > **de concesión de permisos para abrir y offline_access permisos.**</span><span class="sxs-lookup"><span data-stu-id="68802-114">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span><br>
   <span data-ttu-id="68802-115">5\.</span><span class="sxs-lookup"><span data-stu-id="68802-115">5\.</span></span> <span data-ttu-id="68802-116">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="68802-116">Select **Register**.</span></span>

   <span data-ttu-id="68802-117">En la**Web\*\*\*\*de configuraciones** > de la plataforma de **autenticación:** > </span><span class="sxs-lookup"><span data-stu-id="68802-117">In **Authentication** > **Platform configurations** > **Web**:</span></span>

   <span data-ttu-id="68802-118">1\.</span><span class="sxs-lookup"><span data-stu-id="68802-118">1\.</span></span> <span data-ttu-id="68802-119">Confirme que el `https://localhost:5001/authentication/login-callback` URI de **redireccionamiento** está presente.</span><span class="sxs-lookup"><span data-stu-id="68802-119">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span><br>
   <span data-ttu-id="68802-120">2\.</span><span class="sxs-lookup"><span data-stu-id="68802-120">2\.</span></span> <span data-ttu-id="68802-121">En **Implicit grant**, active las casillas de los tokens de **acceso** y los tokens **de identificador.**</span><span class="sxs-lookup"><span data-stu-id="68802-121">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span><br>
   <span data-ttu-id="68802-122">3\.</span><span class="sxs-lookup"><span data-stu-id="68802-122">3\.</span></span> <span data-ttu-id="68802-123">Los valores predeterminados restantes de la aplicación son aceptables para esta experiencia.</span><span class="sxs-lookup"><span data-stu-id="68802-123">The remaining defaults for the app are acceptable for this experience.</span></span><br>
   <span data-ttu-id="68802-124">4\.</span><span class="sxs-lookup"><span data-stu-id="68802-124">4\.</span></span> <span data-ttu-id="68802-125">Seleccione el botón **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="68802-125">Select the **Save** button.</span></span>

   <span data-ttu-id="68802-126">Registre el ID de aplicación (ID `11111111-1111-1111-1111-111111111111`de cliente) (por ejemplo, ).</span><span class="sxs-lookup"><span data-stu-id="68802-126">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

1. <span data-ttu-id="68802-127">Reemplace los marcadores de posición del siguiente comando por la información registrada anteriormente y ejecute el comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="68802-127">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   <span data-ttu-id="68802-128">Para especificar la ubicación de salida, que crea una carpeta de proyecto si no existe, `-o BlazorSample`incluya la opción de salida en el comando con una ruta de acceso (por ejemplo, ).</span><span class="sxs-lookup"><span data-stu-id="68802-128">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="68802-129">El nombre de la carpeta también pasa a formar parte del nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="68802-129">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="68802-130">Después de crear la aplicación, debería ser capaz de:</span><span class="sxs-lookup"><span data-stu-id="68802-130">After creating the app, you should be able to:</span></span>

* <span data-ttu-id="68802-131">Inicie sesión en la aplicación con una cuenta Microsoft.</span><span class="sxs-lookup"><span data-stu-id="68802-131">Log into the app using a Microsoft Account.</span></span>
* <span data-ttu-id="68802-132">Solicite tokens de acceso para las API de Blazor Microsoft con el mismo enfoque que para las aplicaciones independientes siempre que haya configurado la aplicación correctamente.</span><span class="sxs-lookup"><span data-stu-id="68802-132">Request access tokens for Microsoft APIs using the same approach as for standalone Blazor apps provided that you have configured the app correctly.</span></span> <span data-ttu-id="68802-133">Para obtener más información, consulte [Inicio rápido: configurar una aplicación para exponer las API web.](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)</span><span class="sxs-lookup"><span data-stu-id="68802-133">For more information, see [Quickstart: Configure an application to expose web APIs](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span></span>

## <a name="authentication-package"></a><span data-ttu-id="68802-134">Paquete de autenticación</span><span class="sxs-lookup"><span data-stu-id="68802-134">Authentication package</span></span>

<span data-ttu-id="68802-135">Cuando se crea una aplicación para`SingleOrg`usar cuentas de trabajo o educativas (`Microsoft.Authentication.WebAssembly.Msal`), la aplicación recibe automáticamente una referencia de paquete para la biblioteca de autenticación de [Microsoft](/azure/active-directory/develop/msal-overview) ( ).</span><span class="sxs-lookup"><span data-stu-id="68802-135">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="68802-136">El paquete proporciona un conjunto de primitivas que ayudan a la aplicación a autenticar a los usuarios y obtener tokens para llamar a las API protegidas.</span><span class="sxs-lookup"><span data-stu-id="68802-136">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="68802-137">Si agrega autenticación a una aplicación, agregue manualmente el paquete al archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="68802-137">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="68802-138">Reemplace `{VERSION}` en la referencia del paquete `Microsoft.AspNetCore.Blazor.Templates` anterior por <xref:blazor/get-started> la versión del paquete que se muestra en el artículo.</span><span class="sxs-lookup"><span data-stu-id="68802-138">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="68802-139">El `Microsoft.Authentication.WebAssembly.Msal` paquete agrega transitivamente el `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paquete a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="68802-139">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="68802-140">Soporte de servicio de autenticación</span><span class="sxs-lookup"><span data-stu-id="68802-140">Authentication service support</span></span>

<span data-ttu-id="68802-141">La compatibilidad con la autenticación de usuarios `AddMsalAuthentication` se registra `Microsoft.Authentication.WebAssembly.Msal` en el contenedor de servicios con el método de extensión proporcionado por el paquete.</span><span class="sxs-lookup"><span data-stu-id="68802-141">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="68802-142">Este método configura todos los servicios necesarios para que la aplicación interactúe con el proveedor de identidades (IP).</span><span class="sxs-lookup"><span data-stu-id="68802-142">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="68802-143">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="68802-143">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="68802-144">El `AddMsalAuthentication` método acepta una devolución de llamada para configurar los parámetros necesarios para autenticar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="68802-144">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="68802-145">Los valores necesarios para configurar la aplicación se pueden obtener de la configuración de cuentas de Microsoft al registrar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="68802-145">The values required for configuring the app can be obtained from the Microsoft Accounts configuration when you register the app.</span></span>

## <a name="access-token-scopes"></a><span data-ttu-id="68802-146">Acceso a ámbitos de token</span><span class="sxs-lookup"><span data-stu-id="68802-146">Access token scopes</span></span>

<span data-ttu-id="68802-147">La Blazor plantilla WebAssembly no configura automáticamente la aplicación para solicitar un token de acceso para una API segura.</span><span class="sxs-lookup"><span data-stu-id="68802-147">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="68802-148">Para aprovisionar un token como parte del flujo de inicio de sesión, `MsalProviderOptions`agregue el ámbito a los ámbitos de token de acceso predeterminados de:</span><span class="sxs-lookup"><span data-stu-id="68802-148">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="68802-149">Si Azure Portal proporciona un URI de ámbito y **la aplicación produce una excepción no controlada** cuando recibe una respuesta *401 Unauthorized* de la API, intente usar un URI de ámbito que no incluya el esquema y el host.</span><span class="sxs-lookup"><span data-stu-id="68802-149">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="68802-150">Por ejemplo, Azure Portal puede proporcionar uno de los siguientes formatos de URI de ámbito:</span><span class="sxs-lookup"><span data-stu-id="68802-150">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="68802-151">Proporcione el URI de ámbito sin el esquema y el host:</span><span class="sxs-lookup"><span data-stu-id="68802-151">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="68802-152">Para obtener más información, vea <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span><span class="sxs-lookup"><span data-stu-id="68802-152">For more information, see <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span></span>

## <a name="imports-file"></a><span data-ttu-id="68802-153">Archivo de importaciones</span><span class="sxs-lookup"><span data-stu-id="68802-153">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="68802-154">Página de índice</span><span class="sxs-lookup"><span data-stu-id="68802-154">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="68802-155">Componente de la aplicación</span><span class="sxs-lookup"><span data-stu-id="68802-155">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="68802-156">Componente RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="68802-156">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="68802-157">Componente LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="68802-157">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="68802-158">Componente de autenticación</span><span class="sxs-lookup"><span data-stu-id="68802-158">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="68802-159">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="68802-159">Additional resources</span></span>

* [<span data-ttu-id="68802-160">Solicitar tokens de acceso adicionales</span><span class="sxs-lookup"><span data-stu-id="68802-160">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [<span data-ttu-id="68802-161">Inicio rápido: registre una aplicación con la plataforma de identidad de Microsoft</span><span class="sxs-lookup"><span data-stu-id="68802-161">Quickstart: Register an application with the Microsoft identity platform</span></span>](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [<span data-ttu-id="68802-162">Inicio rápido: configure una aplicación para exponer las API web</span><span class="sxs-lookup"><span data-stu-id="68802-162">Quickstart: Configure an application to expose web APIs</span></span>](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
