---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: rick-anderson
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153534"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="79b1c-103">Hospedaje de ASP.NET Core en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="79b1c-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="79b1c-104">Por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="79b1c-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="79b1c-105">La manera recomendada de hospedar una aplicación ASP.NET Core en Windows sin usar IIS es ejecutarla en un [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="79b1c-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="79b1c-106">Cuando se hospeda como un servicio de Windows, la aplicación puede iniciarse automáticamente después de reinicios y bloqueos sin necesidad de intervención humana.</span><span class="sxs-lookup"><span data-stu-id="79b1c-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="79b1c-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="79b1c-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="79b1c-108">Para obtener instrucciones sobre cómo ejecutar la aplicación de ejemplo, vea el archivo *README.md* del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="79b1c-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79b1c-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="79b1c-109">Prerequisites</span></span>

* <span data-ttu-id="79b1c-110">La aplicación debe ejecutarse en el entorno de tiempo de ejecución de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="79b1c-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="79b1c-111">En el archivo *.csproj*, especifique los valores adecuados para [TargetFramework](/nuget/schema/target-frameworks) y [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="79b1c-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="79b1c-112">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="79b1c-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="79b1c-113">Al crear un proyecto en Visual Studio, use la plantilla **Aplicación ASP.NET Core (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="79b1c-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="79b1c-114">Si la aplicación recibe las solicitudes de Internet (no solo desde una red interna), debe usar el servidor web [HTTP.sys](xref:fundamentals/servers/httpsys) (antes conocidos como [WebListener](xref:fundamentals/servers/weblistener) para las aplicaciones ASP.NET Core 1.x) en lugar de [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="79b1c-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="79b1c-115">Se recomienda el uso de IIS como un servidor proxy inverso con Kestrel en implementaciones perimetrales.</span><span class="sxs-lookup"><span data-stu-id="79b1c-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="79b1c-116">Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).</span><span class="sxs-lookup"><span data-stu-id="79b1c-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="get-started"></a><span data-ttu-id="79b1c-117">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="79b1c-117">Get started</span></span>

<span data-ttu-id="79b1c-118">En esta sección se explican los cambios mínimos necesarios para configurar un proyecto de ASP.NET Core existente para que se ejecute en un servicio.</span><span class="sxs-lookup"><span data-stu-id="79b1c-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="79b1c-119">Instale el paquete NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="79b1c-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

2. <span data-ttu-id="79b1c-120">Realice los siguientes cambios en `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="79b1c-120">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="79b1c-121">Llame a `host.RunAsService` en lugar de a `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="79b1c-121">Call `host.RunAsService` instead of `host.Run`.</span></span>

   * <span data-ttu-id="79b1c-122">Si el código llama a `UseContentRoot`, use una ruta de acceso a la ubicación de publicación en lugar de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="79b1c-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="79b1c-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="79b1c-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="79b1c-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="79b1c-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. <span data-ttu-id="79b1c-125">Publique la aplicación en una carpeta.</span><span class="sxs-lookup"><span data-stu-id="79b1c-125">Publish the app to a folder.</span></span> <span data-ttu-id="79b1c-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) o un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) que publique en una carpeta.</span><span class="sxs-lookup"><span data-stu-id="79b1c-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

