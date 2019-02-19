---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: guardrex
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/13/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 081a631c9c3e74c01e15f4b0b272d650c162bd20
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248256"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="97d93-103">Hospedaje de ASP.NET Core en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="97d93-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="97d93-104">Por [Luke Latham](https://github.com/guardrex) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="97d93-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="97d93-105">Una aplicación de ASP.NET Core se puede hospedar en Windows sin usar IIS como [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="97d93-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="97d93-106">Cuando se aloja como un servicio de Windows, la aplicación se inicia automáticamente después de reiniciar el equipo.</span><span class="sxs-lookup"><span data-stu-id="97d93-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="97d93-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="97d93-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="deployment-type"></a><span data-ttu-id="97d93-108">Tipo de implementación</span><span class="sxs-lookup"><span data-stu-id="97d93-108">Deployment type</span></span>

<span data-ttu-id="97d93-109">Puede crear una implementación del servicio de Windows dependiente del marco o autocontenida.</span><span class="sxs-lookup"><span data-stu-id="97d93-109">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="97d93-110">Para obtener información y consejos sobre los escenarios de implementación, consulte [Implementación de aplicaciones .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="97d93-110">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="97d93-111">Implementación dependiente de marco de trabajo</span><span class="sxs-lookup"><span data-stu-id="97d93-111">Framework-dependent deployment</span></span>

<span data-ttu-id="97d93-112">La implementación dependiente de marco de trabajo (FDD) se basa en la presencia de una versión compartida de .NET Core en todo el sistema en el sistema de destino.</span><span class="sxs-lookup"><span data-stu-id="97d93-112">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="97d93-113">Cuando se utiliza el escenario FDD con una aplicación de servicio de Windows de ASP.NET Core, el SDK genera un archivo ejecutable (*\*.exe*), denominado *ejecutable dependiente del marco*.</span><span class="sxs-lookup"><span data-stu-id="97d93-113">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="97d93-114">Implementación autocontenida</span><span class="sxs-lookup"><span data-stu-id="97d93-114">Self-contained deployment</span></span>

<span data-ttu-id="97d93-115">Una implementación autocontenida (SCD) no depende de la presencia de los componentes compartidos en el sistema de destino.</span><span class="sxs-lookup"><span data-stu-id="97d93-115">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="97d93-116">El tiempo de ejecución y las dependencias de la aplicación se implementan con la aplicación en el sistema host.</span><span class="sxs-lookup"><span data-stu-id="97d93-116">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="97d93-117">Convertir un proyecto en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="97d93-117">Convert a project into a Windows Service</span></span>

<span data-ttu-id="97d93-118">Realice los cambios siguientes en un proyecto de ASP.NET Core existente para ejecutar la aplicación como un servicio:</span><span class="sxs-lookup"><span data-stu-id="97d93-118">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="97d93-119">Actualizaciones del archivo del proyecto</span><span class="sxs-lookup"><span data-stu-id="97d93-119">Project file updates</span></span>

<span data-ttu-id="97d93-120">En función de su elección de [tipo de implementación](#deployment-type), actualice el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="97d93-120">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="97d93-121">Implementación dependiente de marco (FDD)</span><span class="sxs-lookup"><span data-stu-id="97d93-121">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="97d93-122">Agregue un [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) de Windows al `<PropertyGroup>` que contiene la plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="97d93-122">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="97d93-123">En el ejemplo siguiente, el RID se establece en `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="97d93-123">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="97d93-124">Agregue la propiedad `<SelfContained>` establecida en `false`.</span><span class="sxs-lookup"><span data-stu-id="97d93-124">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="97d93-125">Estas propiedades indican al SDK que genere un archivo ejecutable (*.exe*) de Windows.</span><span class="sxs-lookup"><span data-stu-id="97d93-125">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="97d93-126">No se requiere un archivo *web.config*, que normalmente se produce cuando se publica una aplicación ASP.NET Core, para una aplicación de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="97d93-126">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="97d93-127">Para deshabilitar la creación de un archivo *web.config* agregue la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="97d93-127">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="97d93-128">Agregue la propiedad `<UseAppHost>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="97d93-128">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="97d93-129">Esta propiedad proporciona el servicio con una ruta de acceso de activación (un archivo ejecutable, *.exe*) para una FDD.</span><span class="sxs-lookup"><span data-stu-id="97d93-129">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="97d93-130">Implementación autocontenida (SCD)</span><span class="sxs-lookup"><span data-stu-id="97d93-130">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="97d93-131">Confirme la presencia del [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) de Windows o agregue un RID al `<PropertyGroup>` que contiene la plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="97d93-131">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="97d93-132">Deshabilite la creación de un archivo *web.config* agregando la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="97d93-132">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="97d93-133">Para publicar para varios RID:</span><span class="sxs-lookup"><span data-stu-id="97d93-133">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="97d93-134">Proporcione los RID en una lista delimitada por punto y coma.</span><span class="sxs-lookup"><span data-stu-id="97d93-134">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="97d93-135">Use el nombre de la propiedad `<RuntimeIdentifiers>` (plural).</span><span class="sxs-lookup"><span data-stu-id="97d93-135">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="97d93-136">Para más información, vea el [Catálogo de identificadores de entorno de ejecución (RID) de .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="97d93-136">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="97d93-137">Agregue una referencia de paquete de [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="97d93-137">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="97d93-138">Para habilitar el registro de eventos de Windows, agregue una referencia de paquete para [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="97d93-138">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="97d93-139">Para obtener más información, consulte la sección [Control de los eventos de inicio y detención](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="97d93-139">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="97d93-140">Actualizaciones de Program.Main</span><span class="sxs-lookup"><span data-stu-id="97d93-140">Program.Main updates</span></span>

<span data-ttu-id="97d93-141">Realice los siguientes cambios en `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="97d93-141">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="97d93-142">Para probar y depurar cuando se ejecuta fuera de un servicio, agregue código para determinar si la aplicación se ejecuta como un servicio o una aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="97d93-142">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="97d93-143">Compruebe si el depurador está asociado o hay presente un argumento de línea de comandos `--console`.</span><span class="sxs-lookup"><span data-stu-id="97d93-143">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="97d93-144">Si alguna de estas condiciones se cumple (la aplicación no se ejecuta como un servicio), llame a <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> en el host web.</span><span class="sxs-lookup"><span data-stu-id="97d93-144">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="97d93-145">Si las condiciones no se cumplen (la aplicación se ejecuta como servicio):</span><span class="sxs-lookup"><span data-stu-id="97d93-145">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="97d93-146">Llame a <xref:System.IO.Directory.SetCurrentDirectory*> y use una ruta de acceso a la ubicación de publicación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="97d93-146">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="97d93-147">No llame a <xref:System.IO.Directory.GetCurrentDirectory*> para obtener la ruta de acceso porque una aplicación de servicio de Windows devuelve una carpeta *C:\\WINDOWS\\system32* cuando se llama a <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="97d93-147">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="97d93-148">Para obtener más información, consulte la sección [Directorio actual y raíz del contenido](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="97d93-148">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="97d93-149">Llame a <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> para ejecutar la aplicación como un servicio.</span><span class="sxs-lookup"><span data-stu-id="97d93-149">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="97d93-150">Dado que el [Proveedor de configuración de línea de comandos](xref:fundamentals/configuration/index#command-line-configuration-provider) requiere pares nombre-valor en los argumentos de línea de comandos, el conmutador `--console` se quita de los argumentos antes de que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> los reciba.</span><span class="sxs-lookup"><span data-stu-id="97d93-150">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="97d93-151">Para escribir en el registro de eventos de Windows, agregue el proveedor EventLog a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="97d93-151">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="97d93-152">Establezca el nivel de registro con la clave `Logging:LogLevel:Default` en el archivo *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="97d93-152">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="97d93-153">Para fines de prueba y demostración, el archivo de configuración de producción de la aplicación de ejemplo establece el nivel de registro en `Information`.</span><span class="sxs-lookup"><span data-stu-id="97d93-153">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="97d93-154">En producción, el valor se establece normalmente en `Error`.</span><span class="sxs-lookup"><span data-stu-id="97d93-154">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="97d93-155">Para obtener más información, vea <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="97d93-155">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

### <a name="publish-the-app"></a><span data-ttu-id="97d93-156">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="97d93-156">Publish the app</span></span>

<span data-ttu-id="97d93-157">Publique la aplicación con [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) o Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="97d93-157">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="97d93-158">Al usar Visual Studio, seleccione **FolderProfile** y configure la **Ubicación de destino** antes de seleccionar el botón **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="97d93-158">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="97d93-159">Para publicar la aplicación de ejemplo mediante herramientas de la interfaz de la línea de comandos (CLI), ejecute el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) en un símbolo del sistema desde la carpeta del proyecto con una configuración de versión pasada a la opción [-c|--configuration](/dotnet/core/tools/dotnet-publish#options).</span><span class="sxs-lookup"><span data-stu-id="97d93-159">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="97d93-160">Use la opción [-o|--output](/dotnet/core/tools/dotnet-publish#options) con una ruta de acceso para publicar en una carpeta fuera de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="97d93-160">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

#### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="97d93-161">Publicación de una implementación dependiente de marco (FDD)</span><span class="sxs-lookup"><span data-stu-id="97d93-161">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="97d93-162">En el ejemplo siguiente, la aplicación está publicada para la carpeta *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="97d93-162">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

#### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="97d93-163">Publicación de una implementación autocontenida (SCD)</span><span class="sxs-lookup"><span data-stu-id="97d93-163">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="97d93-164">El identificador relativo debe especificarse en la propiedad `<RuntimeIdenfifier>` o `<RuntimeIdentifiers>` del archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="97d93-164">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="97d93-165">Proporcione el entorno de ejecución a la opción [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) del comando `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="97d93-165">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="97d93-166">En el ejemplo siguiente, la aplicación está publicada para el entorno de ejecución `win7-x64` en la carpeta *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="97d93-166">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

### <a name="create-a-user-account"></a><span data-ttu-id="97d93-167">Creación de una cuenta de usuario</span><span class="sxs-lookup"><span data-stu-id="97d93-167">Create a user account</span></span>

<span data-ttu-id="97d93-168">Cree una cuenta de usuario para el servicio mediante el comando `net user` de un shell de comandos administrativos:</span><span class="sxs-lookup"><span data-stu-id="97d93-168">Create a user account for the service using the `net user` command from an administrative command shell:</span></span>

```console
net user {USER ACCOUNT} {PASSWORD} /add
```

<span data-ttu-id="97d93-169">el período de expiración de la contraseña predeterminado es de seis semanas.</span><span class="sxs-lookup"><span data-stu-id="97d93-169">The default password expiration is six weeks.</span></span>

<span data-ttu-id="97d93-170">Para la aplicación de ejemplo, cree una cuenta de usuario con el nombre `ServiceUser` y una contraseña.</span><span class="sxs-lookup"><span data-stu-id="97d93-170">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="97d93-171">En el comando siguiente, reemplace `{PASSWORD}` por una [contraseña segura](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="97d93-171">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

```console
net user ServiceUser {PASSWORD} /add
```

<span data-ttu-id="97d93-172">Si necesita agregar el usuario a un grupo, use el comando `net localgroup`, donde `{GROUP}` es el nombre del grupo:</span><span class="sxs-lookup"><span data-stu-id="97d93-172">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

```console
net localgroup {GROUP} {USER ACCOUNT} /add
```

<span data-ttu-id="97d93-173">Para más información, vea [Service User Accounts](/windows/desktop/services/service-user-accounts) (Cuentas de usuario de servicio).</span><span class="sxs-lookup"><span data-stu-id="97d93-173">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="97d93-174">Un enfoque alternativo de administración de usuarios al usar Active Directory consiste en usar cuentas de servicio administradas.</span><span class="sxs-lookup"><span data-stu-id="97d93-174">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="97d93-175">Para obtener más información, consulte [grupo Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) (Información general sobre cuentas de servicio administradas de grupo).</span><span class="sxs-lookup"><span data-stu-id="97d93-175">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

### <a name="set-permissions"></a><span data-ttu-id="97d93-176">Establecer permisos</span><span class="sxs-lookup"><span data-stu-id="97d93-176">Set permissions</span></span>

#### <a name="access-to-the-app-folder"></a><span data-ttu-id="97d93-177">Acceso a la carpeta Aplicación</span><span class="sxs-lookup"><span data-stu-id="97d93-177">Access to the app folder</span></span>

<span data-ttu-id="97d93-178">Conceda acceso de escritura, lectura y ejecución a la carpeta de la aplicación con el comando [icacls](/windows-server/administration/windows-commands/icacls) de un shell de comandos administrativos:</span><span class="sxs-lookup"><span data-stu-id="97d93-178">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command from an administrative command shell:</span></span>

```console
icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
```

* <span data-ttu-id="97d93-179">`{PATH}`: ruta de acceso a la carpeta de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="97d93-179">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="97d93-180">`{USER ACCOUNT}`: cuenta del usuario (SID).</span><span class="sxs-lookup"><span data-stu-id="97d93-180">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="97d93-181">`(OI)`: la marca Object Inherit (Heredar objetos) propaga los permisos a los archivos subordinados.</span><span class="sxs-lookup"><span data-stu-id="97d93-181">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="97d93-182">`(CI)`: la marca Container Inherit (Heredar contenedores) propaga los permisos a las carpetas subordinadas.</span><span class="sxs-lookup"><span data-stu-id="97d93-182">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="97d93-183">`{PERMISSION FLAGS}`: define los permisos de acceso a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="97d93-183">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="97d93-184">Escritura (`W`)</span><span class="sxs-lookup"><span data-stu-id="97d93-184">Write (`W`)</span></span>
  * <span data-ttu-id="97d93-185">Lectura (`R`)</span><span class="sxs-lookup"><span data-stu-id="97d93-185">Read (`R`)</span></span>
  * <span data-ttu-id="97d93-186">Ejecución (`X`)</span><span class="sxs-lookup"><span data-stu-id="97d93-186">Execute (`X`)</span></span>
  * <span data-ttu-id="97d93-187">Total (`F`)</span><span class="sxs-lookup"><span data-stu-id="97d93-187">Full (`F`)</span></span>
  * <span data-ttu-id="97d93-188">Modificación (`M`)</span><span class="sxs-lookup"><span data-stu-id="97d93-188">Modify (`M`)</span></span>
* <span data-ttu-id="97d93-189">`/t`: se aplica de forma recursiva a las carpetas y los archivos subordinados existentes.</span><span class="sxs-lookup"><span data-stu-id="97d93-189">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="97d93-190">Para la aplicación de ejemplo publicada en la carpeta *c:\\svc* y en la cuenta `ServiceUser` con permisos de escritura, lectura y ejecución, use el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="97d93-190">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

```console
icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
```

<span data-ttu-id="97d93-191">Para más información, vea [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="97d93-191">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

#### <a name="log-on-as-a-service"></a><span data-ttu-id="97d93-192">Iniciar sesión como servicio</span><span class="sxs-lookup"><span data-stu-id="97d93-192">Log on as a service</span></span>

<span data-ttu-id="97d93-193">A fin de conceder el privilegio [Iniciar sesión como servicio](/windows/security/threat-protection/security-policy-settings/log-on-as-a-service) a la cuenta de usuario:</span><span class="sxs-lookup"><span data-stu-id="97d93-193">To grant the [Log on as a service](/windows/security/threat-protection/security-policy-settings/log-on-as-a-service) privilege to the user account:</span></span>

1. <span data-ttu-id="97d93-194">Busque las directivas de **asignación de derechos de usuario** en la consola de directiva de seguridad local o la consola de editor de directivas de grupo local.</span><span class="sxs-lookup"><span data-stu-id="97d93-194">Locate the **User Rights Assignment** policies in either the Local Security Policy console or Local Group Policy Editor console.</span></span> <span data-ttu-id="97d93-195">Para obtener instrucciones, consulte: [Definición de la configuración de la directiva de seguridad](/windows/security/threat-protection/security-policy-settings/how-to-configure-security-policy-settings).</span><span class="sxs-lookup"><span data-stu-id="97d93-195">For instructions, see: [Configure security policy settings](/windows/security/threat-protection/security-policy-settings/how-to-configure-security-policy-settings).</span></span>
1. <span data-ttu-id="97d93-196">Busque la directiva `Log on as a service`.</span><span class="sxs-lookup"><span data-stu-id="97d93-196">Locate the `Log on as a service` policy.</span></span> <span data-ttu-id="97d93-197">Haga doble clic en la directiva para abrirla.</span><span class="sxs-lookup"><span data-stu-id="97d93-197">Double-click the policy to open it.</span></span>
1. <span data-ttu-id="97d93-198">Seleccione **Agregar usuario o grupo**.</span><span class="sxs-lookup"><span data-stu-id="97d93-198">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="97d93-199">Seleccione **Avanzado** y seleccione **Buscar ahora**.</span><span class="sxs-lookup"><span data-stu-id="97d93-199">Select **Advanced** and select **Find Now**.</span></span>
1. <span data-ttu-id="97d93-200">Seleccione la cuenta de usuario creada en la sección [Creación de una cuenta de usuario](#create-a-user-account) anteriormente.</span><span class="sxs-lookup"><span data-stu-id="97d93-200">Select the user account created in the [Create a user account](#create-a-user-account) section earlier.</span></span> <span data-ttu-id="97d93-201">Seleccione **Aceptar** para aceptar la selección.</span><span class="sxs-lookup"><span data-stu-id="97d93-201">Select **OK** to accept the selection.</span></span>
1. <span data-ttu-id="97d93-202">Seleccione **Aceptar** una vez confirmado que el nombre de objeto es correcto.</span><span class="sxs-lookup"><span data-stu-id="97d93-202">Select **OK** after confirming that the object name is correct.</span></span>
1. <span data-ttu-id="97d93-203">Seleccione **Aplicar**.</span><span class="sxs-lookup"><span data-stu-id="97d93-203">Select **Apply**.</span></span> <span data-ttu-id="97d93-204">Seleccione **Aceptar** para cerrar la ventana de la directiva.</span><span class="sxs-lookup"><span data-stu-id="97d93-204">Select **OK** to close the policy window.</span></span>

## <a name="manage-the-service"></a><span data-ttu-id="97d93-205">Administración del servicio</span><span class="sxs-lookup"><span data-stu-id="97d93-205">Manage the service</span></span>

### <a name="create-the-service"></a><span data-ttu-id="97d93-206">Crear el servicio</span><span class="sxs-lookup"><span data-stu-id="97d93-206">Create the service</span></span>

<span data-ttu-id="97d93-207">Use la herramienta de línea de comandos [sc.exe](https://technet.microsoft.com/library/bb490995) para crear el servicio a partir de un shell de comandos administrativos.</span><span class="sxs-lookup"><span data-stu-id="97d93-207">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service from an administrative command shell.</span></span> <span data-ttu-id="97d93-208">El valor `binPath` es la ruta de acceso al archivo ejecutable de la aplicación, que incluye el nombre del archivo ejecutable.</span><span class="sxs-lookup"><span data-stu-id="97d93-208">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="97d93-209">**El espacio entre el signo igual y las comillas de cada parámetro y valor es obligatorio.**</span><span class="sxs-lookup"><span data-stu-id="97d93-209">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

```console
sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
```

* <span data-ttu-id="97d93-210">`{SERVICE NAME}`: nombre para asignar al servicio en el [Administrador de control de servicios](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="97d93-210">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
* <span data-ttu-id="97d93-211">`{PATH}`: ruta de acceso al archivo ejecutable del servicio.</span><span class="sxs-lookup"><span data-stu-id="97d93-211">`{PATH}` &ndash; The path to the service executable.</span></span>
* <span data-ttu-id="97d93-212">`{DOMAIN}` &ndash; El dominio de una máquina unida al dominio.</span><span class="sxs-lookup"><span data-stu-id="97d93-212">`{DOMAIN}` &ndash; The domain of a domain-joined machine.</span></span> <span data-ttu-id="97d93-213">Si la máquina no está unida al dominio, use el nombre del equipo local.</span><span class="sxs-lookup"><span data-stu-id="97d93-213">If the machine isn't domain-joined, use the local machine name.</span></span>
* <span data-ttu-id="97d93-214">`{USER ACCOUNT}` &ndash; La cuenta de usuario en la que se ejecuta el servicio.</span><span class="sxs-lookup"><span data-stu-id="97d93-214">`{USER ACCOUNT}` &ndash; The user account under which the service runs.</span></span>
* <span data-ttu-id="97d93-215">`{PASSWORD}`: contraseña de la cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="97d93-215">`{PASSWORD}` &ndash; The user account password.</span></span>

> [!WARNING]
> <span data-ttu-id="97d93-216">No **omita** el parámetro `obj`.</span><span class="sxs-lookup"><span data-stu-id="97d93-216">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="97d93-217">El valor predeterminado de `obj` es la cuenta [LocalSystem](/windows/desktop/services/localsystem-account).</span><span class="sxs-lookup"><span data-stu-id="97d93-217">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="97d93-218">Ejecutar un servicio en la cuenta `LocalSystem` plantea un riesgo de seguridad importante.</span><span class="sxs-lookup"><span data-stu-id="97d93-218">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="97d93-219">Ejecute siempre un servicio con una cuenta de usuario que tenga privilegios restringidos.</span><span class="sxs-lookup"><span data-stu-id="97d93-219">Always run a service with a user account that has restricted privileges.</span></span>

<span data-ttu-id="97d93-220">En el ejemplo siguiente de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="97d93-220">In the following example for the sample app:</span></span>

* <span data-ttu-id="97d93-221">El servicio se denomina **MyService**.</span><span class="sxs-lookup"><span data-stu-id="97d93-221">The service is named **MyService**.</span></span>
* <span data-ttu-id="97d93-222">El servicio publicado reside en la carpeta *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="97d93-222">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="97d93-223">El ejecutable de la aplicación se denomina *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="97d93-223">The app executable is named *SampleApp.exe*.</span></span> <span data-ttu-id="97d93-224">Ponga el valor `binPath` entre comillas dobles (" ").</span><span class="sxs-lookup"><span data-stu-id="97d93-224">Enclose the `binPath` value in double quotation marks (").</span></span>
* <span data-ttu-id="97d93-225">El servicio se ejecuta en la cuenta `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="97d93-225">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="97d93-226">Reemplace `{DOMAIN}` por el dominio de la cuenta de usuario o el nombre de la máquina local.</span><span class="sxs-lookup"><span data-stu-id="97d93-226">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="97d93-227">Ponga el valor `obj` entre comillas dobles (" ").</span><span class="sxs-lookup"><span data-stu-id="97d93-227">Enclose the `obj` value in double quotation marks (").</span></span> <span data-ttu-id="97d93-228">Ejemplo: si el sistema de hospedaje es una máquina local denominada `MairaPC`, establezca `obj` en `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="97d93-228">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
* <span data-ttu-id="97d93-229">Reemplace `{PASSWORD}` con la contraseña de la cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="97d93-229">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="97d93-230">Ponga el valor `password` entre comillas dobles (" ").</span><span class="sxs-lookup"><span data-stu-id="97d93-230">Enclose the `password` value in double quotation marks (").</span></span>

```console
sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
```

> [!IMPORTANT]
> <span data-ttu-id="97d93-231">Asegúrese de que existen los espacios entre los signos igual de los parámetros y los valores de los parámetros.</span><span class="sxs-lookup"><span data-stu-id="97d93-231">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

### <a name="start-the-service"></a><span data-ttu-id="97d93-232">Inicio del servicio</span><span class="sxs-lookup"><span data-stu-id="97d93-232">Start the service</span></span>

<span data-ttu-id="97d93-233">Inicie el servicio con el comando `sc start {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="97d93-233">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

<span data-ttu-id="97d93-234">Use el siguiente comando para iniciar el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="97d93-234">To start the sample app service, use the following command:</span></span>

```console
sc start MyService
```

<span data-ttu-id="97d93-235">Este comando tarda unos segundos en iniciar el servicio.</span><span class="sxs-lookup"><span data-stu-id="97d93-235">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="97d93-236">Determinación del estado del servicio</span><span class="sxs-lookup"><span data-stu-id="97d93-236">Determine the service status</span></span>

<span data-ttu-id="97d93-237">Para comprobar el estado del servicio, use el comando `sc query {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="97d93-237">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="97d93-238">El estado se notifica como uno de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="97d93-238">The status is reported as one of the following values:</span></span>

* `START_PENDING`
* `RUNNING`
* `STOP_PENDING`
* `STOPPED`

<span data-ttu-id="97d93-239">Use el siguiente comando para comprobar el estado del servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="97d93-239">Use the following command to check the status of the sample app service:</span></span>

```console
sc query MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="97d93-240">Examinar un servicio de aplicación web</span><span class="sxs-lookup"><span data-stu-id="97d93-240">Browse a web app service</span></span>

<span data-ttu-id="97d93-241">Si el servicio está en estado `RUNNING` y dicho servicio es una aplicación web, vaya a la aplicación en su ruta de acceso correspondiente (de forma predeterminada, `http://localhost:5000`, que redirige a `https://localhost:5001` cuando se usa el [Middleware de redirección de HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="97d93-241">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="97d93-242">Si es el servicio de la aplicación de ejemplo, vaya a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="97d93-242">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="97d93-243">Detener el servicio</span><span class="sxs-lookup"><span data-stu-id="97d93-243">Stop the service</span></span>

<span data-ttu-id="97d93-244">Detenga el servicio con el comando `sc stop {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="97d93-244">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

<span data-ttu-id="97d93-245">Con el siguiente comando se detiene el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="97d93-245">The following command stops the sample app service:</span></span>

```console
sc stop MyService
```

### <a name="delete-the-service"></a><span data-ttu-id="97d93-246">Eliminación del servicio</span><span class="sxs-lookup"><span data-stu-id="97d93-246">Delete the service</span></span>

<span data-ttu-id="97d93-247">Tras un breve intervalo para detener un servicio, desinstale el servicio con el comando `sc delete {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="97d93-247">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

<span data-ttu-id="97d93-248">Compruebe el estado del servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="97d93-248">Check the status of the sample app service:</span></span>

```console
sc query MyService
```

<span data-ttu-id="97d93-249">Si el servicio de la aplicación de ejemplo está en estado `STOPPED`, use el siguiente comando para desinstalar el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="97d93-249">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

```console
sc delete MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="97d93-250">Control de los eventos de inicio y detención</span><span class="sxs-lookup"><span data-stu-id="97d93-250">Handle starting and stopping events</span></span>

<span data-ttu-id="97d93-251">Para controlar los eventos <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> y <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, realice los siguientes cambios adicionales:</span><span class="sxs-lookup"><span data-stu-id="97d93-251">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="97d93-252">Cree una clase que se derive de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> con los métodos `OnStarting`, `OnStarted` y `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="97d93-252">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="97d93-253">Cree un método de extensión para <xref:Microsoft.AspNetCore.Hosting.IWebHost> que pase `CustomWebHostService` a <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="97d93-253">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="97d93-254">En `Program.Main`, llame al método de extensión `RunAsCustomService` en lugar de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="97d93-254">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="97d93-255">Para ver la ubicación de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> en `Program.Main`, consulte el ejemplo de código que se muestra en la sección [Convertir un proyecto en un servicio de Windows](#convert-a-project-into-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="97d93-255">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="97d93-256">Escenarios de servidor proxy y equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="97d93-256">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="97d93-257">Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="97d93-257">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="97d93-258">Para obtener más información, vea <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="97d93-258">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="97d93-259">Configuración de HTTPS</span><span class="sxs-lookup"><span data-stu-id="97d93-259">Configure HTTPS</span></span>

<span data-ttu-id="97d93-260">Para configurar el servicio con un punto de conexión seguro:</span><span class="sxs-lookup"><span data-stu-id="97d93-260">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="97d93-261">Cree un certificado X.509 para el sistema de hospedaje mediante la adquisición del certificado de la plataforma y con mecanismos de implementación.</span><span class="sxs-lookup"><span data-stu-id="97d93-261">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="97d93-262">Especifique una [configuración de punto de conexión HTTPS de servidor Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) para usar el certificado.</span><span class="sxs-lookup"><span data-stu-id="97d93-262">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="97d93-263">No se admite el uso del certificado de desarrollo HTTPS de ASP.NET Core para proteger un punto de conexión de servicio.</span><span class="sxs-lookup"><span data-stu-id="97d93-263">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="97d93-264">Directorio actual y raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="97d93-264">Current directory and content root</span></span>

<span data-ttu-id="97d93-265">El directorio de trabajo actual devuelto por una llamada a <xref:System.IO.Directory.GetCurrentDirectory*> para un servicio de Windows es la carpeta *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="97d93-265">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="97d93-266">La carpeta *system32* no es una ubicación adecuada para almacenar los archivos de un servicio (por ejemplo los archivos de configuración).</span><span class="sxs-lookup"><span data-stu-id="97d93-266">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="97d93-267">Utilice uno de los siguientes enfoques para mantener los archivos de configuración y los activos de un servicio, así como para acceder a ellos.</span><span class="sxs-lookup"><span data-stu-id="97d93-267">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="97d93-268">Configuración de la ruta de acceso raíz del contenido en la carpeta de la aplicación</span><span class="sxs-lookup"><span data-stu-id="97d93-268">Set the content root path to the app's folder</span></span>

<span data-ttu-id="97d93-269"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> es la misma ruta de acceso proporcionada al argumento `binPath` cuando se crea el servicio.</span><span class="sxs-lookup"><span data-stu-id="97d93-269">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="97d93-270">En lugar de llamar `GetCurrentDirectory` para crear rutas de acceso a los archivos de configuración, llame a <xref:System.IO.Directory.SetCurrentDirectory*> con la ruta de acceso a la raíz del contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="97d93-270">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="97d93-271">En `Program.Main`, determine la ruta de acceso a la carpeta del archivo ejecutable del servicio y use la ruta de acceso para establecer la raíz del contenido de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="97d93-271">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="97d93-272">Almacene los archivos del servicio en una ubicación adecuada en el disco.</span><span class="sxs-lookup"><span data-stu-id="97d93-272">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="97d93-273">Especifique una ruta de acceso absoluta con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> al utilizar un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> a la carpeta que contiene los archivos.</span><span class="sxs-lookup"><span data-stu-id="97d93-273">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="97d93-274">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="97d93-274">Additional resources</span></span>

* <span data-ttu-id="97d93-275">[Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)</span><span class="sxs-lookup"><span data-stu-id="97d93-275">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
