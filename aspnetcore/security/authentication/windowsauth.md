---
title: Configurar la autenticación de Windows en ASP.NET Core
author: scottaddie
description: Obtenga información sobre cómo configurar la autenticación de Windows en ASP.NET Core, usar IIS Express, IIS y HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 04/03/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 9b53f523cf579aeb0e7dd37ccf5f161269a54913
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068266"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="55171-103">Configurar la autenticación de Windows en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="55171-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="55171-104">Por [Scott Addie](https://twitter.com/Scott_Addie) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="55171-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="55171-105">[Autenticación de Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) puede configurarse para las aplicaciones de ASP.NET Core hospedadas con [IIS](xref:host-and-deploy/iis/index) o [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="55171-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="55171-106">Autenticación de Windows se basa en el sistema operativo para autenticar usuarios de aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="55171-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="55171-107">Puede usar la autenticación de Windows cuando el servidor se ejecuta en una red corporativa mediante identidades de dominio de Active Directory o cuentas de Windows para identificar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="55171-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="55171-108">Autenticación de Windows se adapta mejor a los entornos de intranet donde los usuarios, las aplicaciones cliente y servidores web pertenecen al mismo dominio de Windows.</span><span class="sxs-lookup"><span data-stu-id="55171-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="55171-109">Habilitar la autenticación de Windows en una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="55171-109">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="55171-110">El **aplicación Web** plantilla disponible a través de Visual Studio o la CLI de .NET Core puede configurarse para admitir la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="55171-110">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication.</span></span>

# [<a name="visual-studio"></a><span data-ttu-id="55171-111">Programa para la mejora</span><span class="sxs-lookup"><span data-stu-id="55171-111">Visual Studio</span></span>](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a><span data-ttu-id="55171-112">Usa la plantilla de aplicación de autenticación de Windows para un nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="55171-112">Use the Windows Authentication app template for a new project</span></span>

<span data-ttu-id="55171-113">En Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="55171-113">In Visual Studio:</span></span>

1. <span data-ttu-id="55171-114">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="55171-114">Create a new project.</span></span>
1. <span data-ttu-id="55171-115">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="55171-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="55171-116">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="55171-116">Select **Next**.</span></span>
1. <span data-ttu-id="55171-117">Proporcione un nombre en el **nombre del proyecto** campo.</span><span class="sxs-lookup"><span data-stu-id="55171-117">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="55171-118">Confirme la **ubicación** entrada es correcta o proporcionar una ubicación para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="55171-118">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="55171-119">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="55171-119">Select **Create**.</span></span>
1. <span data-ttu-id="55171-120">Seleccione **cambio** en **autenticación**.</span><span class="sxs-lookup"><span data-stu-id="55171-120">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="55171-121">En el **Cambiar autenticación** ventana, seleccione **Windows autenticación**.</span><span class="sxs-lookup"><span data-stu-id="55171-121">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="55171-122">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="55171-122">Select **OK**.</span></span>
1. <span data-ttu-id="55171-123">Seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="55171-123">Select **Web Application**.</span></span>
1. <span data-ttu-id="55171-124">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="55171-124">Select **Create**.</span></span>

<span data-ttu-id="55171-125">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="55171-125">Run the app.</span></span> <span data-ttu-id="55171-126">El nombre de usuario aparece en la interfaz de usuario de la aplicación representada.</span><span class="sxs-lookup"><span data-stu-id="55171-126">The username appears in the rendered app's user interface.</span></span>

### <a name="manual-configuration-for-an-existing-project"></a><span data-ttu-id="55171-127">Configuración manual de un proyecto existente</span><span class="sxs-lookup"><span data-stu-id="55171-127">Manual configuration for an existing project</span></span>

<span data-ttu-id="55171-128">Las propiedades del proyecto le permiten habilitar la autenticación de Windows y deshabilitar la autenticación anónima:</span><span class="sxs-lookup"><span data-stu-id="55171-128">The project's properties allow you to enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="55171-129">Haga clic en el proyecto en Visual Studio **el Explorador de soluciones** y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="55171-129">Right-click the project in Visual Studio's **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="55171-130">Seleccione la pestaña **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="55171-130">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="55171-131">Desactive la casilla de verificación **habilitar la autenticación anónima**.</span><span class="sxs-lookup"><span data-stu-id="55171-131">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="55171-132">Seleccione la casilla de verificación **habilitar la autenticación de Windows**.</span><span class="sxs-lookup"><span data-stu-id="55171-132">Select the check box for **Enable Windows Authentication**.</span></span>

<span data-ttu-id="55171-133">Como alternativa, se pueden configurar las propiedades en el `iisSettings` nodo de la *launchSettings.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="55171-133">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# [<a name="net-core-cli"></a><span data-ttu-id="55171-134">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="55171-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="55171-135">Use la **Windows autenticación** plantilla de aplicación.</span><span class="sxs-lookup"><span data-stu-id="55171-135">Use the **Windows Authentication** app template.</span></span>

<span data-ttu-id="55171-136">Ejecute el [dotnet nuevo](/dotnet/core/tools/dotnet-new) comando con el `webapp` argumento (aplicación Web de ASP.NET Core) y `--auth Windows` cambiar:</span><span class="sxs-lookup"><span data-stu-id="55171-136">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

---

<span data-ttu-id="55171-137">Cuando se modifica un proyecto existente, compruebe que el archivo de proyecto incluye una referencia de paquete para el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) **o** el [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="55171-137">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="55171-138">Habilitar la autenticación de Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="55171-138">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="55171-139">IIS usa el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) para hospedar aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="55171-139">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="55171-140">Autenticación de Windows se configura para IIS a través de la *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="55171-140">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="55171-141">Las siguientes secciones muestran cómo:</span><span class="sxs-lookup"><span data-stu-id="55171-141">The following sections show how to:</span></span>

