---
title: "Configurar la autenticación de Windows en ASP.NET Core"
author: ardalis
description: "En este artículo se describe cómo configurar la autenticación de Windows en ASP.NET Core, mediante IIS Express, IIS, HTTP.sys y WebListener."
ms.author: riande
manager: wpickett
ms.date: 10/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: d4523ca65852de8cfd963838d8bf3caa1d7204cc
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="02221-103">Configurar la autenticación de Windows en una aplicación de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="02221-103">Configure Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="02221-104">Por [Steve Smith](https://ardalis.com) y [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="02221-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="02221-105">Se puede configurar la autenticación de Windows para las aplicaciones ASP.NET Core hospedadas en IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), o [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="02221-105">Windows authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="02221-106">¿Qué es la autenticación de Windows?</span><span class="sxs-lookup"><span data-stu-id="02221-106">What is Windows authentication?</span></span>

<span data-ttu-id="02221-107">Autenticación de Windows se basa en el sistema operativo para autenticar a los usuarios de las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="02221-107">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="02221-108">Puede utilizar la autenticación de Windows cuando el servidor se ejecuta en una red corporativa utilizando las identidades del dominio de Active Directory o a otras cuentas de Windows para identificar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="02221-108">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="02221-109">Autenticación de Windows es más adecuada para entornos de intranet en el que los usuarios, las aplicaciones cliente y los servidores web pertenecen al mismo dominio de Windows.</span><span class="sxs-lookup"><span data-stu-id="02221-109">Windows authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="02221-110">[Obtener más información sobre la autenticación de Windows e instalarla para IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="02221-110">[Learn more about Windows authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="02221-111">Habilitar la autenticación de Windows en una aplicación de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="02221-111">Enable Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="02221-112">La plantilla de aplicación Web de Visual Studio puede configurarse para admitir la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="02221-112">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="02221-113">Usa la plantilla de aplicación de autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="02221-113">Use the Windows authentication app template</span></span>

<span data-ttu-id="02221-114">En Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="02221-114">In Visual Studio:</span></span>
1. <span data-ttu-id="02221-115">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="02221-115">Create a new ASP.NET Core Web Application.</span></span> 
1. <span data-ttu-id="02221-116">En la lista de plantillas, seleccione aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="02221-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="02221-117">Seleccione el **Cambiar autenticación** botón y seleccione **autenticación de Windows**.</span><span class="sxs-lookup"><span data-stu-id="02221-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="02221-118">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="02221-118">Run the app.</span></span> <span data-ttu-id="02221-119">El nombre de usuario aparece en la parte superior derecha de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="02221-119">The username appears in the top right of the app.</span></span>

![Captura de pantalla del explorador de la autenticación de Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="02221-121">Para el trabajo de desarrollo con IIS Express, la plantilla proporciona la configuración necesaria para utilizar la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="02221-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="02221-122">La sección siguiente muestra cómo configurar manualmente una aplicación de ASP.NET Core para la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="02221-122">The following section shows how to manually configure an ASP.NET Core app for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="02221-123">Configuración de Visual Studio para Windows y la autenticación anónima</span><span class="sxs-lookup"><span data-stu-id="02221-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="02221-124">El proyecto de Visual Studio **propiedades** la página **depurar** ficha proporciona casillas de verificación para la autenticación de Windows y la autenticación anónima.</span><span class="sxs-lookup"><span data-stu-id="02221-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Captura de pantalla del explorador de la autenticación de Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="02221-126">Como alternativa, estas dos propiedades pueden configurarse en el *launchSettings.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="02221-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="02221-127">Habilitar la autenticación de Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="02221-127">Enable Windows authentication with IIS</span></span>

<span data-ttu-id="02221-128">IIS usa el [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module) (ANCM) para hospedar aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="02221-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="02221-129">La ANCM flujos autenticación de Windows en IIS de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="02221-129">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="02221-130">Configuración de autenticación de Windows se realiza dentro de IIS, no en el proyecto de aplicación.</span><span class="sxs-lookup"><span data-stu-id="02221-130">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="02221-131">Las secciones siguientes muestran cómo usar el Administrador de IIS para configurar una aplicación ASP.NET Core para usar la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="02221-131">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication.</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="02221-132">Crear un nuevo sitio IIS</span><span class="sxs-lookup"><span data-stu-id="02221-132">Create a new IIS site</span></span>

<span data-ttu-id="02221-133">Especifique un nombre y una carpeta y permitir que se cree un nuevo grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="02221-133">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="02221-134">Personalizar la autenticación</span><span class="sxs-lookup"><span data-stu-id="02221-134">Customize authentication</span></span>

<span data-ttu-id="02221-135">Abra el menú de autenticación para el sitio.</span><span class="sxs-lookup"><span data-stu-id="02221-135">Open the Authentication menu for the site.</span></span>

![Menú de autenticación de IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="02221-137">Deshabilite la autenticación anónima y habilitar la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="02221-137">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Configuración de autenticación IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="02221-139">Publicar el proyecto en la carpeta del sitio IIS</span><span class="sxs-lookup"><span data-stu-id="02221-139">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="02221-140">Con Visual Studio o la CLI de núcleo. NET, publique la aplicación en la carpeta de destino.</span><span class="sxs-lookup"><span data-stu-id="02221-140">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Cuadro de diálogo de publicación de Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="02221-142">Obtenga más información sobre [publicar en IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="02221-142">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="02221-143">Inicie la aplicación para comprobar que funciona la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="02221-143">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a><span data-ttu-id="02221-144">Habilitar la autenticación de Windows con HTTP.sys o WebListener</span><span class="sxs-lookup"><span data-stu-id="02221-144">Enable Windows authentication with HTTP.sys or WebListener</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="02221-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="02221-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="02221-146">Aunque Kestrel no admite la autenticación de Windows, puede usar [HTTP.sys](xref:fundamentals/servers/httpsys) para admitir escenarios hospedados por sí mismo en Windows.</span><span class="sxs-lookup"><span data-stu-id="02221-146">Although Kestrel doesn't support Windows authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="02221-147">En el ejemplo siguiente se configura el host de la aplicación web para usar HTTP.sys con autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="02221-147">The following example configures the app's web host to use HTTP.sys with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="02221-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="02221-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="02221-149">Aunque Kestrel no admite la autenticación de Windows, puede usar [WebListener](xref:fundamentals/servers/weblistener) para admitir escenarios hospedados por sí mismo en Windows.</span><span class="sxs-lookup"><span data-stu-id="02221-149">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="02221-150">En el ejemplo siguiente se configura el host de la aplicación web para usar WebListener con la autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="02221-150">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a><span data-ttu-id="02221-151">Trabajar con la autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="02221-151">Work with Windows authentication</span></span>

<span data-ttu-id="02221-152">El estado de configuración de acceso anónimo determina la manera en que la `[Authorize]` y `[AllowAnonymous]` atributos se utilizan en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="02221-152">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="02221-153">Las dos secciones siguientes explican cómo controlar los Estados de configuración permitidos y no permitidos de acceso anónimo.</span><span class="sxs-lookup"><span data-stu-id="02221-153">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="02221-154">Denegar el acceso anónimo</span><span class="sxs-lookup"><span data-stu-id="02221-154">Disallow anonymous access</span></span>

<span data-ttu-id="02221-155">Cuando se habilita la autenticación de Windows y acceso anónimo está deshabilitado, el `[Authorize]` y `[AllowAnonymous]` atributos no tienen ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="02221-155">When Windows authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="02221-156">Si el sitio IIS (o servidor HTTP.sys o WebListener) está configurado para denegar el acceso anónimo, la solicitud nunca llega a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="02221-156">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="02221-157">Por este motivo, la `[AllowAnonymous]` atributo no se aplica.</span><span class="sxs-lookup"><span data-stu-id="02221-157">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="02221-158">Permitir el acceso anónimo</span><span class="sxs-lookup"><span data-stu-id="02221-158">Allow anonymous access</span></span>

<span data-ttu-id="02221-159">Cuando se habilitan la autenticación de Windows y el acceso anónimo, use la `[Authorize]` y `[AllowAnonymous]` atributos.</span><span class="sxs-lookup"><span data-stu-id="02221-159">When both Windows authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="02221-160">El `[Authorize]` atributo permite proteger partes de la aplicación que realmente requieren autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="02221-160">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows authentication.</span></span> <span data-ttu-id="02221-161">El `[AllowAnonymous]` atributo invalidaciones `[Authorize]` atributo uso dentro de las aplicaciones que permiten el acceso anónimo.</span><span class="sxs-lookup"><span data-stu-id="02221-161">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="02221-162">Vea [autorización sencilla](xref:security/authorization/simple) para obtener detalles de uso de atributos.</span><span class="sxs-lookup"><span data-stu-id="02221-162">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="02221-163">En ASP.NET Core 2.x, el `[Authorize]` atributo requiere una configuración adicional de *Startup.cs* Desafíe solicitudes anónimas para la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="02221-163">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows authentication.</span></span> <span data-ttu-id="02221-164">La configuración recomendada varía ligeramente según el servidor web que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="02221-164">The recommended configuration varies slightly based on the web server being used.</span></span>

#### <a name="iis"></a><span data-ttu-id="02221-165">IIS</span><span class="sxs-lookup"><span data-stu-id="02221-165">IIS</span></span>

<span data-ttu-id="02221-166">Si usa IIS, agregue lo siguiente a la `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="02221-166">If using IIS, add the following to the `ConfigureServices` method:</span></span> 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="02221-167">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="02221-167">HTTP.sys</span></span>

<span data-ttu-id="02221-168">Si utiliza HTTP.sys, agregue lo siguiente a la `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="02221-168">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="02221-169">Suplantación</span><span class="sxs-lookup"><span data-stu-id="02221-169">Impersonation</span></span>

<span data-ttu-id="02221-170">ASP.NET Core no implementa la suplantación.</span><span class="sxs-lookup"><span data-stu-id="02221-170">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="02221-171">Las aplicaciones se ejecutan con la identidad de aplicación para todas las solicitudes, uso de la identidad de proceso o grupo de servidores de aplicación.</span><span class="sxs-lookup"><span data-stu-id="02221-171">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="02221-172">Si necesita realizar una acción en nombre del usuario de forma explícita, use `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="02221-172">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="02221-173">Ejecutar una sola acción en este contexto y, a continuación, cierre el contexto.</span><span class="sxs-lookup"><span data-stu-id="02221-173">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="02221-174">Tenga en cuenta que `RunImpersonated` no es compatible con operaciones asincrónicas y no deben usarse para escenarios complejos.</span><span class="sxs-lookup"><span data-stu-id="02221-174">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="02221-175">Por ejemplo, el ajuste de las solicitudes de todas o cadenas de middleware no es compatible ni recomendable.</span><span class="sxs-lookup"><span data-stu-id="02221-175">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

---
