---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: guardrex
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/25/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 7f19db0a1d12b904daff989bc969daf8d2302bfa
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325788"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="909af-103">Hospedaje de ASP.NET Core en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="909af-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="909af-104">Por [Luke Latham](https://github.com/guardrex) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="909af-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="909af-105">Una aplicación de ASP.NET Core se puede hospedar en Windows sin usar IIS como [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="909af-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="909af-106">Cuando se aloja como un servicio de Windows, la aplicación se inicia automáticamente después de reiniciar el equipo.</span><span class="sxs-lookup"><span data-stu-id="909af-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="909af-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="909af-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="909af-108">Convertir un proyecto en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="909af-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="909af-109">Estos son los cambios mínimos necesarios para configurar un proyecto de ASP.NET Core existente para que se ejecute como un servicio:</span><span class="sxs-lookup"><span data-stu-id="909af-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="909af-110">El archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="909af-110">In the project file:</span></span>

   * <span data-ttu-id="909af-111">Confirme la presencia del [identificador en tiempo de ejecución](/dotnet/core/rid-catalog) de Windows o agréguelo al `<PropertyGroup>` que contiene la plataforma de destino:</span><span class="sxs-lookup"><span data-stu-id="909af-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      <span data-ttu-id="909af-112">Para publicar para varios RID:</span><span class="sxs-lookup"><span data-stu-id="909af-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="909af-113">Proporcione los RID en una lista delimitada por punto y coma.</span><span class="sxs-lookup"><span data-stu-id="909af-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="909af-114">Use el nombre de la propiedad `<RuntimeIdentifiers>` (plural).</span><span class="sxs-lookup"><span data-stu-id="909af-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="909af-115">Para más información, vea el [Catálogo de identificadores de entorno de ejecución (RID) de .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="909af-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="909af-116">Agregue una referencia de paquete de [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="909af-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="909af-117">Realice los siguientes cambios en `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="909af-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="909af-118">Llame a [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) en lugar de a `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="909af-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="909af-119">Llame a [UseContentRoot](xref:fundamentals/host/web-host#content-root) y use una ruta de acceso a la ubicación de publicación de la aplicación en lugar de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="909af-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="909af-120">Publique la aplicación.</span><span class="sxs-lookup"><span data-stu-id="909af-120">Publish the app.</span></span> <span data-ttu-id="909af-121">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) o un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="909af-121">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="909af-122">Al utilizar Visual Studio, seleccione **FolderProfile**.</span><span class="sxs-lookup"><span data-stu-id="909af-122">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="909af-123">Para publicar la aplicación de ejemplo mediante herramientas de la interfaz de la línea de comandos (CLI), ejecute el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) en un símbolo del sistema desde la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="909af-123">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="909af-124">El identificador relativo debe especificarse en la propiedad `<RuntimeIdenfifier>` o `<RuntimeIdentifiers>` del archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="909af-124">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="909af-125">En el ejemplo siguiente, la aplicación se publica en la configuración de lanzamiento para `win7-x64` en tiempo de ejecución:</span><span class="sxs-lookup"><span data-stu-id="909af-125">In the following example, the app is published in Release configuration for the `win7-x64` runtime:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64
   ```

1. <span data-ttu-id="909af-126">Use la herramienta de línea de comandos [sc.exe](https://technet.microsoft.com/library/bb490995) para crear el servicio.</span><span class="sxs-lookup"><span data-stu-id="909af-126">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="909af-127">El valor `binPath` es la ruta de acceso al archivo ejecutable de la aplicación, que incluye el nombre del archivo ejecutable.</span><span class="sxs-lookup"><span data-stu-id="909af-127">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="909af-128">**El espacio entre el signo igual y las comillas al inicio de la cadena de ruta de acceso es obligatorio.**</span><span class="sxs-lookup"><span data-stu-id="909af-128">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="909af-129">En el caso de un servicio publicado en la carpeta del proyecto, use la ruta de acceso a la carpeta *publish* para crear el servicio.</span><span class="sxs-lookup"><span data-stu-id="909af-129">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="909af-130">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="909af-130">In the following example:</span></span>

   * <span data-ttu-id="909af-131">El proyecto reside en la carpeta *c:\\my_services\\AspNetCoreService*.</span><span class="sxs-lookup"><span data-stu-id="909af-131">The project resides in the *c:\\my_services\\AspNetCoreService* folder.</span></span>
   * <span data-ttu-id="909af-132">El proyecto se publica en la configuración `Release`.</span><span class="sxs-lookup"><span data-stu-id="909af-132">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="909af-133">El moniker de la plataforma de destino (TFM) es `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="909af-133">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="909af-134">El identificador del entorno de ejecución (RID) es `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="909af-134">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="909af-135">El ejecutable de la aplicación se denomina *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="909af-135">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="909af-136">El servicio se denomina **MyService**.</span><span class="sxs-lookup"><span data-stu-id="909af-136">The service is named **MyService**.</span></span>

   <span data-ttu-id="909af-137">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="909af-137">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="909af-138">No olvide incluir el espacio entre el argumento `binPath=` y su valor.</span><span class="sxs-lookup"><span data-stu-id="909af-138">Make sure the space is present between the `binPath=` argument and its value.</span></span>

   <span data-ttu-id="909af-139">Para publicar e iniciar el servicio desde otra carpeta:</span><span class="sxs-lookup"><span data-stu-id="909af-139">To publish and start the service from a different folder:</span></span>

      * <span data-ttu-id="909af-140">Use la opción [--output &lt;DIRECTORIO_DE_SALIDA&gt;](/dotnet/core/tools/dotnet-publish#options) en el comando `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="909af-140">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="909af-141">Si utiliza Visual Studio, configure el valor **Ubicación de destino** de la página de la propiedad de publicación **FolderProfile** antes de hacer clic en el botón **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="909af-141">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
      * <span data-ttu-id="909af-142">Cree el servicio con el comando `sc.exe` utilizando la ruta de acceso de la carpeta de salida.</span><span class="sxs-lookup"><span data-stu-id="909af-142">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="909af-143">Incluya el nombre del archivo ejecutable del servicio en la ruta de acceso proporcionada a `binPath`.</span><span class="sxs-lookup"><span data-stu-id="909af-143">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="909af-144">Inicie el servicio con el comando `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="909af-144">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="909af-145">Use el siguiente comando para iniciar el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="909af-145">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="909af-146">Este comando tarda unos segundos en iniciar el servicio.</span><span class="sxs-lookup"><span data-stu-id="909af-146">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="909af-147">Para comprobar el estado del servicio, use el comando `sc query <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="909af-147">To check the status of the service, use the `sc query <SERVICE_NAME>` command.</span></span> <span data-ttu-id="909af-148">El estado se notifica como uno de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="909af-148">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="909af-149">Use el siguiente comando para comprobar el estado del servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="909af-149">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="909af-150">Si el servicio está en estado `RUNNING` y dicho servicio es una aplicación web, vaya a la aplicación en su ruta de acceso correspondiente (de forma predeterminada, `http://localhost:5000`, que redirige a `https://localhost:5001` cuando se usa el [Middleware de redirección de HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="909af-150">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="909af-151">Si es el servicio de la aplicación de ejemplo, vaya a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="909af-151">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="909af-152">Detenga el servicio con el comando `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="909af-152">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="909af-153">Con el siguiente comando se detiene el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="909af-153">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="909af-154">Tras un breve intervalo para detener un servicio, desinstale el servicio con el comando `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="909af-154">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="909af-155">Compruebe el estado del servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="909af-155">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="909af-156">Si el servicio de la aplicación de ejemplo está en estado `STOPPED`, use el siguiente comando para desinstalar el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="909af-156">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="909af-157">Ejecutar la aplicación fuera de un servicio</span><span class="sxs-lookup"><span data-stu-id="909af-157">Run the app outside of a service</span></span>

<span data-ttu-id="909af-158">Probar y depurar una aplicación resulta más sencillo cuando se ejecuta fuera de un servicio, por lo que es habitual agregar código que llame a `RunAsService` solo bajo determinadas condiciones.</span><span class="sxs-lookup"><span data-stu-id="909af-158">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="909af-159">Por ejemplo, la aplicación se puede ejecutar como una aplicación de consola con un argumento de línea de comandos `--console` o si el depurador está asociado:</span><span class="sxs-lookup"><span data-stu-id="909af-159">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="909af-160">Dado que la configuración de ASP.NET Core requiere pares nombre-valor en los argumentos de línea de comandos, el conmutador `--console` se quita antes de que los argumentos se pasen a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="909af-160">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="909af-161">`isService` no se pasa de `Main` a `CreateWebHostBuilder` porque la firma de `CreateWebHostBuilder` debe ser `CreateWebHostBuilder(string[])` para que las [pruebas de integración](xref:test/integration-tests) funcionen correctamente.</span><span class="sxs-lookup"><span data-stu-id="909af-161">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="909af-162">Controlar los eventos de inicio y detención</span><span class="sxs-lookup"><span data-stu-id="909af-162">Handle stopping and starting events</span></span>

<span data-ttu-id="909af-163">Para controlar los eventos [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) y [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), haga los siguientes cambios adicionales:</span><span class="sxs-lookup"><span data-stu-id="909af-163">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="909af-164">Cree una clase que se derive de la clase [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="909af-164">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="909af-165">Cree un método de extensión para [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) que pase el elemento `WebHostService` personalizado a [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="909af-165">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="909af-166">En `Program.Main`, llame al nuevo método de extensión, `RunAsCustomService`, en lugar de a [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="909af-166">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="909af-167">`isService` no se pasa de `Main` a `CreateWebHostBuilder` porque la firma de `CreateWebHostBuilder` debe ser `CreateWebHostBuilder(string[])` para que las [pruebas de integración](xref:test/integration-tests) funcionen correctamente.</span><span class="sxs-lookup"><span data-stu-id="909af-167">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="909af-168">Si el código `WebHostService` personalizado requiere un servicio de inserción de dependencias (como un registrador), obténgalo de la propiedad [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):</span><span class="sxs-lookup"><span data-stu-id="909af-168">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="909af-169">Escenarios de servidor proxy y equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="909af-169">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="909af-170">Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="909af-170">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="909af-171">Para obtener más información, vea <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="909af-171">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="909af-172">Configuración de HTTPS</span><span class="sxs-lookup"><span data-stu-id="909af-172">Configure HTTPS</span></span>

<span data-ttu-id="909af-173">Especifique una [configuración de punto de conexión HTTPS de servidor Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="909af-173">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="909af-174">Directorio actual y raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="909af-174">Current directory and content root</span></span>

<span data-ttu-id="909af-175">El directorio de trabajo actual devuelto por una llamada a `Directory.GetCurrentDirectory()` para un servicio de Windows es la carpeta *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="909af-175">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="909af-176">La carpeta *system32* no es una ubicación adecuada para almacenar los archivos de un servicio (por ejemplo los archivos de configuración).</span><span class="sxs-lookup"><span data-stu-id="909af-176">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="909af-177">Use uno de los enfoques siguientes para mantener y acceder a los recursos y los archivos de configuración de un servicio con [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) cuando se usa una interfaz [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="909af-177">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="909af-178">Use la ruta de acceso raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="909af-178">Use the content root path.</span></span> <span data-ttu-id="909af-179">`IHostingEnvironment.ContentRootPath` es la misma ruta de acceso proporcionada al argumento `binPath` cuando se crea el servicio.</span><span class="sxs-lookup"><span data-stu-id="909af-179">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="909af-180">En lugar de usar `Directory.GetCurrentDirectory()` para crear rutas de acceso a los archivos de configuración, use la ruta de acceso raíz del contenido y mantenga los archivos en la raíz de contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="909af-180">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="909af-181">Almacene los archivos en una ubicación adecuada en el disco.</span><span class="sxs-lookup"><span data-stu-id="909af-181">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="909af-182">Especifique una ruta de acceso absoluta con `SetBasePath` a la carpeta que contiene los archivos.</span><span class="sxs-lookup"><span data-stu-id="909af-182">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="909af-183">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="909af-183">Additional resources</span></span>

* <span data-ttu-id="909af-184">[Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)</span><span class="sxs-lookup"><span data-stu-id="909af-184">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