4. <span data-ttu-id="79b1c-127">Para probar esto, cree e inicie el servicio.</span><span class="sxs-lookup"><span data-stu-id="79b1c-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="79b1c-128">Abra un shell de comandos con privilegios administrativos para usar la herramienta de la línea de comandos [sc.exe](https://technet.microsoft.com/library/bb490995) para crear e iniciar un servicio.</span><span class="sxs-lookup"><span data-stu-id="79b1c-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="79b1c-129">Si el servicio se llamó MyService, se publicó en `c:\svc` y se llamó AspNetCoreService, los comandos son:</span><span class="sxs-lookup"><span data-stu-id="79b1c-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="79b1c-130">El valor `binPath` es la ruta de acceso al archivo ejecutable de la aplicación, que incluye el nombre del archivo ejecutable.</span><span class="sxs-lookup"><span data-stu-id="79b1c-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![Ventana de la consola: ejemplo de creación e inicio](windows-service/_static/create-start.png)

   <span data-ttu-id="79b1c-132">Cuando finalicen estos comandos, vaya a la misma ruta de acceso que cuando se ejecutó como una aplicación de consola (de forma predeterminada, `http://localhost:5000`):</span><span class="sxs-lookup"><span data-stu-id="79b1c-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![Ejecución en un servicio](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="79b1c-134">Proporcionar una forma de ejecutar la aplicación fuera de un servicio</span><span class="sxs-lookup"><span data-stu-id="79b1c-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="79b1c-135">Probar y depurar una aplicación resulta más sencillo cuando se ejecuta fuera de un servicio, por lo que es habitual agregar código que llame a `RunAsService` solo bajo determinadas condiciones.</span><span class="sxs-lookup"><span data-stu-id="79b1c-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="79b1c-136">Por ejemplo, la aplicación se puede ejecutar como una aplicación de consola con un argumento de línea de comandos `--console` o si el depurador está asociado:</span><span class="sxs-lookup"><span data-stu-id="79b1c-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="79b1c-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="79b1c-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="79b1c-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="79b1c-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="79b1c-139">Controlar los eventos de inicio y detención</span><span class="sxs-lookup"><span data-stu-id="79b1c-139">Handle stopping and starting events</span></span>

<span data-ttu-id="79b1c-140">Para controlar los eventos `OnStarting`, `OnStarted` y `OnStopping`, realice los siguientes cambios adicionales:</span><span class="sxs-lookup"><span data-stu-id="79b1c-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="79b1c-141">Cree una clase que derive de `WebHostService`:</span><span class="sxs-lookup"><span data-stu-id="79b1c-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="79b1c-142">Crear un método de extensión para `IWebHost` que pase el elemento `WebHostService` personalizado a `ServiceBase.Run`:</span><span class="sxs-lookup"><span data-stu-id="79b1c-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="79b1c-143">En `Program.Main`, llame al nuevo método de extensión, `RunAsCustomService`, en lugar de a `RunAsService`:</span><span class="sxs-lookup"><span data-stu-id="79b1c-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="79b1c-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="79b1c-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="79b1c-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="79b1c-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

<span data-ttu-id="79b1c-146">Si el código `WebHostService` personalizado requiere un servicio de inserción de dependencias (como un registrador), obténgalo de la propiedad `Services` de `IWebHost`:</span><span class="sxs-lookup"><span data-stu-id="79b1c-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="79b1c-147">Escenarios de servidor proxy y equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="79b1c-147">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="79b1c-148">Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="79b1c-148">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="79b1c-149">Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="79b1c-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="79b1c-150">Agradecimientos</span><span class="sxs-lookup"><span data-stu-id="79b1c-150">Acknowledgments</span></span>

<span data-ttu-id="79b1c-151">El artículo se escribió con ayuda de fuentes publicadas:</span><span class="sxs-lookup"><span data-stu-id="79b1c-151">This article was written with the help of published sources:</span></span>

* <span data-ttu-id="79b1c-152">[Hosting ASP.NET Core as Windows service](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074) (Hospedaje de ASP.NET Core como servicio de Windows)</span><span class="sxs-lookup"><span data-stu-id="79b1c-152">[Hosting ASP.NET Core as Windows service](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)</span></span>
* <span data-ttu-id="79b1c-153">[How to host your ASP.NET Core in a Windows Service](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/) (Cómo hospedar ASP.NET Core en un servicio de Windows)</span><span class="sxs-lookup"><span data-stu-id="79b1c-153">[How to host your ASP.NET Core in a Windows Service](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)</span></span>
