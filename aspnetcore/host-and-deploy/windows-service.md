---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: guardrex
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/03/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 3a254af4d56cb4abc7004a67b0d0b42de2b878b1
ms.sourcegitcommit: 47cc13ab90913af9a2887cef0896bb4e9aba4dd5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2019
ms.locfileid: "67399116"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="0b0a6-103">Hospedaje de ASP.NET Core en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="0b0a6-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="0b0a6-104">Por [Luke Latham](https://github.com/guardrex) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0b0a6-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="0b0a6-105">Una aplicación de ASP.NET Core se puede hospedar en Windows sin usar IIS como [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="0b0a6-106">Cuando se hospeda como un servicio de Windows, la aplicación se inicia automáticamente después de reiniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-106">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="0b0a6-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0b0a6-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b0a6-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="0b0a6-108">Prerequisites</span></span>

* [<span data-ttu-id="0b0a6-109">ASP.NET Core SDK 2.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="0b0a6-109">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="0b0a6-110">PowerShell 6.2 o posterior</span><span class="sxs-lookup"><span data-stu-id="0b0a6-110">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="0b0a6-111">Plantilla Worker Service</span><span class="sxs-lookup"><span data-stu-id="0b0a6-111">Worker Service template</span></span>

<span data-ttu-id="0b0a6-112">La plantilla Worker Service de ASP.NET Core sirve de punto de partida para escribir aplicaciones de servicio de larga duración.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-112">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="0b0a6-113">Para usar la plantilla como base de una aplicación de servicio de Windows:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-113">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="0b0a6-114">Cree una aplicación Worker Service con la plantilla de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-114">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="0b0a6-115">Siga las instrucciones de la sección [Configuración de aplicaciones](#app-configuration) para actualizar la aplicación Worker Service a fin de que se ejecute como un servicio de Windows.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-115">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b0a6-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b0a6-116">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="0b0a6-117">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-117">Create a new project.</span></span>
1. <span data-ttu-id="0b0a6-118">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="0b0a6-119">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-119">Select **Next**.</span></span>
1. <span data-ttu-id="0b0a6-120">Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-120">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="0b0a6-121">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-121">Select **Create**.</span></span>
1. <span data-ttu-id="0b0a6-122">En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, confirme que las opciones **.NET Core** y **ASP.NET Core 3.0** estén seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-122">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="0b0a6-123">Seleccione la plantilla **Worker Service**.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-123">Select the **Worker Service** template.</span></span> <span data-ttu-id="0b0a6-124">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-124">Select **Create**.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="0b0a6-125">Visual Studio Code y CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0b0a6-125">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="0b0a6-126">Desde un shell de comandos, use la plantilla Worker Service (`worker`) con el comando [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-126">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="0b0a6-127">En el ejemplo siguiente, se crea una aplicación Worker Service llamada `ContosoWorkerService`.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-127">In the following example, a Worker Service app is created named `ContosoWorkerService`.</span></span> <span data-ttu-id="0b0a6-128">Al ejecutar el comando, se crea automáticamente una carpeta para la aplicación `ContosoWorkerService`.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-128">A folder for the `ContosoWorkerService` app is created automatically when the command is executed.</span></span>

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="app-configuration"></a><span data-ttu-id="0b0a6-129">Configuración de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="0b0a6-129">App configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0b0a6-130">Se llama a `IHostBuilder.UseWindowsService`, proporcionado por el paquete [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices), cuando se crea el host.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-130">`IHostBuilder.UseWindowsService`, provided by the [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) package, is called when building the host.</span></span> <span data-ttu-id="0b0a6-131">Si la aplicación se ejecuta como un servicio de Windows, el método:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-131">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="0b0a6-132">Establece la vigencia del host en `WindowsServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-132">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="0b0a6-133">Establece la raíz de contenido.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-133">Sets the content root.</span></span>
* <span data-ttu-id="0b0a6-134">Habilita el registro en el registro de eventos con el nombre de la aplicación como nombre de origen predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-134">Enables logging to the event log with the application name as the default source name.</span></span>
  * <span data-ttu-id="0b0a6-135">El nivel de registro puede configurarse con la clave `Logging:LogLevel:Default` en el archivo *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-135">The log level can be configured using the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>
  * <span data-ttu-id="0b0a6-136">Los administradores son los únicos que pueden crear nuevos orígenes de eventos.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-136">Only administrators can create new event sources.</span></span> <span data-ttu-id="0b0a6-137">Cuando no se puede crear un origen de eventos con el nombre de la aplicación, se registra una advertencia para el origen *Aplicación* y los registros de eventos se deshabilitan.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-137">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

[!code-csharp[](windows-service/samples/3.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0b0a6-138">La aplicación requiere referencias de paquetes para [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) y [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-138">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="0b0a6-139">Para probar y depurar cuando se ejecuta fuera de un servicio, agregue código para determinar si la aplicación se ejecuta como un servicio o una aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-139">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="0b0a6-140">Compruebe si el depurador está asociado o hay presente un conmutador `--console`.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-140">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="0b0a6-141">Si alguna de estas condiciones se cumple (la aplicación no se ejecuta como un servicio), llame a <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-141">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="0b0a6-142">Si las condiciones no se cumplen (la aplicación se ejecuta como servicio):</span><span class="sxs-lookup"><span data-stu-id="0b0a6-142">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="0b0a6-143">Llame a <xref:System.IO.Directory.SetCurrentDirectory*> y use una ruta de acceso a la ubicación de publicación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-143">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="0b0a6-144">No llame a <xref:System.IO.Directory.GetCurrentDirectory*> para obtener la ruta de acceso porque una aplicación de servicio de Windows devuelve una carpeta *C:\\WINDOWS\\system32* cuando se llama a <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-144">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="0b0a6-145">Para obtener más información, consulte la sección [Directorio actual y raíz del contenido](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-145">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="0b0a6-146">Este paso se realiza antes de que la aplicación se configure en `CreateWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-146">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="0b0a6-147">Llame a <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> para ejecutar la aplicación como un servicio.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-147">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="0b0a6-148">Dado que el [Proveedor de configuración de línea de comandos](xref:fundamentals/configuration/index#command-line-configuration-provider) requiere pares nombre-valor en los argumentos de línea de comandos, el conmutador `--console` se quita de los argumentos antes de que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> los reciba.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-148">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="0b0a6-149">Para escribir en el registro de eventos de Windows, agregue el proveedor EventLog a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-149">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="0b0a6-150">Establezca el nivel de registro con la clave `Logging:LogLevel:Default` en el archivo *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-150">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="0b0a6-151">En el ejemplo siguiente de la aplicación de ejemplo, se llama a `RunAsCustomService` en lugar de a <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> con el fin de controlar los eventos de duración dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-151">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="0b0a6-152">Para obtener más información, consulte la sección [Control de los eventos de inicio y detención](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-152">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

::: moniker-end

## <a name="deployment-type"></a><span data-ttu-id="0b0a6-153">Tipo de implementación</span><span class="sxs-lookup"><span data-stu-id="0b0a6-153">Deployment type</span></span>

<span data-ttu-id="0b0a6-154">Para obtener información y consejos sobre los escenarios de implementación, consulte [Implementación de aplicaciones .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-154">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="0b0a6-155">Implementación dependiente de marco (FDD)</span><span class="sxs-lookup"><span data-stu-id="0b0a6-155">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="0b0a6-156">La implementación dependiente de marco de trabajo (FDD) se basa en la presencia de una versión compartida de .NET Core en todo el sistema en el sistema de destino.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-156">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="0b0a6-157">Cuando se adopta el escenario FDD siguiendo las instrucciones de este artículo, el SDK genera un archivo ejecutable ( *.exe*), denominado *ejecutable dependiente del marco*.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-157">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0b0a6-158">Agregue los siguientes elementos de propiedad al archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-158">Add the following property elements to the project file:</span></span>

* <span data-ttu-id="0b0a6-159">`<OutputType>`&ndash; Tipo de salida de la aplicación (`Exe` para ejecutable).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-159">`<OutputType>` &ndash; The app's output type (`Exe` for executable).</span></span>
* <span data-ttu-id="0b0a6-160">`<LangVersion>`&ndash; Versión del lenguaje C# (`latest` o `preview`).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-160">`<LangVersion>` &ndash; The C# language version (`latest` or `preview`).</span></span>

<span data-ttu-id="0b0a6-161">No se requiere un archivo *web.config*, que normalmente se produce cuando se publica una aplicación ASP.NET Core, para una aplicación de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-161">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="0b0a6-162">Para deshabilitar la creación de un archivo *web.config* agregue la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-162">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="0b0a6-163">El [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contiene la plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-163">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="0b0a6-164">En el ejemplo siguiente, el RID se establece en `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-164">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="0b0a6-165">La propiedad `<SelfContained>` se establece en `false`.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-165">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="0b0a6-166">Estas propiedades indican al SDK que genere un archivo ejecutable ( *.exe*) para Windows y una aplicación que depende del marco .NET Core compartido.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-166">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="0b0a6-167">No se requiere un archivo *web.config*, que normalmente se produce cuando se publica una aplicación ASP.NET Core, para una aplicación de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-167">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="0b0a6-168">Para deshabilitar la creación de un archivo *web.config* agregue la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-168">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="0b0a6-169">El [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contiene la plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-169">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="0b0a6-170">En el ejemplo siguiente, el RID se establece en `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-170">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="0b0a6-171">La propiedad `<SelfContained>` se establece en `false`.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-171">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="0b0a6-172">Estas propiedades indican al SDK que genere un archivo ejecutable ( *.exe*) para Windows y una aplicación que depende del marco .NET Core compartido.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-172">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="0b0a6-173">La propiedad `<UseAppHost>` se establece en `true`.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-173">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="0b0a6-174">Esta propiedad proporciona el servicio con una ruta de acceso de activación (un archivo ejecutable, *.exe*) para una FDD.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-174">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="0b0a6-175">No se requiere un archivo *web.config*, que normalmente se produce cuando se publica una aplicación ASP.NET Core, para una aplicación de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-175">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="0b0a6-176">Para deshabilitar la creación de un archivo *web.config* agregue la propiedad `<IsTransformWebConfigDisabled>` establecida en `true`.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-176">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="0b0a6-177">Implementación autocontenida (SCD)</span><span class="sxs-lookup"><span data-stu-id="0b0a6-177">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="0b0a6-178">Una implementación autocontenida (SCD) no depende de la presencia de un marco compartido en el sistema host.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-178">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="0b0a6-179">El tiempo de ejecución y las dependencias de la aplicación se implementan con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-179">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="0b0a6-180">Un [identificador en tiempo de ejecución (RID)](/dotnet/core/rid-catalog) se incluye en el `<PropertyGroup>` que contiene la plataforma de destino:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-180">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="0b0a6-181">Para publicar para varios RID:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-181">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="0b0a6-182">Proporcione los RID en una lista delimitada por punto y coma.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-182">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="0b0a6-183">Utilice el nombre de propiedad [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (en plural).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-183">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="0b0a6-184">Para más información, vea el [Catálogo de identificadores de entorno de ejecución (RID) de .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-184">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0b0a6-185">Una propiedad `<SelfContained>` se establece en `true`:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-185">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

::: moniker-end

## <a name="service-user-account"></a><span data-ttu-id="0b0a6-186">Cuenta de usuario de servicio</span><span class="sxs-lookup"><span data-stu-id="0b0a6-186">Service user account</span></span>

<span data-ttu-id="0b0a6-187">Para crear una cuenta de usuario para un servicio, use el cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) de un shell de comandos administrativos de PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-187">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="0b0a6-188">En la actualización de octubre de 2018 de Windows 10 (versión 1809/compilación 10.0.17763) o posterior:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-188">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```PowerShell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="0b0a6-189">En el sistema operativo Windows anterior a la actualización de octubre de 2018 de Windows 10 (versión 1809/compilación 10.0.17763):</span><span class="sxs-lookup"><span data-stu-id="0b0a6-189">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {NAME}"
```

<span data-ttu-id="0b0a6-190">Proporcione una [contraseña segura](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) cuando se le solicite.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-190">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="0b0a6-191">A menos que el parámetro `-AccountExpires` se proporcione para el cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) con una fecha de expiración <xref:System.DateTime>, la cuenta no expirará.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-191">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="0b0a6-192">Para más información, vea [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) y [Service User Accounts](/windows/desktop/services/service-user-accounts) (Cuentas de usuario del servicio).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-192">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="0b0a6-193">Un enfoque alternativo de administración de usuarios al usar Active Directory consiste en usar cuentas de servicio administradas.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-193">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="0b0a6-194">Para obtener más información, consulte [grupo Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) (Información general sobre cuentas de servicio administradas de grupo).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-194">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="0b0a6-195">Derechos para iniciar sesión como servicio</span><span class="sxs-lookup"><span data-stu-id="0b0a6-195">Log on as a service rights</span></span>

<span data-ttu-id="0b0a6-196">Para establecer los derechos de *Iniciar sesión como servicio* para una cuenta de usuario del servicio:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-196">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="0b0a6-197">Abra el editor de la Directiva de seguridad local mediante la ejecución de *secpol.msc*.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-197">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="0b0a6-198">Expanda el nodo **Directivas locales** y, después, seleccione **Asignación de derechos de usuario**.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-198">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="0b0a6-199">Abra la directiva **Iniciar sesión como servicio**.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-199">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="0b0a6-200">Seleccione **Agregar usuario o grupo**.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-200">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="0b0a6-201">Proporcione el nombre de objeto (cuenta de usuario) mediante cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-201">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="0b0a6-202">Escriba la cuenta de usuario (`{DOMAIN OR COMPUTER NAME\USER}`) en el campo del nombre de objeto y seleccione **Aceptar** para agregar el usuario a la directiva.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-202">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="0b0a6-203">Seleccione **Opciones avanzadas**.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-203">Select **Advanced**.</span></span> <span data-ttu-id="0b0a6-204">Seleccione **Buscar ahora**.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-204">Select **Find Now**.</span></span> <span data-ttu-id="0b0a6-205">Seleccione la cuenta de usuario de la lista.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-205">Select the user account from the list.</span></span> <span data-ttu-id="0b0a6-206">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-206">Select **OK**.</span></span> <span data-ttu-id="0b0a6-207">Seleccione **Aceptar** nuevo para agregar el usuario a la directiva.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-207">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="0b0a6-208">Seleccione **Aceptar** o **Aplicar** para aceptar los cambios.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-208">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="0b0a6-209">Creación y administración del servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="0b0a6-209">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="0b0a6-210">Creación de un servicio</span><span class="sxs-lookup"><span data-stu-id="0b0a6-210">Create a service</span></span>

<span data-ttu-id="0b0a6-211">Use los comandos de PowerShell para registrar un servicio.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-211">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="0b0a6-212">Desde un shell de comandos administrativos de PowerShell 6, ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-212">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="0b0a6-213">`{EXE PATH}`&ndash; Ruta de acceso a la carpeta de la aplicación en el host (por ejemplo, `d:\myservice`).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-213">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="0b0a6-214">No incluya el archivo ejecutable de la aplicación en la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-214">Don't include the app's executable in the path.</span></span> <span data-ttu-id="0b0a6-215">No se requiere una barra diagonal al final.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-215">A trailing slash isn't required.</span></span>
* <span data-ttu-id="0b0a6-216">`{DOMAIN OR COMPUTER NAME\USER}` &ndash;Cuenta de usuario de servicio (por ejemplo, `Contoso\ServiceUser`).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-216">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="0b0a6-217">`{NAME}` &ndash;Nombre de servicio (por ejemplo, `MyService`).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-217">`{NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="0b0a6-218">`{EXE FILE PATH}` &ndash; Ruta de acceso del ejecutable de la aplicación (por ejemplo, `d:\myservice\myservice.exe`).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-218">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="0b0a6-219">Incluya el nombre de archivo del ejecutable con la extensión.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-219">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="0b0a6-220">`{DESCRIPTION}` &ndash; Descripción del servicio (por ejemplo, `My sample service`).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-220">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="0b0a6-221">`{DISPLAY NAME}` &ndash; Nombre para mostrar del servicio (por ejemplo, `My Service`).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-221">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="0b0a6-222">Inicio de un servicio</span><span class="sxs-lookup"><span data-stu-id="0b0a6-222">Start a service</span></span>

<span data-ttu-id="0b0a6-223">Inicie el servicio con el siguiente comando de PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-223">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {NAME}
```

<span data-ttu-id="0b0a6-224">Este comando tarda unos segundos en iniciar el servicio.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-224">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="0b0a6-225">Determinación del estado de un servicio</span><span class="sxs-lookup"><span data-stu-id="0b0a6-225">Determine a service's status</span></span>

<span data-ttu-id="0b0a6-226">Para comprobar el estado de un servicio, use el siguiente comando de PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-226">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {NAME}
```

<span data-ttu-id="0b0a6-227">El estado se notifica como uno de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-227">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="0b0a6-228">Detención de un servicio</span><span class="sxs-lookup"><span data-stu-id="0b0a6-228">Stop a service</span></span>

<span data-ttu-id="0b0a6-229">Detenga un servicio con el siguiente comando de PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-229">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="0b0a6-230">Eliminación de un servicio</span><span class="sxs-lookup"><span data-stu-id="0b0a6-230">Remove a service</span></span>

<span data-ttu-id="0b0a6-231">Tras un breve intervalo para detener un servicio, elimine el servicio con el siguiente comando de PowerShell 6:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-231">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {NAME}
```

::: moniker range="< aspnetcore-3.0"

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="0b0a6-232">Control de los eventos de inicio y detención</span><span class="sxs-lookup"><span data-stu-id="0b0a6-232">Handle starting and stopping events</span></span>

<span data-ttu-id="0b0a6-233">Para controlar los eventos <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> y <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-233">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="0b0a6-234">Cree una clase que se derive de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> con los métodos `OnStarting`, `OnStarted` y `OnStopping`:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-234">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="0b0a6-235">Cree un método de extensión para <xref:Microsoft.AspNetCore.Hosting.IWebHost> que pase `CustomWebHostService` a <xref:System.ServiceProcess.ServiceBase.Run*>:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-235">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="0b0a6-236">En `Program.Main`, llame al método de extensión `RunAsCustomService` en lugar de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-236">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="0b0a6-237">Para ver la ubicación de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> en `Program.Main`, consulte el ejemplo de código que se muestra en la sección [Tipo de implementación](#deployment-type).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-237">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="0b0a6-238">Escenarios de servidor proxy y equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="0b0a6-238">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="0b0a6-239">Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-239">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="0b0a6-240">Para más información, consulte <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-240">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="0b0a6-241">Configuración de HTTPS</span><span class="sxs-lookup"><span data-stu-id="0b0a6-241">Configure HTTPS</span></span>

<span data-ttu-id="0b0a6-242">Para configurar el servicio con un punto de conexión seguro:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-242">To configure a service with a secure endpoint:</span></span>

1. <span data-ttu-id="0b0a6-243">Cree un certificado X.509 para el sistema de hospedaje mediante la adquisición del certificado de la plataforma y con mecanismos de implementación.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-243">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="0b0a6-244">Especifique una [configuración de punto de conexión HTTPS de servidor Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) para usar el certificado.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-244">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="0b0a6-245">No se admite el uso del certificado de desarrollo HTTPS de ASP.NET Core para proteger un punto de conexión de servicio.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-245">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="0b0a6-246">Directorio actual y raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="0b0a6-246">Current directory and content root</span></span>

<span data-ttu-id="0b0a6-247">El directorio de trabajo actual devuelto por una llamada a <xref:System.IO.Directory.GetCurrentDirectory*> para un servicio de Windows es la carpeta *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-247">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="0b0a6-248">La carpeta *system32* no es una ubicación adecuada para almacenar los archivos de un servicio (por ejemplo los archivos de configuración).</span><span class="sxs-lookup"><span data-stu-id="0b0a6-248">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="0b0a6-249">Utilice uno de los siguientes enfoques para mantener los archivos de configuración y los activos de un servicio, así como para acceder a ellos.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-249">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="0b0a6-250">Uso de ContentRootPath o ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="0b0a6-250">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="0b0a6-251">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) o <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> para buscar los recursos de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-251">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="0b0a6-252">Configuración de la ruta de acceso raíz del contenido en la carpeta de la aplicación</span><span class="sxs-lookup"><span data-stu-id="0b0a6-252">Set the content root path to the app's folder</span></span>

<span data-ttu-id="0b0a6-253"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> es la misma ruta de acceso proporcionada al argumento `binPath` cuando se crea un servicio.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-253">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="0b0a6-254">En lugar de llamar `GetCurrentDirectory` para crear rutas de acceso a los archivos de configuración, llame a <xref:System.IO.Directory.SetCurrentDirectory*> con la ruta de acceso a la raíz del contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-254">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="0b0a6-255">En `Program.Main`, determine la ruta de acceso a la carpeta del archivo ejecutable del servicio y use la ruta de acceso para establecer la raíz del contenido de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="0b0a6-255">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

::: moniker-end

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="0b0a6-256">Almacenamiento de los archivos de un servicio en una ubicación adecuada en el disco</span><span class="sxs-lookup"><span data-stu-id="0b0a6-256">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="0b0a6-257">Especifique una ruta de acceso absoluta con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> al utilizar un <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> a la carpeta que contiene los archivos.</span><span class="sxs-lookup"><span data-stu-id="0b0a6-257">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0b0a6-258">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0b0a6-258">Additional resources</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="0b0a6-259">[Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)</span><span class="sxs-lookup"><span data-stu-id="0b0a6-259">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="0b0a6-260">[Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)</span><span class="sxs-lookup"><span data-stu-id="0b0a6-260">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
