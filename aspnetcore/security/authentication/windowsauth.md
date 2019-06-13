---
title: Configurar la autenticación de Windows en ASP.NET Core
author: scottaddie
description: Obtenga información sobre cómo configurar la autenticación de Windows en ASP.NET Core para IIS y HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/12/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 93f833adff95f25d570947cd1a9035d652f522c2
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034952"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="26ebf-103">Configurar la autenticación de Windows en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="26ebf-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="26ebf-104">Por [Scott Addie](https://twitter.com/Scott_Addie) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="26ebf-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="26ebf-105">Se puede configurar la autenticación de Windows (también conocida como autenticación Negotiate, Kerberos o NTLM) para las aplicaciones de ASP.NET Core hospedadas con [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), o [HTTP.sys](xref:fundamentals/servers/httpsys) .</span><span class="sxs-lookup"><span data-stu-id="26ebf-105">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="26ebf-106">Se puede configurar la autenticación de Windows (también conocida como autenticación Negotiate, Kerberos o NTLM) para las aplicaciones de ASP.NET Core hospedadas con [IIS](xref:host-and-deploy/iis/index) o [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="26ebf-106">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

<span data-ttu-id="26ebf-107">Autenticación de Windows se basa en el sistema operativo para autenticar usuarios de aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="26ebf-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="26ebf-108">Puede usar la autenticación de Windows cuando el servidor se ejecuta en una red corporativa mediante identidades de dominio de Active Directory o cuentas de Windows para identificar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="26ebf-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="26ebf-109">Autenticación de Windows se adapta mejor a los entornos de intranet donde los usuarios, las aplicaciones cliente y servidores web pertenecen al mismo dominio de Windows.</span><span class="sxs-lookup"><span data-stu-id="26ebf-109">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

> [!NOTE]
> <span data-ttu-id="26ebf-110">No se admite la autenticación de Windows con HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="26ebf-110">Windows Authentication isn't supported with HTTP/2.</span></span> <span data-ttu-id="26ebf-111">Desafíos de autenticación se pueden enviar en las respuestas HTTP/2, pero el cliente debe cambiar a HTTP/1.1 antes de autenticar.</span><span class="sxs-lookup"><span data-stu-id="26ebf-111">Authentication challenges can be sent on HTTP/2 responses, but the client must downgrade to HTTP/1.1 before authenticating.</span></span>

## <a name="iisiis-express"></a><span data-ttu-id="26ebf-112">IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="26ebf-112">IIS/IIS Express</span></span>

<span data-ttu-id="26ebf-113">Agregar servicios de autenticación mediante la invocación <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> espacio de nombres) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="26ebf-113">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a><span data-ttu-id="26ebf-114">Iniciar configuración (depurador)</span><span class="sxs-lookup"><span data-stu-id="26ebf-114">Launch settings (debugger)</span></span>

<span data-ttu-id="26ebf-115">Solo afecta la configuración para la configuración de inicio la *Properties/launchSettings.json* de archivos para IIS Express y no configura IIS para la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="26ebf-115">Configuration for launch settings only affects the *Properties/launchSettings.json* file for IIS Express and doesn't configure IIS for Windows Authentication.</span></span> <span data-ttu-id="26ebf-116">Configuración del servidor se explica en el [IIS](#iis) sección.</span><span class="sxs-lookup"><span data-stu-id="26ebf-116">Server configuration is explained in the [IIS](#iis) section.</span></span>

<span data-ttu-id="26ebf-117">El **aplicación Web** plantilla disponible a través de Visual Studio o la CLI de .NET Core puede configurarse para admitir la autenticación de Windows, que actualiza el *Properties/launchSettings.json* archivo de forma automática.</span><span class="sxs-lookup"><span data-stu-id="26ebf-117">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="26ebf-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="26ebf-118">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="26ebf-119">**nuevo proyecto**</span><span class="sxs-lookup"><span data-stu-id="26ebf-119">**New project**</span></span>

