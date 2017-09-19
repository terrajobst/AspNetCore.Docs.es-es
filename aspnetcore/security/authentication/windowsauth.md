---
title: "Configurar la autenticación de Windows en ASP.NET Core"
author: ardalis
description: "Cómo configurar la autenticación de Windows en ASP.NET Core"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: f724584b43eb2be105cc8a207d5c7b6fec558881
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2017
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="493a6-104">Configurar la autenticación de Windows en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="493a6-104">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="493a6-105">Por [Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="493a6-105">By [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="493a6-106">Autenticación de Windows puede configurarse para las aplicaciones ASP.NET Core hospedadas por IIS o WebListener.</span><span class="sxs-lookup"><span data-stu-id="493a6-106">Windows authentication can be configured for ASP.NET Core apps hosted with IIS or WebListener.</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="493a6-107">¿Qué es la autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="493a6-107">What is Windows authentication</span></span>

<span data-ttu-id="493a6-108">Autenticación de Windows se basa en el sistema operativo para autenticar a los usuarios de las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="493a6-108">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="493a6-109">Puede utilizar la autenticación de Windows cuando el servidor se ejecuta en una red corporativa utilizando las identidades del dominio de Active Directory o a otras cuentas de Windows para identificar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="493a6-109">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="493a6-110">Autenticación de Windows es una forma segura de autenticación recomendada adecuada para entornos de intranet donde los usuarios, las aplicaciones cliente y los servidores web pertenecen al mismo dominio de Windows.</span><span class="sxs-lookup"><span data-stu-id="493a6-110">Windows authentication is a secure form of authentication best suited to intranet environments where users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="493a6-111">[Más información acerca de la autenticación de Windows e instalar para IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="493a6-111">[Learn more about Windows Authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enabling-windows-authentication-in-an-aspnet-core-application"></a><span data-ttu-id="493a6-112">Habilitar la autenticación de Windows en una aplicación de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="493a6-112">Enabling Windows authentication in an ASP.NET Core application</span></span>

<span data-ttu-id="493a6-113">La plantilla de aplicación Web de Visual Studio puede configurarse para admitir la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="493a6-113">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="using-the-windows-authentication-app-template"></a><span data-ttu-id="493a6-114">Con la plantilla de aplicación de autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="493a6-114">Using the Windows authentication app template</span></span>

<span data-ttu-id="493a6-115">En Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="493a6-115">In Visual Studio:</span></span>
* <span data-ttu-id="493a6-116">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="493a6-116">Create a new ASP.NET Core Web Application.</span></span> 
* <span data-ttu-id="493a6-117">En la lista de plantillas, seleccione aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="493a6-117">Select Web Application from the list of templates.</span></span>
* <span data-ttu-id="493a6-118">Seleccione el botón de cambio de la autenticación y seleccione **autenticación de Windows**.</span><span class="sxs-lookup"><span data-stu-id="493a6-118">Select the Change Authentication button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="493a6-119">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="493a6-119">Run the app.</span></span> <span data-ttu-id="493a6-120">El nombre de usuario aparece en la parte superior derecha de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="493a6-120">The username appears in the top right of the app.</span></span>

![Captura de pantalla del explorador de la autenticación de Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="493a6-122">Para el trabajo de desarrollo con IIS Express, la plantilla proporciona la configuración necesaria para utilizar la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="493a6-122">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="493a6-123">La sección siguiente muestra cómo configurar una aplicación ASP.NET Core manualmente para la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="493a6-123">The next section shows how to configure an ASP.NET Core app manually for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="493a6-124">Configuración de Visual Studio para Windows y la autenticación anónima</span><span class="sxs-lookup"><span data-stu-id="493a6-124">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="493a6-125">La página de propiedades de Visual Studio, pestaña depurar proporciona casillas de verificación para la autenticación de Windows y la autenticación anónima.</span><span class="sxs-lookup"><span data-stu-id="493a6-125">The Visual Studio properties page, debug tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Captura de pantalla del explorador de la autenticación de Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="493a6-127">También puede configurar estas propiedades en el `launchSettings.json` archivo:</span><span class="sxs-lookup"><span data-stu-id="493a6-127">You can also configure these properties in the `launchSettings.json` file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": true,
    "anonymousAuthentication": false,
    "iisExpress": {
      "applicationUrl": "http://localhost:52171/",
      "sslPort": 0
    }
  } // additional options trimmed
}
```

## <a name="enabling-windows-authentication-with-iis"></a><span data-ttu-id="493a6-128">Habilitar la autenticación de Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="493a6-128">Enabling Windows Authentication with IIS</span></span>

<span data-ttu-id="493a6-129">IIS usa el [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module) (ANCM) para hospedar aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="493a6-129">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="493a6-130">La ANCM flujos autenticación de Windows en IIS de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="493a6-130">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="493a6-131">Configuración de autenticación de Windows se realiza dentro de IIS, no en el proyecto de aplicación.</span><span class="sxs-lookup"><span data-stu-id="493a6-131">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="493a6-132">Las secciones siguientes muestran cómo usar el Administrador de IIS para configurar una aplicación ASP.NET Core para usar la autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="493a6-132">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication:</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="493a6-133">Crear un nuevo sitio IIS</span><span class="sxs-lookup"><span data-stu-id="493a6-133">Create a new IIS site</span></span>

<span data-ttu-id="493a6-134">Especifique un nombre y una carpeta y permitir que se cree un nuevo grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="493a6-134">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="493a6-135">Personalizar la autenticación</span><span class="sxs-lookup"><span data-stu-id="493a6-135">Customize Authentication</span></span>

<span data-ttu-id="493a6-136">Abra el menú de autenticación para el sitio.</span><span class="sxs-lookup"><span data-stu-id="493a6-136">Open the Authentication menu for the site.</span></span>

![Menú de autenticación de IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="493a6-138">Deshabilite la autenticación anónima y habilitar la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="493a6-138">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Configuración de autenticación IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="493a6-140">Publicar el proyecto en la carpeta del sitio IIS</span><span class="sxs-lookup"><span data-stu-id="493a6-140">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="493a6-141">Con Visual Studio o la CLI de núcleo de .NET *publicar* la aplicación a la carpeta de destino.</span><span class="sxs-lookup"><span data-stu-id="493a6-141">Using Visual Studio or the .NET Core CLI, *publish* your app to the destination folder.</span></span>

![Cuadro de diálogo de publicación de Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="493a6-143">Obtenga más información sobre [publicar en IIS](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="493a6-143">Learn more about [publishing to IIS](xref:publishing/iis).</span></span>

<span data-ttu-id="493a6-144">Inicie la aplicación para comprobar que funciona la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="493a6-144">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enabling-windows-authentication-with-weblistener"></a><span data-ttu-id="493a6-145">Habilitar la autenticación de Windows con WebListener</span><span class="sxs-lookup"><span data-stu-id="493a6-145">Enabling Windows authentication with WebListener</span></span>

<span data-ttu-id="493a6-146">Aunque Kestrel no admite la autenticación de Windows, puede usar [WebListener](xref:fundamentals/servers/weblistener) para admitir escenarios hospedados por sí mismo en Windows.</span><span class="sxs-lookup"><span data-stu-id="493a6-146">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="493a6-147">En el ejemplo siguiente se configura el host de la aplicación web para usar WebListener con la autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="493a6-147">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseWebListener(options =>
            {
                options.ListenerSettings.Authentication.Schemes = 
                    AuthenticationSchemes.Negotiate | AuthenticationSchemes.NTLM;
                options.ListenerSettings.Authentication.AllowAnonymous = false;
            })
            .UseContentRoot(Directory.GetCurrentDirectory())
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

## <a name="working-with-windows-authentication"></a><span data-ttu-id="493a6-148">Trabajar con autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="493a6-148">Working with Windows authentication</span></span>

<span data-ttu-id="493a6-149">Si la aplicación utiliza la autenticación de Windows y el acceso anónimo, puede usar el ``[Authorize]`` y ``[AllowAnonymous]`` atributos.</span><span class="sxs-lookup"><span data-stu-id="493a6-149">If your app uses Windows authentication and anonymous access, you can use the ``[Authorize]`` and ``[AllowAnonymous]`` attributes.</span></span> <span data-ttu-id="493a6-150">Aplicaciones que no tienen anónimo realice habilitado no requieren ``[Authorize]``; de la aplicación se considerará que requiera autenticación, se rechazan las solicitudes anónimas.</span><span class="sxs-lookup"><span data-stu-id="493a6-150">Apps that do not have anonymous enabled do not require ``[Authorize]``; the  app is treated as requiring authentication, anonymous requests are rejected.</span></span> <span data-ttu-id="493a6-151">Tenga en cuenta que si se configura el sitio de IIS **no** para permitir el acceso anónimo, el ``[AllowAnonymous]`` atributo hace **no** permitir solicitudes anónimas.</span><span class="sxs-lookup"><span data-stu-id="493a6-151">Note, if the IIS site is configured **not** to allow anonymous access, the ``[AllowAnonymous]`` attribute does **not** allow anonymous requests.</span></span> <span data-ttu-id="493a6-152">El ``[AllowAnonymous]`` atributo invalidaciones ``[Authorize]`` atributo uso dentro de las aplicaciones que permiten el acceso anónimo.</span><span class="sxs-lookup"><span data-stu-id="493a6-152">The ``[AllowAnonymous]`` attribute overrides ``[Authorize]`` attribute usage within apps that allow anonymous access.</span></span>

### <a name="impersonation"></a><span data-ttu-id="493a6-153">Suplantación</span><span class="sxs-lookup"><span data-stu-id="493a6-153">Impersonation</span></span>

<span data-ttu-id="493a6-154">ASP.NET Core no lleva a cabo la suplantación.</span><span class="sxs-lookup"><span data-stu-id="493a6-154">ASP.NET Core does not implement impersonation.</span></span> <span data-ttu-id="493a6-155">Las aplicaciones se ejecutan con la identidad de aplicación para todas las solicitudes, uso de la identidad de proceso o grupo de servidores de aplicación.</span><span class="sxs-lookup"><span data-stu-id="493a6-155">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="493a6-156">Si necesita realizar una acción en nombre del usuario de forma explícita, use ``WindowsIdentity.RunImpersonated``.</span><span class="sxs-lookup"><span data-stu-id="493a6-156">If you need to explicitly perform an action on behalf of a user, use ``WindowsIdentity.RunImpersonated``.</span></span> <span data-ttu-id="493a6-157">Ejecutar una sola acción en este contexto y, a continuación, cierre el contexto.</span><span class="sxs-lookup"><span data-stu-id="493a6-157">Run a single action in this context and then close the context.</span></span> <span data-ttu-id="493a6-158">Tenga en cuenta que ``RunImpersonated`` no es compatible con async y no debe usarse para escenarios complejos.</span><span class="sxs-lookup"><span data-stu-id="493a6-158">Note that ``RunImpersonated`` does not support async and should not be used for complex scenarios.</span></span> <span data-ttu-id="493a6-159">Por ejemplo, las solicitudes de todas o cadenas de middleware de ajuste no es admiten o recomendado.</span><span class="sxs-lookup"><span data-stu-id="493a6-159">For example, wrapping entire requests or middleware chains is not supported or recommended.</span></span>
