---
title: Hospedaje de ASP.NET Core en Windows con IIS
author: guardrex
description: "Obtenga información sobre cómo hospedar aplicaciones de ASP.NET Core en Windows Server Internet Information Services (IIS)."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/index
ms.openlocfilehash: 1df438af2394f41b686413cd1ce5ad73a9416ec5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="942a2-103">Hospedaje de ASP.NET Core en Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="942a2-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="942a2-104">Por [Luke Latham](https://github.com/guardrex) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="942a2-104">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="942a2-105">Sistemas operativos admitidos</span><span class="sxs-lookup"><span data-stu-id="942a2-105">Supported operating systems</span></span>

<span data-ttu-id="942a2-106">Los siguientes sistemas operativos son compatibles:</span><span class="sxs-lookup"><span data-stu-id="942a2-106">The following operating systems are supported:</span></span>

* <span data-ttu-id="942a2-107">Windows 7 y más recientes</span><span class="sxs-lookup"><span data-stu-id="942a2-107">Windows 7 and newer</span></span>
* <span data-ttu-id="942a2-108">Windows Server 2008 R2 y más recientes&#8224;</span><span class="sxs-lookup"><span data-stu-id="942a2-108">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="942a2-109">&#8224;Conceptualmente, la configuración de IIS que se describe en este documento también se aplica al hospedaje de aplicaciones de ASP.NET Core en IIS de Nano Server, pero vea [ASP.NET Core con IIS en Nano Server](xref:tutorials/nano-server) para obtener instrucciones concretas.</span><span class="sxs-lookup"><span data-stu-id="942a2-109">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="942a2-110">El [servidor HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominado [WebListener](xref:fundamentals/servers/weblistener)) no funciona en una configuración de proxy inverso con IIS.</span><span class="sxs-lookup"><span data-stu-id="942a2-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) won't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="942a2-111">Use el [servidor Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="942a2-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="942a2-112">Configuración de IIS</span><span class="sxs-lookup"><span data-stu-id="942a2-112">IIS configuration</span></span>

<span data-ttu-id="942a2-113">Habilite el rol **Servidor web (IIS)** y establezca los servicios de rol.</span><span class="sxs-lookup"><span data-stu-id="942a2-113">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="942a2-114">Sistemas operativos de escritorio Windows</span><span class="sxs-lookup"><span data-stu-id="942a2-114">Windows desktop operating systems</span></span>

<span data-ttu-id="942a2-115">Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="942a2-115">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="942a2-116">Abra el grupo de **Internet Information Services** y **Herramientas de administración web**.</span><span class="sxs-lookup"><span data-stu-id="942a2-116">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="942a2-117">Active la casilla de **Consola de administración de IIS**.</span><span class="sxs-lookup"><span data-stu-id="942a2-117">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="942a2-118">Active la casilla de **Servicios World Wide Web**.</span><span class="sxs-lookup"><span data-stu-id="942a2-118">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="942a2-119">Acepte las características predeterminadas de **Servicios World Wide Web** o personalice las características de IIS.</span><span class="sxs-lookup"><span data-stu-id="942a2-119">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

