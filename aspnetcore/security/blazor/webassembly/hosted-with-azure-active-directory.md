---
title: Proteja un ASP.NET Core Blazor aplicación hospedada en webassembly con Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: 0803e436d66ef7df3c68739e674a8652fde11166
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083666"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a><span data-ttu-id="e90f9-102">Proteja un ASP.NET Core Blazor aplicación hospedada en webassembly con Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e90f9-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory</span></span>

<span data-ttu-id="e90f9-103">Por [Javier Calvarro Nelson](https://github.com/javiercn) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e90f9-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]



<span data-ttu-id="e90f9-104">En este artículo se describe cómo crear una [aplicación hospedada en WebassemblyBlazor](xref:blazor/hosting-models#blazor-webassembly) que usa [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="e90f9-104">This article describes how to create a [Blazor WebAssembly hosted app](xref:blazor/hosting-models#blazor-webassembly) that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="e90f9-105">Registrar aplicaciones en AAD B2C y crear una solución</span><span class="sxs-lookup"><span data-stu-id="e90f9-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="e90f9-106">Creación de un inquilino</span><span class="sxs-lookup"><span data-stu-id="e90f9-106">Create a tenant</span></span>

<span data-ttu-id="e90f9-107">Siga las instrucciones de [Inicio rápido: configuración de un inquilino](/azure/active-directory/develop/quickstart-create-new-tenant) para crear un inquilino en AAD.</span><span class="sxs-lookup"><span data-stu-id="e90f9-107">Follow the guidance in [Quickstart: Set up a tenant](/azure/active-directory/develop/quickstart-create-new-tenant) to create a tenant in AAD.</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="e90f9-108">Registrar una aplicación de API de servidor</span><span class="sxs-lookup"><span data-stu-id="e90f9-108">Register a server API app</span></span>

<span data-ttu-id="e90f9-109">Siga las instrucciones de [Inicio rápido: registro de una aplicación con la plataforma de Microsoft Identity y los](/azure/active-directory/develop/quickstart-register-app) temas de Azure AAD subsiguientes para registrar una aplicación de AAD para la *aplicación de API de servidor* en el área de **Azure Active Directory** > **registros de aplicaciones** de la Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="e90f9-109">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="e90f9-110">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="e90f9-110">Select **New registration**.</span></span>
1. <span data-ttu-id="e90f9-111">Proporcione un **nombre** para la aplicación (por ejemplo, **Blazor servidor AAD**).</span><span class="sxs-lookup"><span data-stu-id="e90f9-111">Provide a **Name** for the app (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="e90f9-112">Elija un **tipo de cuenta compatible**.</span><span class="sxs-lookup"><span data-stu-id="e90f9-112">Choose a **Supported account types**.</span></span> <span data-ttu-id="e90f9-113">Solo puede seleccionar **cuentas en este directorio de la organización** (un solo inquilino) para esta experiencia.</span><span class="sxs-lookup"><span data-stu-id="e90f9-113">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="e90f9-114">La *aplicación de API de servidor* no requiere un **URI de redirección** en este escenario, por lo que deje la lista desplegable establecida en **Web** y no escriba un URI de redirección.</span><span class="sxs-lookup"><span data-stu-id="e90f9-114">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="e90f9-115">Deshabilite la casilla **permisos** > **conceder permisos de administrador a OpenID y offline_access** .</span><span class="sxs-lookup"><span data-stu-id="e90f9-115">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="e90f9-116">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="e90f9-116">Select **Register**.</span></span>

<span data-ttu-id="e90f9-117">En **permisos**de la API, quite el **Microsoft Graph** > **User. Read** , ya que la aplicación no requiere el inicio de sesión o el acceso a UER Profile.</span><span class="sxs-lookup"><span data-stu-id="e90f9-117">In **API permissions**, remove the **Microsoft Graph** > **User.Read** permission, as the app doesn't require sign in or uer profile access.</span></span>

<span data-ttu-id="e90f9-118">En **exponer una API**:</span><span class="sxs-lookup"><span data-stu-id="e90f9-118">In **Expose an API**:</span></span>

1. <span data-ttu-id="e90f9-119">Seleccione **Agregar un ámbito**.</span><span class="sxs-lookup"><span data-stu-id="e90f9-119">Select **Add a scope**.</span></span>
1. <span data-ttu-id="e90f9-120">Seleccione **Guardar y continuar**.</span><span class="sxs-lookup"><span data-stu-id="e90f9-120">Select **Save and continue**.</span></span>
1. <span data-ttu-id="e90f9-121">Proporcione un **nombre de ámbito** (por ejemplo, `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="e90f9-121">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="e90f9-122">Proporcione un **nombre para mostrar del consentimiento del administrador** (por ejemplo, `Access API`).</span><span class="sxs-lookup"><span data-stu-id="e90f9-122">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="e90f9-123">Proporcione una **Descripción del consentimiento del administrador** (por ejemplo, `Allows the app to access server app API endpoints.`).</span><span class="sxs-lookup"><span data-stu-id="e90f9-123">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="e90f9-124">Confirme que el **Estado** está establecido en **habilitado**.</span><span class="sxs-lookup"><span data-stu-id="e90f9-124">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="e90f9-125">Seleccione la opción **Agregar un ámbito**.</span><span class="sxs-lookup"><span data-stu-id="e90f9-125">Select **Add scope**.</span></span>

<span data-ttu-id="e90f9-126">Registre la siguiente información:</span><span class="sxs-lookup"><span data-stu-id="e90f9-126">Record the following information:</span></span>

* <span data-ttu-id="e90f9-127">*Aplicación de API de servidor* IDENTIFICADOR de aplicación (ID. de cliente) (por ejemplo, `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="e90f9-127">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="e90f9-128">IDENTIFICADOR de directorio (ID. de inquilino) (por ejemplo, `222222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="e90f9-128">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="e90f9-129">Dominio del inquilino de AAD (por ejemplo, `contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="e90f9-129">AAD Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>
* <span data-ttu-id="e90f9-130">Ámbito predeterminado (por ejemplo, `API.Access`)</span><span class="sxs-lookup"><span data-stu-id="e90f9-130">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="e90f9-131">Registrar una aplicación de cliente</span><span class="sxs-lookup"><span data-stu-id="e90f9-131">Register a client app</span></span>

<span data-ttu-id="e90f9-132">Siga las instrucciones de [Inicio rápido: registro de una aplicación con la plataforma de Microsoft Identity y los](/azure/active-directory/develop/quickstart-register-app) temas de Azure AAD subsiguientes para registrar una aplicación de AAD para la *aplicación cliente* en el área de **Azure Active Directory** > **registros de aplicaciones** de la Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="e90f9-132">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register a AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="e90f9-133">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="e90f9-133">Select **New registration**.</span></span>
1. <span data-ttu-id="e90f9-134">Proporcione un **nombre** para la aplicación (por ejemplo, **Blazor cliente AAD**).</span><span class="sxs-lookup"><span data-stu-id="e90f9-134">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="e90f9-135">Elija un **tipo de cuenta compatible**.</span><span class="sxs-lookup"><span data-stu-id="e90f9-135">Choose a **Supported account types**.</span></span> <span data-ttu-id="e90f9-136">Solo puede seleccionar **cuentas en este directorio de la organización** (un solo inquilino) para esta experiencia.</span><span class="sxs-lookup"><span data-stu-id="e90f9-136">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="e90f9-137">Deje la lista desplegable **URI de redirección** establecida en **Web**y proporcione un URI de redireccionamiento de `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="e90f9-137">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="e90f9-138">Deshabilite la casilla **permisos** > **conceder permisos de administrador a OpenID y offline_access** .</span><span class="sxs-lookup"><span data-stu-id="e90f9-138">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="e90f9-139">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="e90f9-139">Select **Register**.</span></span>

<span data-ttu-id="e90f9-140">En **autenticación** > **configuraciones de plataforma** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="e90f9-140">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="e90f9-141">Confirme que el **URI de redirección** de `https://localhost:5001/authentication/login-callback` está presente.</span><span class="sxs-lookup"><span data-stu-id="e90f9-141">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="e90f9-142">En **concesión implícita**, active las casillas de verificación de **tokens de acceso** y **tokens de identificador**.</span><span class="sxs-lookup"><span data-stu-id="e90f9-142">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="e90f9-143">Los valores predeterminados restantes de la aplicación son aceptables para esta experiencia.</span><span class="sxs-lookup"><span data-stu-id="e90f9-143">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="e90f9-144">Seleccione el botón **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="e90f9-144">Select the **Save** button.</span></span>

<span data-ttu-id="e90f9-145">En **permisos de API**:</span><span class="sxs-lookup"><span data-stu-id="e90f9-145">In **API permissions**:</span></span>

1. <span data-ttu-id="e90f9-146">Confirme que la aplicación tiene **Microsoft Graph** > **usuario.** permiso de lectura.</span><span class="sxs-lookup"><span data-stu-id="e90f9-146">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="e90f9-147">Seleccione **Agregar un permiso** seguido de **mis API**.</span><span class="sxs-lookup"><span data-stu-id="e90f9-147">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="e90f9-148">Seleccione la *aplicación de API de servidor* en la columna **nombre** (por ejemplo, **Blazor servidor AAD**).</span><span class="sxs-lookup"><span data-stu-id="e90f9-148">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="e90f9-149">Abra la lista de **API** .</span><span class="sxs-lookup"><span data-stu-id="e90f9-149">Open the **API** list.</span></span>
1. <span data-ttu-id="e90f9-150">Habilite el acceso a la API (por ejemplo, `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="e90f9-150">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="e90f9-151">Seleccione **Agregar permisos**.</span><span class="sxs-lookup"><span data-stu-id="e90f9-151">Select **Add permissions**.</span></span>
1. <span data-ttu-id="e90f9-152">Seleccione el botón **conceder contenido de administración para {nombre de inquilino}** .</span><span class="sxs-lookup"><span data-stu-id="e90f9-152">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="e90f9-153">Seleccione **Sí** para confirmar la acción.</span><span class="sxs-lookup"><span data-stu-id="e90f9-153">Select **Yes** to confirm.</span></span>

<span data-ttu-id="e90f9-154">Registre el identificador de aplicación de la aplicación *cliente* (identificador de cliente) (por ejemplo, `33333333-3333-3333-3333-333333333333`).</span><span class="sxs-lookup"><span data-stu-id="e90f9-154">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="e90f9-155">Creación de la aplicación</span><span class="sxs-lookup"><span data-stu-id="e90f9-155">Create the app</span></span>

<span data-ttu-id="e90f9-156">Reemplace los marcadores de posición en el siguiente comando por la información registrada anteriormente y ejecute el comando en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="e90f9-156">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP CLIENT ID}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

<span data-ttu-id="e90f9-157">Para especificar la ubicación de salida, que crea una carpeta de proyecto si no existe, incluya la opción output en el comando con una ruta de acceso (por ejemplo, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="e90f9-157">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="e90f9-158">El nombre de la carpeta también se convierte en parte del nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="e90f9-158">The folder name also becomes part of the project's name.</span></span>

> [!NOTE]
> <span data-ttu-id="e90f9-159">Consulte la sección [compatibilidad con el servicio de autenticación](#Authentication service support) para obtener un cambio de configuración importante en el ámbito de token de acceso predeterminado.</span><span class="sxs-lookup"><span data-stu-id="e90f9-159">See the [Authentication service support](#Authentication service support) section for an important configuration change to the default access token scope.</span></span> <span data-ttu-id="e90f9-160">El valor proporcionado por la plantilla Blazor webassembly debe cambiarse manualmente después de crear la *aplicación cliente* a partir de la plantilla.</span><span class="sxs-lookup"><span data-stu-id="e90f9-160">The value provided by the Blazor WebAssembly template must be manually changed after the *Client app* is created from the template.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="e90f9-161">Configuración de la aplicación de servidor</span><span class="sxs-lookup"><span data-stu-id="e90f9-161">Server app configuration</span></span>

<span data-ttu-id="e90f9-162">*Esta sección pertenece a la aplicación de **servidor** de la solución.*</span><span class="sxs-lookup"><span data-stu-id="e90f9-162">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="e90f9-163">Paquete de autenticación</span><span class="sxs-lookup"><span data-stu-id="e90f9-163">Authentication package</span></span>

<span data-ttu-id="e90f9-164">La compatibilidad para autenticar y autorizar llamadas a ASP.NET Core API Web la proporciona el `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span><span class="sxs-lookup"><span data-stu-id="e90f9-164">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="e90f9-165">Compatibilidad con el servicio de autenticación</span><span class="sxs-lookup"><span data-stu-id="e90f9-165">Authentication service support</span></span>

<span data-ttu-id="e90f9-166">El método `AddAuthentication` configura los servicios de autenticación dentro de la aplicación y configura el controlador de portador JWT como el método de autenticación predeterminado.</span><span class="sxs-lookup"><span data-stu-id="e90f9-166">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="e90f9-167">El método `AddAzureADBearer` configura los parámetros específicos en el controlador de portador JWT necesario para validar los tokens emitidos por el Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="e90f9-167">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="e90f9-168">`UseAuthentication` y `UseAuthorization` Asegúrese de que:</span><span class="sxs-lookup"><span data-stu-id="e90f9-168">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="e90f9-169">La aplicación intenta analizar y validar los tokens en las solicitudes entrantes.</span><span class="sxs-lookup"><span data-stu-id="e90f9-169">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="e90f9-170">Se produce un error en cualquier solicitud que intente obtener acceso a un recurso protegido sin credenciales adecuadas.</span><span class="sxs-lookup"><span data-stu-id="e90f9-170">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a><span data-ttu-id="e90f9-171">Configuración de la aplicación</span><span class="sxs-lookup"><span data-stu-id="e90f9-171">App settings</span></span>

<span data-ttu-id="e90f9-172">El archivo *appSettings. JSON* contiene las opciones para configurar el controlador de portador JWT que se usa para validar los tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="e90f9-172">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{API CLIENT ID}",
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="e90f9-173">Controlador de WeatherForecast</span><span class="sxs-lookup"><span data-stu-id="e90f9-173">WeatherForecast controller</span></span>

<span data-ttu-id="e90f9-174">El controlador WeatherForecast (*Controllers/WeatherForecastController. CS*) expone una API protegida con el `[Authorize]` atributo que se aplica al controlador.</span><span class="sxs-lookup"><span data-stu-id="e90f9-174">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="e90f9-175">Es **importante** comprender esto:</span><span class="sxs-lookup"><span data-stu-id="e90f9-175">It's **important** to understand that:</span></span>

* <span data-ttu-id="e90f9-176">El `[Authorize]` atributo de este controlador de API es lo único que protege esta API frente al acceso no autorizado.</span><span class="sxs-lookup"><span data-stu-id="e90f9-176">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="e90f9-177">El atributo `[Authorize]` usado en la aplicación webassembly de Blazor solo sirve como sugerencia a la aplicación de que el usuario debe estar autorizado para que la aplicación funcione correctamente.</span><span class="sxs-lookup"><span data-stu-id="e90f9-177">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

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

## <a name="client-app-configuration"></a><span data-ttu-id="e90f9-178">Configuración de la aplicación cliente</span><span class="sxs-lookup"><span data-stu-id="e90f9-178">Client app configuration</span></span>

<span data-ttu-id="e90f9-179">*Esta sección pertenece a la aplicación **cliente** de la solución.*</span><span class="sxs-lookup"><span data-stu-id="e90f9-179">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="e90f9-180">Paquete de autenticación</span><span class="sxs-lookup"><span data-stu-id="e90f9-180">Authentication package</span></span>

<span data-ttu-id="e90f9-181">Cuando se crea una aplicación para usar cuentas profesionales o educativas (`SingleOrg`), la aplicación recibe automáticamente una referencia de paquete para la [biblioteca de autenticación de Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="e90f9-181">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="e90f9-182">El paquete proporciona un conjunto de primitivas que ayudan a la aplicación a autenticar a los usuarios y obtener tokens para llamar a las API protegidas.</span><span class="sxs-lookup"><span data-stu-id="e90f9-182">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="e90f9-183">Si agrega la autenticación a una aplicación, agregue manualmente el paquete al archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="e90f9-183">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="e90f9-184">Reemplace `{VERSION}` en la referencia de paquete anterior por la versión del paquete de `Microsoft.AspNetCore.Blazor.Templates` que se muestra en el artículo <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="e90f9-184">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="e90f9-185">El paquete de `Microsoft.Authentication.WebAssembly.Msal` agrega de manera transitiva el paquete de `Microsoft.AspNetCore.Components.WebAssembly.Authentication` a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e90f9-185">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="e90f9-186">Compatibilidad con el servicio de autenticación</span><span class="sxs-lookup"><span data-stu-id="e90f9-186">Authentication service support</span></span>

<span data-ttu-id="e90f9-187">La compatibilidad con la autenticación de usuarios se registra en el contenedor de servicios con el método de extensión `AddMsalAuthentication` proporcionado por el paquete de `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="e90f9-187">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="e90f9-188">Este método configura todos los servicios necesarios para que la aplicación interactúe con el proveedor de identidades (IP).</span><span class="sxs-lookup"><span data-stu-id="e90f9-188">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="e90f9-189">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="e90f9-189">*Program.cs*:</span></span>

<span data-ttu-id="e90f9-190">Cuando se genera la *aplicación cliente* , el ámbito del token de acceso predeterminado tiene el formato `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`.</span><span class="sxs-lookup"><span data-stu-id="e90f9-190">When the *Client app* is generated, the default access token scope is of the format `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`.</span></span> <span data-ttu-id="e90f9-191">**Quite la parte `api://` del valor de ámbito.**</span><span class="sxs-lookup"><span data-stu-id="e90f9-191">**Remove the `api://` portion of the scope value.**</span></span> <span data-ttu-id="e90f9-192">Este problema se solucionará en una versión preliminar futura.</span><span class="sxs-lookup"><span data-stu-id="e90f9-192">This issue will be addressed in a future preview release.</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> <span data-ttu-id="e90f9-193">El ámbito del token de acceso predeterminado debe tener el formato `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (por ejemplo, `11111111-1111-1111-1111-111111111111/API.Access`).</span><span class="sxs-lookup"><span data-stu-id="e90f9-193">The default access token scope must be in the format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (for example, `11111111-1111-1111-1111-111111111111/API.Access`).</span></span> <span data-ttu-id="e90f9-194">Si se proporciona un esquema o esquema y un host a la configuración de ámbito (como se muestra en Azure portal), la *aplicación cliente* genera una excepción no controlada cuando recibe una respuesta *401 no autorizada* de la *aplicación de API de servidor*.</span><span class="sxs-lookup"><span data-stu-id="e90f9-194">If a scheme or scheme and host is provided to the scope setting (as shown in the Azure Portal), the *Client app* throws an unhandled exception when it receives a *401 Unauthorized* response from the *Server API app*.</span></span>

<span data-ttu-id="e90f9-195">El método `AddMsalAuthentication` acepta una devolución de llamada para configurar los parámetros necesarios para autenticar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="e90f9-195">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="e90f9-196">Los valores necesarios para configurar la aplicación pueden obtenerse a partir de la configuración de AAD de Azure portal cuando se registra la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e90f9-196">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="e90f9-197">Los ámbitos de token de acceso predeterminados representan la lista de ámbitos de token de acceso que son:</span><span class="sxs-lookup"><span data-stu-id="e90f9-197">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="e90f9-198">Se incluye de forma predeterminada en la solicitud de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="e90f9-198">Included by default in the sign in request.</span></span>
* <span data-ttu-id="e90f9-199">Se usa para aprovisionar un token de acceso inmediatamente después de la autenticación.</span><span class="sxs-lookup"><span data-stu-id="e90f9-199">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="e90f9-200">Todos los ámbitos deben pertenecer a la misma aplicación por reglas de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e90f9-200">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="e90f9-201">Se pueden agregar ámbitos adicionales para las aplicaciones de API adicionales según sea necesario:</span><span class="sxs-lookup"><span data-stu-id="e90f9-201">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{SCOPE}");
});
```

### <a name="index-page"></a><span data-ttu-id="e90f9-202">Página de índice</span><span class="sxs-lookup"><span data-stu-id="e90f9-202">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a><span data-ttu-id="e90f9-203">Componente de aplicación</span><span class="sxs-lookup"><span data-stu-id="e90f9-203">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="e90f9-204">Componente RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="e90f9-204">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="e90f9-205">Componente LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="e90f9-205">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="e90f9-206">Componente de autenticación</span><span class="sxs-lookup"><span data-stu-id="e90f9-206">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="e90f9-207">Componente FetchData</span><span class="sxs-lookup"><span data-stu-id="e90f9-207">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="e90f9-208">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e90f9-208">Additional resources</span></span>

* <xref:security/authentication/azure-active-directory/index>