* <span data-ttu-id="55171-142">Proporcionar una variable local *web.config* archivo que activa la autenticación de Windows en el servidor cuando se implementa la aplicación.</span><span class="sxs-lookup"><span data-stu-id="55171-142">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="55171-143">Use el Administrador de IIS para configurar el *web.config* archivos de una aplicación de ASP.NET Core que ya se ha implementado en el servidor.</span><span class="sxs-lookup"><span data-stu-id="55171-143">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="55171-144">Configuración de IIS</span><span class="sxs-lookup"><span data-stu-id="55171-144">IIS configuration</span></span>

<span data-ttu-id="55171-145">Si aún no lo ha hecho, habilite IIS para hospedar aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="55171-145">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="55171-146">Para obtener más información, consulta <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="55171-146">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="55171-147">Habilite el servicio de rol IIS para la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="55171-147">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="55171-148">Para obtener más información, consulte [Habilitar autenticación de Windows en servicios de rol de IIS (consulte el paso 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="55171-148">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="55171-149">Middleware de integración de IIS está configurado para autenticar automáticamente las solicitudes de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="55171-149">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="55171-150">Para obtener más información, consulte [Host ASP.NET Core en Windows con IIS: Las opciones de IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="55171-150">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="55171-151">El módulo ASP.NET Core está configurado para reenviar el token de autenticación de Windows para la aplicación de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="55171-151">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="55171-152">Para obtener más información, consulte [referencia de configuración del módulo ASP.NET Core: Atributos del elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="55171-152">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="55171-153">Crear un nuevo sitio IIS</span><span class="sxs-lookup"><span data-stu-id="55171-153">Create a new IIS site</span></span>

<span data-ttu-id="55171-154">Especifique un nombre y una carpeta y permitir que se cree un nuevo grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="55171-154">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="enable-windows-authentication-for-the-app-in-iis"></a><span data-ttu-id="55171-155">Habilitar la autenticación de Windows para la aplicación en IIS</span><span class="sxs-lookup"><span data-stu-id="55171-155">Enable Windows Authentication for the app in IIS</span></span>

<span data-ttu-id="55171-156">Use **cualquier** de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="55171-156">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="55171-157">[Configuración de desarrollo antes de publicar la aplicación](#development-side-configuration-with-a-local-webconfig-file) (*recomendado*)</span><span class="sxs-lookup"><span data-stu-id="55171-157">[Development-side configuration before publishing the app](#development-side-configuration-with-a-local-webconfig-file) (*Recommended*)</span></span>
* [<span data-ttu-id="55171-158">Configuración del servidor después de publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="55171-158">Server-side configuration after publishing the app</span></span>](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a><span data-ttu-id="55171-159">Configuración de desarrollo con un archivo local web.config</span><span class="sxs-lookup"><span data-stu-id="55171-159">Development-side configuration with a local web.config file</span></span>

<span data-ttu-id="55171-160">Realice los pasos siguientes **antes** [publicar e implementar el proyecto](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="55171-160">Perform the following steps **before** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

<span data-ttu-id="55171-161">Agregue el siguiente *web.config* archivo a la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="55171-161">Add the following *web.config* file to the project root:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

<span data-ttu-id="55171-162">Cuando se publica el proyecto mediante el SDK (sin el `<IsTransformWebConfigDisabled>` propiedad establecida en `true` en el archivo de proyecto), publicado *web.config* archivo incluye la `<location><system.webServer><security><authentication>` sección.</span><span class="sxs-lookup"><span data-stu-id="55171-162">When the project is published by the SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="55171-163">Para obtener más información sobre la `<IsTransformWebConfigDisabled>` propiedad, vea <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="55171-163">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

#### <a name="server-side-configuration-with-the-iis-manager"></a><span data-ttu-id="55171-164">Configuración del servidor con el Administrador de IIS</span><span class="sxs-lookup"><span data-stu-id="55171-164">Server-side configuration with the IIS Manager</span></span>

<span data-ttu-id="55171-165">Realice los pasos siguientes **después** [publicar e implementar el proyecto](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="55171-165">Perform the following steps **after** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

1. <span data-ttu-id="55171-166">En el Administrador de IIS, seleccione el sitio IIS en el **sitios** nodo de la **conexiones** barra lateral.</span><span class="sxs-lookup"><span data-stu-id="55171-166">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
1. <span data-ttu-id="55171-167">Haga doble clic en **autenticación** en el **IIS** área.</span><span class="sxs-lookup"><span data-stu-id="55171-167">Double-click **Authentication** in the **IIS** area.</span></span>
1. <span data-ttu-id="55171-168">Seleccione **autenticación anónima**.</span><span class="sxs-lookup"><span data-stu-id="55171-168">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="55171-169">Seleccione **deshabilitar** en el **acciones** barra lateral.</span><span class="sxs-lookup"><span data-stu-id="55171-169">Select **Disable** in the **Actions** sidebar.</span></span>
1. <span data-ttu-id="55171-170">Seleccione **Windows autenticación**.</span><span class="sxs-lookup"><span data-stu-id="55171-170">Select **Windows Authentication**.</span></span> <span data-ttu-id="55171-171">Seleccione **habilitar** en el **acciones** barra lateral.</span><span class="sxs-lookup"><span data-stu-id="55171-171">Select **Enable** in the **Actions** sidebar.</span></span>

<span data-ttu-id="55171-172">Cuando se realizan estas acciones, el Administrador de IIS modifica la aplicación *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="55171-172">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="55171-173">Un `<system.webServer><security><authentication>` se agrega el nodo con la configuración actualizada para `anonymousAuthentication` y `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="55171-173">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

<span data-ttu-id="55171-174">El `<system.webServer>` sección agregada a la *web.config* archivo por el Administrador de IIS está fuera de la aplicación `<location>` sección agregada por el SDK de .NET Core cuando se publique la aplicación.</span><span class="sxs-lookup"><span data-stu-id="55171-174">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="55171-175">Dado que se agrega la sección fuera de la `<location>` nodo, la configuración se hereda por cualquier [las aplicaciones secundarias](xref:host-and-deploy/iis/index#sub-applications) a la aplicación actual.</span><span class="sxs-lookup"><span data-stu-id="55171-175">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="55171-176">Para impedir la herencia, mover agregado `<security>` sección dentro de la `<location><system.webServer>` sección suministradas por el SDK.</span><span class="sxs-lookup"><span data-stu-id="55171-176">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the SDK provided.</span></span>

<span data-ttu-id="55171-177">Cuando se usa el Administrador de IIS para agregar la configuración de IIS, solo afecta a la aplicación *web.config* archivo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="55171-177">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="55171-178">Una implementación posterior de la aplicación podría sobrescribir la configuración en el servidor si la copia del servidor de *web.config* se sustituye por el proyecto *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="55171-178">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="55171-179">Use **cualquier** de los métodos siguientes para administrar la configuración:</span><span class="sxs-lookup"><span data-stu-id="55171-179">Use **either** of the following approaches to manage the settings:</span></span>

* <span data-ttu-id="55171-180">Use el Administrador de IIS para restablecer la configuración en el *web.config* archivo después de que el archivo se sobrescribe en la implementación.</span><span class="sxs-lookup"><span data-stu-id="55171-180">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
* <span data-ttu-id="55171-181">Agregar un *archivo web.config* a la aplicación localmente con la configuración.</span><span class="sxs-lookup"><span data-stu-id="55171-181">Add a *web.config file* to the app locally with the settings.</span></span> <span data-ttu-id="55171-182">Para obtener más información, consulte el [configuración del desarrollo](#development-side-configuration-with-a-local-webconfig-file) sección.</span><span class="sxs-lookup"><span data-stu-id="55171-182">For more information, see the [Development-side configuration](#development-side-configuration-with-a-local-webconfig-file) section.</span></span>

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a><span data-ttu-id="55171-183">Publicar e implementar el proyecto a la carpeta del sitio IIS</span><span class="sxs-lookup"><span data-stu-id="55171-183">Publish and deploy your project to the IIS site folder</span></span>

<span data-ttu-id="55171-184">Con Visual Studio o la CLI de .NET Core, publicar e implementar la aplicación en la carpeta de destino.</span><span class="sxs-lookup"><span data-stu-id="55171-184">Using Visual Studio or the .NET Core CLI, publish and deploy the app to the destination folder.</span></span>

<span data-ttu-id="55171-185">Para obtener más información sobre el hospedaje con IIS, publicación e implementación, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="55171-185">For more information on hosting with IIS, publishing, and deployment, see the following topics:</span></span>

* [<span data-ttu-id="55171-186">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="55171-186">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

<span data-ttu-id="55171-187">Inicie la aplicación para comprobar que funciona la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="55171-187">Launch the app to verify Windows Authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="55171-188">Habilitar la autenticación de Windows con HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="55171-188">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="55171-189">Aunque Kestrel no admite la autenticación de Windows, puede usar [HTTP.sys](xref:fundamentals/servers/httpsys) para admitir los escenarios Auto-hospedados en Windows.</span><span class="sxs-lookup"><span data-stu-id="55171-189">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="55171-190">El ejemplo siguiente configura el host de la aplicación web para usar HTTP.sys con autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="55171-190">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="55171-191">HTTP.sys delega en la autenticación de modo kernel con el protocolo de autenticación de Kerberos.</span><span class="sxs-lookup"><span data-stu-id="55171-191">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="55171-192">La autenticación de modo usuario no se admite con Kerberos y HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="55171-192">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="55171-193">Se debe usar la cuenta de equipo para descifrar el token o el vale de Kerberos que se obtiene de Active Directory y que el cliente reenvía al servidor para autenticar al usuario.</span><span class="sxs-lookup"><span data-stu-id="55171-193">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="55171-194">Registre el nombre de entidad de seguridad de servicio (SPN) para el host, no el usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="55171-194">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="55171-195">HTTP.sys no se admite en Nano Server versión 1709 o posterior.</span><span class="sxs-lookup"><span data-stu-id="55171-195">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="55171-196">Para usar autenticación de Windows y HTTP.sys con Nano Server, use un [contenedor de Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="55171-196">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="55171-197">Para obtener más información sobre Server Core, vea [¿qué es la opción de instalación Server Core en Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="55171-197">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="work-with-windows-authentication"></a><span data-ttu-id="55171-198">Trabajar con la autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="55171-198">Work with Windows Authentication</span></span>

<span data-ttu-id="55171-199">Estado de configuración del acceso anónimo determina la manera en que el `[Authorize]` y `[AllowAnonymous]` atributos se utilizan en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="55171-199">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="55171-200">Las dos secciones siguientes explican cómo controlar los Estados de configuración de permitidos y no permitidos del acceso anónimo.</span><span class="sxs-lookup"><span data-stu-id="55171-200">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="55171-201">No permitir el acceso anónimo</span><span class="sxs-lookup"><span data-stu-id="55171-201">Disallow anonymous access</span></span>

<span data-ttu-id="55171-202">Cuando está habilitada la autenticación de Windows y acceso anónimo está deshabilitado, el `[Authorize]` y `[AllowAnonymous]` atributos no tienen ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="55171-202">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="55171-203">Si el sitio IIS (o HTTP.sys) está configurado para no permitir el acceso anónimo, la solicitud nunca llega a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="55171-203">If the IIS site (or HTTP.sys) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="55171-204">Por este motivo, la `[AllowAnonymous]` atributo no es aplicable.</span><span class="sxs-lookup"><span data-stu-id="55171-204">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="55171-205">Permitir el acceso anónimo</span><span class="sxs-lookup"><span data-stu-id="55171-205">Allow anonymous access</span></span>

<span data-ttu-id="55171-206">Cuando se habilitan tanto la autenticación de Windows como el acceso anónimo, utilice el `[Authorize]` y `[AllowAnonymous]` atributos.</span><span class="sxs-lookup"><span data-stu-id="55171-206">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="55171-207">El `[Authorize]` atributo permite proteger partes de la aplicación que realmente requiera autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="55171-207">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="55171-208">El `[AllowAnonymous]` invalidaciones de atributo `[Authorize]` atributo uso dentro de las aplicaciones que permite el acceso anónimo.</span><span class="sxs-lookup"><span data-stu-id="55171-208">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="55171-209">Consulte [autorización sencilla](xref:security/authorization/simple) para obtener detalles de uso del atributo.</span><span class="sxs-lookup"><span data-stu-id="55171-209">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="55171-210">En ASP.NET Core 2.x, el `[Authorize]` atributo requiere configuración adicional en *Startup.cs* Desafíe a las solicitudes anónimas para la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="55171-210">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="55171-211">La configuración recomendada varía ligeramente según el servidor web que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="55171-211">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="55171-212">De forma predeterminada, se presentan a los usuarios que no tienen autorización para acceder a una página con una respuesta HTTP 403 vacía.</span><span class="sxs-lookup"><span data-stu-id="55171-212">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="55171-213">El [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) puede configurarse para proporcionar a los usuarios una mejor experiencia de "Acceso denegado".</span><span class="sxs-lookup"><span data-stu-id="55171-213">The [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="55171-214">IIS</span><span class="sxs-lookup"><span data-stu-id="55171-214">IIS</span></span>

<span data-ttu-id="55171-215">Si usa IIS, agregue lo siguiente a la `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="55171-215">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="55171-216">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="55171-216">HTTP.sys</span></span>

<span data-ttu-id="55171-217">Si usa HTTP.sys, agregue lo siguiente a la `ConfigureServices` método:</span><span class="sxs-lookup"><span data-stu-id="55171-217">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="55171-218">Suplantación</span><span class="sxs-lookup"><span data-stu-id="55171-218">Impersonation</span></span>

<span data-ttu-id="55171-219">ASP.NET Core no implementa la suplantación.</span><span class="sxs-lookup"><span data-stu-id="55171-219">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="55171-220">Las aplicaciones se ejecutan con la identidad de la aplicación para todas las solicitudes, utilizando la identidad de proceso o grupo de servidores de aplicación.</span><span class="sxs-lookup"><span data-stu-id="55171-220">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="55171-221">Si necesita realizar explícitamente una acción en nombre de un usuario, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) en un [software intermedio alineado terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="55171-221">If you need to explicitly perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="55171-222">Ejecutar una sola acción en este contexto y, a continuación, cierre el contexto.</span><span class="sxs-lookup"><span data-stu-id="55171-222">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` <span data-ttu-id="55171-223">no es compatible con operaciones asincrónicas y no puede utilizarse para escenarios complejos.</span><span class="sxs-lookup"><span data-stu-id="55171-223">doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="55171-224">Por ejemplo, el ajuste de las solicitudes de todas o las cadenas de middleware no es compatible ni recomendable.</span><span class="sxs-lookup"><span data-stu-id="55171-224">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

### <a name="claims-transformations"></a><span data-ttu-id="55171-225">Transformaciones de notificaciones</span><span class="sxs-lookup"><span data-stu-id="55171-225">Claims transformations</span></span>

<span data-ttu-id="55171-226">Al hospedar con el modo en proceso IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> no se llama internamente para inicializar un usuario.</span><span class="sxs-lookup"><span data-stu-id="55171-226">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="55171-227">Por tanto, se usa una implementación de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> para transformar las notificaciones después de que cada autenticación no se active de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="55171-227">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="55171-228">Para obtener más información y un ejemplo de código que activa las transformaciones de notificaciones cuando se hospedan en proceso, consulte <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="55171-228">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>
