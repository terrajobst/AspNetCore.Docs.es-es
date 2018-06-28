---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: guardrex
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 5eba685bbe55d43bb063a01798bc691a1ba0d6fc
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613091"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="eb6ce-103">Hospedaje de ASP.NET Core en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="eb6ce-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="eb6ce-104">Por [Luke Latham](https://github.com/guardrex) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="eb6ce-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="eb6ce-105">Una aplicación de ASP.NET Core se puede hospedar en Windows sin usar IIS como [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="eb6ce-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="eb6ce-106">Cuando se hospeda como un servicio de Windows, la aplicación puede iniciarse automáticamente después de reinicios y bloqueos sin necesidad de intervención humana.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="eb6ce-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eb6ce-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="eb6ce-108">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="eb6ce-108">Get started</span></span>

<span data-ttu-id="eb6ce-109">Estos son los cambios mínimos necesarios para configurar un proyecto de ASP.NET Core existente para que se ejecute en un servicio:</span><span class="sxs-lookup"><span data-stu-id="eb6ce-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="eb6ce-110">El archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="eb6ce-110">In the project file:</span></span>

   1. <span data-ttu-id="eb6ce-111">Confirme la presencia del identificador en tiempo de ejecución o agregarlo al **\<PropertyGroup>** que contiene el marco de destino:</span><span class="sxs-lookup"><span data-stu-id="eb6ce-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. <span data-ttu-id="eb6ce-112">Agregue una referencia de paquete de [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="eb6ce-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="eb6ce-113">Realice los siguientes cambios en `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="eb6ce-114">Llame a [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) en lugar de a `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="eb6ce-115">Si el código llama a `UseContentRoot`, use una ruta de acceso a la ubicación de publicación de la aplicación en lugar de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-115">If the code calls `UseContentRoot`, use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     <span data-ttu-id="eb6ce-116">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span><span class="sxs-lookup"><span data-stu-id="eb6ce-116">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span></span>

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     <span data-ttu-id="eb6ce-117">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span><span class="sxs-lookup"><span data-stu-id="eb6ce-117">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span></span>

     ::: moniker-end

1. <span data-ttu-id="eb6ce-118">Publique la aplicación en una carpeta.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-118">Publish the app to a folder.</span></span> <span data-ttu-id="eb6ce-119">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) o un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) que publique en una carpeta.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-119">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

   <span data-ttu-id="eb6ce-120">Para publicar la aplicación de ejemplo desde la línea de comandos, ejecute el siguiente comando en una ventana de la consola desde la carpeta de proyecto:</span><span class="sxs-lookup"><span data-stu-id="eb6ce-120">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release --output c:\svc
   ```

1. <span data-ttu-id="eb6ce-121">Use la herramienta de línea de comandos [sc.exe](https://technet.microsoft.com/library/bb490995) para crear el servicio (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`).</span><span class="sxs-lookup"><span data-stu-id="eb6ce-121">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`).</span></span> <span data-ttu-id="eb6ce-122">El valor `binPath` es la ruta de acceso al archivo ejecutable de la aplicación, que incluye el nombre del archivo ejecutable.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-122">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="eb6ce-123">**El espacio entre el signo igual y las comillas que dan inicio a la cadena de ruta de acceso es obligatorio.**</span><span class="sxs-lookup"><span data-stu-id="eb6ce-123">**The space between the equal sign and the quote character that starts the path is required.**</span></span>

   <span data-ttu-id="eb6ce-124">En el caso de la aplicación de ejemplo y el comando que siguen, el servicio:</span><span class="sxs-lookup"><span data-stu-id="eb6ce-124">For the sample app and command that follows, the service is:</span></span>

   * <span data-ttu-id="eb6ce-125">Se denomina **MyService**.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-125">Named **MyService**.</span></span>
   * <span data-ttu-id="eb6ce-126">Se publica en la carpeta *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-126">Published to *c:\\svc* folder.</span></span>
   * <span data-ttu-id="eb6ce-127">Tiene un archivo ejecutable de la aplicación denominado *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-127">Has an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="eb6ce-128">Abra un shell de comandos con privilegios de administrador y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="eb6ce-128">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   ```

   <span data-ttu-id="eb6ce-129">**No olvide incluir el espacio entre el argumento `binPath=` y su valor correspondiente.**</span><span class="sxs-lookup"><span data-stu-id="eb6ce-129">**Make sure the space is present between the `binPath=` argument and its value.**</span></span>

