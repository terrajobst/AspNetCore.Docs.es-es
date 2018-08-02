---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: guardrex
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 4aded0b87ca14a5c09844cc378efb1ac0c12a289
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342161"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="32c82-103">Hospedaje de ASP.NET Core en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="32c82-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="32c82-104">Por [Luke Latham](https://github.com/guardrex) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="32c82-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="32c82-105">Una aplicación de ASP.NET Core se puede hospedar en Windows sin usar IIS como [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="32c82-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="32c82-106">Cuando se hospeda como un servicio de Windows, la aplicación puede iniciarse automáticamente después de reinicios y bloqueos sin necesidad de intervención humana.</span><span class="sxs-lookup"><span data-stu-id="32c82-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="32c82-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="32c82-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="32c82-108">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="32c82-108">Get started</span></span>

<span data-ttu-id="32c82-109">Estos son los cambios mínimos necesarios para configurar un proyecto de ASP.NET Core existente para que se ejecute en un servicio:</span><span class="sxs-lookup"><span data-stu-id="32c82-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="32c82-110">El archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="32c82-110">In the project file:</span></span>

   1. <span data-ttu-id="32c82-111">Confirme la presencia del identificador en tiempo de ejecución o agregarlo al **\<PropertyGroup>** que contiene el marco de destino:</span><span class="sxs-lookup"><span data-stu-id="32c82-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>

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

   1. <span data-ttu-id="32c82-112">Agregue una referencia de paquete de [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="32c82-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="32c82-113">Realice los siguientes cambios en `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="32c82-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="32c82-114">Llame a [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) en lugar de a `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="32c82-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="32c82-115">Llame a [UseContentRoot](xref:fundamentals/host/web-host#content-root) y use una ruta de acceso a la ubicación de publicación de la aplicación en lugar de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="32c82-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,12)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="32c82-116">Publique la aplicación.</span><span class="sxs-lookup"><span data-stu-id="32c82-116">Publish the app.</span></span> <span data-ttu-id="32c82-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) o un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="32c82-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="32c82-118">Al utilizar Visual Studio, seleccione **FolderProfile**.</span><span class="sxs-lookup"><span data-stu-id="32c82-118">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="32c82-119">Para publicar la aplicación de ejemplo desde la línea de comandos, ejecute el siguiente comando en una ventana de la consola desde la carpeta de proyecto:</span><span class="sxs-lookup"><span data-stu-id="32c82-119">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="32c82-120">Use la herramienta de línea de comandos [sc.exe](https://technet.microsoft.com/library/bb490995) para crear el servicio.</span><span class="sxs-lookup"><span data-stu-id="32c82-120">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="32c82-121">El valor `binPath` es la ruta de acceso al archivo ejecutable de la aplicación, que incluye el nombre del archivo ejecutable.</span><span class="sxs-lookup"><span data-stu-id="32c82-121">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="32c82-122">**El espacio entre el signo igual y las comillas al inicio de la cadena de ruta de acceso es obligatorio.**</span><span class="sxs-lookup"><span data-stu-id="32c82-122">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="32c82-123">En el caso de un servicio publicado en la carpeta del proyecto, use la ruta de acceso a la carpeta *publish* para crear el servicio.</span><span class="sxs-lookup"><span data-stu-id="32c82-123">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="32c82-124">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="32c82-124">In the following example:</span></span>

   * <span data-ttu-id="32c82-125">El proyecto se encuentra en la carpeta `c:\my_services\AspNetCoreService`.</span><span class="sxs-lookup"><span data-stu-id="32c82-125">The project resides in the `c:\my_services\AspNetCoreService` folder.</span></span>
   * <span data-ttu-id="32c82-126">El proyecto se publica en la configuración `Release`.</span><span class="sxs-lookup"><span data-stu-id="32c82-126">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="32c82-127">El moniker de la plataforma de destino (TFM) es `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="32c82-127">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="32c82-128">El identificador del entorno de ejecución (RID) es `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="32c82-128">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="32c82-129">El ejecutable de la aplicación se denomina *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="32c82-129">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="32c82-130">El servicio se denomina **MyService**.</span><span class="sxs-lookup"><span data-stu-id="32c82-130">The service is named **MyService**.</span></span>

   <span data-ttu-id="32c82-131">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="32c82-131">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="32c82-132">No olvide incluir el espacio entre el argumento `binPath=` y su valor.</span><span class="sxs-lookup"><span data-stu-id="32c82-132">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="32c82-133">Para publicar e iniciar el servicio desde otra carpeta:</span><span class="sxs-lookup"><span data-stu-id="32c82-133">To publish and start the service from a different folder:</span></span>
   
      1. <span data-ttu-id="32c82-134">Use la opción [--output &lt;DIRECTORIO_DE_SALIDA&gt;](/dotnet/core/tools/dotnet-publish#options) en el comando `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="32c82-134">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="32c82-135">Si utiliza Visual Studio, configure el valor **Ubicación de destino** de la página de la propiedad de publicación **FolderProfile** antes de hacer clic en el botón **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="32c82-135">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
   1. <span data-ttu-id="32c82-136">Cree el servicio con el comando `sc.exe` utilizando la ruta de acceso de la carpeta de salida.</span><span class="sxs-lookup"><span data-stu-id="32c82-136">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="32c82-137">Incluya el nombre del archivo ejecutable del servicio en la ruta de acceso proporcionada a `binPath`.</span><span class="sxs-lookup"><span data-stu-id="32c82-137">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="32c82-138">Inicie el servicio con el comando `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="32c82-138">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="32c82-139">Use el siguiente comando para iniciar el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="32c82-139">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="32c82-140">Este comando tarda unos segundos en iniciar el servicio.</span><span class="sxs-lookup"><span data-stu-id="32c82-140">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="32c82-141">El comando `sc query <SERVICE_NAME>` se puede usar para comprobar el estado del servicio con objeto de conocer su estado:</span><span class="sxs-lookup"><span data-stu-id="32c82-141">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="32c82-142">Use el siguiente comando para comprobar el estado del servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="32c82-142">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="32c82-143">Si el servicio está en estado `RUNNING` y dicho servicio es una aplicación web, vaya a la aplicación en su ruta de acceso correspondiente (de forma predeterminada, `http://localhost:5000`, que redirige a `https://localhost:5001` cuando se usa el [Middleware de redirección de HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="32c82-143">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="32c82-144">Si es el servicio de la aplicación de ejemplo, vaya a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="32c82-144">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="32c82-145">Detenga el servicio con el comando `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="32c82-145">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="32c82-146">Con el siguiente comando se detiene el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="32c82-146">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="32c82-147">Tras un breve intervalo para detener un servicio, desinstale el servicio con el comando `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="32c82-147">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="32c82-148">Compruebe el estado del servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="32c82-148">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="32c82-149">Si el servicio de la aplicación de ejemplo está en estado `STOPPED`, use el siguiente comando para desinstalar el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="32c82-149">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="32c82-150">Proporcionar una forma de ejecutar la aplicación fuera de un servicio</span><span class="sxs-lookup"><span data-stu-id="32c82-150">Provide a way to run outside of a service</span></span>

<span data-ttu-id="32c82-151">Probar y depurar una aplicación resulta más sencillo cuando se ejecuta fuera de un servicio, por lo que es habitual agregar código que llame a `RunAsService` solo bajo determinadas condiciones.</span><span class="sxs-lookup"><span data-stu-id="32c82-151">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="32c82-152">Por ejemplo, la aplicación se puede ejecutar como una aplicación de consola con un argumento de línea de comandos `--console` o si el depurador está asociado:</span><span class="sxs-lookup"><span data-stu-id="32c82-152">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="32c82-153">Dado que la configuración de ASP.NET Core requiere pares nombre-valor en los argumentos de línea de comandos, el conmutador `--console` se quita antes de que los argumentos se pasen a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="32c82-153">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="32c82-154">`isService` no se pasa de `Main` a `CreateWebHostBuilder` porque la firma de `CreateWebHostBuilder` debe ser `CreateWebHostBuilder(string[])` para que las [pruebas de integración](xref:test/integration-tests) funcionen correctamente.</span><span class="sxs-lookup"><span data-stu-id="32c82-154">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="32c82-155">Controlar los eventos de inicio y detención</span><span class="sxs-lookup"><span data-stu-id="32c82-155">Handle stopping and starting events</span></span>

<span data-ttu-id="32c82-156">Para controlar los eventos [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) y [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), haga los siguientes cambios adicionales:</span><span class="sxs-lookup"><span data-stu-id="32c82-156">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="32c82-157">Cree una clase que se derive de la clase [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="32c82-157">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="32c82-158">Cree un método de extensión para [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) que pase el elemento `WebHostService` personalizado a [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="32c82-158">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="32c82-159">En `Program.Main`, llame al nuevo método de extensión, `RunAsCustomService`, en lugar de a [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="32c82-159">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=14)]

   > [!NOTE]
   > <span data-ttu-id="32c82-160">`isService` no se pasa de `Main` a `CreateWebHostBuilder` porque la firma de `CreateWebHostBuilder` debe ser `CreateWebHostBuilder(string[])` para que las [pruebas de integración](xref:test/integration-tests) funcionen correctamente.</span><span class="sxs-lookup"><span data-stu-id="32c82-160">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="32c82-161">Si el código `WebHostService` personalizado requiere un servicio de inserción de dependencias (como un registrador), obténgalo de la propiedad [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):</span><span class="sxs-lookup"><span data-stu-id="32c82-161">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="32c82-162">Escenarios de servidor proxy y equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="32c82-162">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="32c82-163">Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="32c82-163">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="32c82-164">Para obtener más información, vea <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="32c82-164">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="32c82-165">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="32c82-165">Additional resources</span></span>

* <span data-ttu-id="32c82-166">[Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)</span><span class="sxs-lookup"><span data-stu-id="32c82-166">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
