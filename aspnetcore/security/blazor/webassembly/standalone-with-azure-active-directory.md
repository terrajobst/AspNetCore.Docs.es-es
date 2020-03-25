---
title: Proteja un ASP.NET Core Blazor aplicación independiente webassembly con Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory
ms.openlocfilehash: e12c38ed42a4e2714d785ef8f03097246c40d36e
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218989"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a><span data-ttu-id="2cad8-102">Proteja un ASP.NET Core Blazor aplicación independiente webassembly con Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2cad8-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory</span></span>

<span data-ttu-id="2cad8-103">Por [Javier Calvarro Nelson](https://github.com/javiercn) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2cad8-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="2cad8-104">Para crear una aplicación independiente Blazor webassembly que usa [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) para la autenticación:</span><span class="sxs-lookup"><span data-stu-id="2cad8-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication:</span></span>

<span data-ttu-id="2cad8-105">[Cree un inquilino de AAD y una aplicación web](/azure/active-directory/develop/v2-overview):</span><span class="sxs-lookup"><span data-stu-id="2cad8-105">[Create an AAD tenant and web application](/azure/active-directory/develop/v2-overview):</span></span>

<span data-ttu-id="2cad8-106">Registre una aplicación de AAD en el área de **registros de aplicaciones** de > de **Azure Active Directory** del Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="2cad8-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="2cad8-107">Proporcione un **nombre** para la aplicación (por ejemplo, **Blazor cliente AAD**).</span><span class="sxs-lookup"><span data-stu-id="2cad8-107">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="2cad8-108">Elija un **tipo de cuenta compatible**.</span><span class="sxs-lookup"><span data-stu-id="2cad8-108">Choose a **Supported account types**.</span></span> <span data-ttu-id="2cad8-109">Puede seleccionar **cuentas en este directorio de la organización solo** para esta experiencia.</span><span class="sxs-lookup"><span data-stu-id="2cad8-109">You may select **Accounts in this organizational directory only** for this experience.</span></span>
1. <span data-ttu-id="2cad8-110">Deje la lista desplegable **URI de redirección** establecida en **Web**y proporcione un URI de redireccionamiento de `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="2cad8-110">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="2cad8-111">Deshabilite la casilla **permisos** > **conceder permisos de administrador a OpenID y offline_access** .</span><span class="sxs-lookup"><span data-stu-id="2cad8-111">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="2cad8-112">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="2cad8-112">Select **Register**.</span></span>

<span data-ttu-id="2cad8-113">En **autenticación** > **configuraciones de plataforma** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="2cad8-113">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="2cad8-114">Confirme que el **URI de redirección** de `https://localhost:5001/authentication/login-callback` está presente.</span><span class="sxs-lookup"><span data-stu-id="2cad8-114">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="2cad8-115">En **concesión implícita**, active las casillas de verificación de **tokens de acceso** y **tokens de identificador**.</span><span class="sxs-lookup"><span data-stu-id="2cad8-115">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="2cad8-116">Los valores predeterminados restantes de la aplicación son aceptables para esta experiencia.</span><span class="sxs-lookup"><span data-stu-id="2cad8-116">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="2cad8-117">Seleccione el botón **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="2cad8-117">Select the **Save** button.</span></span>

<span data-ttu-id="2cad8-118">Registre la siguiente información:</span><span class="sxs-lookup"><span data-stu-id="2cad8-118">Record the following information:</span></span>

* <span data-ttu-id="2cad8-119">IDENTIFICADOR de aplicación (ID. de cliente) (por ejemplo, `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="2cad8-119">Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="2cad8-120">IDENTIFICADOR de directorio (ID. de inquilino) (por ejemplo, `22222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="2cad8-120">Directory ID (Tenant ID) (for example, `22222222-2222-2222-2222-222222222222`)</span></span>

<span data-ttu-id="2cad8-121">Reemplace los marcadores de posición en el siguiente comando por la información registrada anteriormente y ejecute el comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="2cad8-121">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="2cad8-122">Para especificar la ubicación de salida, que crea una carpeta de proyecto si no existe, incluya la opción output en el comando con una ruta de acceso (por ejemplo, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="2cad8-122">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="2cad8-123">El nombre de la carpeta también se convierte en parte del nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="2cad8-123">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="2cad8-124">Paquete de autenticación</span><span class="sxs-lookup"><span data-stu-id="2cad8-124">Authentication package</span></span>

<span data-ttu-id="2cad8-125">Cuando se crea una aplicación para usar cuentas profesionales o educativas (`SingleOrg`), la aplicación recibe automáticamente una referencia de paquete para la [biblioteca de autenticación de Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="2cad8-125">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="2cad8-126">El paquete proporciona un conjunto de primitivas que ayudan a la aplicación a autenticar a los usuarios y obtener tokens para llamar a las API protegidas.</span><span class="sxs-lookup"><span data-stu-id="2cad8-126">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="2cad8-127">Si agrega la autenticación a una aplicación, agregue manualmente el paquete al archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2cad8-127">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="2cad8-128">Reemplace `{VERSION}` en la referencia de paquete anterior por la versión del paquete de `Microsoft.AspNetCore.Blazor.Templates` que se muestra en el artículo <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="2cad8-128">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="2cad8-129">El paquete de `Microsoft.Authentication.WebAssembly.Msal` agrega de manera transitiva el paquete de `Microsoft.AspNetCore.Components.WebAssembly.Authentication` a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2cad8-129">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="2cad8-130">Compatibilidad con el servicio de autenticación</span><span class="sxs-lookup"><span data-stu-id="2cad8-130">Authentication service support</span></span>

<span data-ttu-id="2cad8-131">La compatibilidad con la autenticación de usuarios se registra en el contenedor de servicios con el método de extensión `AddMsalAuthentication` proporcionado por el paquete de `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="2cad8-131">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="2cad8-132">Este método configura todos los servicios necesarios para que la aplicación interactúe con el proveedor de identidades (IP).</span><span class="sxs-lookup"><span data-stu-id="2cad8-132">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="2cad8-133">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="2cad8-133">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="2cad8-134">El método `AddMsalAuthentication` acepta una devolución de llamada para configurar los parámetros necesarios para autenticar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="2cad8-134">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="2cad8-135">Los valores necesarios para configurar la aplicación pueden obtenerse a partir de la configuración de AAD de Azure portal cuando se registra la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2cad8-135">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="2cad8-136">La plantilla Blazor webassembly no configura automáticamente la aplicación para solicitar un token de acceso para una API segura.</span><span class="sxs-lookup"><span data-stu-id="2cad8-136">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="2cad8-137">Para aprovisionar un token como parte del flujo de inicio de sesión, agregue el ámbito a los ámbitos de token de acceso predeterminados del `MsalProviderOptions`:</span><span class="sxs-lookup"><span data-stu-id="2cad8-137">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> <span data-ttu-id="2cad8-138">El ámbito del token de acceso predeterminado debe tener el formato `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (por ejemplo, `11111111-1111-1111-1111-111111111111/API.Access`).</span><span class="sxs-lookup"><span data-stu-id="2cad8-138">The default access token scope must be in the format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (for example, `11111111-1111-1111-1111-111111111111/API.Access`).</span></span> <span data-ttu-id="2cad8-139">Si se proporciona un esquema o esquema y un host a la configuración de ámbito (como se muestra en Azure portal), la *aplicación cliente* genera una excepción no controlada cuando recibe una respuesta *401 no autorizada* de la *aplicación de API de servidor*.</span><span class="sxs-lookup"><span data-stu-id="2cad8-139">If a scheme or scheme and host is provided to the scope setting (as shown in the Azure Portal), the *Client app* throws an unhandled exception when it receives a *401 Unauthorized* response from the *Server API app*.</span></span>

## <a name="index-page"></a><span data-ttu-id="2cad8-140">Página de índice</span><span class="sxs-lookup"><span data-stu-id="2cad8-140">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="2cad8-141">Componente de aplicación</span><span class="sxs-lookup"><span data-stu-id="2cad8-141">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="2cad8-142">Componente RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="2cad8-142">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="2cad8-143">Componente LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="2cad8-143">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="2cad8-144">Componente de autenticación</span><span class="sxs-lookup"><span data-stu-id="2cad8-144">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="2cad8-145">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2cad8-145">Additional resources</span></span>

* <xref:security/authentication/azure-active-directory/index>