1. <span data-ttu-id="26ebf-120">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="26ebf-120">Create a new project.</span></span>
1. <span data-ttu-id="26ebf-121">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="26ebf-121">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="26ebf-122">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="26ebf-122">Select **Next**.</span></span>
1. <span data-ttu-id="26ebf-123">Proporcione un nombre en el **nombre del proyecto** campo.</span><span class="sxs-lookup"><span data-stu-id="26ebf-123">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="26ebf-124">Confirme la **ubicación** entrada es correcta o proporcionar una ubicación para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="26ebf-124">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="26ebf-125">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="26ebf-125">Select **Create**.</span></span>
1. <span data-ttu-id="26ebf-126">Seleccione **cambio** en **autenticación**.</span><span class="sxs-lookup"><span data-stu-id="26ebf-126">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="26ebf-127">En el **Cambiar autenticación** ventana, seleccione **Windows autenticación**.</span><span class="sxs-lookup"><span data-stu-id="26ebf-127">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="26ebf-128">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="26ebf-128">Select **OK**.</span></span>
1. <span data-ttu-id="26ebf-129">Seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="26ebf-129">Select **Web Application**.</span></span>
1. <span data-ttu-id="26ebf-130">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="26ebf-130">Select **Create**.</span></span>

<span data-ttu-id="26ebf-131">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ebf-131">Run the app.</span></span> <span data-ttu-id="26ebf-132">El nombre de usuario aparece en la interfaz de usuario de la aplicación representada.</span><span class="sxs-lookup"><span data-stu-id="26ebf-132">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="26ebf-133">**Proyecto existente**</span><span class="sxs-lookup"><span data-stu-id="26ebf-133">**Existing project**</span></span>

<span data-ttu-id="26ebf-134">Las propiedades del proyecto habilitar la autenticación de Windows y deshabilitar la autenticación anónima:</span><span class="sxs-lookup"><span data-stu-id="26ebf-134">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="26ebf-135">Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="26ebf-135">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="26ebf-136">Seleccione la pestaña **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="26ebf-136">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="26ebf-137">Desactive la casilla de verificación **habilitar la autenticación anónima**.</span><span class="sxs-lookup"><span data-stu-id="26ebf-137">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="26ebf-138">Seleccione la casilla de verificación **habilitar la autenticación de Windows**.</span><span class="sxs-lookup"><span data-stu-id="26ebf-138">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="26ebf-139">Guarde y cierre la página de propiedades.</span><span class="sxs-lookup"><span data-stu-id="26ebf-139">Save and close the property page.</span></span>

<span data-ttu-id="26ebf-140">Como alternativa, se pueden configurar las propiedades en el `iisSettings` nodo de la *launchSettings.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="26ebf-140">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="26ebf-141">Visual Studio Code y CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="26ebf-141">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="26ebf-142">**nuevo proyecto**</span><span class="sxs-lookup"><span data-stu-id="26ebf-142">**New project**</span></span>

<span data-ttu-id="26ebf-143">Ejecute el [dotnet nuevo](/dotnet/core/tools/dotnet-new) comando con el `webapp` argumento (aplicación Web de ASP.NET Core) y `--auth Windows` cambiar:</span><span class="sxs-lookup"><span data-stu-id="26ebf-143">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="26ebf-144">**Proyecto existente**</span><span class="sxs-lookup"><span data-stu-id="26ebf-144">**Existing project**</span></span>

<span data-ttu-id="26ebf-145">Actualización de la `iisSettings` nodo de la *launchSettings.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="26ebf-145">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="26ebf-146">Cuando se modifica un proyecto existente, compruebe que el archivo de proyecto incluye una referencia de paquete para el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) **o** el [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="26ebf-146">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

### <a name="iis"></a><span data-ttu-id="26ebf-147">IIS</span><span class="sxs-lookup"><span data-stu-id="26ebf-147">IIS</span></span>

<span data-ttu-id="26ebf-148">IIS usa el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) para hospedar aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="26ebf-148">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="26ebf-149">Autenticación de Windows se configura para IIS a través de la *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="26ebf-149">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="26ebf-150">Las siguientes secciones muestran cómo:</span><span class="sxs-lookup"><span data-stu-id="26ebf-150">The following sections show how to:</span></span>

