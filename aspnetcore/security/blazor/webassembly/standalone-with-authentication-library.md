---
title: Protección de un ASP.NET Core Blazor aplicación independiente webassembly con la biblioteca de autenticación
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: ea50d94835b044f9c3d6a0561868f081d32cb62a
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219015"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a><span data-ttu-id="5b35c-102">Protección de un ASP.NET Core Blazor aplicación independiente webassembly con la biblioteca de autenticación</span><span class="sxs-lookup"><span data-stu-id="5b35c-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with the Authentication library</span></span>

<span data-ttu-id="5b35c-103">Por [Javier Calvarro Nelson](https://github.com/javiercn) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5b35c-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="5b35c-104">*Por Azure Active Directory (AAD) y Azure Active Directory B2C (AAD B2C), no siga las instrucciones de este tema. Vea los temas de AAD y AAD B2C en este nodo de tabla de contenido.*</span><span class="sxs-lookup"><span data-stu-id="5b35c-104">*For Azure Active Directory (AAD) and Azure Active Directory B2C (AAD B2C), don't follow the guidance in this topic. See the AAD and AAD B2C topics in this table of contents node.*</span></span>

<span data-ttu-id="5b35c-105">Para crear una Blazor aplicación independiente webassembly que use `Microsoft.AspNetCore.Components.WebAssembly.Authentication` Library, ejecute el siguiente comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="5b35c-105">To create a Blazor WebAssembly standalone app that uses `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library, execute the following command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual
```

<span data-ttu-id="5b35c-106">Para especificar la ubicación de salida, que crea una carpeta de proyecto si no existe, incluya la opción output en el comando con una ruta de acceso (por ejemplo, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="5b35c-106">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="5b35c-107">El nombre de la carpeta también se convierte en parte del nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="5b35c-107">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="5b35c-108">En Visual Studio, [cree una aplicación Blazor Webassembly](xref:blazor/get-started).</span><span class="sxs-lookup"><span data-stu-id="5b35c-108">In Visual Studio, [create a Blazor WebAssembly app](xref:blazor/get-started).</span></span> <span data-ttu-id="5b35c-109">Establezca la **autenticación** en **cuentas de usuario individuales** con la opción **almacenar cuentas de usuario en la aplicación** .</span><span class="sxs-lookup"><span data-stu-id="5b35c-109">Set **Authentication** to **Individual User Accounts** with the **Store user accounts in-app** option.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="5b35c-110">Paquete de autenticación</span><span class="sxs-lookup"><span data-stu-id="5b35c-110">Authentication package</span></span>

<span data-ttu-id="5b35c-111">Cuando se crea una aplicación para usar cuentas de usuario individuales, la aplicación recibe automáticamente una referencia de paquete para el paquete de `Microsoft.AspNetCore.Components.WebAssembly.Authentication` en el archivo de proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5b35c-111">When an app is created to use Individual User Accounts, the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="5b35c-112">El paquete proporciona un conjunto de primitivas que ayudan a la aplicación a autenticar a los usuarios y obtener tokens para llamar a las API protegidas.</span><span class="sxs-lookup"><span data-stu-id="5b35c-112">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="5b35c-113">Si agrega la autenticación a una aplicación, agregue manualmente el paquete al archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="5b35c-113">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="5b35c-114">Reemplace `{VERSION}` en la referencia de paquete anterior por la versión del paquete de `Microsoft.AspNetCore.Blazor.Templates` que se muestra en el artículo <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="5b35c-114">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="5b35c-115">Compatibilidad con el servicio de autenticación</span><span class="sxs-lookup"><span data-stu-id="5b35c-115">Authentication service support</span></span>

<span data-ttu-id="5b35c-116">La compatibilidad con la autenticación de usuarios se registra en el contenedor de servicios con el método de extensión `AddOidcAuthentication` proporcionado por el paquete de `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="5b35c-116">Support for authenticating users is registered in the service container with the `AddOidcAuthentication` extension method provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="5b35c-117">Este método configura todos los servicios necesarios para que la aplicación interactúe con el proveedor de identidades (IP).</span><span class="sxs-lookup"><span data-stu-id="5b35c-117">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="5b35c-118">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="5b35c-118">*Program.cs*:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="5b35c-119">La compatibilidad con la autenticación para aplicaciones independientes se ofrece mediante Open ID Connect (OIDC).</span><span class="sxs-lookup"><span data-stu-id="5b35c-119">Authentication support for standalone apps is offered using Open ID Connect (OIDC).</span></span> <span data-ttu-id="5b35c-120">El método `AddOidcAuthentication` acepta una devolución de llamada para configurar los parámetros necesarios para autenticar una aplicación mediante OIDC.</span><span class="sxs-lookup"><span data-stu-id="5b35c-120">The `AddOidcAuthentication` method accepts a callback to configure the parameters required to authenticate an app using OIDC.</span></span> <span data-ttu-id="5b35c-121">Los valores necesarios para configurar la aplicación pueden obtenerse a partir de la dirección IP compatible con OIDC.</span><span class="sxs-lookup"><span data-stu-id="5b35c-121">The values required for configuring the app can be obtained from the OIDC-compliant IP.</span></span> <span data-ttu-id="5b35c-122">Obtenga los valores al registrar la aplicación, que suele producirse en su portal en línea.</span><span class="sxs-lookup"><span data-stu-id="5b35c-122">Obtain the values when you register the app, which typically occurs in their online portal.</span></span>

## <a name="index-page"></a><span data-ttu-id="5b35c-123">Página de índice</span><span class="sxs-lookup"><span data-stu-id="5b35c-123">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

## <a name="app-component"></a><span data-ttu-id="5b35c-124">Componente de aplicación</span><span class="sxs-lookup"><span data-stu-id="5b35c-124">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="5b35c-125">Componente RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="5b35c-125">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="5b35c-126">Componente LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="5b35c-126">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="5b35c-127">Componente de autenticación</span><span class="sxs-lookup"><span data-stu-id="5b35c-127">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
