---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: guardrex
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: b02e627af875f15a81d68b0d625a2eccf25c0657
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333803"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="8a574-103">Hospedaje de ASP.NET Core en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="8a574-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="8a574-104">Por [Luke Latham](https://github.com/guardrex) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8a574-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="8a574-105">Una aplicación de ASP.NET Core se puede hospedar en Windows sin usar IIS como [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="8a574-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="8a574-106">Cuando se hospeda como un servicio de Windows, la aplicación se inicia automáticamente después de reiniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="8a574-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="8a574-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8a574-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8a574-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="8a574-108">Prerequisites</span></span>

* [<span data-ttu-id="8a574-109">ASP.NET Core SDK 2.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="8a574-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="8a574-110">PowerShell 6.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="8a574-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="8a574-111">Plantilla Worker Service</span><span class="sxs-lookup"><span data-stu-id="8a574-111">Worker Service template</span></span>

<span data-ttu-id="8a574-112">La plantilla Worker Service de ASP.NET Core sirve de punto de partida para escribir aplicaciones de servicio de larga duración.</span><span class="sxs-lookup"><span data-stu-id="8a574-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="8a574-113">Para usar la plantilla como base de una aplicación de servicio de Windows:</span><span class="sxs-lookup"><span data-stu-id="8a574-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="8a574-114">Cree una aplicación Worker Service con la plantilla de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8a574-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="8a574-115">Siga las instrucciones de la sección [Configuración de aplicaciones](#app-configuration) para actualizar la aplicación Worker Service a fin de que se ejecute como un servicio de Windows.</span><span class="sxs-lookup"><span data-stu-id="8a574-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="8a574-116">Configuración de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="8a574-116">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8a574-117">Se llama a `IHostBuilder.UseWindowsService`, proporcionado por el paquete [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices), cuando se crea el host.</span><span class="sxs-lookup"><span data-stu-id="8a574-117">`IHostBuilder.UseWindowsService`, provided by the [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) package, is called when building the host.</span></span> <span data-ttu-id="8a574-118">Si la aplicación se ejecuta como un servicio de Windows, el método:</span><span class="sxs-lookup"><span data-stu-id="8a574-118">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="8a574-119">Establece la vigencia del host en `WindowsServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="8a574-119">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="8a574-120">Establece la [raíz del contenido](xref:fundamentals/index#content-root).</span><span class="sxs-lookup"><span data-stu-id="8a574-120">Sets the [content root](xref:fundamentals/index#content-root).</span></span>
* <span data-ttu-id="8a574-121">Habilita el registro en el registro de eventos con el nombre de la aplicación como nombre de origen predeterminado.</span><span class="sxs-lookup"><span data-stu-id="8a574-121">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="8a574-122">El nivel de registro puede configurarse con la clave `Logging:LogLevel:Default` en el archivo *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="8a574-122">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="8a574-123">Los administradores son los únicos que pueden crear nuevos orígenes de eventos.</span><span class="sxs-lookup"><span data-stu-id="8a574-123">Only administrators can create new event sources.</span></span> <span data-ttu-id="8a574-124">Cuando no se puede crear un origen de eventos con el nombre de la aplicación, se registra una advertencia para el origen *Aplicación* y los registros de eventos se deshabilitan.</span><span class="sxs-lookup"><span data-stu-id="8a574-124">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8a574-125">La aplicación requiere referencias de paquetes para [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) y [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="8a574-125">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="8a574-126">Para probar y depurar cuando se ejecuta fuera de un servicio, agregue código para determinar si la aplicación se ejecuta como un servicio o una aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="8a574-126">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="8a574-127">Compruebe si el depurador está asociado o hay presente un conmutador `--console`.</span><span class="sxs-lookup"><span data-stu-id="8a574-127">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="8a574-128">Si alguna de estas condiciones se cumple (la aplicación no se ejecuta como un servicio), llame a <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="8a574-128">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="8a574-129">Si las condiciones no se cumplen (la aplicación se ejecuta como servicio):</span><span class="sxs-lookup"><span data-stu-id="8a574-129">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="8a574-130">Llame a <xref:System.IO.Directory.SetCurrentDirectory*> y use una ruta de acceso a la ubicación de publicación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8a574-130">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="8a574-131">No llame a <xref:System.IO.Directory.GetCurrentDirectory*> para obtener la ruta de acceso porque una aplicación de servicio de Windows devuelve una carpeta *C:\\WINDOWS\\system32* cuando se llama a <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="8a574-131">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="8a574-132">Para obtener más información, consulte la sección [Directorio actual y raíz del contenido](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="8a574-132">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="8a574-133">Este paso se realiza antes de que la aplicación se configure en `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8a574-133">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="8a574-134">Llame a <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> para ejecutar la aplicación como un servicio.</span><span class="sxs-lookup"><span data-stu-id="8a574-134">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="8a574-135">Dado que el [Proveedor de configuración de línea de comandos](xref:fundamentals/configuration/index#command-line-configuration-provider) requiere pares nombre-valor en los argumentos de línea de comandos, el conmutador `--console` se quita de los argumentos antes de que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> los reciba.</span><span class="sxs-lookup"><span data-stu-id="8a574-135">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="8a574-136">Para escribir en el registro de eventos de Windows, agregue el proveedor EventLog a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="8a574-136">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="8a574-137">Establezca el nivel de registro con la clave `Logging:LogLevel:Default` en el archivo *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="8a574-137">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="8a574-138">En el ejemplo siguiente de la aplicación de ejemplo, se llama a `RunAsCustomService` en lugar de a <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> con el fin de controlar los eventos de duración dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8a574-138">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="8a574-139">Para obtener más información, consulte la sección [Control de los eventos de inicio y detención](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="8a574-139">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="8a574-140">Tipo de implementación</span><span class="sxs-lookup"><span data-stu-id="8a574-140">Deployment type</span></span>

<span data-ttu-id="8a574-141">Para obtener información y consejos sobre los escenarios de implementación, consulte [Implementación de aplicaciones .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="8a574-141">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="8a574-142">Implementación dependiente de marco (FDD)</span><span class="sxs-lookup"><span data-stu-id="8a574-142">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="8a574-143">La implementación dependiente de marco de trabajo (FDD) se basa en la presencia de una versión compartida de .NET Core en todo el sistema en el sistema de destino.</span><span class="sxs-lookup"><span data-stu-id="8a574-143">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="8a574-144">Cuando se adopta el escenario FDD siguiendo las instrucciones de este artículo, el SDK genera un archivo ejecutable ( *.exe*), denominado *ejecutable dependiente del marco*.</span><span class="sxs-lookup"><span data-stu-id="8a574-144">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8a574-145">Agregue los siguientes elementos de propiedad al archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="8a574-145">Add the following property elements to the project file:</span></span>

* <span data-ttu-id="8a574-146">`<OutputType>`&ndash; Tipo de salida de la aplicación (`Exe` para ejecutable).</span><span class="sxs-lookup"><span data-stu-id="8a574-146">`<OutputType>` &ndash; The app's output type (`Exe` for executable).</span></span>
* <span data-ttu-id="8a574-147">`<LangVersion>`&ndash; Versión del lenguaje C# (`latest` o `preview`).</span><span class="sxs-lookup"><span data-stu-id="8a574-147">`<LangVersion>` &ndash; The C# language version (`latest` or `preview`).</span></span>

<span data-ttu-id="8a574-148">No se requiere un archivo *web.config*, que normalmente se produce cuando se publica una aplicación ASP.NET Core, para una aplicación de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="8a574-148">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="8a574-149">Para deshabilitar la creación de un archivo *web.config* agregue la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="8a574-149">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="8a574-150">El [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contiene la plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="8a574-150">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="8a574-151">En el ejemplo siguiente, el RID se establece en `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="8a574-151">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="8a574-152">La propiedad `<SelfContained>` se establece en `false`.</span><span class="sxs-lookup"><span data-stu-id="8a574-152">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="8a574-153">Estas propiedades indican al SDK que genere un archivo ejecutable ( *.exe*) para Windows y una aplicación que depende del marco .NET Core compartido.</span><span class="sxs-lookup"><span data-stu-id="8a574-153">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="8a574-154">No se requiere un archivo *web.config*, que normalmente se produce cuando se publica una aplicación ASP.NET Core, para una aplicación de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="8a574-154">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="8a574-155">Para deshabilitar la creación de un archivo *web.config* agregue la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="8a574-155">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="8a574-156">El [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contiene la plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="8a574-156">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="8a574-157">En el ejemplo siguiente, el RID se establece en `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="8a574-157">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="8a574-158">La propiedad `<SelfContained>` se establece en `false`.</span><span class="sxs-lookup"><span data-stu-id="8a574-158">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="8a574-159">Estas propiedades indican al SDK que genere un archivo ejecutable ( *.exe*) para Windows y una aplicación que depende del marco .NET Core compartido.</span><span class="sxs-lookup"><span data-stu-id="8a574-159">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="8a574-160">La propiedad `<UseAppHost>` se establece en `true`.</span><span class="sxs-lookup"><span data-stu-id="8a574-160">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="8a574-161">Esta propiedad proporciona el servicio con una ruta de acceso de activación (un archivo ejecutable, *.exe*) para una FDD.</span><span class="sxs-lookup"><span data-stu-id="8a574-161">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="8a574-162">No se requiere un archivo *web.config*, que normalmente se produce cuando se publica una aplicación ASP.NET Core, para una aplicación de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="8a574-162">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="8a574-163">Para deshabilitar la creación de un archivo *web.config* agregue la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="8a574-163">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="8a574-164">Implementación autocontenida (SCD)</span><span class="sxs-lookup"><span data-stu-id="8a574-164">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="8a574-165">Una implementación autocontenida (SCD) no depende de la presencia de un marco compartido en el sistema host.</span><span class="sxs-lookup"><span data-stu-id="8a574-165">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="8a574-166">El tiempo de ejecución y las dependencias de la aplicación se implementan con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8a574-166">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="8a574-167">Un [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) se incluye en el `<PropertyGroup>` que contiene la plataforma de destino:</span><span class="sxs-lookup"><span data-stu-id="8a574-167">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="8a574-168">Para publicar para varios RID:</span><span class="sxs-lookup"><span data-stu-id="8a574-168">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="8a574-169">Proporcione los RID en una lista delimitada por punto y coma.</span><span class="sxs-lookup"><span data-stu-id="8a574-169">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="8a574-170">Utilice el nombre de propiedad [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (en plural).</span><span class="sxs-lookup"><span data-stu-id="8a574-170">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="8a574-171">Para más información, vea el [Catálogo de identificadores de entorno de ejecución (RID) de .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="8a574-171">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8a574-172">Una propiedad `<SelfContained>` se establece en `true`:</span><span class="sxs-lookup"><span data-stu-id="8a574-172">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="8a574-173">Cuenta de usuario de servicio</span><span class="sxs-lookup"><span data-stu-id="8a574-173">Service user account</span></span>

<span data-ttu-id="8a574-174">Para crear una cuenta de usuario para un servicio, use el cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) de un shell de comandos administrativos de PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="8a574-174">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="8a574-175">En la actualización de octubre de 2018 de Windows 10 (versión 1809/compilación 10.0.17763) o posterior:</span><span class="sxs-lookup"><span data-stu-id="8a574-175">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="8a574-176">En el sistema operativo Windows anterior a la actualización de octubre de 2018 de Windows 10 (versión 1809/compilación 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="8a574-176">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

<span data-ttu-id="8a574-177">Proporcione una [contraseña segura](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) cuando se le solicite.</span><span class="sxs-lookup"><span data-stu-id="8a574-177">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="8a574-178">A menos que el parámetro `-AccountExpires` se proporcione para el cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) con una fecha de expiración <xref:System.DateTime>, la cuenta no expirará.</span><span class="sxs-lookup"><span data-stu-id="8a574-178">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="8a574-179">Para más información, vea [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) y [Service User Accounts](/windows/desktop/services/service-user-accounts) (Cuentas de usuario del servicio).</span><span class="sxs-lookup"><span data-stu-id="8a574-179">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="8a574-180">Un enfoque alternativo de administración de usuarios al usar Active Directory consiste en usar cuentas de servicio administradas.</span><span class="sxs-lookup"><span data-stu-id="8a574-180">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="8a574-181">Para obtener más información, consulte [grupo Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) (Información general sobre cuentas de servicio administradas de grupo).</span><span class="sxs-lookup"><span data-stu-id="8a574-181">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="8a574-182">Derechos para iniciar sesión como servicio</span><span class="sxs-lookup"><span data-stu-id="8a574-182">Log on as a service rights</span></span>

<span data-ttu-id="8a574-183">Para establecer los derechos de *Iniciar sesión como servicio* para una cuenta de usuario del servicio:</span><span class="sxs-lookup"><span data-stu-id="8a574-183">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="8a574-184">Abra el editor de la Directiva de seguridad local mediante la ejecución de *secpol.msc*.</span><span class="sxs-lookup"><span data-stu-id="8a574-184">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="8a574-185">Expanda el nodo **Directivas locales** y, después, seleccione **Asignación de derechos de usuario**.</span><span class="sxs-lookup"><span data-stu-id="8a574-185">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="8a574-186">Abra la directiva **Iniciar sesión como servicio**.</span><span class="sxs-lookup"><span data-stu-id="8a574-186">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="8a574-187">Seleccione **Agregar usuario o grupo**.</span><span class="sxs-lookup"><span data-stu-id="8a574-187">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="8a574-188">Proporcione el nombre de objeto (cuenta de usuario) mediante cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="8a574-188">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="8a574-189">Escriba la cuenta de usuario (`{DOMAIN OR COMPUTER NAME\USER}`) en el campo del nombre de objeto y seleccione **Aceptar** para agregar el usuario a la directiva.</span><span class="sxs-lookup"><span data-stu-id="8a574-189">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="8a574-190">Seleccione **Opciones avanzadas**.</span><span class="sxs-lookup"><span data-stu-id="8a574-190">Select **Advanced**.</span></span> <span data-ttu-id="8a574-191">Seleccione **Buscar ahora**.</span><span class="sxs-lookup"><span data-stu-id="8a574-191">Select **Find Now**.</span></span> <span data-ttu-id="8a574-192">Seleccione la cuenta de usuario de la lista.</span><span class="sxs-lookup"><span data-stu-id="8a574-192">Select the user account from the list.</span></span> <span data-ttu-id="8a574-193">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8a574-193">Select **OK**.</span></span> <span data-ttu-id="8a574-194">Seleccione **Aceptar** nuevo para agregar el usuario a la directiva.</span><span class="sxs-lookup"><span data-stu-id="8a574-194">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="8a574-195">Seleccione **Aceptar** o **Aplicar** para aceptar los cambios.</span><span class="sxs-lookup"><span data-stu-id="8a574-195">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="8a574-196">Creación y administración del servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="8a574-196">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="8a574-197">Creación de un servicio</span><span class="sxs-lookup"><span data-stu-id="8a574-197">Create a service</span></span>

<span data-ttu-id="8a574-198">Use los comandos de PowerShell para registrar un servicio.</span><span class="sxs-lookup"><span data-stu-id="8a574-198">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="8a574-199">Desde un shell de comandos administrativos de PowerShell 6, ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="8a574-199">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="8a574-200">`{EXE PATH}`&ndash; Ruta de acceso a la carpeta de la aplicación en el host (por ejemplo, `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="8a574-200">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="8a574-201">No incluya el archivo ejecutable de la aplicación en la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="8a574-201">Don't include the app's executable in the path.</span></span> <span data-ttu-id="8a574-202">No se requiere una barra diagonal al final.</span><span class="sxs-lookup"><span data-stu-id="8a574-202">A trailing slash isn't required.</span></span>
* <span data-ttu-id="8a574-203">`{DOMAIN OR COMPUTER NAME\USER}` &ndash;Cuenta de usuario de servicio (por ejemplo, `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="8a574-203">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="8a574-204">`{NAME}` &ndash;Nombre de servicio (por ejemplo, `MyService`).</span><span class="sxs-lookup"><span data-stu-id="8a574-204">`{NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="8a574-205">`{EXE FILE PATH}` &ndash; Ruta de acceso del ejecutable de la aplicación (por ejemplo, `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="8a574-205">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="8a574-206">Incluya el nombre de archivo del ejecutable con la extensión.</span><span class="sxs-lookup"><span data-stu-id="8a574-206">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="8a574-207">`{DESCRIPTION}` &ndash; Descripción del servicio (por ejemplo, `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="8a574-207">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="8a574-208">`{DISPLAY NAME}` &ndash; Nombre para mostrar del servicio (por ejemplo, `My Service`).</span><span class="sxs-lookup"><span data-stu-id="8a574-208">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="8a574-209">Inicio de un servicio</span><span class="sxs-lookup"><span data-stu-id="8a574-209">Start a service</span></span>

<span data-ttu-id="8a574-210">Inicie el servicio con el siguiente comando de PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="8a574-210">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {NAME}
```

<span data-ttu-id="8a574-211">Este comando tarda unos segundos en iniciar el servicio.</span><span class="sxs-lookup"><span data-stu-id="8a574-211">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="8a574-212">Determinación del estado de un servicio</span><span class="sxs-lookup"><span data-stu-id="8a574-212">Determine a service's status</span></span>

<span data-ttu-id="8a574-213">Para comprobar el estado de un servicio, use el siguiente comando de PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="8a574-213">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {NAME}
```

<span data-ttu-id="8a574-214">El estado se notifica como uno de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="8a574-214">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="8a574-215">Detención de un servicio</span><span class="sxs-lookup"><span data-stu-id="8a574-215">Stop a service</span></span>

<span data-ttu-id="8a574-216">Detenga un servicio con el siguiente comando de PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="8a574-216">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="8a574-217">Eliminación de un servicio</span><span class="sxs-lookup"><span data-stu-id="8a574-217">Remove a service</span></span>

<span data-ttu-id="8a574-218">Tras un breve intervalo para detener un servicio, elimine el servicio con el siguiente comando de PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="8a574-218">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="8a574-219">Control de los eventos de inicio y detención</span><span class="sxs-lookup"><span data-stu-id="8a574-219">Handle starting and stopping events</span></span>

<span data-ttu-id="8a574-220">Para controlar los eventos <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> y <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>:</span><span class="sxs-lookup"><span data-stu-id="8a574-220">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="8a574-221">Cree una clase que se derive de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> con los métodos `OnStarting`, `OnStarted` y `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="8a574-221">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="8a574-222">Cree un método de extensión para <xref:Microsoft.AspNetCore.Hosting.IWebHost> que pase `CustomWebHostService` a <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="8a574-222">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="8a574-223">En `Program.Main`, llame al método de extensión `RunAsCustomService` en lugar de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="8a574-223">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="8a574-224">Para ver la ubicación de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> en `Program.Main`, consulte el ejemplo de código que se muestra en la sección [Tipo de implementación](#deployment-type).</span><span class="sxs-lookup"><span data-stu-id="8a574-224">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="8a574-225">Escenarios de servidor proxy y equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="8a574-225">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="8a574-226">Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="8a574-226">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="8a574-227">Para más información, consulte <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="8a574-227">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="8a574-228">Configuración de extremos</span><span class="sxs-lookup"><span data-stu-id="8a574-228">Configure endpoints</span></span>

<span data-ttu-id="8a574-229">ASP.NET Core se enlaza a `http://localhost:5000` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="8a574-229">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="8a574-230">Configure la dirección URL y el puerto estableciendo la variable de entorno `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="8a574-230">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="8a574-231">Para información sobre otros enfoques de configuración de direcciones URL y puertos, consulte el artículo en cuestión:</span><span class="sxs-lookup"><span data-stu-id="8a574-231">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="8a574-232">En la guía anterior se trata la compatibilidad con los puntos de conexión HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8a574-232">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="8a574-233">Por ejemplo, configure la aplicación para HTTPS cuando se use la autenticación con un servicio de Windows.</span><span class="sxs-lookup"><span data-stu-id="8a574-233">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="8a574-234">No se admite el uso del certificado de desarrollo HTTPS de ASP.NET Core para proteger un punto de conexión de servicio.</span><span class="sxs-lookup"><span data-stu-id="8a574-234">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="8a574-235">Directorio actual y raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="8a574-235">Current directory and content root</span></span>

<span data-ttu-id="8a574-236">El directorio de trabajo actual devuelto por una llamada a <xref:System.IO.Directory.GetCurrentDirectory*> para un servicio de Windows es la carpeta *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="8a574-236">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="8a574-237">La carpeta *system32* no es una ubicación adecuada para almacenar los archivos de un servicio (por ejemplo los archivos de configuración).</span><span class="sxs-lookup"><span data-stu-id="8a574-237">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="8a574-238">Utilice uno de los siguientes enfoques para mantener los archivos de configuración y los activos de un servicio, así como para acceder a ellos.</span><span class="sxs-lookup"><span data-stu-id="8a574-238">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="8a574-239">Uso de ContentRootPath o ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="8a574-239">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="8a574-240">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) o <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> para buscar los recursos de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="8a574-240">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="8a574-241">Configuración de la ruta de acceso raíz del contenido en la carpeta de la aplicación</span><span class="sxs-lookup"><span data-stu-id="8a574-241">Set the content root path to the app's folder</span></span>

<span data-ttu-id="8a574-242"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> es la misma ruta de acceso proporcionada al argumento `binPath` cuando se crea un servicio.</span><span class="sxs-lookup"><span data-stu-id="8a574-242">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="8a574-243">En lugar de llamar a `GetCurrentDirectory` para crear rutas de acceso a los archivos de configuración, llame a <xref:System.IO.Directory.SetCurrentDirectory*> con la ruta de acceso a la [raíz del contenido](xref:fundamentals/index#content-root) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8a574-243">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="8a574-244">En `Program.Main`, determine la ruta de acceso a la carpeta del archivo ejecutable del servicio y use la ruta de acceso para establecer la raíz del contenido de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="8a574-244">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="8a574-245">Almacenamiento de los archivos de un servicio en una ubicación adecuada en el disco</span><span class="sxs-lookup"><span data-stu-id="8a574-245">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="8a574-246">Especifique una ruta de acceso absoluta con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> al utilizar un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> a la carpeta que contiene los archivos.</span><span class="sxs-lookup"><span data-stu-id="8a574-246">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8a574-247">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8a574-247">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="8a574-248">[Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)</span><span class="sxs-lookup"><span data-stu-id="8a574-248">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="8a574-249">[Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)</span><span class="sxs-lookup"><span data-stu-id="8a574-249">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