1. <span data-ttu-id="eb6ce-130">Inicie el servicio con el comando `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-130">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="eb6ce-131">Use el siguiente comando para iniciar el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb6ce-131">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="eb6ce-132">Este comando tarda unos segundos en iniciar el servicio.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-132">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="eb6ce-133">El comando `sc query <SERVICE_NAME>` se puede usar para comprobar el estado del servicio con objeto de conocer su estado:</span><span class="sxs-lookup"><span data-stu-id="eb6ce-133">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="eb6ce-134">Use el siguiente comando para comprobar el estado del servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb6ce-134">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="eb6ce-135">Si el servicio está en estado `RUNNING` y dicho servicio es una aplicación web, vaya a la aplicación en su ruta de acceso correspondiente (de forma predeterminada, `http://localhost:5000`, que redirige a `https://localhost:5001` cuando se usa el [Middleware de redirección de HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="eb6ce-135">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="eb6ce-136">Si es el servicio de la aplicación de ejemplo, vaya a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-136">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="eb6ce-137">Detenga el servicio con el comando `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-137">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="eb6ce-138">Con el siguiente comando se detiene el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb6ce-138">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="eb6ce-139">Tras un breve intervalo para detener un servicio, desinstale el servicio con el comando `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-139">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="eb6ce-140">Compruebe el estado del servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb6ce-140">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="eb6ce-141">Si el servicio de la aplicación de ejemplo está en estado `STOPPED`, use el siguiente comando para desinstalar el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb6ce-141">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="eb6ce-142">Proporcionar una forma de ejecutar la aplicación fuera de un servicio</span><span class="sxs-lookup"><span data-stu-id="eb6ce-142">Provide a way to run outside of a service</span></span>

<span data-ttu-id="eb6ce-143">Probar y depurar una aplicación resulta más sencillo cuando se ejecuta fuera de un servicio, por lo que es habitual agregar código que llame a `RunAsService` solo bajo determinadas condiciones.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-143">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="eb6ce-144">Por ejemplo, la aplicación se puede ejecutar como una aplicación de consola con un argumento de línea de comandos `--console` o si el depurador está asociado:</span><span class="sxs-lookup"><span data-stu-id="eb6ce-144">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="eb6ce-145">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="eb6ce-145">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span></span>

<span data-ttu-id="eb6ce-146">Dado que la configuración de ASP.NET Core requiere pares nombre-valor en los argumentos de línea de comandos, el conmutador `--console` se quita antes de que los argumentos se pasen a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="eb6ce-146">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="eb6ce-147">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="eb6ce-147">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span></span>

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="eb6ce-148">Controlar los eventos de inicio y detención</span><span class="sxs-lookup"><span data-stu-id="eb6ce-148">Handle stopping and starting events</span></span>

<span data-ttu-id="eb6ce-149">Para controlar los eventos [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) y [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), haga los siguientes cambios adicionales:</span><span class="sxs-lookup"><span data-stu-id="eb6ce-149">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="eb6ce-150">Cree una clase que se derive de la clase [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="eb6ce-150">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="eb6ce-151">Cree un método de extensión para [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) que pase el elemento `WebHostService` personalizado a [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="eb6ce-151">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="eb6ce-152">En `Program.Main`, llame al nuevo método de extensión, `RunAsCustomService`, en lugar de a [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="eb6ce-152">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   <span data-ttu-id="eb6ce-153">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="eb6ce-153">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   <span data-ttu-id="eb6ce-154">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="eb6ce-154">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

<span data-ttu-id="eb6ce-155">Si el código `WebHostService` personalizado requiere un servicio de inserción de dependencias (como un registrador), obténgalo de la propiedad [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):</span><span class="sxs-lookup"><span data-stu-id="eb6ce-155">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="eb6ce-156">Escenarios de servidor proxy y equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="eb6ce-156">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="eb6ce-157">Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="eb6ce-157">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="eb6ce-158">Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="eb6ce-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="eb6ce-159">Configuración del punto de conexión de Kestrel</span><span class="sxs-lookup"><span data-stu-id="eb6ce-159">Kestrel endpoint configuration</span></span>

<span data-ttu-id="eb6ce-160">Para más información sobre la configuración del punto de conexión de Kestrel, así como la configuración de HTTPS y la compatibilidad con SNI, vea [Configuración de punto de conexión de Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="eb6ce-160">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
