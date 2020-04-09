---
title: Proteja una Blazor aplicación independiente de ASP.NET Core WebAssembly con la biblioteca de autenticación
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: 893fff10df37e1c2be549604f4cb83cd20049108
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977046"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a><span data-ttu-id="e4d46-102">Proteja una Blazor aplicación independiente de ASP.NET Core WebAssembly con la biblioteca de autenticación</span><span class="sxs-lookup"><span data-stu-id="e4d46-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with the Authentication library</span></span>

<span data-ttu-id="e4d46-103">Por [Javier Calvarro Nelson](https://github.com/javiercn) y Luke [Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e4d46-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="e4d46-104">*Para Azure Active Directory (AAD) y Azure Active Directory B2C (AAD B2C), no siga las instrucciones de este tema. Consulte los temas AAD y AAD B2C en esta tabla de nodo de contenido.*</span><span class="sxs-lookup"><span data-stu-id="e4d46-104">*For Azure Active Directory (AAD) and Azure Active Directory B2C (AAD B2C), don't follow the guidance in this topic. See the AAD and AAD B2C topics in this table of contents node.*</span></span>

<span data-ttu-id="e4d46-105">Para crear Blazor una aplicación independiente `Microsoft.AspNetCore.Components.WebAssembly.Authentication` de WebAssembly que utilice la biblioteca, ejecute el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="e4d46-105">To create a Blazor WebAssembly standalone app that uses `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library, execute the following command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual
```

<span data-ttu-id="e4d46-106">Para especificar la ubicación de salida, que crea una carpeta de proyecto si no existe, `-o BlazorSample`incluya la opción de salida en el comando con una ruta de acceso (por ejemplo, ).</span><span class="sxs-lookup"><span data-stu-id="e4d46-106">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="e4d46-107">El nombre de la carpeta también pasa a formar parte del nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="e4d46-107">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="e4d46-108">En Visual Studio, [cree una Blazor aplicación WebAssembly](xref:blazor/get-started).</span><span class="sxs-lookup"><span data-stu-id="e4d46-108">In Visual Studio, [create a Blazor WebAssembly app](xref:blazor/get-started).</span></span> <span data-ttu-id="e4d46-109">Establezca **Autenticación** en Cuentas de **usuario individuales** con la opción **Almacenar cuentas de usuario en la aplicación.**</span><span class="sxs-lookup"><span data-stu-id="e4d46-109">Set **Authentication** to **Individual User Accounts** with the **Store user accounts in-app** option.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="e4d46-110">Paquete de autenticación</span><span class="sxs-lookup"><span data-stu-id="e4d46-110">Authentication package</span></span>

<span data-ttu-id="e4d46-111">Cuando se crea una aplicación para usar cuentas de usuario `Microsoft.AspNetCore.Components.WebAssembly.Authentication` individuales, la aplicación recibe automáticamente una referencia de paquete para el paquete en el archivo de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e4d46-111">When an app is created to use Individual User Accounts, the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="e4d46-112">El paquete proporciona un conjunto de primitivas que ayudan a la aplicación a autenticar a los usuarios y obtener tokens para llamar a las API protegidas.</span><span class="sxs-lookup"><span data-stu-id="e4d46-112">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="e4d46-113">Si agrega autenticación a una aplicación, agregue manualmente el paquete al archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="e4d46-113">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="e4d46-114">Reemplace `{VERSION}` en la referencia del paquete `Microsoft.AspNetCore.Blazor.Templates` anterior por <xref:blazor/get-started> la versión del paquete que se muestra en el artículo.</span><span class="sxs-lookup"><span data-stu-id="e4d46-114">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="e4d46-115">Soporte de servicio de autenticación</span><span class="sxs-lookup"><span data-stu-id="e4d46-115">Authentication service support</span></span>

<span data-ttu-id="e4d46-116">La compatibilidad con la autenticación de usuarios `AddOidcAuthentication` se registra `Microsoft.AspNetCore.Components.WebAssembly.Authentication` en el contenedor de servicios con el método de extensión proporcionado por el paquete.</span><span class="sxs-lookup"><span data-stu-id="e4d46-116">Support for authenticating users is registered in the service container with the `AddOidcAuthentication` extension method provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="e4d46-117">Este método configura todos los servicios necesarios para que la aplicación interactúe con el proveedor de identidades (IP).</span><span class="sxs-lookup"><span data-stu-id="e4d46-117">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="e4d46-118">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="e4d46-118">*Program.cs*:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="e4d46-119">La compatibilidad de autenticación para aplicaciones independientes se ofrece mediante Open ID Connect (OIDC).</span><span class="sxs-lookup"><span data-stu-id="e4d46-119">Authentication support for standalone apps is offered using Open ID Connect (OIDC).</span></span> <span data-ttu-id="e4d46-120">El `AddOidcAuthentication` método acepta una devolución de llamada para configurar los parámetros necesarios para autenticar una aplicación mediante OIDC.</span><span class="sxs-lookup"><span data-stu-id="e4d46-120">The `AddOidcAuthentication` method accepts a callback to configure the parameters required to authenticate an app using OIDC.</span></span> <span data-ttu-id="e4d46-121">Los valores necesarios para configurar la aplicación se pueden obtener de la dirección IP compatible con OIDC.</span><span class="sxs-lookup"><span data-stu-id="e4d46-121">The values required for configuring the app can be obtained from the OIDC-compliant IP.</span></span> <span data-ttu-id="e4d46-122">Obtenga los valores al registrar la aplicación, que normalmente se produce en su portal en línea.</span><span class="sxs-lookup"><span data-stu-id="e4d46-122">Obtain the values when you register the app, which typically occurs in their online portal.</span></span>

## <a name="access-token-scopes"></a><span data-ttu-id="e4d46-123">Acceso a ámbitos de token</span><span class="sxs-lookup"><span data-stu-id="e4d46-123">Access token scopes</span></span>

<span data-ttu-id="e4d46-124">La Blazor plantilla WebAssembly no configura automáticamente la aplicación para solicitar un token de acceso para una API segura.</span><span class="sxs-lookup"><span data-stu-id="e4d46-124">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="e4d46-125">Para aprovisionar un token como parte del flujo de inicio de sesión, agregue el ámbito a los ámbitos de token predeterminados `OidcProviderOptions`de:</span><span class="sxs-lookup"><span data-stu-id="e4d46-125">To provision a token as part of the sign-in flow, add the scope to the default token scopes of the `OidcProviderOptions`:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="e4d46-126">Si Azure Portal proporciona un URI de ámbito y **la aplicación produce una excepción no controlada** cuando recibe una respuesta *401 Unauthorized* de la API, intente usar un URI de ámbito que no incluya el esquema y el host.</span><span class="sxs-lookup"><span data-stu-id="e4d46-126">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="e4d46-127">Por ejemplo, Azure Portal puede proporcionar uno de los siguientes formatos de URI de ámbito:</span><span class="sxs-lookup"><span data-stu-id="e4d46-127">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="e4d46-128">Proporcione el URI de ámbito sin el esquema y el host:</span><span class="sxs-lookup"><span data-stu-id="e4d46-128">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="e4d46-129">Para obtener más información, vea <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span><span class="sxs-lookup"><span data-stu-id="e4d46-129">For more information, see <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span></span>

## <a name="imports-file"></a><span data-ttu-id="e4d46-130">Archivo de importaciones</span><span class="sxs-lookup"><span data-stu-id="e4d46-130">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="e4d46-131">Página de índice</span><span class="sxs-lookup"><span data-stu-id="e4d46-131">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

## <a name="app-component"></a><span data-ttu-id="e4d46-132">Componente de la aplicación</span><span class="sxs-lookup"><span data-stu-id="e4d46-132">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="e4d46-133">Componente RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="e4d46-133">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="e4d46-134">Componente LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="e4d46-134">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="e4d46-135">Componente de autenticación</span><span class="sxs-lookup"><span data-stu-id="e4d46-135">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="e4d46-136">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e4d46-136">Additional resources</span></span>

* [<span data-ttu-id="e4d46-137">Solicitar tokens de acceso adicionales</span><span class="sxs-lookup"><span data-stu-id="e4d46-137">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