* <span data-ttu-id="26ebf-151">Proporcionar una variable local *web.config* archivo que activa la autenticación de Windows en el servidor cuando se implementa la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ebf-151">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="26ebf-152">Use el Administrador de IIS para configurar el *web.config* archivos de una aplicación de ASP.NET Core que ya se ha implementado en el servidor.</span><span class="sxs-lookup"><span data-stu-id="26ebf-152">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="26ebf-153">Si aún no lo ha hecho, habilite IIS para hospedar aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="26ebf-153">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="26ebf-154">Para obtener más información, consulta <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="26ebf-154">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="26ebf-155">Habilite el servicio de rol IIS para la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="26ebf-155">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="26ebf-156">Para obtener más información, consulte [Habilitar autenticación de Windows en servicios de rol de IIS (consulte el paso 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="26ebf-156">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="26ebf-157">[Middleware de IIS Integration](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) está configurado para autenticar automáticamente las solicitudes de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="26ebf-157">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="26ebf-158">Para obtener más información, consulte [Host ASP.NET Core en Windows con IIS: Las opciones de IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="26ebf-158">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="26ebf-159">El módulo ASP.NET Core está configurado para reenviar el token de autenticación de Windows para la aplicación de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="26ebf-159">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="26ebf-160">Para obtener más información, consulte [referencia de configuración del módulo ASP.NET Core: Atributos del elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="26ebf-160">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="26ebf-161">Use **cualquier** de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="26ebf-161">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="26ebf-162">**Antes de publicar e implementar el proyecto,** agregue las siguientes *web.config* archivo a la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="26ebf-162">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="26ebf-163">Cuando se publica el proyecto mediante el SDK de .NET Core (sin el `<IsTransformWebConfigDisabled>` propiedad establecida en `true` en el archivo de proyecto), publicado *web.config* archivo incluye la `<location><system.webServer><security><authentication>` sección.</span><span class="sxs-lookup"><span data-stu-id="26ebf-163">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="26ebf-164">Para obtener más información sobre la `<IsTransformWebConfigDisabled>` propiedad, vea <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="26ebf-164">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="26ebf-165">**Después de publicar e implementar el proyecto,** realizar la configuración de servidor con el Administrador de IIS:</span><span class="sxs-lookup"><span data-stu-id="26ebf-165">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="26ebf-166">En el Administrador de IIS, seleccione el sitio IIS en el **sitios** nodo de la **conexiones** barra lateral.</span><span class="sxs-lookup"><span data-stu-id="26ebf-166">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="26ebf-167">Haga doble clic en **autenticación** en el **IIS** área.</span><span class="sxs-lookup"><span data-stu-id="26ebf-167">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="26ebf-168">Seleccione **autenticación anónima**.</span><span class="sxs-lookup"><span data-stu-id="26ebf-168">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="26ebf-169">Seleccione **deshabilitar** en el **acciones** barra lateral.</span><span class="sxs-lookup"><span data-stu-id="26ebf-169">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="26ebf-170">Seleccione **Windows autenticación**.</span><span class="sxs-lookup"><span data-stu-id="26ebf-170">Select **Windows Authentication**.</span></span> <span data-ttu-id="26ebf-171">Seleccione **habilitar** en el **acciones** barra lateral.</span><span class="sxs-lookup"><span data-stu-id="26ebf-171">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="26ebf-172">Cuando se realizan estas acciones, el Administrador de IIS modifica la aplicación *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="26ebf-172">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="26ebf-173">Un `<system.webServer><security><authentication>` se agrega el nodo con la configuración actualizada para `anonymousAuthentication` y `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="26ebf-173">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="26ebf-174">El `<system.webServer>` sección agregada a la *web.config* archivo por el Administrador de IIS está fuera de la aplicación `<location>` sección agregada por el SDK de .NET Core cuando se publique la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ebf-174">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="26ebf-175">Dado que se agrega la sección fuera de la `<location>` nodo, la configuración se hereda por cualquier [las aplicaciones secundarias](xref:host-and-deploy/iis/index#sub-applications) a la aplicación actual.</span><span class="sxs-lookup"><span data-stu-id="26ebf-175">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="26ebf-176">Para impedir la herencia, mover agregado `<security>` sección dentro de la `<location><system.webServer>` sección suministradas por el SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="26ebf-176">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="26ebf-177">Cuando se usa el Administrador de IIS para agregar la configuración de IIS, solo afecta a la aplicación *web.config* archivo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="26ebf-177">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="26ebf-178">Una implementación posterior de la aplicación podría sobrescribir la configuración en el servidor si la copia del servidor de *web.config* se sustituye por el proyecto *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="26ebf-178">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="26ebf-179">Use **cualquier** de los métodos siguientes para administrar la configuración:</span><span class="sxs-lookup"><span data-stu-id="26ebf-179">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="26ebf-180">Use el Administrador de IIS para restablecer la configuración en el *web.config* archivo después de que el archivo se sobrescribe en la implementación.</span><span class="sxs-lookup"><span data-stu-id="26ebf-180">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="26ebf-181">Agregar un *archivo web.config* a la aplicación localmente con la configuración.</span><span class="sxs-lookup"><span data-stu-id="26ebf-181">Add a *web.config file* to the app locally with the settings.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a><span data-ttu-id="26ebf-182">Kestrel</span><span class="sxs-lookup"><span data-stu-id="26ebf-182">Kestrel</span></span>

 <span data-ttu-id="26ebf-183">El [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) se puede usar el paquete NuGet con [Kestrel](xref:fundamentals/servers/kestrel) para admitir la autenticación de Windows mediante Negotiate, Kerberos y NTLM en Windows, Linux y macOS.</span><span class="sxs-lookup"><span data-stu-id="26ebf-183">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) NuGet package can be used with [Kestrel](xref:fundamentals/servers/kestrel) to support Windows Authentication using Negotiate, Kerberos, and NTLM on Windows, Linux, and macOS.</span></span>

