---
title: Proteja un ASP.NET Core Blazor aplicación hospedada en webassembly con Azure Active Directory B2C
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/22/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory-b2c
ms.openlocfilehash: 0083f179f85371d4751fb179194417681fc1a01d
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219069"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory-b2c"></a><span data-ttu-id="f5929-102">Proteja un ASP.NET Core Blazor aplicación hospedada en webassembly con Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="f5929-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory B2C</span></span>

<span data-ttu-id="f5929-103">Por [Javier Calvarro Nelson](https://github.com/javiercn) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f5929-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="f5929-104">En este artículo se describe cómo crear una aplicación independiente Blazor webassembly que usa [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="f5929-104">This article describes how to create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="f5929-105">Registrar aplicaciones en AAD B2C y crear una solución</span><span class="sxs-lookup"><span data-stu-id="f5929-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="f5929-106">Creación de un inquilino</span><span class="sxs-lookup"><span data-stu-id="f5929-106">Create a tenant</span></span>

<span data-ttu-id="f5929-107">Siga las instrucciones de [Tutorial: creación de un inquilino de Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-create-tenant) para crear un inquilino de AAD B2C y registre la siguiente información:</span><span class="sxs-lookup"><span data-stu-id="f5929-107">Follow the guidance in [Tutorial: Create an Azure Active Directory B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) to create an AAD B2C tenant and record the following information:</span></span>

* <span data-ttu-id="f5929-108">AAD B2C instancia (por ejemplo, `https://contoso.b2clogin.com/`, que incluye la barra diagonal final)</span><span class="sxs-lookup"><span data-stu-id="f5929-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span>
* <span data-ttu-id="f5929-109">AAD B2C dominio del inquilino (por ejemplo, `contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="f5929-109">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="f5929-110">Registrar una aplicación de API de servidor</span><span class="sxs-lookup"><span data-stu-id="f5929-110">Register a server API app</span></span>

<span data-ttu-id="f5929-111">Siga las instrucciones de [Tutorial: registro de una aplicación en Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) para registrar una aplicación de AAD para la *aplicación de API de servidor* en el área **Azure Active Directory** > **registros de aplicaciones** del Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="f5929-111">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="f5929-112">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="f5929-112">Select **New registration**.</span></span>
1. <span data-ttu-id="f5929-113">Proporcione un **nombre** para la aplicación (por ejemplo, **Blazor Server AAD B2C**).</span><span class="sxs-lookup"><span data-stu-id="f5929-113">Provide a **Name** for the app (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="f5929-114">En **tipos de cuenta compatibles**, seleccione **cuentas en cualquier directorio de la organización o cualquier proveedor de identidades. Para autenticar a los usuarios con Azure AD B2C.**</span><span class="sxs-lookup"><span data-stu-id="f5929-114">For **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span> <span data-ttu-id="f5929-115">(multiinquilino) para esta experiencia.</span><span class="sxs-lookup"><span data-stu-id="f5929-115">(multi-tenant) for this experience.</span></span>
1. <span data-ttu-id="f5929-116">La *aplicación de API de servidor* no requiere un **URI de redirección** en este escenario, por lo que deje la lista desplegable establecida en **Web** y no escriba un URI de redirección.</span><span class="sxs-lookup"><span data-stu-id="f5929-116">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="f5929-117">Confirme que **los permisos** > **conceder permisos de administrador a OpenID y offline_access** está habilitado.</span><span class="sxs-lookup"><span data-stu-id="f5929-117">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="f5929-118">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="f5929-118">Select **Register**.</span></span>

<span data-ttu-id="f5929-119">En **exponer una API**:</span><span class="sxs-lookup"><span data-stu-id="f5929-119">In **Expose an API**:</span></span>

1. <span data-ttu-id="f5929-120">Seleccione **Agregar un ámbito**.</span><span class="sxs-lookup"><span data-stu-id="f5929-120">Select **Add a scope**.</span></span>
1. <span data-ttu-id="f5929-121">Seleccione **Guardar y continuar**.</span><span class="sxs-lookup"><span data-stu-id="f5929-121">Select **Save and continue**.</span></span>
1. <span data-ttu-id="f5929-122">Proporcione un **nombre de ámbito** (por ejemplo, `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="f5929-122">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="f5929-123">Proporcione un **nombre para mostrar del consentimiento del administrador** (por ejemplo, `Access API`).</span><span class="sxs-lookup"><span data-stu-id="f5929-123">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="f5929-124">Proporcione una **Descripción del consentimiento del administrador** (por ejemplo, `Allows the app to access server app API endpoints.`).</span><span class="sxs-lookup"><span data-stu-id="f5929-124">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="f5929-125">Confirme que el **Estado** está establecido en **habilitado**.</span><span class="sxs-lookup"><span data-stu-id="f5929-125">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="f5929-126">Seleccione la opción **Agregar un ámbito**.</span><span class="sxs-lookup"><span data-stu-id="f5929-126">Select **Add scope**.</span></span>

<span data-ttu-id="f5929-127">Registre la siguiente información:</span><span class="sxs-lookup"><span data-stu-id="f5929-127">Record the following information:</span></span>

* <span data-ttu-id="f5929-128">*Aplicación de API de servidor* IDENTIFICADOR de aplicación (ID. de cliente) (por ejemplo, `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="f5929-128">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="f5929-129">IDENTIFICADOR de directorio (ID. de inquilino) (por ejemplo, `222222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="f5929-129">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="f5929-130">*Aplicación de API de servidor* URI de ID. de aplicación (por ejemplo, `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, el valor predeterminado del Azure Portal podría ser el ID. de cliente)</span><span class="sxs-lookup"><span data-stu-id="f5929-130">*Server API app* App ID URI (for example, `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, the Azure portal might default the value to the Client ID)</span></span>
* <span data-ttu-id="f5929-131">Ámbito predeterminado (por ejemplo, `API.Access`)</span><span class="sxs-lookup"><span data-stu-id="f5929-131">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="f5929-132">Registrar una aplicación de cliente</span><span class="sxs-lookup"><span data-stu-id="f5929-132">Register a client app</span></span>

<span data-ttu-id="f5929-133">Siga las instrucciones de [Tutorial: registro de una aplicación en Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) de nuevo para registrar una aplicación de AAD para la *aplicación cliente* en el área **Azure Active Directory** > **registros de aplicaciones** del Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="f5929-133">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) again to register an AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="f5929-134">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="f5929-134">Select **New registration**.</span></span>
1. <span data-ttu-id="f5929-135">Proporcione un **nombre** para la aplicación (por ejemplo, **Blazor cliente AAD B2C**).</span><span class="sxs-lookup"><span data-stu-id="f5929-135">Provide a **Name** for the app (for example, **Blazor Client AAD B2C**).</span></span>
1. <span data-ttu-id="f5929-136">En **tipos de cuenta compatibles**, seleccione **cuentas en cualquier directorio de la organización o cualquier proveedor de identidades. Para autenticar a los usuarios con Azure AD B2C.**</span><span class="sxs-lookup"><span data-stu-id="f5929-136">For **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span> <span data-ttu-id="f5929-137">(multiinquilino) para esta experiencia.</span><span class="sxs-lookup"><span data-stu-id="f5929-137">(multi-tenant) for this experience.</span></span>
1. <span data-ttu-id="f5929-138">Deje la lista desplegable **URI de redirección** establecida en **Web**y proporcione un URI de redireccionamiento de `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="f5929-138">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="f5929-139">Confirme que **los permisos** > **conceder permisos de administrador a OpenID y offline_access** está habilitado.</span><span class="sxs-lookup"><span data-stu-id="f5929-139">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="f5929-140">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="f5929-140">Select **Register**.</span></span>

<span data-ttu-id="f5929-141">En **autenticación** > **configuraciones de plataforma** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="f5929-141">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="f5929-142">Confirme que el **URI de redirección** de `https://localhost:5001/authentication/login-callback` está presente.</span><span class="sxs-lookup"><span data-stu-id="f5929-142">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="f5929-143">En **concesión implícita**, active las casillas de verificación de **tokens de acceso** y **tokens de identificador**.</span><span class="sxs-lookup"><span data-stu-id="f5929-143">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="f5929-144">Los valores predeterminados restantes de la aplicación son aceptables para esta experiencia.</span><span class="sxs-lookup"><span data-stu-id="f5929-144">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="f5929-145">Seleccione el botón **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="f5929-145">Select the **Save** button.</span></span>

<span data-ttu-id="f5929-146">En **permisos de API**:</span><span class="sxs-lookup"><span data-stu-id="f5929-146">In **API permissions**:</span></span>

1. <span data-ttu-id="f5929-147">Confirme que la aplicación tiene **Microsoft Graph** > **usuario.** permiso de lectura.</span><span class="sxs-lookup"><span data-stu-id="f5929-147">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="f5929-148">Seleccione **Agregar un permiso** seguido de **mis API**.</span><span class="sxs-lookup"><span data-stu-id="f5929-148">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="f5929-149">Seleccione la *aplicación de API de servidor* en la columna **nombre** (por ejemplo, **Blazor Server AAD B2C**).</span><span class="sxs-lookup"><span data-stu-id="f5929-149">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="f5929-150">Abra la lista de **API** .</span><span class="sxs-lookup"><span data-stu-id="f5929-150">Open the **API** list.</span></span>
1. <span data-ttu-id="f5929-151">Habilite el acceso a la API (por ejemplo, `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="f5929-151">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="f5929-152">Seleccione **Agregar permisos**.</span><span class="sxs-lookup"><span data-stu-id="f5929-152">Select **Add permissions**.</span></span>
1. <span data-ttu-id="f5929-153">Seleccione el botón **conceder contenido de administración para {nombre de inquilino}** .</span><span class="sxs-lookup"><span data-stu-id="f5929-153">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="f5929-154">Seleccione **Sí** para confirmar la acción.</span><span class="sxs-lookup"><span data-stu-id="f5929-154">Select **Yes** to confirm.</span></span>

<span data-ttu-id="f5929-155">En **Home** > **Azure ad B2C** > los **flujos de usuario**:</span><span class="sxs-lookup"><span data-stu-id="f5929-155">In **Home** > **Azure AD B2C** > **User flows**:</span></span>

[<span data-ttu-id="f5929-156">Creación de un flujo de usuario de inicio de sesión e inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="f5929-156">Create a sign-up and sign-in user flow</span></span>](/azure/active-directory-b2c/tutorial-create-user-flows)

<span data-ttu-id="f5929-157">Como mínimo, seleccione el atributo de usuario **notificaciones de aplicación** > **nombre para mostrar** para rellenar el `context.User.Identity.Name` en el componente `LoginDisplay` (*Shared/LoginDisplay. Razor*).</span><span class="sxs-lookup"><span data-stu-id="f5929-157">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

<span data-ttu-id="f5929-158">Registre la siguiente información:</span><span class="sxs-lookup"><span data-stu-id="f5929-158">Record the following information:</span></span>

* <span data-ttu-id="f5929-159">Registre el identificador de aplicación de la aplicación *cliente* (identificador de cliente) (por ejemplo, `33333333-3333-3333-3333-333333333333`).</span><span class="sxs-lookup"><span data-stu-id="f5929-159">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>
* <span data-ttu-id="f5929-160">Registre el nombre del flujo de usuario de inicio de sesión y de registro creado para la aplicación (por ejemplo, `B2C_1_signupsignin`).</span><span class="sxs-lookup"><span data-stu-id="f5929-160">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="f5929-161">Creación de la aplicación</span><span class="sxs-lookup"><span data-stu-id="f5929-161">Create the app</span></span>

<span data-ttu-id="f5929-162">Reemplace los marcadores de posición en el siguiente comando por la información registrada anteriormente y ejecute el comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="f5929-162">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho -ssp "{SIGN UP OR SIGN IN POLICY}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="f5929-163">Para especificar la ubicación de salida, que crea una carpeta de proyecto si no existe, incluya la opción output en el comando con una ruta de acceso (por ejemplo, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="f5929-163">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="f5929-164">El nombre de la carpeta también se convierte en parte del nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f5929-164">The folder name also becomes part of the project's name.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="f5929-165">Configuración de la aplicación de servidor</span><span class="sxs-lookup"><span data-stu-id="f5929-165">Server app configuration</span></span>

<span data-ttu-id="f5929-166">*Esta sección pertenece a la aplicación de **servidor** de la solución.*</span><span class="sxs-lookup"><span data-stu-id="f5929-166">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="f5929-167">Paquete de autenticación</span><span class="sxs-lookup"><span data-stu-id="f5929-167">Authentication package</span></span>

<span data-ttu-id="f5929-168">La compatibilidad para autenticar y autorizar llamadas a ASP.NET Core API Web la proporciona el `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span><span class="sxs-lookup"><span data-stu-id="f5929-168">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="f5929-169">Compatibilidad con el servicio de autenticación</span><span class="sxs-lookup"><span data-stu-id="f5929-169">Authentication service support</span></span>

<span data-ttu-id="f5929-170">El método `AddAuthentication` configura los servicios de autenticación dentro de la aplicación y configura el controlador de portador JWT como el método de autenticación predeterminado.</span><span class="sxs-lookup"><span data-stu-id="f5929-170">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="f5929-171">El método `AddAzureADBearer` configura los parámetros específicos en el controlador de portador JWT necesario para validar los tokens emitidos por el Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="f5929-171">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="f5929-172">`UseAuthentication` y `UseAuthorization` Asegúrese de que:</span><span class="sxs-lookup"><span data-stu-id="f5929-172">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="f5929-173">La aplicación intenta analizar y validar los tokens en las solicitudes entrantes.</span><span class="sxs-lookup"><span data-stu-id="f5929-173">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="f5929-174">Se produce un error en cualquier solicitud que intente obtener acceso a un recurso protegido sin credenciales adecuadas.</span><span class="sxs-lookup"><span data-stu-id="f5929-174">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a><span data-ttu-id="f5929-175">Configuración de la aplicación</span><span class="sxs-lookup"><span data-stu-id="f5929-175">App settings</span></span>

<span data-ttu-id="f5929-176">El archivo *appSettings. JSON* contiene las opciones para configurar el controlador de portador JWT que se usa para validar los tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="f5929-176">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://{ORGANIZATION}.b2clogin.com/",
    "ClientId": "{API CLIENT ID}",
    "Domain": "{DOMAIN}",
    "SignUpSignInPolicyId": "{SIGN UP OR SIGN IN POLICY}"
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="f5929-177">Controlador de WeatherForecast</span><span class="sxs-lookup"><span data-stu-id="f5929-177">WeatherForecast controller</span></span>

<span data-ttu-id="f5929-178">El controlador WeatherForecast (*Controllers/WeatherForecastController. CS*) expone una API protegida con el `[Authorize]` atributo que se aplica al controlador.</span><span class="sxs-lookup"><span data-stu-id="f5929-178">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="f5929-179">Es **importante** comprender esto:</span><span class="sxs-lookup"><span data-stu-id="f5929-179">It's **important** to understand that:</span></span>

* <span data-ttu-id="f5929-180">El `[Authorize]` atributo de este controlador de API es lo único que protege esta API frente al acceso no autorizado.</span><span class="sxs-lookup"><span data-stu-id="f5929-180">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="f5929-181">El atributo `[Authorize]` usado en la aplicación webassembly de Blazor solo sirve como sugerencia a la aplicación de que el usuario debe estar autorizado para que la aplicación funcione correctamente.</span><span class="sxs-lookup"><span data-stu-id="f5929-181">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="f5929-182">Configuración de la aplicación cliente</span><span class="sxs-lookup"><span data-stu-id="f5929-182">Client app configuration</span></span>

<span data-ttu-id="f5929-183">*Esta sección pertenece a la aplicación **cliente** de la solución.*</span><span class="sxs-lookup"><span data-stu-id="f5929-183">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="f5929-184">Paquete de autenticación</span><span class="sxs-lookup"><span data-stu-id="f5929-184">Authentication package</span></span>

<span data-ttu-id="f5929-185">Cuando se crea una aplicación para usar una cuenta de B2C individual (`IndividualB2C`), la aplicación recibe automáticamente una referencia de paquete para la [biblioteca de autenticación de Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="f5929-185">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="f5929-186">El paquete proporciona un conjunto de primitivas que ayudan a la aplicación a autenticar a los usuarios y obtener tokens para llamar a las API protegidas.</span><span class="sxs-lookup"><span data-stu-id="f5929-186">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="f5929-187">Si agrega la autenticación a una aplicación, agregue manualmente el paquete al archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="f5929-187">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="f5929-188">Reemplace `{VERSION}` en la referencia de paquete anterior por la versión del paquete de `Microsoft.AspNetCore.Blazor.Templates` que se muestra en el artículo <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="f5929-188">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="f5929-189">El paquete de `Microsoft.Authentication.WebAssembly.Msal` agrega de manera transitiva el paquete de `Microsoft.AspNetCore.Components.WebAssembly.Authentication` a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5929-189">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="f5929-190">Compatibilidad con el servicio de autenticación</span><span class="sxs-lookup"><span data-stu-id="f5929-190">Authentication service support</span></span>

<span data-ttu-id="f5929-191">La compatibilidad con la autenticación de usuarios se registra en el contenedor de servicios con el método de extensión `AddMsalAuthentication` proporcionado por el paquete de `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="f5929-191">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="f5929-192">Este método configura todos los servicios necesarios para que la aplicación interactúe con el proveedor de identidades (IP).</span><span class="sxs-lookup"><span data-stu-id="f5929-192">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="f5929-193">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="f5929-193">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{APP ID URI}/{DEFAULT SCOPE}");
});
```

<span data-ttu-id="f5929-194">El método `AddMsalAuthentication` acepta una devolución de llamada para configurar los parámetros necesarios para autenticar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5929-194">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="f5929-195">Los valores necesarios para configurar la aplicación pueden obtenerse a partir de la configuración de AAD de Azure portal cuando se registra la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5929-195">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="f5929-196">La plantilla Blazor webassembly configura automáticamente la aplicación para solicitar un token de acceso para una API segura para el ámbito predeterminado proporcionado al comando `dotnet new` (`{APP ID URI}/{DEFAULT SCOPE}`).</span><span class="sxs-lookup"><span data-stu-id="f5929-196">The Blazor WebAssembly template automatically configures the app to request an access token for a secure API for the default scope provided to the `dotnet new` command (`{APP ID URI}/{DEFAULT SCOPE}`).</span></span>

<span data-ttu-id="f5929-197">Los ámbitos de token de acceso predeterminados representan la lista de ámbitos de token de acceso que son:</span><span class="sxs-lookup"><span data-stu-id="f5929-197">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="f5929-198">Se incluye de forma predeterminada en la solicitud de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="f5929-198">Included by default in the sign in request.</span></span>
* <span data-ttu-id="f5929-199">Se usa para aprovisionar un token de acceso inmediatamente después de la autenticación.</span><span class="sxs-lookup"><span data-stu-id="f5929-199">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="f5929-200">Todos los ámbitos deben pertenecer a la misma aplicación por reglas de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f5929-200">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="f5929-201">Se pueden agregar ámbitos adicionales para las aplicaciones de API adicionales según sea necesario:</span><span class="sxs-lookup"><span data-stu-id="f5929-201">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{APP ID URI}/{SCOPE}");
});
```

### <a name="index-page"></a><span data-ttu-id="f5929-202">Página de índice</span><span class="sxs-lookup"><span data-stu-id="f5929-202">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a><span data-ttu-id="f5929-203">Componente de aplicación</span><span class="sxs-lookup"><span data-stu-id="f5929-203">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="f5929-204">Componente RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="f5929-204">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="f5929-205">Componente LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="f5929-205">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="f5929-206">Componente de autenticación</span><span class="sxs-lookup"><span data-stu-id="f5929-206">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="f5929-207">Componente FetchData</span><span class="sxs-lookup"><span data-stu-id="f5929-207">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="f5929-208">Ejecución la aplicación</span><span class="sxs-lookup"><span data-stu-id="f5929-208">Run the app</span></span>

<span data-ttu-id="f5929-209">Ejecute la aplicación desde el proyecto de servidor.</span><span class="sxs-lookup"><span data-stu-id="f5929-209">Run the app from the Server project.</span></span> <span data-ttu-id="f5929-210">Al usar Visual Studio, seleccione el proyecto de servidor en **Explorador de soluciones** y seleccione el botón **Ejecutar** en la barra de herramientas o inicie la aplicación en el menú **depurar** .</span><span class="sxs-lookup"><span data-stu-id="f5929-210">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="f5929-211">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f5929-211">Additional resources</span></span>

* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="f5929-212">Tutorial: Creación de un inquilino de Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="f5929-212">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
