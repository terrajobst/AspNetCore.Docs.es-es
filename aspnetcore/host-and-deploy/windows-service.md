---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: guardrex
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/21/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: ab36bc1b2827c80bb1e7b9e8cee558b346a991f8
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251427"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="64b82-103">Hospedaje de ASP.NET Core en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="64b82-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="64b82-104">Por [Luke Latham](https://github.com/guardrex) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="64b82-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="64b82-105">Una aplicación de ASP.NET Core se puede hospedar en Windows sin usar IIS como [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="64b82-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="64b82-106">Cuando se hospeda como un servicio de Windows, la aplicación se inicia automáticamente después de reiniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="64b82-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="64b82-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="64b82-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64b82-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="64b82-108">Prerequisites</span></span>

* [<span data-ttu-id="64b82-109">ASP.NET Core SDK 2.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="64b82-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="64b82-110">PowerShell 6.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="64b82-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="app-configuration"></a><span data-ttu-id="64b82-111">Configuración de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="64b82-111">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="64b82-112">Se llama a `IHostBuilder.UseWindowsService`, proporcionado por el paquete [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices), cuando se crea el host.</span><span class="sxs-lookup"><span data-stu-id="64b82-112">`IHostBuilder.UseWindowsService`, provided by the [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) package, is called when building the host.</span></span> <span data-ttu-id="64b82-113">Si la aplicación se ejecuta como un servicio de Windows, el método:</span><span class="sxs-lookup"><span data-stu-id="64b82-113">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="64b82-114">Establece la vigencia del host en `WindowsServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="64b82-114">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="64b82-115">Establece la raíz de contenido.</span><span class="sxs-lookup"><span data-stu-id="64b82-115">Sets the content root.</span></span>
* <span data-ttu-id="64b82-116">Habilita el registro en el registro de eventos con el nombre de la aplicación como nombre de origen predeterminado.</span><span class="sxs-lookup"><span data-stu-id="64b82-116">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="64b82-117">El nivel de registro puede configurarse con la clave `Logging:LogLevel:Default` en el archivo *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="64b82-117">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="64b82-118">Los administradores son los únicos que pueden crear nuevos orígenes de eventos.</span><span class="sxs-lookup"><span data-stu-id="64b82-118">Only administrators can create new event sources.</span></span> <span data-ttu-id="64b82-119">Cuando no se puede crear un origen de eventos con el nombre de la aplicación, se registra una advertencia para el origen *Aplicación* y los registros de eventos se deshabilitan.</span><span class="sxs-lookup"><span data-stu-id="64b82-119">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="64b82-120">La aplicación requiere referencias de paquetes para [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) y [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="64b82-120">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="64b82-121">Para probar y depurar cuando se ejecuta fuera de un servicio, agregue código para determinar si la aplicación se ejecuta como un servicio o una aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="64b82-121">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="64b82-122">Compruebe si el depurador está asociado o hay presente un conmutador `--console`.</span><span class="sxs-lookup"><span data-stu-id="64b82-122">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="64b82-123">Si alguna de estas condiciones se cumple (la aplicación no se ejecuta como un servicio), llame a <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="64b82-123">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="64b82-124">Si las condiciones no se cumplen (la aplicación se ejecuta como servicio):</span><span class="sxs-lookup"><span data-stu-id="64b82-124">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="64b82-125">Llame a <xref:System.IO.Directory.SetCurrentDirectory*> y use una ruta de acceso a la ubicación de publicación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="64b82-125">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="64b82-126">No llame a <xref:System.IO.Directory.GetCurrentDirectory*> para obtener la ruta de acceso porque una aplicación de servicio de Windows devuelve una carpeta *C:\\WINDOWS\\system32* cuando se llama a <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="64b82-126">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="64b82-127">Para obtener más información, consulte la sección [Directorio actual y raíz del contenido](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="64b82-127">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="64b82-128">Este paso se realiza antes de que la aplicación se configure en `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="64b82-128">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="64b82-129">Llame a <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> para ejecutar la aplicación como un servicio.</span><span class="sxs-lookup"><span data-stu-id="64b82-129">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="64b82-130">Dado que el [Proveedor de configuración de línea de comandos](xref:fundamentals/configuration/index#command-line-configuration-provider) requiere pares nombre-valor en los argumentos de línea de comandos, el conmutador `--console` se quita de los argumentos antes de que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> los reciba.</span><span class="sxs-lookup"><span data-stu-id="64b82-130">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="64b82-131">Para escribir en el registro de eventos de Windows, agregue el proveedor EventLog a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="64b82-131">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="64b82-132">Establezca el nivel de registro con la clave `Logging:LogLevel:Default` en el archivo *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="64b82-132">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="64b82-133">En el ejemplo siguiente de la aplicación de ejemplo, se llama a `RunAsCustomService` en lugar de a <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> con el fin de controlar los eventos de duración dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="64b82-133">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="64b82-134">Para obtener más información, consulte la sección [Control de los eventos de inicio y detención](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="64b82-134">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="64b82-135">Tipo de implementación</span><span class="sxs-lookup"><span data-stu-id="64b82-135">Deployment type</span></span>

<span data-ttu-id="64b82-136">Para obtener información y consejos sobre los escenarios de implementación, consulte [Implementación de aplicaciones .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="64b82-136">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="64b82-137">Implementación dependiente de marco (FDD)</span><span class="sxs-lookup"><span data-stu-id="64b82-137">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="64b82-138">La implementación dependiente de marco de trabajo (FDD) se basa en la presencia de una versión compartida de .NET Core en todo el sistema en el sistema de destino.</span><span class="sxs-lookup"><span data-stu-id="64b82-138">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="64b82-139">Cuando se adopta el escenario FDD siguiendo las instrucciones de este artículo, el SDK genera un archivo ejecutable ( *.exe*), denominado *ejecutable dependiente del marco*.</span><span class="sxs-lookup"><span data-stu-id="64b82-139">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="64b82-140">Agregue los siguientes elementos de propiedad al archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="64b82-140">Add the following property elements to the project file:</span></span>

* <span data-ttu-id="64b82-141">`<OutputType>`&ndash; Tipo de salida de la aplicación (`Exe` para ejecutable).</span><span class="sxs-lookup"><span data-stu-id="64b82-141">`<OutputType>` &ndash; The app's output type (`Exe` for executable).</span></span>
* <span data-ttu-id="64b82-142">`<LangVersion>`&ndash; Versión del lenguaje C# (`latest` o `preview`).</span><span class="sxs-lookup"><span data-stu-id="64b82-142">`<LangVersion>` &ndash; The C# language version (`latest` or `preview`).</span></span>

<span data-ttu-id="64b82-143">No se requiere un archivo *web.config*, que normalmente se produce cuando se publica una aplicación ASP.NET Core, para una aplicación de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="64b82-143">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="64b82-144">Para deshabilitar la creación de un archivo *web.config* agregue la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="64b82-144">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <OutputType>Exe</OutputType>
  <LangVersion>preview</LangVersion>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="64b82-145">El [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contiene la plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="64b82-145">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="64b82-146">En el ejemplo siguiente, el RID se establece en `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="64b82-146">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="64b82-147">La propiedad `<SelfContained>` se establece en `false`.</span><span class="sxs-lookup"><span data-stu-id="64b82-147">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="64b82-148">Estas propiedades indican al SDK que genere un archivo ejecutable ( *.exe*) para Windows y una aplicación que depende del marco .NET Core compartido.</span><span class="sxs-lookup"><span data-stu-id="64b82-148">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="64b82-149">No se requiere un archivo *web.config*, que normalmente se produce cuando se publica una aplicación ASP.NET Core, para una aplicación de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="64b82-149">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="64b82-150">Para deshabilitar la creación de un archivo *web.config* agregue la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="64b82-150">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="64b82-151">El [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contiene la plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="64b82-151">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="64b82-152">En el ejemplo siguiente, el RID se establece en `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="64b82-152">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="64b82-153">La propiedad `<SelfContained>` se establece en `false`.</span><span class="sxs-lookup"><span data-stu-id="64b82-153">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="64b82-154">Estas propiedades indican al SDK que genere un archivo ejecutable ( *.exe*) para Windows y una aplicación que depende del marco .NET Core compartido.</span><span class="sxs-lookup"><span data-stu-id="64b82-154">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="64b82-155">La propiedad `<UseAppHost>` se establece en `true`.</span><span class="sxs-lookup"><span data-stu-id="64b82-155">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="64b82-156">Esta propiedad proporciona el servicio con una ruta de acceso de activación (un archivo ejecutable, *.exe*) para una FDD.</span><span class="sxs-lookup"><span data-stu-id="64b82-156">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="64b82-157">No se requiere un archivo *web.config*, que normalmente se produce cuando se publica una aplicación ASP.NET Core, para una aplicación de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="64b82-157">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="64b82-158">Para deshabilitar la creación de un archivo *web.config* agregue la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="64b82-158">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="64b82-159">Implementación autocontenida (SCD)</span><span class="sxs-lookup"><span data-stu-id="64b82-159">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="64b82-160">Una implementación autocontenida (SCD) no depende de la presencia de un marco compartido en el sistema host.</span><span class="sxs-lookup"><span data-stu-id="64b82-160">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="64b82-161">El tiempo de ejecución y las dependencias de la aplicación se implementan con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="64b82-161">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="64b82-162">Un [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) se incluye en el `<PropertyGroup>` que contiene la plataforma de destino:</span><span class="sxs-lookup"><span data-stu-id="64b82-162">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="64b82-163">Para publicar para varios RID:</span><span class="sxs-lookup"><span data-stu-id="64b82-163">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="64b82-164">Proporcione los RID en una lista delimitada por punto y coma.</span><span class="sxs-lookup"><span data-stu-id="64b82-164">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="64b82-165">Utilice el nombre de propiedad [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (en plural).</span><span class="sxs-lookup"><span data-stu-id="64b82-165">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="64b82-166">Para más información, vea el [Catálogo de identificadores de entorno de ejecución (RID) de .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="64b82-166">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="64b82-167">Una propiedad `<SelfContained>` se establece en `true`:</span><span class="sxs-lookup"><span data-stu-id="64b82-167">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="64b82-168">Cuenta de usuario de servicio</span><span class="sxs-lookup"><span data-stu-id="64b82-168">Service user account</span></span>

<span data-ttu-id="64b82-169">Para crear una cuenta de usuario para un servicio, use el cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) de un shell de comandos administrativos de PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="64b82-169">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="64b82-170">En la actualización de octubre de 2018 de Windows 10 (versión 1809/compilación 10.0.17763) o posterior:</span><span class="sxs-lookup"><span data-stu-id="64b82-170">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="64b82-171">En el sistema operativo Windows anterior a la actualización de octubre de 2018 de Windows 10 (versión 1809/compilación 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="64b82-171">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

<span data-ttu-id="64b82-172">Proporcione una [contraseña segura](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) cuando se le solicite.</span><span class="sxs-lookup"><span data-stu-id="64b82-172">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="64b82-173">A menos que el parámetro `-AccountExpires` se proporcione para el cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) con una fecha de expiración <xref:System.DateTime>, la cuenta no expirará.</span><span class="sxs-lookup"><span data-stu-id="64b82-173">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="64b82-174">Para más información, vea [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) y [Service User Accounts](/windows/desktop/services/service-user-accounts) (Cuentas de usuario del servicio).</span><span class="sxs-lookup"><span data-stu-id="64b82-174">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="64b82-175">Un enfoque alternativo de administración de usuarios al usar Active Directory consiste en usar cuentas de servicio administradas.</span><span class="sxs-lookup"><span data-stu-id="64b82-175">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="64b82-176">Para obtener más información, consulte [grupo Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) (Información general sobre cuentas de servicio administradas de grupo).</span><span class="sxs-lookup"><span data-stu-id="64b82-176">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="64b82-177">Derechos para iniciar sesión como servicio</span><span class="sxs-lookup"><span data-stu-id="64b82-177">Log on as a service rights</span></span>

<span data-ttu-id="64b82-178">Para establecer los derechos de *Iniciar sesión como servicio* para una cuenta de usuario del servicio:</span><span class="sxs-lookup"><span data-stu-id="64b82-178">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="64b82-179">Abra el editor de la Directiva de seguridad local mediante la ejecución de *secpool.msc*.</span><span class="sxs-lookup"><span data-stu-id="64b82-179">Open the Local Security Policy editor by running *secpool.msc*.</span></span>
1. <span data-ttu-id="64b82-180">Expanda el nodo **Directivas locales** y, después, seleccione **Asignación de derechos de usuario**.</span><span class="sxs-lookup"><span data-stu-id="64b82-180">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="64b82-181">Abra la directiva **Iniciar sesión como servicio**.</span><span class="sxs-lookup"><span data-stu-id="64b82-181">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="64b82-182">Seleccione **Agregar usuario o grupo**.</span><span class="sxs-lookup"><span data-stu-id="64b82-182">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="64b82-183">Proporcione el nombre de objeto (cuenta de usuario) mediante cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="64b82-183">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="64b82-184">Escriba la cuenta de usuario (`{DOMAIN OR COMPUTER NAME\USER}`) en el campo del nombre de objeto y seleccione **Aceptar** para agregar el usuario a la directiva.</span><span class="sxs-lookup"><span data-stu-id="64b82-184">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="64b82-185">Seleccione **Opciones avanzadas**.</span><span class="sxs-lookup"><span data-stu-id="64b82-185">Select **Advanced**.</span></span> <span data-ttu-id="64b82-186">Seleccione **Buscar ahora**.</span><span class="sxs-lookup"><span data-stu-id="64b82-186">Select **Find Now**.</span></span> <span data-ttu-id="64b82-187">Seleccione la cuenta de usuario de la lista.</span><span class="sxs-lookup"><span data-stu-id="64b82-187">Select the user account from the list.</span></span> <span data-ttu-id="64b82-188">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="64b82-188">Select **OK**.</span></span> <span data-ttu-id="64b82-189">Seleccione **Aceptar** nuevo para agregar el usuario a la directiva.</span><span class="sxs-lookup"><span data-stu-id="64b82-189">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="64b82-190">Seleccione **Aceptar** o **Aplicar** para aceptar los cambios.</span><span class="sxs-lookup"><span data-stu-id="64b82-190">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="64b82-191">Creación y administración del servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="64b82-191">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="64b82-192">Creación de un servicio</span><span class="sxs-lookup"><span data-stu-id="64b82-192">Create a service</span></span>

<span data-ttu-id="64b82-193">Use los comandos de PowerShell para registrar un servicio.</span><span class="sxs-lookup"><span data-stu-id="64b82-193">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="64b82-194">Desde un shell de comandos administrativos de PowerShell 6, ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="64b82-194">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="64b82-195">`{EXE PATH}`&ndash; Ruta de acceso a la carpeta de la aplicación en el host (por ejemplo, `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="64b82-195">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="64b82-196">No incluya el archivo ejecutable de la aplicación en la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="64b82-196">Don't include the app's executable in the path.</span></span> <span data-ttu-id="64b82-197">No se requiere una barra diagonal al final.</span><span class="sxs-lookup"><span data-stu-id="64b82-197">A trailing slash isn't required.</span></span>
* <span data-ttu-id="64b82-198">`{DOMAIN OR COMPUTER NAME\USER}` &ndash;Cuenta de usuario de servicio (por ejemplo, `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="64b82-198">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="64b82-199">`{NAME}` &ndash;Nombre de servicio (por ejemplo, `MyService`).</span><span class="sxs-lookup"><span data-stu-id="64b82-199">`{NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="64b82-200">`{EXE FILE PATH}` &ndash; Ruta de acceso del ejecutable de la aplicación (por ejemplo, `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="64b82-200">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="64b82-201">Incluya el nombre de archivo del ejecutable con la extensión.</span><span class="sxs-lookup"><span data-stu-id="64b82-201">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="64b82-202">`{DESCRIPTION}` &ndash; Descripción del servicio (por ejemplo, `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="64b82-202">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="64b82-203">`{DISPLAY NAME}` &ndash; Nombre para mostrar del servicio (por ejemplo, `My Service`).</span><span class="sxs-lookup"><span data-stu-id="64b82-203">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="64b82-204">Inicio de un servicio</span><span class="sxs-lookup"><span data-stu-id="64b82-204">Start a service</span></span>

<span data-ttu-id="64b82-205">Inicie el servicio con el siguiente comando de PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="64b82-205">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {NAME}
```

<span data-ttu-id="64b82-206">Este comando tarda unos segundos en iniciar el servicio.</span><span class="sxs-lookup"><span data-stu-id="64b82-206">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="64b82-207">Determinación del estado de un servicio</span><span class="sxs-lookup"><span data-stu-id="64b82-207">Determine a service's status</span></span>

<span data-ttu-id="64b82-208">Para comprobar el estado de un servicio, use el siguiente comando de PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="64b82-208">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {NAME}
```

<span data-ttu-id="64b82-209">El estado se notifica como uno de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="64b82-209">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="64b82-210">Detención de un servicio</span><span class="sxs-lookup"><span data-stu-id="64b82-210">Stop a service</span></span>

<span data-ttu-id="64b82-211">Detenga un servicio con el siguiente comando de PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="64b82-211">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="64b82-212">Eliminación de un servicio</span><span class="sxs-lookup"><span data-stu-id="64b82-212">Remove a service</span></span>

<span data-ttu-id="64b82-213">Tras un breve intervalo para detener un servicio, elimine el servicio con el siguiente comando de PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="64b82-213">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="64b82-214">Control de los eventos de inicio y detención</span><span class="sxs-lookup"><span data-stu-id="64b82-214">Handle starting and stopping events</span></span>

<span data-ttu-id="64b82-215">Para controlar los eventos <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> y <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>:</span><span class="sxs-lookup"><span data-stu-id="64b82-215">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="64b82-216">Cree una clase que se derive de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> con los métodos `OnStarting`, `OnStarted` y `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="64b82-216">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="64b82-217">Cree un método de extensión para <xref:Microsoft.AspNetCore.Hosting.IWebHost> que pase `CustomWebHostService` a <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="64b82-217">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="64b82-218">En `Program.Main`, llame al método de extensión `RunAsCustomService` en lugar de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="64b82-218">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="64b82-219">Para ver la ubicación de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> en `Program.Main`, consulte el ejemplo de código que se muestra en la sección [Tipo de implementación](#deployment-type).</span><span class="sxs-lookup"><span data-stu-id="64b82-219">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="64b82-220">Escenarios de servidor proxy y equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="64b82-220">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="64b82-221">Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="64b82-221">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="64b82-222">Para obtener más información, vea <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="64b82-222">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="64b82-223">Configuración de HTTPS</span><span class="sxs-lookup"><span data-stu-id="64b82-223">Configure HTTPS</span></span>

<span data-ttu-id="64b82-224">Para configurar el servicio con un punto de conexión seguro:</span><span class="sxs-lookup"><span data-stu-id="64b82-224">To configure a service with a secure endpoint:</span></span>

1. <span data-ttu-id="64b82-225">Cree un certificado X.509 para el sistema de hospedaje mediante la adquisición del certificado de la plataforma y con mecanismos de implementación.</span><span class="sxs-lookup"><span data-stu-id="64b82-225">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="64b82-226">Especifique una [configuración de punto de conexión HTTPS de servidor Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) para usar el certificado.</span><span class="sxs-lookup"><span data-stu-id="64b82-226">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="64b82-227">No se admite el uso del certificado de desarrollo HTTPS de ASP.NET Core para proteger un punto de conexión de servicio.</span><span class="sxs-lookup"><span data-stu-id="64b82-227">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="64b82-228">Directorio actual y raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="64b82-228">Current directory and content root</span></span>

<span data-ttu-id="64b82-229">El directorio de trabajo actual devuelto por una llamada a <xref:System.IO.Directory.GetCurrentDirectory*> para un servicio de Windows es la carpeta *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="64b82-229">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="64b82-230">La carpeta *system32* no es una ubicación adecuada para almacenar los archivos de un servicio (por ejemplo los archivos de configuración).</span><span class="sxs-lookup"><span data-stu-id="64b82-230">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="64b82-231">Utilice uno de los siguientes enfoques para mantener los archivos de configuración y los activos de un servicio, así como para acceder a ellos.</span><span class="sxs-lookup"><span data-stu-id="64b82-231">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="64b82-232">Uso de ContentRootPath o ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="64b82-232">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="64b82-233">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) o <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> para buscar los recursos de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="64b82-233">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="64b82-234">Configuración de la ruta de acceso raíz del contenido en la carpeta de la aplicación</span><span class="sxs-lookup"><span data-stu-id="64b82-234">Set the content root path to the app's folder</span></span>

<span data-ttu-id="64b82-235"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> es la misma ruta de acceso proporcionada al argumento `binPath` cuando se crea un servicio.</span><span class="sxs-lookup"><span data-stu-id="64b82-235">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="64b82-236">En lugar de llamar `GetCurrentDirectory` para crear rutas de acceso a los archivos de configuración, llame a <xref:System.IO.Directory.SetCurrentDirectory*> con la ruta de acceso a la raíz del contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="64b82-236">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="64b82-237">En `Program.Main`, determine la ruta de acceso a la carpeta del archivo ejecutable del servicio y use la ruta de acceso para establecer la raíz del contenido de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="64b82-237">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="64b82-238">Almacenamiento de los archivos de un servicio en una ubicación adecuada en el disco</span><span class="sxs-lookup"><span data-stu-id="64b82-238">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="64b82-239">Especifique una ruta de acceso absoluta con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> al utilizar un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> a la carpeta que contiene los archivos.</span><span class="sxs-lookup"><span data-stu-id="64b82-239">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="64b82-240">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="64b82-240">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="64b82-241">[Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)</span><span class="sxs-lookup"><span data-stu-id="64b82-241">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="64b82-242">[Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)</span><span class="sxs-lookup"><span data-stu-id="64b82-242">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