> [!WARNING]
> <span data-ttu-id="26ebf-184">Las credenciales pueden conservarse entre las solicitudes en una conexión.</span><span class="sxs-lookup"><span data-stu-id="26ebf-184">Credentials can be persisted across requests on a connection.</span></span> <span data-ttu-id="26ebf-185">*Negociar la autenticación no debe usarse con servidores proxy a menos que el proxy mantiene una afinidad de conexión de 1:1 (una conexión persistente) con Kestrel.*</span><span class="sxs-lookup"><span data-stu-id="26ebf-185">*Negotiate authentication must not be used with proxies unless the proxy maintains a 1:1 connection affinity (a persistent connection) with Kestrel.*</span></span> <span data-ttu-id="26ebf-186">Esto significa que no se debe usar autenticación de negociación con Kestrel detrás de IIS [fuera de proceso de módulo de ASP.NET Core (ANCM)](xref:host-and-deploy/iis/index#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="26ebf-186">This means that Negotiate authentication must not be used with Kestrel behind the IIS [ASP.NET Core Module (ANCM) out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model).</span></span>

 <span data-ttu-id="26ebf-187">Agregar servicios de autenticación mediante la invocación <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` espacio de nombres) y `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` espacio de nombres) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="26ebf-187">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` namespace) and `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` namespace) in `Startup.ConfigureServices`:</span></span>

 ```csharp
services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

<span data-ttu-id="26ebf-188">Agregue el Middleware de autenticación mediante una llamada a <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="26ebf-188">Add Authentication Middleware by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> in `Startup.Configure`:</span></span>

 ```csharp
app.UseAuthentication();

