---
title: Proteja un ASP.NET Core Blazor aplicación independiente webassembly con Azure Active Directory B2C
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: bb03ef1e6d216cfc06e2b91919c64f92f2ef634e
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219277"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a><span data-ttu-id="9b9a9-102">Proteja un ASP.NET Core Blazor aplicación independiente webassembly con Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="9b9a9-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory B2C</span></span>

<span data-ttu-id="9b9a9-103">Por [Javier Calvarro Nelson](https://github.com/javiercn) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9b9a9-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="9b9a9-104">Para crear una aplicación independiente Blazor webassembly que usa [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) para la autenticación:</span><span class="sxs-lookup"><span data-stu-id="9b9a9-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication:</span></span>

1. <span data-ttu-id="9b9a9-105">Siga las instrucciones de los temas siguientes para crear un inquilino y registrar una aplicación web en Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="9b9a9-105">Follow the guidance in the following topics to create a tenant and register a web app in the Azure Portal:</span></span>

   * <span data-ttu-id="9b9a9-106">[Cree un inquilino de AAD B2C](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; registre la información siguiente:</span><span class="sxs-lookup"><span data-stu-id="9b9a9-106">[Create an AAD B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; Record the following information:</span></span>

     <span data-ttu-id="9b9a9-107">1\.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-107">1\.</span></span> <span data-ttu-id="9b9a9-108">AAD B2C instancia (por ejemplo, `https://contoso.b2clogin.com/`, que incluye la barra diagonal final)</span><span class="sxs-lookup"><span data-stu-id="9b9a9-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span><br>
     <span data-ttu-id="9b9a9-109">2\.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-109">2\.</span></span> <span data-ttu-id="9b9a9-110">AAD B2C dominio del inquilino (por ejemplo, `contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="9b9a9-110">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

   * <span data-ttu-id="9b9a9-111">[Registrar una aplicación web](/azure/active-directory-b2c/tutorial-register-applications) &ndash; realizar las siguientes selecciones durante el registro de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="9b9a9-111">[Register a web application](/azure/active-directory-b2c/tutorial-register-applications) &ndash; Make the following selections during app registration:</span></span>

     <span data-ttu-id="9b9a9-112">1\.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-112">1\.</span></span> <span data-ttu-id="9b9a9-113">Establezca **aplicación web/API Web** en **sí**.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-113">Set **Web App / Web API** to **Yes**.</span></span><br>
     <span data-ttu-id="9b9a9-114">2\.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-114">2\.</span></span> <span data-ttu-id="9b9a9-115">Establezca **permitir flujo implícito** en **sí**.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-115">Set **Allow implicit flow** to **Yes**.</span></span><br>
     <span data-ttu-id="9b9a9-116">3\.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-116">3\.</span></span> <span data-ttu-id="9b9a9-117">Agregue una **dirección URL de respuesta** de `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-117">Add a **Reply URL** of `https://localhost:5001/authentication/login-callback`.</span></span>

     <span data-ttu-id="9b9a9-118">Registre el identificador de aplicación (ID. de cliente) (por ejemplo, `11111111-1111-1111-1111-111111111111`).</span><span class="sxs-lookup"><span data-stu-id="9b9a9-118">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

   * <span data-ttu-id="9b9a9-119">[Cree flujos de usuario](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash; crear un flujo de usuario de inicio de sesión e inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-119">[Create user flows](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash; Create a sign-up and sign-in user flow.</span></span>

     <span data-ttu-id="9b9a9-120">Como mínimo, seleccione el atributo de usuario **notificaciones de aplicación** > **nombre para mostrar** para rellenar el `context.User.Identity.Name` en el componente `LoginDisplay` (*Shared/LoginDisplay. Razor*).</span><span class="sxs-lookup"><span data-stu-id="9b9a9-120">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

     <span data-ttu-id="9b9a9-121">Registre el nombre del flujo de usuario de inicio de sesión y de registro creado para la aplicación (por ejemplo, `B2C_1_signupsignin`).</span><span class="sxs-lookup"><span data-stu-id="9b9a9-121">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

1. <span data-ttu-id="9b9a9-122">Reemplace los marcadores de posición en el siguiente comando por la información registrada anteriormente y ejecute el comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="9b9a9-122">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   <span data-ttu-id="9b9a9-123">Para especificar la ubicación de salida, que crea una carpeta de proyecto si no existe, incluya la opción output en el comando con una ruta de acceso (por ejemplo, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="9b9a9-123">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="9b9a9-124">El nombre de la carpeta también se convierte en parte del nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-124">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="9b9a9-125">Paquete de autenticación</span><span class="sxs-lookup"><span data-stu-id="9b9a9-125">Authentication package</span></span>

<span data-ttu-id="9b9a9-126">Cuando se crea una aplicación para usar una cuenta de B2C individual (`IndividualB2C`), la aplicación recibe automáticamente una referencia de paquete para la [biblioteca de autenticación de Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="9b9a9-126">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="9b9a9-127">El paquete proporciona un conjunto de primitivas que ayudan a la aplicación a autenticar a los usuarios y obtener tokens para llamar a las API protegidas.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-127">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="9b9a9-128">Si agrega la autenticación a una aplicación, agregue manualmente el paquete al archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="9b9a9-128">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="9b9a9-129">Reemplace `{VERSION}` en la referencia de paquete anterior por la versión del paquete de `Microsoft.AspNetCore.Blazor.Templates` que se muestra en el artículo <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-129">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="9b9a9-130">El paquete de `Microsoft.Authentication.WebAssembly.Msal` agrega de manera transitiva el paquete de `Microsoft.AspNetCore.Components.WebAssembly.Authentication` a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-130">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="9b9a9-131">Compatibilidad con el servicio de autenticación</span><span class="sxs-lookup"><span data-stu-id="9b9a9-131">Authentication service support</span></span>

<span data-ttu-id="9b9a9-132">La compatibilidad con la autenticación de usuarios se registra en el contenedor de servicios con el método de extensión `AddMsalAuthentication` proporcionado por el paquete de `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-132">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="9b9a9-133">Este método configura todos los servicios necesarios para que la aplicación interactúe con el proveedor de identidades (IP).</span><span class="sxs-lookup"><span data-stu-id="9b9a9-133">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="9b9a9-134">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9b9a9-134">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
});
```

<span data-ttu-id="9b9a9-135">El método `AddMsalAuthentication` acepta una devolución de llamada para configurar los parámetros necesarios para autenticar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-135">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="9b9a9-136">Los valores necesarios para configurar la aplicación pueden obtenerse a partir de la configuración de AAD de Azure portal cuando se registra la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-136">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="9b9a9-137">La plantilla Blazor webassembly no configura automáticamente la aplicación para solicitar un token de acceso para una API segura.</span><span class="sxs-lookup"><span data-stu-id="9b9a9-137">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="9b9a9-138">Para aprovisionar un token como parte del flujo de inicio de sesión, agregue el ámbito a los ámbitos de token de acceso predeterminados del `MsalProviderOptions`:</span><span class="sxs-lookup"><span data-stu-id="9b9a9-138">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{API ID URI}/{SCOPE}");
});
```

## <a name="index-page"></a><span data-ttu-id="9b9a9-139">Página de índice</span><span class="sxs-lookup"><span data-stu-id="9b9a9-139">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="9b9a9-140">Componente de aplicación</span><span class="sxs-lookup"><span data-stu-id="9b9a9-140">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="9b9a9-141">Componente RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="9b9a9-141">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="9b9a9-142">Componente LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="9b9a9-142">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="9b9a9-143">Componente de autenticación</span><span class="sxs-lookup"><span data-stu-id="9b9a9-143">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="9b9a9-144">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9b9a9-144">Additional resources</span></span>

* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="9b9a9-145">Tutorial: Creación de un inquilino de Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="9b9a9-145">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
