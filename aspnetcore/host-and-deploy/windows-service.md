---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: guardrex
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/04/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: ec3a37fd859df7592fa0d6d9cc0109942a570e7a
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086985"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="d5895-103">Hospedaje de ASP.NET Core en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="d5895-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="d5895-104">Por [Luke Latham](https://github.com/guardrex) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d5895-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d5895-105">Una aplicación de ASP.NET Core se puede hospedar en Windows sin usar IIS como [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="d5895-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="d5895-106">Cuando se aloja como un servicio de Windows, la aplicación se inicia automáticamente después de reiniciar el equipo.</span><span class="sxs-lookup"><span data-stu-id="d5895-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="d5895-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d5895-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5895-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d5895-108">Prerequisites</span></span>

* [<span data-ttu-id="d5895-109">PowerShell 6.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="d5895-109">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

> [!NOTE]
> <span data-ttu-id="d5895-110">Para sistemas operativos Windows anteriores a la actualización de octubre de 2018 de Windows 10 (versión 1809/compilación 10.0.17763), el módulo [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) debe importarse con el [módulo WindowsCompatibility](https://github.com/PowerShell/WindowsCompatibility) para poder acceder al cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) utilizado en la sección [Crear una cuenta de usuario](#create-a-user-account):</span><span class="sxs-lookup"><span data-stu-id="d5895-110">For Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763), the [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) module must be imported with the [WindowsCompatibility module](https://github.com/PowerShell/WindowsCompatibility) to gain access to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet used in the [Create a user account](#create-a-user-account) section:</span></span>
>
> ```powershell
> Install-Module WindowsCompatibility -Scope CurrentUser
> Import-WinModule Microsoft.PowerShell.LocalAccounts
> ```

## <a name="deployment-type"></a><span data-ttu-id="d5895-111">Tipo de implementación</span><span class="sxs-lookup"><span data-stu-id="d5895-111">Deployment type</span></span>

<span data-ttu-id="d5895-112">Puede crear una implementación del servicio de Windows dependiente del marco o autocontenida.</span><span class="sxs-lookup"><span data-stu-id="d5895-112">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="d5895-113">Para obtener información y consejos sobre los escenarios de implementación, consulte [Implementación de aplicaciones .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="d5895-113">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="d5895-114">Implementación dependiente de marco de trabajo</span><span class="sxs-lookup"><span data-stu-id="d5895-114">Framework-dependent deployment</span></span>

<span data-ttu-id="d5895-115">La implementación dependiente de marco de trabajo (FDD) se basa en la presencia de una versión compartida de .NET Core en todo el sistema en el sistema de destino.</span><span class="sxs-lookup"><span data-stu-id="d5895-115">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="d5895-116">Cuando se utiliza el escenario FDD con una aplicación de servicio de Windows de ASP.NET Core, el SDK genera un archivo ejecutable (*\*.exe*), denominado *ejecutable dependiente del marco*.</span><span class="sxs-lookup"><span data-stu-id="d5895-116">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="d5895-117">Implementación autocontenida</span><span class="sxs-lookup"><span data-stu-id="d5895-117">Self-contained deployment</span></span>

<span data-ttu-id="d5895-118">Una implementación autocontenida (SCD) no depende de la presencia de los componentes compartidos en el sistema de destino.</span><span class="sxs-lookup"><span data-stu-id="d5895-118">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="d5895-119">El tiempo de ejecución y las dependencias de la aplicación se implementan con la aplicación en el sistema host.</span><span class="sxs-lookup"><span data-stu-id="d5895-119">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="d5895-120">Convertir un proyecto en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="d5895-120">Convert a project into a Windows Service</span></span>

<span data-ttu-id="d5895-121">Realice los cambios siguientes en un proyecto de ASP.NET Core existente para ejecutar la aplicación como un servicio:</span><span class="sxs-lookup"><span data-stu-id="d5895-121">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="d5895-122">Actualizaciones del archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="d5895-122">Project file updates</span></span>

<span data-ttu-id="d5895-123">En función de su elección de [tipo de implementación](#deployment-type), actualice el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="d5895-123">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="d5895-124">Implementación dependiente de marco (FDD)</span><span class="sxs-lookup"><span data-stu-id="d5895-124">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="d5895-125">Agregue un [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) de Windows al `<PropertyGroup>` que contiene la plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="d5895-125">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="d5895-126">En el ejemplo siguiente, el RID se establece en `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="d5895-126">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="d5895-127">Agregue la propiedad `<SelfContained>` establecida en `false`.</span><span class="sxs-lookup"><span data-stu-id="d5895-127">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="d5895-128">Estas propiedades indican al SDK que genere un archivo ejecutable (*.exe*) de Windows.</span><span class="sxs-lookup"><span data-stu-id="d5895-128">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="d5895-129">No se requiere un archivo *web.config*, que normalmente se produce cuando se publica una aplicación ASP.NET Core, para una aplicación de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="d5895-129">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="d5895-130">Para deshabilitar la creación de un archivo *web.config* agregue la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="d5895-130">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="d5895-131">Agregue la propiedad `<UseAppHost>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="d5895-131">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="d5895-132">Esta propiedad proporciona el servicio con una ruta de acceso de activación (un archivo ejecutable, *.exe*) para una FDD.</span><span class="sxs-lookup"><span data-stu-id="d5895-132">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="d5895-133">Implementación autocontenida (SCD)</span><span class="sxs-lookup"><span data-stu-id="d5895-133">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="d5895-134">Confirme la presencia del [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) de Windows o agregue un RID al `<PropertyGroup>` que contiene la plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="d5895-134">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="d5895-135">Deshabilite la creación de un archivo *web.config* agregando la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="d5895-135">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="d5895-136">Para publicar para varios RID:</span><span class="sxs-lookup"><span data-stu-id="d5895-136">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="d5895-137">Proporcione los RID en una lista delimitada por punto y coma.</span><span class="sxs-lookup"><span data-stu-id="d5895-137">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="d5895-138">Use el nombre de la propiedad `<RuntimeIdentifiers>` (plural).</span><span class="sxs-lookup"><span data-stu-id="d5895-138">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="d5895-139">Para más información, vea el [Catálogo de identificadores de entorno de ejecución (RID) de .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="d5895-139">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="d5895-140">Agregue una referencia de paquete de [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="d5895-140">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="d5895-141">Para habilitar el registro de eventos de Windows, agregue una referencia de paquete para [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="d5895-141">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="d5895-142">Para obtener más información, consulte la sección [Control de los eventos de inicio y detención](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="d5895-142">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="d5895-143">Actualizaciones de Program.Main</span><span class="sxs-lookup"><span data-stu-id="d5895-143">Program.Main updates</span></span>

<span data-ttu-id="d5895-144">Realice los siguientes cambios en `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="d5895-144">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="d5895-145">Para probar y depurar cuando se ejecuta fuera de un servicio, agregue código para determinar si la aplicación se ejecuta como un servicio o una aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="d5895-145">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="d5895-146">Compruebe si el depurador está asociado o hay presente un argumento de línea de comandos `--console`.</span><span class="sxs-lookup"><span data-stu-id="d5895-146">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="d5895-147">Si alguna de estas condiciones se cumple (la aplicación no se ejecuta como un servicio), llame a <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> en el host web.</span><span class="sxs-lookup"><span data-stu-id="d5895-147">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="d5895-148">Si las condiciones no se cumplen (la aplicación se ejecuta como servicio):</span><span class="sxs-lookup"><span data-stu-id="d5895-148">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="d5895-149">Llame a <xref:System.IO.Directory.SetCurrentDirectory*> y use una ruta de acceso a la ubicación de publicación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d5895-149">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="d5895-150">No llame a <xref:System.IO.Directory.GetCurrentDirectory*> para obtener la ruta de acceso porque una aplicación de servicio de Windows devuelve una carpeta *C:\\WINDOWS\\system32* cuando se llama a <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="d5895-150">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="d5895-151">Para obtener más información, consulte la sección [Directorio actual y raíz del contenido](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="d5895-151">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="d5895-152">Llame a <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> para ejecutar la aplicación como un servicio.</span><span class="sxs-lookup"><span data-stu-id="d5895-152">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="d5895-153">Dado que el [Proveedor de configuración de línea de comandos](xref:fundamentals/configuration/index#command-line-configuration-provider) requiere pares nombre-valor en los argumentos de línea de comandos, el conmutador `--console` se quita de los argumentos antes de que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> los reciba.</span><span class="sxs-lookup"><span data-stu-id="d5895-153">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="d5895-154">Para escribir en el registro de eventos de Windows, agregue el proveedor EventLog a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="d5895-154">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="d5895-155">Establezca el nivel de registro con la clave `Logging:LogLevel:Default` en el archivo *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="d5895-155">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="d5895-156">Para fines de prueba y demostración, el archivo de configuración de producción de la aplicación de ejemplo establece el nivel de registro en `Information`.</span><span class="sxs-lookup"><span data-stu-id="d5895-156">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="d5895-157">En producción, el valor se establece normalmente en `Error`.</span><span class="sxs-lookup"><span data-stu-id="d5895-157">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="d5895-158">Para obtener más información, vea <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="d5895-158">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a><span data-ttu-id="d5895-159">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="d5895-159">Publish the app</span></span>

<span data-ttu-id="d5895-160">Publique la aplicación con [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) o Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d5895-160">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="d5895-161">Al usar Visual Studio, seleccione **FolderProfile** y configure la **Ubicación de destino** antes de seleccionar el botón **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="d5895-161">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="d5895-162">Para publicar la aplicación de ejemplo mediante herramientas de la interfaz de la línea de comandos (CLI), ejecute el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) en un shell de comandos de Windows desde la carpeta del proyecto con una configuración de versión pasada a la opción [-c|--configuration](/dotnet/core/tools/dotnet-publish#options).</span><span class="sxs-lookup"><span data-stu-id="d5895-162">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command in a Windows command shell from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="d5895-163">Use la opción [-o|--output](/dotnet/core/tools/dotnet-publish#options) con una ruta de acceso para publicar en una carpeta fuera de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d5895-163">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="d5895-164">Publicación de una implementación dependiente de marco (FDD)</span><span class="sxs-lookup"><span data-stu-id="d5895-164">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="d5895-165">En el ejemplo siguiente, la aplicación está publicada para la carpeta *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="d5895-165">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="d5895-166">Publicación de una implementación autocontenida (SCD)</span><span class="sxs-lookup"><span data-stu-id="d5895-166">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="d5895-167">El identificador relativo debe especificarse en la propiedad `<RuntimeIdenfifier>` o `<RuntimeIdentifiers>` del archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="d5895-167">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="d5895-168">Proporcione el entorno de ejecución a la opción [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) del comando `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="d5895-168">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="d5895-169">En el ejemplo siguiente, la aplicación está publicada para el entorno de ejecución `win7-x64` en la carpeta *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="d5895-169">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a><span data-ttu-id="d5895-170">Creación de una cuenta de usuario</span><span class="sxs-lookup"><span data-stu-id="d5895-170">Create a user account</span></span>

<span data-ttu-id="d5895-171">Cree una cuenta de usuario para el servicio mediante el cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) de un shell de comandos administrativos de PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="d5895-171">Create a user account for the service using the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell:</span></span>

```powershell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="d5895-172">Proporcione una [contraseña segura](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) cuando se le solicite.</span><span class="sxs-lookup"><span data-stu-id="d5895-172">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="d5895-173">Para la aplicación de ejemplo, cree una cuenta de usuario con el nombre `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="d5895-173">For the sample app, create a user account with the name `ServiceUser`.</span></span>

```powershell
New-LocalUser -Name ServiceUser
```

<span data-ttu-id="d5895-174">A menos que el parámetro `-AccountExpires` se proporcione para el cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) con una fecha de expiración <xref:System.DateTime>, la cuenta no expirará.</span><span class="sxs-lookup"><span data-stu-id="d5895-174">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="d5895-175">Para más información, vea [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) y [Service User Accounts](/windows/desktop/services/service-user-accounts) (Cuentas de usuario del servicio).</span><span class="sxs-lookup"><span data-stu-id="d5895-175">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="d5895-176">Un enfoque alternativo de administración de usuarios al usar Active Directory consiste en usar cuentas de servicio administradas.</span><span class="sxs-lookup"><span data-stu-id="d5895-176">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="d5895-177">Para obtener más información, consulte [grupo Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) (Información general sobre cuentas de servicio administradas de grupo).</span><span class="sxs-lookup"><span data-stu-id="d5895-177">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="set-permission-log-on-as-a-service"></a><span data-ttu-id="d5895-178">Establecimiento de permisos: inicio de sesión como servicio</span><span class="sxs-lookup"><span data-stu-id="d5895-178">Set permission: Log on as a service</span></span>

<span data-ttu-id="d5895-179">Conceda acceso de escritura, lectura y ejecución a la carpeta de la aplicación con el comando [icacls](/windows-server/administration/windows-commands/icacls) de un shell de comandos administrativos de PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="d5895-179">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command an administrative PowerShell 6 command shell.</span></span>

```powershell
icacls "{PATH}" /grant "{USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS}" /t
```

* <span data-ttu-id="d5895-180">`{PATH}`: ruta de acceso a la carpeta de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d5895-180">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="d5895-181">`{USER ACCOUNT}`: cuenta del usuario (SID).</span><span class="sxs-lookup"><span data-stu-id="d5895-181">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="d5895-182">`(OI)`: la marca Object Inherit (Heredar objetos) propaga los permisos a los archivos subordinados.</span><span class="sxs-lookup"><span data-stu-id="d5895-182">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="d5895-183">`(CI)`: la marca Container Inherit (Heredar contenedores) propaga los permisos a las carpetas subordinadas.</span><span class="sxs-lookup"><span data-stu-id="d5895-183">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="d5895-184">`{PERMISSION FLAGS}`: define los permisos de acceso a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d5895-184">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="d5895-185">Escritura (`W`)</span><span class="sxs-lookup"><span data-stu-id="d5895-185">Write (`W`)</span></span>
  * <span data-ttu-id="d5895-186">Lectura (`R`)</span><span class="sxs-lookup"><span data-stu-id="d5895-186">Read (`R`)</span></span>
  * <span data-ttu-id="d5895-187">Ejecución (`X`)</span><span class="sxs-lookup"><span data-stu-id="d5895-187">Execute (`X`)</span></span>
  * <span data-ttu-id="d5895-188">Total (`F`)</span><span class="sxs-lookup"><span data-stu-id="d5895-188">Full (`F`)</span></span>
  * <span data-ttu-id="d5895-189">Modificación (`M`)</span><span class="sxs-lookup"><span data-stu-id="d5895-189">Modify (`M`)</span></span>
* <span data-ttu-id="d5895-190">`/t`: se aplica de forma recursiva a las carpetas y los archivos subordinados existentes.</span><span class="sxs-lookup"><span data-stu-id="d5895-190">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="d5895-191">Para la aplicación de ejemplo publicada en la carpeta *c:\\svc* y en la cuenta `ServiceUser` con permisos de escritura, lectura y ejecución, use el siguiente comando de un shell de comandos administrativos de PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="d5895-191">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command an administrative PowerShell 6 command shell.</span></span>

```powershell
icacls "c:\svc" /grant "ServiceUser:(OI)(CI)WRX" /t
```

<span data-ttu-id="d5895-192">Para más información, vea [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="d5895-192">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="create-the-service"></a><span data-ttu-id="d5895-193">Crear el servicio</span><span class="sxs-lookup"><span data-stu-id="d5895-193">Create the service</span></span>

<span data-ttu-id="d5895-194">Use el script de PowerShell [RegisterService.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) para registrar el servicio.</span><span class="sxs-lookup"><span data-stu-id="d5895-194">Use the [RegisterService.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) PowerShell script to register the service.</span></span> <span data-ttu-id="d5895-195">Desde un shell de comandos administrativos de PowerShell 6, ejecute el script con el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="d5895-195">From an administrative PowerShell 6 command shell, execute the script with the following command:</span></span>

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Exe "{PATH TO EXE}\{ASSEMBLY NAME}.exe" 
    -User {DOMAIN\USER}
```

<span data-ttu-id="d5895-196">En el ejemplo siguiente de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d5895-196">In the following example for the sample app:</span></span>

* <span data-ttu-id="d5895-197">El servicio se denomina **MyService**.</span><span class="sxs-lookup"><span data-stu-id="d5895-197">The service is named **MyService**.</span></span>
* <span data-ttu-id="d5895-198">El servicio publicado reside en la carpeta *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="d5895-198">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="d5895-199">El ejecutable de la aplicación se denomina *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="d5895-199">The app executable is named *SampleApp.exe*.</span></span>
* <span data-ttu-id="d5895-200">El servicio se ejecuta en la cuenta `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="d5895-200">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="d5895-201">En el comando del ejemplo siguiente, el nombre del equipo local es `Desktop-PC`.</span><span class="sxs-lookup"><span data-stu-id="d5895-201">In the following example command, the local machine name is `Desktop-PC`.</span></span> <span data-ttu-id="d5895-202">Reemplace `Desktop-PC` por el nombre de equipo o el dominio del sistema.</span><span class="sxs-lookup"><span data-stu-id="d5895-202">Replace `Desktop-PC` with the computer name or domain for your system.</span></span>

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Exe "c:\svc\SampleApp.exe" 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a><span data-ttu-id="d5895-203">Administración del servicio</span><span class="sxs-lookup"><span data-stu-id="d5895-203">Manage the service</span></span>

### <a name="start-the-service"></a><span data-ttu-id="d5895-204">Inicio del servicio</span><span class="sxs-lookup"><span data-stu-id="d5895-204">Start the service</span></span>

<span data-ttu-id="d5895-205">Inicie el servicio con el comando `Start-Service -Name {NAME}` de PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="d5895-205">Start the service with the `Start-Service -Name {NAME}` PowerShell 6 command.</span></span>

<span data-ttu-id="d5895-206">Use el siguiente comando para iniciar el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d5895-206">To start the sample app service, use the following command:</span></span>

```powershell
Start-Service -Name MyService
```

<span data-ttu-id="d5895-207">Este comando tarda unos segundos en iniciar el servicio.</span><span class="sxs-lookup"><span data-stu-id="d5895-207">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="d5895-208">Determinación del estado del servicio</span><span class="sxs-lookup"><span data-stu-id="d5895-208">Determine the service status</span></span>

<span data-ttu-id="d5895-209">Para comprobar el estado del servicio, use el comando `Get-Service -Name {NAME}` de PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="d5895-209">To check the status of the service, use the `Get-Service -Name {NAME}` PowerShell 6 command.</span></span> <span data-ttu-id="d5895-210">El estado se notifica como uno de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="d5895-210">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

<span data-ttu-id="d5895-211">Use el siguiente comando para comprobar el estado del servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d5895-211">Use the following command to check the status of the sample app service:</span></span>

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="d5895-212">Examinar un servicio de aplicación web</span><span class="sxs-lookup"><span data-stu-id="d5895-212">Browse a web app service</span></span>

<span data-ttu-id="d5895-213">Si el servicio está en estado `RUNNING` y dicho servicio es una aplicación web, vaya a la aplicación en su ruta de acceso correspondiente (de forma predeterminada, `http://localhost:5000`, que redirige a `https://localhost:5001` cuando se usa el [Middleware de redirección de HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="d5895-213">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="d5895-214">Si es el servicio de la aplicación de ejemplo, vaya a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d5895-214">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="d5895-215">Detener el servicio</span><span class="sxs-lookup"><span data-stu-id="d5895-215">Stop the service</span></span>

<span data-ttu-id="d5895-216">Detenga el servicio con el comando `Stop-Service -Name {NAME}` de PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="d5895-216">Stop the service with the `Stop-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="d5895-217">Con el siguiente comando se detiene el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d5895-217">The following command stops the sample app service:</span></span>

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a><span data-ttu-id="d5895-218">Eliminación del servicio</span><span class="sxs-lookup"><span data-stu-id="d5895-218">Remove the service</span></span>

<span data-ttu-id="d5895-219">Tras un breve intervalo para detener un servicio, elimine el servicio con el comando `Remove-Service -Name {NAME}` de PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="d5895-219">After a short delay to stop a service, remove the service with the `Remove-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="d5895-220">Con el siguiente comando se quita el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d5895-220">The following command removes the sample app service:</span></span>

```powershell
Remove-Service -Name MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="d5895-221">Control de los eventos de inicio y detención</span><span class="sxs-lookup"><span data-stu-id="d5895-221">Handle starting and stopping events</span></span>

<span data-ttu-id="d5895-222">Para controlar los eventos <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> y <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, realice los siguientes cambios adicionales:</span><span class="sxs-lookup"><span data-stu-id="d5895-222">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="d5895-223">Cree una clase que se derive de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> con los métodos `OnStarting`, `OnStarted` y `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="d5895-223">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="d5895-224">Cree un método de extensión para <xref:Microsoft.AspNetCore.Hosting.IWebHost> que pase `CustomWebHostService` a <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="d5895-224">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="d5895-225">En `Program.Main`, llame al método de extensión `RunAsCustomService` en lugar de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="d5895-225">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="d5895-226">Para ver la ubicación de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> en `Program.Main`, consulte el ejemplo de código que se muestra en la sección [Convertir un proyecto en un servicio de Windows](#convert-a-project-into-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="d5895-226">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="d5895-227">Escenarios de servidor proxy y equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="d5895-227">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="d5895-228">Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="d5895-228">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="d5895-229">Para obtener más información, vea <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="d5895-229">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="d5895-230">Configuración de HTTPS</span><span class="sxs-lookup"><span data-stu-id="d5895-230">Configure HTTPS</span></span>

<span data-ttu-id="d5895-231">Para configurar el servicio con un punto de conexión seguro:</span><span class="sxs-lookup"><span data-stu-id="d5895-231">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="d5895-232">Cree un certificado X.509 para el sistema de hospedaje mediante la adquisición del certificado de la plataforma y con mecanismos de implementación.</span><span class="sxs-lookup"><span data-stu-id="d5895-232">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="d5895-233">Especifique una [configuración de punto de conexión HTTPS de servidor Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) para usar el certificado.</span><span class="sxs-lookup"><span data-stu-id="d5895-233">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="d5895-234">No se admite el uso del certificado de desarrollo HTTPS de ASP.NET Core para proteger un punto de conexión de servicio.</span><span class="sxs-lookup"><span data-stu-id="d5895-234">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="d5895-235">Directorio actual y raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="d5895-235">Current directory and content root</span></span>

<span data-ttu-id="d5895-236">El directorio de trabajo actual devuelto por una llamada a <xref:System.IO.Directory.GetCurrentDirectory*> para un servicio de Windows es la carpeta *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="d5895-236">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="d5895-237">La carpeta *system32* no es una ubicación adecuada para almacenar los archivos de un servicio (por ejemplo los archivos de configuración).</span><span class="sxs-lookup"><span data-stu-id="d5895-237">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="d5895-238">Utilice uno de los siguientes enfoques para mantener los archivos de configuración y los activos de un servicio, así como para acceder a ellos.</span><span class="sxs-lookup"><span data-stu-id="d5895-238">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="d5895-239">Configuración de la ruta de acceso raíz del contenido en la carpeta de la aplicación</span><span class="sxs-lookup"><span data-stu-id="d5895-239">Set the content root path to the app's folder</span></span>

<span data-ttu-id="d5895-240"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> es la misma ruta de acceso proporcionada al argumento `binPath` cuando se crea el servicio.</span><span class="sxs-lookup"><span data-stu-id="d5895-240">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="d5895-241">En lugar de llamar `GetCurrentDirectory` para crear rutas de acceso a los archivos de configuración, llame a <xref:System.IO.Directory.SetCurrentDirectory*> con la ruta de acceso a la raíz del contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d5895-241">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="d5895-242">En `Program.Main`, determine la ruta de acceso a la carpeta del archivo ejecutable del servicio y use la ruta de acceso para establecer la raíz del contenido de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="d5895-242">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="d5895-243">Almacene los archivos del servicio en una ubicación adecuada en el disco.</span><span class="sxs-lookup"><span data-stu-id="d5895-243">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="d5895-244">Especifique una ruta de acceso absoluta con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> al utilizar un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> a la carpeta que contiene los archivos.</span><span class="sxs-lookup"><span data-stu-id="d5895-244">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d5895-245">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d5895-245">Additional resources</span></span>

* <span data-ttu-id="d5895-246">[Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)</span><span class="sxs-lookup"><span data-stu-id="d5895-246">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