app.UseMvc();
```

<span data-ttu-id="26ebf-189">Para obtener más información sobre el software intermedio, consulte <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="26ebf-189">For more information on middleware, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="26ebf-190">Se permiten las solicitudes anónimas.</span><span class="sxs-lookup"><span data-stu-id="26ebf-190">Anonymous requests are allowed.</span></span> <span data-ttu-id="26ebf-191">Use [autorización de ASP.NET Core](xref:security/authorization/introduction) a las solicitudes anónimas para la autenticación de desafío.</span><span class="sxs-lookup"><span data-stu-id="26ebf-191">Use [ASP.NET Core Authorization](xref:security/authorization/introduction) to challenge anonymous requests for authentication.</span></span>

### <a name="windows-environment-configuration"></a><span data-ttu-id="26ebf-192">Configuración del entorno de Windows</span><span class="sxs-lookup"><span data-stu-id="26ebf-192">Windows environment configuration</span></span>

<span data-ttu-id="26ebf-193">El [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) componente realiza la autenticación de modo de usuario.</span><span class="sxs-lookup"><span data-stu-id="26ebf-193">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) component performs User Mode authentication.</span></span> <span data-ttu-id="26ebf-194">Nombres de entidad de servicio (SPN) debe agregarse a la cuenta de usuario que ejecuta el servicio, no la cuenta de equipo.</span><span class="sxs-lookup"><span data-stu-id="26ebf-194">Service Principal Names (SPNs) must be added to the user account running the service, not the machine account.</span></span> <span data-ttu-id="26ebf-195">Ejecutar `setspn -S HTTP/mysrevername.mydomain.com myuser` en un shell de comandos administrativas.</span><span class="sxs-lookup"><span data-stu-id="26ebf-195">Execute `setspn -S HTTP/mysrevername.mydomain.com myuser` in an administrative command shell.</span></span>

### <a name="linux-and-macos-environment-configuration"></a><span data-ttu-id="26ebf-196">Configuración del entorno de Linux y macOS</span><span class="sxs-lookup"><span data-stu-id="26ebf-196">Linux and macOS environment configuration</span></span>

<span data-ttu-id="26ebf-197">Las instrucciones para unir una máquina Linux o macOS a un dominio de Windows están disponibles en el [conectar Studio datos de Azure a SQL Server mediante la autenticación de Windows - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) artículo.</span><span class="sxs-lookup"><span data-stu-id="26ebf-197">Instructions for joining a Linux or macOS machine to a Windows domain are available in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article.</span></span> <span data-ttu-id="26ebf-198">Las instrucciones de creación una cuenta de equipo para la máquina de Linux en el dominio.</span><span class="sxs-lookup"><span data-stu-id="26ebf-198">The instructions create a machine account for the Linux machine on the domain.</span></span> <span data-ttu-id="26ebf-199">Los SPN deben agregarse a esa cuenta de equipo.</span><span class="sxs-lookup"><span data-stu-id="26ebf-199">SPNs must be added to that machine account.</span></span>

> [!NOTE]
> <span data-ttu-id="26ebf-200">Al seguir las instrucciones de la [conectar Studio datos de Azure a SQL Server mediante la autenticación de Windows - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) artículo, reemplace `python-software-properties` con `python3-software-properties` si es necesario.</span><span class="sxs-lookup"><span data-stu-id="26ebf-200">When following the guidance in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article, replace `python-software-properties` with `python3-software-properties` if needed.</span></span>

<span data-ttu-id="26ebf-201">Una vez que la máquina Linux o macOS está unida al dominio, se requieren pasos adicionales para proporcionar un [archivo keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) con los SPN:</span><span class="sxs-lookup"><span data-stu-id="26ebf-201">Once the Linux or macOS machine is joined to the domain, additional steps are required to provide a [keytab file](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) with the SPNs:</span></span>

* <span data-ttu-id="26ebf-202">En el controlador de dominio, agregue SPN del servicio web nuevo a la cuenta de equipo:</span><span class="sxs-lookup"><span data-stu-id="26ebf-202">On the domain controller, add new web service SPNs to the machine account:</span></span>
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* <span data-ttu-id="26ebf-203">Use [ktpass](/windows-server/administration/windows-commands/ktpass) para generar un archivo keytab:</span><span class="sxs-lookup"><span data-stu-id="26ebf-203">Use [ktpass](/windows-server/administration/windows-commands/ktpass) to generate a keytab file:</span></span>
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * <span data-ttu-id="26ebf-204">Algunos campos deben especificarse en mayúsculas como se indica.</span><span class="sxs-lookup"><span data-stu-id="26ebf-204">Some fields must be specified in uppercase as indicated.</span></span>
* <span data-ttu-id="26ebf-205">Copie el archivo keytab en el equipo Linux o macOS.</span><span class="sxs-lookup"><span data-stu-id="26ebf-205">Copy the keytab file to the Linux or macOS machine.</span></span>
* <span data-ttu-id="26ebf-206">Seleccione el archivo keytab a través de una variable de entorno: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span><span class="sxs-lookup"><span data-stu-id="26ebf-206">Select the keytab file via an environment variable: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span></span>
* <span data-ttu-id="26ebf-207">Invocar `klist` para mostrar los SPN disponibles actualmente para su uso.</span><span class="sxs-lookup"><span data-stu-id="26ebf-207">Invoke `klist` to show the SPNs currently available for use.</span></span>

> [!NOTE]
> <span data-ttu-id="26ebf-208">Un archivo keytab contiene las credenciales de acceso de dominio y debe protegerse en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="26ebf-208">A keytab file contains domain access credentials and must be protected accordingly.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="26ebf-209">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="26ebf-209">HTTP.sys</span></span>

<span data-ttu-id="26ebf-210">[HTTP.sys](xref:fundamentals/servers/httpsys) admite la autenticación de Windows de modo Kernel con Negotiate, NTLM o autenticación básica.</span><span class="sxs-lookup"><span data-stu-id="26ebf-210">[HTTP.sys](xref:fundamentals/servers/httpsys) supports Kernel Mode Windows Authentication using Negotiate, NTLM, or Basic authentication.</span></span>

<span data-ttu-id="26ebf-211">Agregar servicios de autenticación mediante la invocación <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> espacio de nombres) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="26ebf-211">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="26ebf-212">Configurar el host de la aplicación web para usar HTTP.sys con autenticación de Windows (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="26ebf-212">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="26ebf-213"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> en el <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="26ebf-213"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="26ebf-214">HTTP.sys delega en la autenticación de modo kernel con el protocolo de autenticación de Kerberos.</span><span class="sxs-lookup"><span data-stu-id="26ebf-214">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="26ebf-215">La autenticación de modo usuario no se admite con Kerberos y HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="26ebf-215">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="26ebf-216">Se debe usar la cuenta de equipo para descifrar el token o el vale de Kerberos que se obtiene de Active Directory y que el cliente reenvía al servidor para autenticar al usuario.</span><span class="sxs-lookup"><span data-stu-id="26ebf-216">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="26ebf-217">Registre el nombre de entidad de seguridad de servicio (SPN) para el host, no el usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ebf-217">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="26ebf-218">HTTP.sys no se admite en Nano Server versión 1709 o posterior.</span><span class="sxs-lookup"><span data-stu-id="26ebf-218">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="26ebf-219">Para usar autenticación de Windows y HTTP.sys con Nano Server, use un [contenedor de Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="26ebf-219">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="26ebf-220">Para obtener más información sobre Server Core, vea [¿qué es la opción de instalación Server Core en Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="26ebf-220">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="26ebf-221">Autorizar a los usuarios</span><span class="sxs-lookup"><span data-stu-id="26ebf-221">Authorize users</span></span>

<span data-ttu-id="26ebf-222">Estado de configuración del acceso anónimo determina la manera en que el `[Authorize]` y `[AllowAnonymous]` atributos se utilizan en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ebf-222">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="26ebf-223">Las dos secciones siguientes explican cómo controlar los Estados de configuración de permitidos y no permitidos del acceso anónimo.</span><span class="sxs-lookup"><span data-stu-id="26ebf-223">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="26ebf-224">No permitir el acceso anónimo</span><span class="sxs-lookup"><span data-stu-id="26ebf-224">Disallow anonymous access</span></span>

<span data-ttu-id="26ebf-225">Cuando está habilitada la autenticación de Windows y acceso anónimo está deshabilitado, el `[Authorize]` y `[AllowAnonymous]` atributos no tienen ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="26ebf-225">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="26ebf-226">Si un sitio de IIS está configurado para no permitir el acceso anónimo, la solicitud nunca llega a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ebf-226">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="26ebf-227">Por este motivo, la `[AllowAnonymous]` atributo no es aplicable.</span><span class="sxs-lookup"><span data-stu-id="26ebf-227">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="26ebf-228">Permitir el acceso anónimo</span><span class="sxs-lookup"><span data-stu-id="26ebf-228">Allow anonymous access</span></span>

<span data-ttu-id="26ebf-229">Cuando se habilitan tanto la autenticación de Windows como el acceso anónimo, utilice el `[Authorize]` y `[AllowAnonymous]` atributos.</span><span class="sxs-lookup"><span data-stu-id="26ebf-229">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="26ebf-230">El `[Authorize]` atributo permite proteger los puntos de conexión de la aplicación que requieren autenticación.</span><span class="sxs-lookup"><span data-stu-id="26ebf-230">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="26ebf-231">El `[AllowAnonymous]` atributo invalida la `[Authorize]` atributo en las aplicaciones que permiten el acceso anónimo.</span><span class="sxs-lookup"><span data-stu-id="26ebf-231">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="26ebf-232">Para obtener detalles de uso de atributo, vea <xref:security/authorization/simple>.</span><span class="sxs-lookup"><span data-stu-id="26ebf-232">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="26ebf-233">De forma predeterminada, se presentan a los usuarios que no tienen autorización para acceder a una página con una respuesta HTTP 403 vacía.</span><span class="sxs-lookup"><span data-stu-id="26ebf-233">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="26ebf-234">El [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) puede configurarse para proporcionar a los usuarios una mejor experiencia de "Acceso denegado".</span><span class="sxs-lookup"><span data-stu-id="26ebf-234">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="26ebf-235">Suplantación</span><span class="sxs-lookup"><span data-stu-id="26ebf-235">Impersonation</span></span>

<span data-ttu-id="26ebf-236">ASP.NET Core no implementa la suplantación.</span><span class="sxs-lookup"><span data-stu-id="26ebf-236">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="26ebf-237">Las aplicaciones se ejecutan con la identidad de la aplicación para todas las solicitudes, utilizando la identidad de proceso o grupo de servidores de aplicación.</span><span class="sxs-lookup"><span data-stu-id="26ebf-237">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="26ebf-238">Si la aplicación debe realizar una acción en nombre de un usuario, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) en un [software intermedio alineado terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="26ebf-238">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="26ebf-239">Ejecutar una sola acción en este contexto y, a continuación, cierre el contexto.</span><span class="sxs-lookup"><span data-stu-id="26ebf-239">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="26ebf-240">`RunImpersonated` no es compatible con operaciones asincrónicas y no puede utilizarse para escenarios complejos.</span><span class="sxs-lookup"><span data-stu-id="26ebf-240">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="26ebf-241">Por ejemplo, el ajuste de las solicitudes de todas o las cadenas de middleware no es compatible ni recomendable.</span><span class="sxs-lookup"><span data-stu-id="26ebf-241">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="26ebf-242">Mientras el [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) paquete habilita la autenticación de Windows, Linux y macOS, suplantación solo se admite en Windows.</span><span class="sxs-lookup"><span data-stu-id="26ebf-242">While the [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package enables authentication on Windows, Linux, and macOS, impersonation is only supported on Windows.</span></span>

::: moniker-end

## <a name="claims-transformations"></a><span data-ttu-id="26ebf-243">Transformaciones de notificaciones</span><span class="sxs-lookup"><span data-stu-id="26ebf-243">Claims transformations</span></span>

<span data-ttu-id="26ebf-244">Al hospedar con el modo en proceso IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> no se llama internamente para inicializar un usuario.</span><span class="sxs-lookup"><span data-stu-id="26ebf-244">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="26ebf-245">Por tanto, se usa una implementación de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> para transformar las notificaciones después de que cada autenticación no se active de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="26ebf-245">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="26ebf-246">Para obtener más información y un ejemplo de código que activa las transformaciones de notificaciones cuando se hospedan en proceso, consulte <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="26ebf-246">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="26ebf-247">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="26ebf-247">Additional resources</span></span>

* [<span data-ttu-id="26ebf-248">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="26ebf-248">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
