---
title: Proteja una Blazor aplicación independiente de ASP.NET Core WebAssembly con Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory
ms.openlocfilehash: 7e132723657b7e12803b67ec12c3a33f1945baa3
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977006"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a><span data-ttu-id="74667-102">Proteja una Blazor aplicación independiente de ASP.NET Core WebAssembly con Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="74667-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory</span></span>

<span data-ttu-id="74667-103">Por [Javier Calvarro Nelson](https://github.com/javiercn) y Luke [Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="74667-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="74667-104">Para crear Blazor una aplicación independiente de WebAssembly que use [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) para la autenticación:</span><span class="sxs-lookup"><span data-stu-id="74667-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication:</span></span>

<span data-ttu-id="74667-105">[Cree un inquilino de AAD y una aplicación web:](/azure/active-directory/develop/v2-overview)</span><span class="sxs-lookup"><span data-stu-id="74667-105">[Create an AAD tenant and web application](/azure/active-directory/develop/v2-overview):</span></span>

<span data-ttu-id="74667-106">Registre una aplicación de AAD en el área**registros** de aplicaciones de **Azure Active Directory** > de Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="74667-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="74667-107">Proporcione un **nombre** para la aplicación (por ejemplo, \*\* Blazor Client AAD\*\*).</span><span class="sxs-lookup"><span data-stu-id="74667-107">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="74667-108">Elija un **tipo de cuenta admitida.**</span><span class="sxs-lookup"><span data-stu-id="74667-108">Choose a **Supported account types**.</span></span> <span data-ttu-id="74667-109">Puede seleccionar **Cuentas en este directorio organizativo solo** para esta experiencia.</span><span class="sxs-lookup"><span data-stu-id="74667-109">You may select **Accounts in this organizational directory only** for this experience.</span></span>
1. <span data-ttu-id="74667-110">Deje el descenso desplegable URI de **redirección** establecido `https://localhost:5001/authentication/login-callback`en **Web**y proporcione un URI de redireccionamiento de .</span><span class="sxs-lookup"><span data-stu-id="74667-110">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="74667-111">Deshabilite la casilla de verificación **Permisos** > **de concesión de permisos para abrir y offline_access permisos.**</span><span class="sxs-lookup"><span data-stu-id="74667-111">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="74667-112">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="74667-112">Select **Register**.</span></span>

<span data-ttu-id="74667-113">En la**Web\*\*\*\*de configuraciones** > de la plataforma de **autenticación:** > </span><span class="sxs-lookup"><span data-stu-id="74667-113">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="74667-114">Confirme que el `https://localhost:5001/authentication/login-callback` URI de **redireccionamiento** está presente.</span><span class="sxs-lookup"><span data-stu-id="74667-114">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="74667-115">En **Implicit grant**, active las casillas de los tokens de **acceso** y los tokens **de identificador.**</span><span class="sxs-lookup"><span data-stu-id="74667-115">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="74667-116">Los valores predeterminados restantes de la aplicación son aceptables para esta experiencia.</span><span class="sxs-lookup"><span data-stu-id="74667-116">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="74667-117">Seleccione el botón **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="74667-117">Select the **Save** button.</span></span>

<span data-ttu-id="74667-118">Registre la siguiente información:</span><span class="sxs-lookup"><span data-stu-id="74667-118">Record the following information:</span></span>

* <span data-ttu-id="74667-119">ID de aplicación (ID de `11111111-1111-1111-1111-111111111111`cliente) (por ejemplo, )</span><span class="sxs-lookup"><span data-stu-id="74667-119">Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="74667-120">ID de directorio (ID de `22222222-2222-2222-2222-222222222222`inquilino) (por ejemplo, )</span><span class="sxs-lookup"><span data-stu-id="74667-120">Directory ID (Tenant ID) (for example, `22222222-2222-2222-2222-222222222222`)</span></span>

<span data-ttu-id="74667-121">Reemplace los marcadores de posición del siguiente comando por la información registrada anteriormente y ejecute el comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="74667-121">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="74667-122">Para especificar la ubicación de salida, que crea una carpeta de proyecto si no existe, `-o BlazorSample`incluya la opción de salida en el comando con una ruta de acceso (por ejemplo, ).</span><span class="sxs-lookup"><span data-stu-id="74667-122">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="74667-123">El nombre de la carpeta también pasa a formar parte del nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="74667-123">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="74667-124">Paquete de autenticación</span><span class="sxs-lookup"><span data-stu-id="74667-124">Authentication package</span></span>

<span data-ttu-id="74667-125">Cuando se crea una aplicación para`SingleOrg`usar cuentas de trabajo o educativas (`Microsoft.Authentication.WebAssembly.Msal`), la aplicación recibe automáticamente una referencia de paquete para la biblioteca de autenticación de [Microsoft](/azure/active-directory/develop/msal-overview) ( ).</span><span class="sxs-lookup"><span data-stu-id="74667-125">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="74667-126">El paquete proporciona un conjunto de primitivas que ayudan a la aplicación a autenticar a los usuarios y obtener tokens para llamar a las API protegidas.</span><span class="sxs-lookup"><span data-stu-id="74667-126">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="74667-127">Si agrega autenticación a una aplicación, agregue manualmente el paquete al archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="74667-127">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="74667-128">Reemplace `{VERSION}` en la referencia del paquete `Microsoft.AspNetCore.Blazor.Templates` anterior por <xref:blazor/get-started> la versión del paquete que se muestra en el artículo.</span><span class="sxs-lookup"><span data-stu-id="74667-128">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="74667-129">El `Microsoft.Authentication.WebAssembly.Msal` paquete agrega transitivamente el `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paquete a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74667-129">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="74667-130">Soporte de servicio de autenticación</span><span class="sxs-lookup"><span data-stu-id="74667-130">Authentication service support</span></span>

<span data-ttu-id="74667-131">La compatibilidad con la autenticación de usuarios `AddMsalAuthentication` se registra `Microsoft.Authentication.WebAssembly.Msal` en el contenedor de servicios con el método de extensión proporcionado por el paquete.</span><span class="sxs-lookup"><span data-stu-id="74667-131">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="74667-132">Este método configura todos los servicios necesarios para que la aplicación interactúe con el proveedor de identidades (IP).</span><span class="sxs-lookup"><span data-stu-id="74667-132">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="74667-133">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="74667-133">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="74667-134">El `AddMsalAuthentication` método acepta una devolución de llamada para configurar los parámetros necesarios para autenticar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="74667-134">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="74667-135">Los valores necesarios para configurar la aplicación se pueden obtener de la configuración de AAD de Azure Portal al registrar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74667-135">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

## <a name="access-token-scopes"></a><span data-ttu-id="74667-136">Acceso a ámbitos de token</span><span class="sxs-lookup"><span data-stu-id="74667-136">Access token scopes</span></span>

<span data-ttu-id="74667-137">La Blazor plantilla WebAssembly no configura automáticamente la aplicación para solicitar un token de acceso para una API segura.</span><span class="sxs-lookup"><span data-stu-id="74667-137">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="74667-138">Para aprovisionar un token como parte del flujo de inicio de sesión, `MsalProviderOptions`agregue el ámbito a los ámbitos de token de acceso predeterminados de:</span><span class="sxs-lookup"><span data-stu-id="74667-138">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="74667-139">Si Azure Portal proporciona un URI de ámbito y **la aplicación produce una excepción no controlada** cuando recibe una respuesta *401 Unauthorized* de la API, intente usar un URI de ámbito que no incluya el esquema y el host.</span><span class="sxs-lookup"><span data-stu-id="74667-139">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="74667-140">Por ejemplo, Azure Portal puede proporcionar uno de los siguientes formatos de URI de ámbito:</span><span class="sxs-lookup"><span data-stu-id="74667-140">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="74667-141">Proporcione el URI de ámbito sin el esquema y el host:</span><span class="sxs-lookup"><span data-stu-id="74667-141">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="74667-142">Para obtener más información, vea <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span><span class="sxs-lookup"><span data-stu-id="74667-142">For more information, see <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span></span>

## <a name="imports-file"></a><span data-ttu-id="74667-143">Archivo de importaciones</span><span class="sxs-lookup"><span data-stu-id="74667-143">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="74667-144">Página de índice</span><span class="sxs-lookup"><span data-stu-id="74667-144">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="74667-145">Componente de la aplicación</span><span class="sxs-lookup"><span data-stu-id="74667-145">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="74667-146">Componente RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="74667-146">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="74667-147">Componente LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="74667-147">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="74667-148">Componente de autenticación</span><span class="sxs-lookup"><span data-stu-id="74667-148">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="74667-149">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="74667-149">Additional resources</span></span>

* [<span data-ttu-id="74667-150">Solicitar tokens de acceso adicionales</span><span class="sxs-lookup"><span data-stu-id="74667-150">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* <xref:security/authentication/azure-active-directory/index>
* [<span data-ttu-id="74667-151">Documentación de la plataforma de identidad de Microsoft</span><span class="sxs-lookup"><span data-stu-id="74667-151">Microsoft identity platform documentation</span></span>](/azure/active-directory/develop/)