![Consola de administración de IIS y Servicios World Wide Web se activan en Características de Windows.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="942a2-121">Sistemas operativos de servidor Windows</span><span class="sxs-lookup"><span data-stu-id="942a2-121">Windows Server operating systems</span></span>

<span data-ttu-id="942a2-122">En los sistemas operativos de servidor, use el asistente **Agregar roles y características** a través del menú **Administrar** o el vínculo de **Administrador del servidor**.</span><span class="sxs-lookup"><span data-stu-id="942a2-122">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="942a2-123">En el paso **Roles de servidor**, active la casilla de **Servidor web (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="942a2-123">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![El rol Servidor web (IIS) se activa en el paso Seleccionar roles de servidor.](index/_static/server-roles-ws2016.png)

<span data-ttu-id="942a2-125">En el paso **Servicios de rol**, seleccione los servicios de rol IIS que quiera o acepte los servicios de rol predeterminados proporcionados.</span><span class="sxs-lookup"><span data-stu-id="942a2-125">On the **Role services** step, select the IIS role services desired or accept the default role services provided.</span></span>

![Los servicios de rol predeterminados se activan en el paso Seleccionar servicios de rol.](index/_static/role-services-ws2016.png)

<span data-ttu-id="942a2-127">Continúe con el paso **Confirmación** para instalar el rol y los servicios de servidor web.</span><span class="sxs-lookup"><span data-stu-id="942a2-127">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="942a2-128">No es necesario reiniciar el servidor ni IIS después de instalar el rol Servidor web (IIS).</span><span class="sxs-lookup"><span data-stu-id="942a2-128">A server/IIS restart isn't required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="942a2-129">Instalación del lote de hospedaje .NET Core Windows Server</span><span class="sxs-lookup"><span data-stu-id="942a2-129">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="942a2-130">Instale el [lote de hospedaje .NET Core Windows Server](https://aka.ms/dotnetcore-2-windowshosting) en el sistema host.</span><span class="sxs-lookup"><span data-stu-id="942a2-130">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on the hosting system.</span></span> <span data-ttu-id="942a2-131">El lote instala .NET Core Runtime, .NET Core Library y el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="942a2-131">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="942a2-132">El módulo crea el proxy inverso entre IIS y el servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="942a2-132">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="942a2-133">Si el sistema no tiene conexión a Internet, obtenga e instale [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) antes de instalar el lote de hospedaje .NET Core Windows Server.</span><span class="sxs-lookup"><span data-stu-id="942a2-133">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="942a2-134">Reinicie el sistema o ejecute **net stop was /y** seguido de **net start w3svc** desde un símbolo del sistema para obtener un cambio en la ruta de acceso del sistema.</span><span class="sxs-lookup"><span data-stu-id="942a2-134">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="942a2-135">Para obtener información sobre la configuración compartida de IIS, vea [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration) (Módulo de ASP.NET Core con configuración compartida de IIS).</span><span class="sxs-lookup"><span data-stu-id="942a2-135">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="942a2-136">Instalación de Web Deploy al publicar con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="942a2-136">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="942a2-137">Al implementar aplicaciones en servidores con [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), instale la versión más reciente de Web Deploy en el servidor.</span><span class="sxs-lookup"><span data-stu-id="942a2-137">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="942a2-138">Para instalar Web Deploy, use el [Instalador de plataforma web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) u obtenga un instalador directamente desde el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="942a2-138">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="942a2-139">El método preferido es usar WebPI.</span><span class="sxs-lookup"><span data-stu-id="942a2-139">The preferred method is to use WebPI.</span></span> <span data-ttu-id="942a2-140">WebPI ofrece una instalación independiente y una configuración para los proveedores de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="942a2-140">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="942a2-141">Configuración de aplicación</span><span class="sxs-lookup"><span data-stu-id="942a2-141">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="942a2-142">Habilitación de los componentes de IISIntegration</span><span class="sxs-lookup"><span data-stu-id="942a2-142">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="942a2-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="942a2-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="942a2-144">Los archivos *Program.cs* estándar llaman a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para empezar a configurar un host.</span><span class="sxs-lookup"><span data-stu-id="942a2-144">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="942a2-145">`CreateDefaultBuilder` configura [Kestrel](xref:fundamentals/servers/kestrel) como el servidor web y habilita IIS Integration configurando la ruta de acceso base y el puerto para el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):</span><span class="sxs-lookup"><span data-stu-id="942a2-145">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="942a2-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="942a2-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="942a2-147">Incluya una dependencia en el paquete [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) de dependencias de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="942a2-147">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="942a2-148">Incorpore middleware de IIS Integration a la aplicación al agregar el método de extensión *UseIISIntegration* a *WebHostBuilder*:</span><span class="sxs-lookup"><span data-stu-id="942a2-148">Incorporate IIS Integration middleware into the app by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="942a2-149">Tanto `UseKestrel` como `UseIISIntegration` son necesarios.</span><span class="sxs-lookup"><span data-stu-id="942a2-149">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="942a2-150">La llamada al código `UseIISIntegration` no afecta a la portabilidad del código.</span><span class="sxs-lookup"><span data-stu-id="942a2-150">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="942a2-151">Si la aplicación no se ejecuta detrás de IIS (por ejemplo, si se ejecuta directamente en Kestrel), `UseIISIntegration` no es posible realizar ninguna operación.</span><span class="sxs-lookup"><span data-stu-id="942a2-151">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="942a2-152">Para obtener más información sobre el hospedaje, consulte [Hosting in ASP.NET Core](xref:fundamentals/hosting) (Hospedaje en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="942a2-152">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="942a2-153">Opciones de IIS</span><span class="sxs-lookup"><span data-stu-id="942a2-153">IIS options</span></span>

<span data-ttu-id="942a2-154">Para configurar las opciones de IIS, incluya una configuración de servicio para `IISOptions` en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="942a2-154">To configure IIS options, include a service configuration for `IISOptions` in `ConfigureServices`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="942a2-155">Opción</span><span class="sxs-lookup"><span data-stu-id="942a2-155">Option</span></span>                         | <span data-ttu-id="942a2-156">Default</span><span class="sxs-lookup"><span data-stu-id="942a2-156">Default</span></span> | <span data-ttu-id="942a2-157">Parámetro</span><span class="sxs-lookup"><span data-stu-id="942a2-157">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="942a2-158">Si `true`, el middleware de autenticación configura el `HttpContext.User` y responde a los desafíos genéricos.</span><span class="sxs-lookup"><span data-stu-id="942a2-158">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="942a2-159">Si `false`, el middleware de autenticación solo proporciona una identidad (`HttpContext.User`) y responde a los desafíos cuando se le solicita explícitamente mediante el `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="942a2-159">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="942a2-160">Autenticación de Windows debe estar habilitado en IIS para que `AutomaticAuthentication` funcione.</span><span class="sxs-lookup"><span data-stu-id="942a2-160">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="942a2-161">Establece el nombre para mostrar que se muestra a los usuarios en las páginas de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="942a2-161">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="942a2-162">Si `HttpContext.Connection.ClientCertificate` y el encabezado de solicitud `true` está presente, se rellena `MS-ASPNETCORE-CLIENTCERT`.</span><span class="sxs-lookup"><span data-stu-id="942a2-162">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="942a2-163">web.config</span><span class="sxs-lookup"><span data-stu-id="942a2-163">web.config</span></span>

<span data-ttu-id="942a2-164">El objetivo principal del archivo *web.config* consiste en configurar el [módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="942a2-164">The *web.config* file's primary purpose is to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="942a2-165">Puede que, como alternativa, proporcione una configuración de IIS adicional.</span><span class="sxs-lookup"><span data-stu-id="942a2-165">It may optionally provide additional IIS configuration settings.</span></span> <span data-ttu-id="942a2-166">La creación, transformación y publicación de *web.config* se controlan con el SDK web de .NET Core (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="942a2-166">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="942a2-167">El SDK se establece en la parte superior del archivo del proyecto, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span><span class="sxs-lookup"><span data-stu-id="942a2-167">The SDK is set  at the top of the project file, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="942a2-168">Para evitar que el SDK transforme el archivo *web.config*, agregue la propiedad **\<IsTransformWebConfigDisabled>** al archivo del proyecto con un valor de `true`:</span><span class="sxs-lookup"><span data-stu-id="942a2-168">To prevent the SDK from transforming the *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to the project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="942a2-169">Si el proyecto incluye un archivo *web.config*, se transforma con los elementos *processPath* y *arguments* correctos para configurar el [módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) y se mueve a la [salida publicada](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="942a2-169">If a *web.config* file is in the project, it's transformed with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span> <span data-ttu-id="942a2-170">La transformación no modifica los valores de configuración de IIS del archivo.</span><span class="sxs-lookup"><span data-stu-id="942a2-170">The transformation doesn't modify IIS configuration settings in the file.</span></span>

### <a name="webconfig-location"></a><span data-ttu-id="942a2-171">Ubicación de web.config</span><span class="sxs-lookup"><span data-stu-id="942a2-171">web.config location</span></span>

<span data-ttu-id="942a2-172">Las aplicaciones de .NET Core se hospedan a través de un servidor proxy inverso entre IIS y el servidor Kestrel.</span><span class="sxs-lookup"><span data-stu-id="942a2-172">.NET Core apps are hosted via a reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="942a2-173">Para crear el servidor proxy inverso, el archivo *web.config* debe estar presente en la ruta de acceso raíz del contenido (normalmente la ruta de acceso base de la aplicación) de la aplicación implementada, que es la ruta de acceso física del sitio web proporcionada a IIS.</span><span class="sxs-lookup"><span data-stu-id="942a2-173">In order to create the reverse proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="942a2-174">El archivo *web.config* debe estar en la raíz de la aplicación para habilitar la publicación de varias aplicaciones mediante Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="942a2-174">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="942a2-175">Los archivos confidenciales están en la ruta de acceso física de la aplicación, incluidas las subcarpetas, como *\<nombre_de_ensamblado>.runtimeconfig.json*, *\<nombre_de_ensamblado>.xml* (comentarios de documentación XML) y *\<nombre_de_ensamblado>.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="942a2-175">Sensitive files exist on the app's physical path, including subfolders, such as *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (XML Documentation comments), and *\<assembly_name>.deps.json*.</span></span> <span data-ttu-id="942a2-176">Cuando el archivo *web.config* está presente y configura el sitio, IIS impide que se proporcionen estos archivos confidenciales.</span><span class="sxs-lookup"><span data-stu-id="942a2-176">When the *web.config* file is present and configures the site, IIS prevents these sensitive files from being served.</span></span> <span data-ttu-id="942a2-177">**Por lo tanto, es importante que el archivo *web.config* no se cambie de nombre ni se quite de la implementación accidentalmente.**</span><span class="sxs-lookup"><span data-stu-id="942a2-177">**Therefore, it's important that the *web.config* file isn't accidently renamed or removed from the deployment.**</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="942a2-178">Crear el sitio web de IIS</span><span class="sxs-lookup"><span data-stu-id="942a2-178">Create the IIS Website</span></span>

1. <span data-ttu-id="942a2-179">En el sistema IIS de destino, cree una carpeta que contenga las carpetas y los archivos publicados de la aplicación, que se describen en [Estructura de directorios](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="942a2-179">On the target IIS system, create a folder to contain the app's published folders and files, which are described in [Directory Structure](xref:host-and-deploy/directory-structure).</span></span>

2. <span data-ttu-id="942a2-180">Dentro de la carpeta, cree una carpeta *logs* para los registros de stdout cuando esté habilitado el registro de stdout.</span><span class="sxs-lookup"><span data-stu-id="942a2-180">Within the folder, create a *logs* folder to hold stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="942a2-181">Si la aplicación se ha implementado con una carpeta *logs* en la carga, omita este paso.</span><span class="sxs-lookup"><span data-stu-id="942a2-181">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="942a2-182">Para obtener instrucciones sobre la manera en que MSBuild crea la carpeta *logs*, vea el tema [Directory structure](xref:host-and-deploy/directory-structure) (Estructura de directorios).</span><span class="sxs-lookup"><span data-stu-id="942a2-182">For instructions on how to make MSBuild create the *logs* folder, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

3. <span data-ttu-id="942a2-183">En **Administrador de IIS**, cree un nuevo sitio web.</span><span class="sxs-lookup"><span data-stu-id="942a2-183">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="942a2-184">Proporcione el **Nombre del sitio** y establezca la **Ruta de acceso física** a la carpeta de implementación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="942a2-184">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="942a2-185">Proporcione la configuración de **Enlace** y cree el sitio web.</span><span class="sxs-lookup"><span data-stu-id="942a2-185">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="942a2-186">Establezca el grupo de aplicaciones en **Sin código administrado**.</span><span class="sxs-lookup"><span data-stu-id="942a2-186">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="942a2-187">ASP.NET Core se ejecuta en un proceso independiente y administra el runtime.</span><span class="sxs-lookup"><span data-stu-id="942a2-187">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="942a2-188">Abra la ventana **Agregar sitio web**.</span><span class="sxs-lookup"><span data-stu-id="942a2-188">Open the **Add Website** window.</span></span>

   ![Haga clic en "Agregar sitio web" en el menú contextual de Sitios.](index/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="942a2-190">Configure el sitio web.</span><span class="sxs-lookup"><span data-stu-id="942a2-190">Configure the website.</span></span>

   ![Proporcione el nombre del sitio, la ruta de acceso física y el nombre de host en el paso Agregar sitio web.](index/_static/add-website-ws2016.png)

7. <span data-ttu-id="942a2-192">En el panel **Grupos de aplicaciones**, abra la ventana **Modificar grupo de aplicaciones**. Para ello, haga clic con el botón derecho en el grupo de aplicaciones del sitio web y seleccione **Configuración básica...** en el menú emergente.</span><span class="sxs-lookup"><span data-stu-id="942a2-192">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's app pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![Seleccione Configuración básica en el menú contextual del grupo de aplicaciones.](index/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="942a2-194">Establezca la **Versión de .NET CLR** en **Sin código administrado**.</span><span class="sxs-lookup"><span data-stu-id="942a2-194">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![Establezca Sin código administrado para Versión de .NET CLR.](index/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="942a2-196">Nota: El establecimiento de **Versión de .NET CLR** en **Sin código administrado** es opcional.</span><span class="sxs-lookup"><span data-stu-id="942a2-196">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="942a2-197">ASP.NET Core no se basa en la carga de CLR de escritorio.</span><span class="sxs-lookup"><span data-stu-id="942a2-197">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="942a2-198">Confirme que la identidad del modelo de proceso tiene los permisos adecuados.</span><span class="sxs-lookup"><span data-stu-id="942a2-198">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="942a2-199">Si cambia la identidad predeterminada del grupo de aplicaciones (**Modelo de proceso** > **Identidad**) de **ApplicationPoolIdentity** a otra identidad, compruebe que la nueva identidad tenga los permisos necesarios para obtener acceso a la carpeta de la aplicación, la base de datos y otros recursos necesarios.</span><span class="sxs-lookup"><span data-stu-id="942a2-199">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-app"></a><span data-ttu-id="942a2-200">Implementación de la aplicación</span><span class="sxs-lookup"><span data-stu-id="942a2-200">Deploy the app</span></span>

<span data-ttu-id="942a2-201">Implemente la aplicación en la carpeta que ha creado en el sistema IIS de destino.</span><span class="sxs-lookup"><span data-stu-id="942a2-201">Deploy the app to the folder created on the target IIS system.</span></span> <span data-ttu-id="942a2-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) es el mecanismo recomendado para la implementación.</span><span class="sxs-lookup"><span data-stu-id="942a2-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

<span data-ttu-id="942a2-203">Confirme que la aplicación publicada para la implementación no se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="942a2-203">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="942a2-204">Los archivos de la carpeta *publish* quedan bloqueados mientras se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="942a2-204">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="942a2-205">Los archivos bloqueados no se pueden sobrescribir.</span><span class="sxs-lookup"><span data-stu-id="942a2-205">Locked files can't be overwritten.</span></span> <span data-ttu-id="942a2-206">Para liberar archivos bloqueados de una implementación, detenga el grupo de aplicaciones:</span><span class="sxs-lookup"><span data-stu-id="942a2-206">To release locked files in a deployment, stop the app pool:</span></span>

* <span data-ttu-id="942a2-207">Manualmente en el Administrador de IIS en el servidor.</span><span class="sxs-lookup"><span data-stu-id="942a2-207">Manually in the IIS Manager on the server.</span></span>
* <span data-ttu-id="942a2-208">Mediante Web Deploy con una referencia a `Microsoft.NET.Sdk.Web` en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="942a2-208">Using Web Deploy and referencing `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="942a2-209">Se coloca un archivo *app_offline.htm* en la raíz del directorio de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="942a2-209">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="942a2-210">Cuando el archivo está presente, el módulo de ASP.NET Core cierra correctamente la aplicación y proporciona el archivo *app_offline.htm* durante la implementación.</span><span class="sxs-lookup"><span data-stu-id="942a2-210">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="942a2-211">Para más información, vea [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm) (Referencia de configuración del módulo de ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="942a2-211">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>
* <span data-ttu-id="942a2-212">Use PowerShell para detener y reiniciar el grupo de aplicaciones (requiere PowerShell 5 o posterior):</span><span class="sxs-lookup"><span data-stu-id="942a2-212">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="942a2-213">Web Deploy con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="942a2-213">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="942a2-214">Vea el tema [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) (Perfiles de publicación de Visual Studio para la implementación de aplicaciones de ASP.NET Core) para obtener más información sobre cómo crear un perfil de publicación para su uso con Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="942a2-214">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="942a2-215">Si el proveedor de hospedaje proporciona un perfil de publicación o admite la creación de uno, descargue su perfil e impórtelo mediante el cuadro de diálogo **Publicar** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="942a2-215">If the hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Cuadro de diálogo Publicar](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="942a2-217">Web Deploy fuera de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="942a2-217">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="942a2-218">También puede usar [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) fuera de Visual Studio desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="942a2-218">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="942a2-219">Para más información, vea [Web Deployment Tool (Herramienta de implementación web)](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="942a2-219">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="942a2-220">Alternativas a Web Deploy</span><span class="sxs-lookup"><span data-stu-id="942a2-220">Alternatives to Web Deploy</span></span>

<span data-ttu-id="942a2-221">Use cualquiera de los métodos disponibles para mover la aplicación al sistema de hospedaje, como Xcopy, Robocopy o PowerShell.</span><span class="sxs-lookup"><span data-stu-id="942a2-221">Use any of several methods to move the app to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="942a2-222">Los usuarios de Visual Studio pueden usar los [Ejemplos de publicación](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span><span class="sxs-lookup"><span data-stu-id="942a2-222">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="942a2-223">Examinar el sitio web</span><span class="sxs-lookup"><span data-stu-id="942a2-223">Browse the website</span></span>

![El explorador Microsoft Edge ha cargado la página de inicio de IIS.](index/_static/browsewebsite.png)

## <a name="data-protection"></a><span data-ttu-id="942a2-225">Protección de datos</span><span class="sxs-lookup"><span data-stu-id="942a2-225">Data protection</span></span>

<span data-ttu-id="942a2-226">Hay varios middlewares de ASP.NET que usan la protección de datos, incluidos los empleados en la autenticación.</span><span class="sxs-lookup"><span data-stu-id="942a2-226">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="942a2-227">Incluso si no se llama a las API de protección de datos desde el código del usuario, se debe configurar la protección de datos con un script de implementación o en el código de usuario para crear un almacén de claves persistente.</span><span class="sxs-lookup"><span data-stu-id="942a2-227">Even if Data Protection APIs aren't called from user's code, Data Protection should be configured with a deployment script or in user code to create a persistent key store.</span></span> <span data-ttu-id="942a2-228">Si no se configura la protección de datos, las claves se conservan en memoria y se descartan cuando se reinicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="942a2-228">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="942a2-229">Si el conjunto de claves se almacena en memoria cuando se reinicia la aplicación:</span><span class="sxs-lookup"><span data-stu-id="942a2-229">If the keyring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="942a2-230">Todos los tokens de autenticación de formularios se invalidan.</span><span class="sxs-lookup"><span data-stu-id="942a2-230">All forms authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="942a2-231">Los usuarios tienen que iniciar sesión de nuevo en la siguiente solicitud.</span><span class="sxs-lookup"><span data-stu-id="942a2-231">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="942a2-232">Ya no se pueden descifrar los datos protegidos con el conjunto de claves.</span><span class="sxs-lookup"><span data-stu-id="942a2-232">Any data protected with the keyring can no longer be decrypted.</span></span>

<span data-ttu-id="942a2-233">Para configurar la protección de datos en IIS, use **uno** de los enfoques siguientes:</span><span class="sxs-lookup"><span data-stu-id="942a2-233">To configure Data Protection under IIS, use **one** of the following approaches:</span></span>

### <a name="create-a-data-protection-registry-hive"></a><span data-ttu-id="942a2-234">Crear un subárbol del Registro de protección de datos</span><span class="sxs-lookup"><span data-stu-id="942a2-234">Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="942a2-235">Las claves de protección de datos usadas por las aplicaciones de ASP.NET se almacenan en subárboles del Registro externos a las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="942a2-235">Data Protection keys used by ASP.NET apps are stored in registry hives external to the apps.</span></span> <span data-ttu-id="942a2-236">Para conservar las claves de una determinada aplicación, cree un subárbol del Registro para el grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="942a2-236">To persist the keys for a given app, create a registry hive for the app pool.</span></span>

<span data-ttu-id="942a2-237">En las instalaciones independientes de IIS que no son de granja de servidores web, puede usar el [script de PowerShell Provision-AutoGenKeys.ps1 de protección de datos](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) para cada grupo de aplicaciones usado con una aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="942a2-237">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="942a2-238">Este script crea una clave del Registro especial en el registro HKLM que solo se incluye en la ACL de la cuenta de proceso de trabajo.</span><span class="sxs-lookup"><span data-stu-id="942a2-238">This script creates a special registry key in the HKLM registry that's ACLed only to the worker process account.</span></span> <span data-ttu-id="942a2-239">Las claves se cifran en reposo mediante DPAPI con una clave de equipo.</span><span class="sxs-lookup"><span data-stu-id="942a2-239">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

<span data-ttu-id="942a2-240">En escenarios de granja de servidores web, una aplicación puede configurarse para usar una ruta de acceso UNC para almacenar su conjunto de claves de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="942a2-240">In web farm scenarios, an app can be configured to use a UNC path to store its data protection keyring.</span></span> <span data-ttu-id="942a2-241">De forma predeterminada, las claves de protección de datos no se cifran.</span><span class="sxs-lookup"><span data-stu-id="942a2-241">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="942a2-242">Asegúrese de que los permisos de archivo de un recurso compartido de este tipo se limitan a la cuenta de Windows como la que se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="942a2-242">Ensure that the file permissions for such a share are limited to the Windows account the app runs as.</span></span> <span data-ttu-id="942a2-243">Además, puede usar un certificado X509 para proteger las claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="942a2-243">In addition, an X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="942a2-244">Considere un mecanismo que permita a los usuarios cargar certificados: coloque los certificados en el almacén de certificados de confianza del usuario y asegúrese de que están disponibles en todos los equipos en los que se ejecuta la aplicación del usuario.</span><span class="sxs-lookup"><span data-stu-id="942a2-244">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="942a2-245">Vea [Configuración de la protección de datos](xref:security/data-protection/configuration/overview) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="942a2-245">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="942a2-246">Configurar el grupo de aplicaciones de IIS para cargar el perfil de usuario</span><span class="sxs-lookup"><span data-stu-id="942a2-246">Configure the IIS Application Pool to load the user profile</span></span>

<span data-ttu-id="942a2-247">Esta opción está en la sección **Modelo de proceso**, en la **Configuración avanzada** del grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="942a2-247">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="942a2-248">Establezca Cargar perfil de usuario en `True`.</span><span class="sxs-lookup"><span data-stu-id="942a2-248">Set Load User Profile to `True`.</span></span> <span data-ttu-id="942a2-249">Esto almacena las claves en el directorio del perfil de usuario y las protege mediante DPAPI con una clave específica de la cuenta de usuario usada para el grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="942a2-249">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="use-the-file-system-as-a-key-ring-store"></a><span data-ttu-id="942a2-250">Usar el sistema de archivos como un almacén de conjunto de claves</span><span class="sxs-lookup"><span data-stu-id="942a2-250">Use the file system as a key ring store</span></span>

<span data-ttu-id="942a2-251">Ajuste el código de la aplicación para [usar el sistema de archivos como un almacén de conjunto de claves](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="942a2-251">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="942a2-252">Use un certificado X509 para proteger el conjunto de claves y asegúrese de que sea un certificado de confianza.</span><span class="sxs-lookup"><span data-stu-id="942a2-252">Use an X509 certificate to protect the keyring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="942a2-253">Si es un certificado autofirmado, colóquelo en el almacén raíz de confianza.</span><span class="sxs-lookup"><span data-stu-id="942a2-253">If it's a self signed certificate, place the certificate in the Trusted Root store.</span></span>

<span data-ttu-id="942a2-254">Cuando se usa IIS en una granja de servidores web:</span><span class="sxs-lookup"><span data-stu-id="942a2-254">When using IIS in a web farm:</span></span>

* <span data-ttu-id="942a2-255">Use un recurso compartido de archivos al que puedan acceder todos los equipos.</span><span class="sxs-lookup"><span data-stu-id="942a2-255">Use a file share that all machines can access.</span></span>
* <span data-ttu-id="942a2-256">Implemente un certificado X509 en cada equipo.</span><span class="sxs-lookup"><span data-stu-id="942a2-256">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="942a2-257">Configure la [protección de datos en el código](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="942a2-257">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

### <a name="set-a-machine-wide-policy-for-data-protection"></a><span data-ttu-id="942a2-258">Establecimiento de una directiva a nivel de equipo para la protección de datos</span><span class="sxs-lookup"><span data-stu-id="942a2-258">Set a machine-wide policy for data protection</span></span>

<span data-ttu-id="942a2-259">El sistema de protección de datos tiene compatibilidad limitada con el establecimiento de una [directiva de equipo](xref:security/data-protection/configuration/machine-wide-policy) predeterminada para todas las aplicaciones que usan las API de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="942a2-259">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="942a2-260">Vea la documentación de [protección de datos](xref:security/data-protection/index) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="942a2-260">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="942a2-261">Configuración de aplicaciones secundarias</span><span class="sxs-lookup"><span data-stu-id="942a2-261">Configuration of sub-applications</span></span>

<span data-ttu-id="942a2-262">Las aplicaciones secundarias agregadas bajo la aplicación raíz no deben incluir el módulo de ASP.NET Core como controlador.</span><span class="sxs-lookup"><span data-stu-id="942a2-262">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="942a2-263">Si se agrega el módulo como controlador al archivo *web.config* de una aplicación secundaria, aparece un error 500.19 (Error interno del servidor) que hace referencia al archivo de configuración erróneo al intentar examinar la aplicación secundaria.</span><span class="sxs-lookup"><span data-stu-id="942a2-263">If the module is added as a handler in a sub-app's *web.config* file, a 500.19 (Internal Server Error) referencing the faulty config file is received when attempting to browse the sub-app.</span></span> <span data-ttu-id="942a2-264">En el ejemplo siguiente se muestra el contenido de un archivo publicado *web.config* para una aplicación secundaria de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="942a2-264">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="942a2-265">Al hospedar una aplicación secundaria que no es de ASP.NET Core bajo una aplicación de ASP.NET Core, quite explícitamente el controlador heredado del archivo *web.config* de la aplicación secundaria:</span><span class="sxs-lookup"><span data-stu-id="942a2-265">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="942a2-266">Para más información sobre la configuración del módulo de ASP.NET Core, vea el tema [Introducción al módulo de ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) y [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module) (Referencia de configuración del módulo de ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="942a2-266">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="942a2-267">Configuración de IIS con web.config</span><span class="sxs-lookup"><span data-stu-id="942a2-267">Configuration of IIS with web.config</span></span>

<span data-ttu-id="942a2-268">La configuración de IIS aún se ve afectada por la sección **\<system.webServer>** de *web.config* en las características de IIS que se aplican a una configuración de proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="942a2-268">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="942a2-269">Si IIS está configurado en el nivel de sistema para usar compresión dinámica, puede deshabilitar esa configuración para una aplicación con el elemento **\<urlCompression>** del archivo *web.config* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="942a2-269">If IIS is configured at the system level to use dynamic compression, that setting can be disabled for an app with the **\<urlCompression>** element in the app's *web.config* file.</span></span> <span data-ttu-id="942a2-270">Para más información, vea la [referencia de configuración para \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module) (Referencia de configuración del módulo de ASP.NET Core) y [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules) (Uso de módulos de IIS con ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="942a2-270">For more information, see the [configuration reference for \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module) and [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="942a2-271">Si tiene que establecer variables de entorno para aplicaciones individuales que se ejecutan en grupos de aplicaciones aislados (se admite en IIS 10.0+), vea la sección *AppCmd.exe command* (Comando AppCmd.exe) del tema [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) (Variables de entorno <environmentVariables>) de la documentación de referencia de IIS.</span><span class="sxs-lookup"><span data-stu-id="942a2-271">If there's a need to set environment variables for individual apps running in isolated app pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="942a2-272">Secciones de configuración de web.config</span><span class="sxs-lookup"><span data-stu-id="942a2-272">Configuration sections of web.config</span></span>

<span data-ttu-id="942a2-273">Las aplicaciones de ASP.NET Core no usan para la configuración las secciones de configuración de aplicaciones de ASP.NET Framework en *web.config*:</span><span class="sxs-lookup"><span data-stu-id="942a2-273">Configruation sections of ASP.NET Framework apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="942a2-274">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="942a2-274">**\<system.web>**</span></span>
* <span data-ttu-id="942a2-275">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="942a2-275">**\<appSettings>**</span></span>
* <span data-ttu-id="942a2-276">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="942a2-276">**\<connectionStrings>**</span></span>
* <span data-ttu-id="942a2-277">**\<location>**</span><span class="sxs-lookup"><span data-stu-id="942a2-277">**\<location>**</span></span>

<span data-ttu-id="942a2-278">Las aplicaciones de ASP.NET Core se configuran mediante otros proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="942a2-278">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="942a2-279">Para obtener más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="942a2-279">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="942a2-280">Grupos de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="942a2-280">Application Pools</span></span>

<span data-ttu-id="942a2-281">Al hospedar varios sitios web en un único sistema, aísle las aplicaciones entre sí mediante la ejecución de cada una de ellas en su propio grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="942a2-281">When hosting multiple websites on a single system, isolate the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="942a2-282">El valor predeterminado del cuadro de diálogo **Agregar sitio web** es esta configuración.</span><span class="sxs-lookup"><span data-stu-id="942a2-282">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="942a2-283">Cuando se proporciona el **Nombre del sitio**, el texto se transfiere automáticamente al cuadro de texto **Grupo de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="942a2-283">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="942a2-284">Al agregar el sitio web se crea un grupo de aplicaciones con el nombre del sitio.</span><span class="sxs-lookup"><span data-stu-id="942a2-284">A new app pool is created using the site name when the website is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="942a2-285">Identidad del grupo de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="942a2-285">Application Pool Identity</span></span>

<span data-ttu-id="942a2-286">Una cuenta de identidad del grupo de aplicaciones permite ejecutar una aplicación en una cuenta única sin tener que crear ni administrar dominios o cuentas locales.</span><span class="sxs-lookup"><span data-stu-id="942a2-286">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="942a2-287">En IIS 8.0 y versiones posteriores, el proceso de trabajo de administración de IIS (WAS) crea una cuenta virtual con el nombre del nuevo grupo de aplicaciones y ejecuta los procesos de trabajo del grupo de aplicaciones en esta cuenta de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="942a2-287">On IIS 8.0+, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="942a2-288">En la Consola de administración de IIS, en la opción **Configuración avanzada** del grupo de aplicaciones, asegúrese de que la **Identidad** está establecida para usar **ApplicationPoolIdentity**:</span><span class="sxs-lookup"><span data-stu-id="942a2-288">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Cuadro de diálogo Configuración avanzada del grupo de aplicaciones](index/_static/apppool-identity.png)

<span data-ttu-id="942a2-290">El proceso de administración de IIS crea un identificador seguro con el nombre del grupo de aplicaciones en el sistema de seguridad de Windows.</span><span class="sxs-lookup"><span data-stu-id="942a2-290">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="942a2-291">Los recursos se pueden proteger mediante esta identidad, aunque no es una cuenta de usuario real ni se muestra en la consola de administración de usuario de Windows.</span><span class="sxs-lookup"><span data-stu-id="942a2-291">Resources can be secured by using this identity; however, this identity isn't a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="942a2-292">Si el proceso de trabajo de IIS requiere acceso con privilegios elevados a la aplicación, modifique la lista de control de acceso (ACL) del directorio que contiene la aplicación:</span><span class="sxs-lookup"><span data-stu-id="942a2-292">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="942a2-293">Abra el Explorador de Windows y vaya al directorio.</span><span class="sxs-lookup"><span data-stu-id="942a2-293">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="942a2-294">Haga clic con el botón derecho en el directorio y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="942a2-294">Right-click on the directory and select **Properties**.</span></span>

3. <span data-ttu-id="942a2-295">En la pestaña **Seguridad**, haga clic en el botón **Editar** y en el botón **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="942a2-295">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="942a2-296">Haga clic en el botón **Ubicaciones** y asegúrese de seleccionar el sistema.</span><span class="sxs-lookup"><span data-stu-id="942a2-296">Select the **Locations** button and make sure the system is selected.</span></span>

5. <span data-ttu-id="942a2-297">Escriba **IIS AppPool\DefaultAppPool** en el cuadro de texto **Escribir los nombres de objeto para seleccionar**.</span><span class="sxs-lookup"><span data-stu-id="942a2-297">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

   ![Selección del cuadro de diálogo de usuarios o grupos para la carpeta de la aplicación](index/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="942a2-299">Haga clic en el botón **Comprobar nombres**.</span><span class="sxs-lookup"><span data-stu-id="942a2-299">Select the **Check Names** button.</span></span> <span data-ttu-id="942a2-300">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="942a2-300">Select **OK**.</span></span>

   ![Selección del cuadro de diálogo de usuarios o grupos para la carpeta de la aplicación](index/_static/select-users-or-groups-2.png)

<span data-ttu-id="942a2-302">También puede hacerlo mediante un símbolo del sistema con la herramienta **ICACLS**:</span><span class="sxs-lookup"><span data-stu-id="942a2-302">This can also be accomplished via a command prompt using the **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a><span data-ttu-id="942a2-303">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="942a2-303">Additional resources</span></span>

* [<span data-ttu-id="942a2-304">Solución de problemas de ASP.NET Core en IIS</span><span class="sxs-lookup"><span data-stu-id="942a2-304">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="942a2-305">Referencia de errores comunes de Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="942a2-305">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="942a2-306">Introducción al módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="942a2-306">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="942a2-307">Referencia de configuración del módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="942a2-307">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="942a2-308">Uso de módulos de IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="942a2-308">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="942a2-309">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="942a2-309">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="942a2-310">Sitio oficial de Microsoft IIS</span><span class="sxs-lookup"><span data-stu-id="942a2-310">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="942a2-311">Biblioteca de TechNet de Microsoft: Windows Server</span><span class="sxs-lookup"><span data-stu-id="942a2-311">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
