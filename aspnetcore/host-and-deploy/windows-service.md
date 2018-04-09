---
title: Host principal de ASP.NET en un servicio de Windows
author: tdykstra
description: Obtenga información acerca de cómo hospedar una aplicación de ASP.NET Core en un servicio de Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: b0b27f274de1ca88b20bf582127132527b553ce0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="f11d1-103">Host principal de ASP.NET en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="f11d1-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="f11d1-104">Por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f11d1-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f11d1-105">La manera recomendada para hospedar una aplicación de ASP.NET Core en Windows sin usar IIS es ejecutarlo en un [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="f11d1-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="f11d1-106">Cuando se hospeda como un servicio de Windows, la aplicación puede automáticamente inicio después se reinicia y se bloquea sin necesidad de intervención humana.</span><span class="sxs-lookup"><span data-stu-id="f11d1-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="f11d1-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f11d1-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="f11d1-108">Para obtener instrucciones sobre cómo ejecutar la aplicación de ejemplo, vea el ejemplo *README.md* archivo.</span><span class="sxs-lookup"><span data-stu-id="f11d1-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f11d1-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f11d1-109">Prerequisites</span></span>

* <span data-ttu-id="f11d1-110">Debe ejecutar la aplicación en el tiempo de ejecución de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f11d1-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="f11d1-111">En el *.csproj* de archivos, especifique los valores adecuados para [TargetFramework](/nuget/schema/target-frameworks) y [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="f11d1-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="f11d1-112">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f11d1-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="f11d1-113">Al crear un proyecto en Visual Studio, utilice el **aplicación de ASP.NET Core (.NET Framework)** plantilla.</span><span class="sxs-lookup"><span data-stu-id="f11d1-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="f11d1-114">Si la aplicación recibe las solicitudes de Internet (no solo desde una red interna), debe utilizar el [HTTP.sys](xref:fundamentals/servers/httpsys) servidor web (anteriormente conocidos como [WebListener](xref:fundamentals/servers/weblistener) para las aplicaciones de ASP.NET Core 1.x) en lugar de [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="f11d1-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="f11d1-115">IIS se recomienda para su uso como un servidor proxy inverso con Kestrel para implementaciones de borde.</span><span class="sxs-lookup"><span data-stu-id="f11d1-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="f11d1-116">Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).</span><span class="sxs-lookup"><span data-stu-id="f11d1-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="get-started"></a><span data-ttu-id="f11d1-117">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="f11d1-117">Get started</span></span>

<span data-ttu-id="f11d1-118">En esta sección se explica los cambios mínimos necesarios para configurar un proyecto de ASP.NET Core existente para que se ejecute en un servicio.</span><span class="sxs-lookup"><span data-stu-id="f11d1-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="f11d1-119">Instale el paquete NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="f11d1-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

2. <span data-ttu-id="f11d1-120">Realice los cambios siguientes en `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="f11d1-120">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="f11d1-121">Llame a `host.RunAsService` en lugar de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="f11d1-121">Call `host.RunAsService` instead of `host.Run`.</span></span>

   * <span data-ttu-id="f11d1-122">Si el código llama `UseContentRoot`, use una ruta de acceso a la ubicación de publicación en lugar de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="f11d1-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f11d1-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f11d1-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f11d1-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f11d1-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   * * *

3. <span data-ttu-id="f11d1-125">Publique la aplicación en una carpeta.</span><span class="sxs-lookup"><span data-stu-id="f11d1-125">Publish the app to a folder.</span></span> <span data-ttu-id="f11d1-126">Use [publicar dotnet](/dotnet/articles/core/tools/dotnet-publish) o un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) que publica en una carpeta.</span><span class="sxs-lookup"><span data-stu-id="f11d1-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

4. <span data-ttu-id="f11d1-127">Probar al crear e iniciar el servicio.</span><span class="sxs-lookup"><span data-stu-id="f11d1-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="f11d1-128">Abra un shell de comandos del sistema con privilegios administrativos para usar el [sc.exe](https://technet.microsoft.com/library/bb490995) herramienta de línea de comandos para crear e iniciar un servicio.</span><span class="sxs-lookup"><span data-stu-id="f11d1-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="f11d1-129">Si el servicio se denomina MyService, publica en `c:\svc`, y con el nombre AspNetCoreService, los comandos son:</span><span class="sxs-lookup"><span data-stu-id="f11d1-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="f11d1-130">El `binPath` valor es la ruta de acceso al archivo ejecutable de la aplicación, que incluye el nombre del archivo ejecutable.</span><span class="sxs-lookup"><span data-stu-id="f11d1-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![Ventana de la consola crear e iniciar el ejemplo](windows-service/_static/create-start.png)

   <span data-ttu-id="f11d1-132">Cuando termine de estos comandos, vaya a la misma ruta que cuando se ejecuta como una aplicación de consola (de forma predeterminada, `http://localhost:5000`):</span><span class="sxs-lookup"><span data-stu-id="f11d1-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![Ejecución en un servicio](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="f11d1-134">Proporcionan una manera para que se ejecute fuera de un servicio</span><span class="sxs-lookup"><span data-stu-id="f11d1-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="f11d1-135">Es más fácil de probar y depurar cuando se ejecuta fuera de un servicio, por lo que es habitual para agregar código que llama a `RunAsService` únicamente bajo ciertas condiciones.</span><span class="sxs-lookup"><span data-stu-id="f11d1-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="f11d1-136">Por ejemplo, puede ejecutar la aplicación como una aplicación de consola con una `--console` si el depurador se adjunta o argumento de línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="f11d1-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f11d1-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f11d1-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f11d1-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f11d1-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

* * *
## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="f11d1-139">Controlar detener e iniciar los eventos</span><span class="sxs-lookup"><span data-stu-id="f11d1-139">Handle stopping and starting events</span></span>

<span data-ttu-id="f11d1-140">Para controlar `OnStarting`, `OnStarted`, y `OnStopping` eventos, realice los siguientes cambios adicionales:</span><span class="sxs-lookup"><span data-stu-id="f11d1-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="f11d1-141">Cree una clase que deriva de `WebHostService`:</span><span class="sxs-lookup"><span data-stu-id="f11d1-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="f11d1-142">Crear un método de extensión para `IWebHost` que pasa personalizado `WebHostService` a `ServiceBase.Run`:</span><span class="sxs-lookup"><span data-stu-id="f11d1-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="f11d1-143">En `Program.Main`, llamar al nuevo método de extensión, `RunAsCustomService`, en lugar de `RunAsService`:</span><span class="sxs-lookup"><span data-stu-id="f11d1-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f11d1-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f11d1-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f11d1-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f11d1-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   * * *
<span data-ttu-id="f11d1-146">Si la opción de instalación `WebHostService` código requiere un servicio de inserción de dependencias (como un registrador), lo obtenga el `Services` propiedad de `IWebHost`:</span><span class="sxs-lookup"><span data-stu-id="f11d1-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="f11d1-147">Servidor proxy y escenarios de equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="f11d1-147">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="f11d1-148">Servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un servidor proxy o el equilibrador de carga pueden requerir una configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="f11d1-148">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="f11d1-149">Para obtener más información, consulte [configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="f11d1-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="f11d1-150">Agradecimientos</span><span class="sxs-lookup"><span data-stu-id="f11d1-150">Acknowledgments</span></span>

<span data-ttu-id="f11d1-151">En este artículo se escribió con la Ayuda de orígenes publicadas:</span><span class="sxs-lookup"><span data-stu-id="f11d1-151">This article was written with the help of published sources:</span></span>

* [<span data-ttu-id="f11d1-152">Hospedaje de ASP.NET Core como servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="f11d1-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="f11d1-153">Cómo hospedar el núcleo de ASP.NET en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="f11d1-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
