---
title: Configurar la autenticación de Windows en ASP.NET Core
author: ardalis
description: En este artículo se describe cómo configurar la autenticación de Windows en ASP.NET Core, usar IIS Express, IIS, HTTP.sys y WebListener.
ms.author: riande
ms.date: 08/18/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: a8066d248c0d4db1d1f61b2a14bdb4656a2f4265
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312417"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="d0694-103">Configurar la autenticación de Windows en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0694-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="d0694-104">Por [Steve Smith](https://ardalis.com) y [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="d0694-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="d0694-105">Se puede configurar la autenticación de Windows para las aplicaciones de ASP.NET Core hospedadas en IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), o [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="d0694-105">Windows Authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="windows-authentication"></a><span data-ttu-id="d0694-106">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="d0694-106">Windows Authentication</span></span>

<span data-ttu-id="d0694-107">Autenticación de Windows se basa en el sistema operativo para autenticar usuarios de aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d0694-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="d0694-108">Puede usar la autenticación de Windows cuando el servidor se ejecuta en una red corporativa mediante identidades de dominio de Active Directory o a otras cuentas de Windows para identificar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="d0694-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="d0694-109">Autenticación de Windows se adapta mejor a los entornos de intranet en el que los usuarios, las aplicaciones cliente y servidores web pertenecen al mismo dominio de Windows.</span><span class="sxs-lookup"><span data-stu-id="d0694-109">Windows Authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="d0694-110">[Más información acerca de la autenticación de Windows e instalarla para IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="d0694-110">[Learn more about Windows Authentication and installing it for IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="d0694-111">Habilitar la autenticación de Windows en una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0694-111">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="d0694-112">La plantilla de aplicación Web de Visual Studio puede configurarse para admitir la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="d0694-112">The Visual Studio Web Application template can be configured to support Windows Authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="d0694-113">Usa la plantilla de aplicación de autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="d0694-113">Use the Windows Authentication app template</span></span>

<span data-ttu-id="d0694-114">En Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d0694-114">In Visual Studio:</span></span>

1. <span data-ttu-id="d0694-115">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d0694-115">Create a new ASP.NET Core Web Application.</span></span>
1. <span data-ttu-id="d0694-116">Seleccione la aplicación Web en la lista de plantillas.</span><span class="sxs-lookup"><span data-stu-id="d0694-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="d0694-117">Seleccione el **Cambiar autenticación** y seleccione **Windows autenticación**.</span><span class="sxs-lookup"><span data-stu-id="d0694-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="d0694-118">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0694-118">Run the app.</span></span> <span data-ttu-id="d0694-119">El nombre de usuario aparece en la parte superior derecha de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0694-119">The username appears in the top right of the app.</span></span>

![Captura de pantalla del explorador de la autenticación de Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="d0694-121">Para el trabajo de desarrollo con IIS Express, la plantilla proporciona toda la configuración necesaria para utilizar la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="d0694-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows Authentication.</span></span> <span data-ttu-id="d0694-122">En la sección siguiente se muestra cómo configurar manualmente una aplicación ASP.NET Core para la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="d0694-122">The following section shows how to manually configure an ASP.NET Core app for Windows Authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="d0694-123">Configuración de Visual Studio para Windows y la autenticación anónima</span><span class="sxs-lookup"><span data-stu-id="d0694-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="d0694-124">El proyecto de Visual Studio **propiedades** la página **depurar** ficha proporciona casillas de verificación para la autenticación de Windows y la autenticación anónima.</span><span class="sxs-lookup"><span data-stu-id="d0694-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows Authentication and anonymous authentication.</span></span>

![Captura de pantalla del explorador de la autenticación de Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="d0694-126">Como alternativa, se pueden configurar estas dos propiedades en el *launchSettings.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="d0694-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="d0694-127">Habilitar la autenticación de Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="d0694-127">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="d0694-128">IIS usa el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para hospedar aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d0694-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="d0694-129">Autenticación de Windows se configura en IIS, no la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0694-129">Windows Authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="d0694-130">Las secciones siguientes muestran cómo usar el Administrador de IIS para configurar una aplicación ASP.NET Core para usar la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="d0694-130">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows Authentication.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="d0694-131">Configuración de IIS</span><span class="sxs-lookup"><span data-stu-id="d0694-131">IIS configuration</span></span>

<span data-ttu-id="d0694-132">Habilite el servicio de rol IIS para la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="d0694-132">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="d0694-133">Para obtener más información, consulte [Habilitar autenticación de Windows en servicios de rol de IIS (consulte el paso 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="d0694-133">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="d0694-134">Middleware de integración de IIS está configurado para autenticar automáticamente las solicitudes de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d0694-134">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="d0694-135">Para obtener más información, consulte [Host ASP.NET Core en Windows con IIS: las opciones de IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="d0694-135">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="d0694-136">El módulo ASP.NET Core está configurado para reenviar el token de autenticación de Windows para la aplicación de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d0694-136">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="d0694-137">Para obtener más información, consulte [referencia de configuración del módulo ASP.NET Core: los atributos del elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="d0694-137">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="d0694-138">Crear un nuevo sitio IIS</span><span class="sxs-lookup"><span data-stu-id="d0694-138">Create a new IIS site</span></span>

<span data-ttu-id="d0694-139">Especifique un nombre y una carpeta y permitir que se cree un nuevo grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="d0694-139">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="d0694-140">Personalizar la autenticación</span><span class="sxs-lookup"><span data-stu-id="d0694-140">Customize authentication</span></span>

<span data-ttu-id="d0694-141">Abra las características de autenticación para el sitio.</span><span class="sxs-lookup"><span data-stu-id="d0694-141">Open the Authentication features for the site.</span></span>

![Menú de la autenticación de IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="d0694-143">Deshabilitar la autenticación anónima y habilitar la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="d0694-143">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Configuración de autenticación IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="d0694-145">Publique el proyecto en la carpeta del sitio IIS</span><span class="sxs-lookup"><span data-stu-id="d0694-145">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="d0694-146">Con Visual Studio o la CLI de .NET Core, publicar la aplicación en la carpeta de destino.</span><span class="sxs-lookup"><span data-stu-id="d0694-146">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Cuadro de diálogo de publicación de Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="d0694-148">Obtenga más información sobre [publicar en IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="d0694-148">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="d0694-149">Inicie la aplicación para comprobar que funciona la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="d0694-149">Launch the app to verify Windows Authentication is working.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="d0694-150">Habilitar la autenticación de Windows con HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d0694-150">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="d0694-151">Aunque Kestrel no admite la autenticación de Windows, puede usar [HTTP.sys](xref:fundamentals/servers/httpsys) para admitir los escenarios Auto-hospedados en Windows.</span><span class="sxs-lookup"><span data-stu-id="d0694-151">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="d0694-152">El ejemplo siguiente configura el host de la aplicación web para usar HTTP.sys con autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="d0694-152">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="d0694-153">HTTP.sys delega en la autenticación de modo kernel con el protocolo de autenticación de Kerberos.</span><span class="sxs-lookup"><span data-stu-id="d0694-153">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="d0694-154">La autenticación de modo usuario no se admite con Kerberos y HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="d0694-154">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="d0694-155">Se debe usar la cuenta de equipo para descifrar el token o el vale de Kerberos que se obtiene de Active Directory y que el cliente reenvía al servidor para autenticar al usuario.</span><span class="sxs-lookup"><span data-stu-id="d0694-155">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="d0694-156">Registre el nombre de entidad de seguridad de servicio (SPN) para el host, no el usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0694-156">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a><span data-ttu-id="d0694-157">Habilitar la autenticación de Windows con WebListener</span><span class="sxs-lookup"><span data-stu-id="d0694-157">Enable Windows Authentication with WebListener</span></span>

<span data-ttu-id="d0694-158">Aunque Kestrel no admite la autenticación de Windows, puede usar [WebListener](xref:fundamentals/servers/weblistener) para admitir los escenarios Auto-hospedados en Windows.</span><span class="sxs-lookup"><span data-stu-id="d0694-158">Although Kestrel doesn't support Windows Authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="d0694-159">El ejemplo siguiente configura el host de la aplicación web para el uso de WebListener con autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="d0694-159">The following example configures the app's web host to use WebListener with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> <span data-ttu-id="d0694-160">WebListener delega en la autenticación de modo kernel con el protocolo de autenticación de Kerberos.</span><span class="sxs-lookup"><span data-stu-id="d0694-160">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="d0694-161">La autenticación de modo usuario no se admite con Kerberos y WebListener.</span><span class="sxs-lookup"><span data-stu-id="d0694-161">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="d0694-162">Se debe usar la cuenta de equipo para descifrar el token o el vale de Kerberos que se obtiene de Active Directory y que el cliente reenvía al servidor para autenticar al usuario.</span><span class="sxs-lookup"><span data-stu-id="d0694-162">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="d0694-163">Registre el nombre de entidad de seguridad de servicio (SPN) para el host, no el usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0694-163">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

## <a name="work-with-windows-authentication"></a><span data-ttu-id="d0694-164">Trabajar con la autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="d0694-164">Work with Windows Authentication</span></span>

<span data-ttu-id="d0694-165">Estado de configuración del acceso anónimo determina la manera en que el `[Authorize]` y `[AllowAnonymous]` atributos se utilizan en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0694-165">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="d0694-166">Las dos secciones siguientes explican cómo controlar los Estados de configuración de permitidos y no permitidos del acceso anónimo.</span><span class="sxs-lookup"><span data-stu-id="d0694-166">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="d0694-167">No permitir el acceso anónimo</span><span class="sxs-lookup"><span data-stu-id="d0694-167">Disallow anonymous access</span></span>

<span data-ttu-id="d0694-168">Cuando está habilitada la autenticación de Windows y acceso anónimo está deshabilitado, el `[Authorize]` y `[AllowAnonymous]` atributos no tienen ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="d0694-168">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="d0694-169">Si el sitio IIS (o servidor HTTP.sys o WebListener) está configurado para no permitir el acceso anónimo, la solicitud nunca llega a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0694-169">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="d0694-170">Por este motivo, la `[AllowAnonymous]` atributo no es aplicable.</span><span class="sxs-lookup"><span data-stu-id="d0694-170">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="d0694-171">Permitir el acceso anónimo</span><span class="sxs-lookup"><span data-stu-id="d0694-171">Allow anonymous access</span></span>

<span data-ttu-id="d0694-172">Cuando se habilitan tanto la autenticación de Windows como el acceso anónimo, utilice el `[Authorize]` y `[AllowAnonymous]` atributos.</span><span class="sxs-lookup"><span data-stu-id="d0694-172">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="d0694-173">El `[Authorize]` atributo permite proteger partes de la aplicación que realmente requiera autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="d0694-173">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="d0694-174">El `[AllowAnonymous]` invalidaciones de atributo `[Authorize]` atributo uso dentro de las aplicaciones que permite el acceso anónimo.</span><span class="sxs-lookup"><span data-stu-id="d0694-174">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="d0694-175">Consulte [autorización sencilla](xref:security/authorization/simple) para obtener detalles de uso del atributo.</span><span class="sxs-lookup"><span data-stu-id="d0694-175">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="d0694-176">En ASP.NET Core 2.x, el `[Authorize]` atributo requiere configuración adicional en *Startup.cs* Desafíe a las solicitudes anónimas para la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="d0694-176">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="d0694-177">La configuración recomendada varía ligeramente según el servidor web que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="d0694-177">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="d0694-178">De forma predeterminada, se presentan a los usuarios que no tienen autorización para acceder a una página con una respuesta HTTP 403 vacía.</span><span class="sxs-lookup"><span data-stu-id="d0694-178">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="d0694-179">El [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) puede configurarse para proporcionar a los usuarios una mejor experiencia de "Acceso denegado".</span><span class="sxs-lookup"><span data-stu-id="d0694-179">The [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="d0694-180">IIS</span><span class="sxs-lookup"><span data-stu-id="d0694-180">IIS</span></span>

<span data-ttu-id="d0694-181">Si usa IIS, agregue lo siguiente a la `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="d0694-181">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="d0694-182">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d0694-182">HTTP.sys</span></span>

<span data-ttu-id="d0694-183">Si usa HTTP.sys, agregue lo siguiente a la `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="d0694-183">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="d0694-184">Suplantación</span><span class="sxs-lookup"><span data-stu-id="d0694-184">Impersonation</span></span>

<span data-ttu-id="d0694-185">ASP.NET Core no implementa la suplantación.</span><span class="sxs-lookup"><span data-stu-id="d0694-185">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="d0694-186">Las aplicaciones se ejecutan con la identidad de aplicación para todas las solicitudes, utilizando la identidad de proceso o grupo de servidores de aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0694-186">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="d0694-187">Si necesita realizar explícitamente una acción en nombre de un usuario, use `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="d0694-187">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="d0694-188">Ejecutar una sola acción en este contexto y, a continuación, cierre el contexto.</span><span class="sxs-lookup"><span data-stu-id="d0694-188">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="d0694-189">Tenga en cuenta que `RunImpersonated` no es compatible con operaciones asincrónicas y no puede utilizarse para escenarios complejos.</span><span class="sxs-lookup"><span data-stu-id="d0694-189">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="d0694-190">Por ejemplo, el ajuste de las solicitudes de todas o las cadenas de middleware no es compatible ni recomendable.</span><span class="sxs-lookup"><span data-stu-id="d0694-190">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>
