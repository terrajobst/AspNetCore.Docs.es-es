---
title: Host en un servicio de Windows
author: tdykstra
description: "Obtenga información acerca de cómo hospedar una aplicación de ASP.NET Core en un servicio de Windows."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: c14a1f62bce4d06be3b8e6356f45cd5e330a0751
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="f034b-103">Hospedar una aplicación ASP.NET básica en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="f034b-103">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="f034b-104">Por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f034b-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f034b-105">La manera recomendada para hospedar una aplicación de ASP.NET Core en Windows sin usar IIS es ejecutarlo en un [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="f034b-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="f034b-106">Cuando se hospeda como un servicio de Windows, la aplicación puede automáticamente inicio después se reinicia y se bloquea sin necesidad de intervención humana.</span><span class="sxs-lookup"><span data-stu-id="f034b-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="f034b-107">[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f034b-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="f034b-108">Para obtener instrucciones sobre cómo ejecutar la aplicación de ejemplo, vea el ejemplo *README.md* archivo.</span><span class="sxs-lookup"><span data-stu-id="f034b-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f034b-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f034b-109">Prerequisites</span></span>

* <span data-ttu-id="f034b-110">Debe ejecutar la aplicación en el tiempo de ejecución de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f034b-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="f034b-111">En el *.csproj* de archivos, especifique los valores adecuados para [TargetFramework](/nuget/schema/target-frameworks) y [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="f034b-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="f034b-112">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f034b-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="f034b-113">Al crear un proyecto en Visual Studio, utilice el **aplicación de ASP.NET Core (.NET Framework)** plantilla.</span><span class="sxs-lookup"><span data-stu-id="f034b-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="f034b-114">Si la aplicación recibe las solicitudes de Internet (no solo desde una red interna), debe utilizar el [HTTP.sys](xref:fundamentals/servers/httpsys) servidor web (anteriormente conocidos como [WebListener](xref:fundamentals/servers/weblistener) para las aplicaciones de ASP.NET Core 1.x) en lugar de [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="f034b-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="f034b-115">IIS se recomienda para su uso como un servidor proxy inverso con Kestrel para implementaciones de borde.</span><span class="sxs-lookup"><span data-stu-id="f034b-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="f034b-116">Para más información, vea [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy) (Cuándo se debe usar Kestrel con un proxy inverso).</span><span class="sxs-lookup"><span data-stu-id="f034b-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="f034b-117">Introducción</span><span class="sxs-lookup"><span data-stu-id="f034b-117">Getting started</span></span>

<span data-ttu-id="f034b-118">En esta sección se explica los cambios mínimos necesarios para configurar un proyecto de ASP.NET Core existente para que se ejecute en un servicio.</span><span class="sxs-lookup"><span data-stu-id="f034b-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="f034b-119">Instale el paquete NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="f034b-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="f034b-120">Realice los cambios siguientes en `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="f034b-120">Make the following changes in `Program.Main`:</span></span>
  
   * <span data-ttu-id="f034b-121">Llame a `host.RunAsService` en lugar de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="f034b-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
   * <span data-ttu-id="f034b-122">Si el código llama `UseContentRoot`, use una ruta de acceso a la ubicación de publicación en lugar de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="f034b-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f034b-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f034b-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f034b-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f034b-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

1. <span data-ttu-id="f034b-125">Publique la aplicación en una carpeta.</span><span class="sxs-lookup"><span data-stu-id="f034b-125">Publish the app to a folder.</span></span> <span data-ttu-id="f034b-126">Use [publicar dotnet](/dotnet/articles/core/tools/dotnet-publish) o un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) que publica en una carpeta.</span><span class="sxs-lookup"><span data-stu-id="f034b-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

1. <span data-ttu-id="f034b-127">Probar al crear e iniciar el servicio.</span><span class="sxs-lookup"><span data-stu-id="f034b-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="f034b-128">Abra un shell de comandos del sistema con privilegios administrativos para usar el [sc.exe](https://technet.microsoft.com/library/bb490995) herramienta de línea de comandos para crear e iniciar un servicio.</span><span class="sxs-lookup"><span data-stu-id="f034b-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="f034b-129">Si el servicio se denomina MyService, publica en `c:\svc`, y con el nombre AspNetCoreService, los comandos son:</span><span class="sxs-lookup"><span data-stu-id="f034b-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="f034b-130">El `binPath` valor es la ruta de acceso al archivo ejecutable de la aplicación, que incluye el nombre del archivo ejecutable.</span><span class="sxs-lookup"><span data-stu-id="f034b-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![Ventana de la consola crear e iniciar el ejemplo](windows-service/_static/create-start.png)

   <span data-ttu-id="f034b-132">Cuando termine de estos comandos, vaya a la misma ruta que cuando se ejecuta como una aplicación de consola (de forma predeterminada, `http://localhost:5000`):</span><span class="sxs-lookup"><span data-stu-id="f034b-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![Ejecución en un servicio](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="f034b-134">Proporcionan una manera para que se ejecute fuera de un servicio</span><span class="sxs-lookup"><span data-stu-id="f034b-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="f034b-135">Es más fácil de probar y depurar cuando se ejecuta fuera de un servicio, por lo que es habitual para agregar código que llama a `RunAsService` únicamente bajo ciertas condiciones.</span><span class="sxs-lookup"><span data-stu-id="f034b-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="f034b-136">Por ejemplo, puede ejecutar la aplicación como una aplicación de consola con una `--console` si el depurador se adjunta o argumento de línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="f034b-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f034b-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f034b-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f034b-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f034b-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="f034b-139">Controlar detener e iniciar los eventos</span><span class="sxs-lookup"><span data-stu-id="f034b-139">Handle stopping and starting events</span></span>

<span data-ttu-id="f034b-140">Para controlar `OnStarting`, `OnStarted`, y `OnStopping` eventos, realice los siguientes cambios adicionales:</span><span class="sxs-lookup"><span data-stu-id="f034b-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="f034b-141">Cree una clase que deriva de `WebHostService`:</span><span class="sxs-lookup"><span data-stu-id="f034b-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

1. <span data-ttu-id="f034b-142">Crear un método de extensión para `IWebHost` que pasa personalizado `WebHostService` a `ServiceBase.Run`:</span><span class="sxs-lookup"><span data-stu-id="f034b-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

1. <span data-ttu-id="f034b-143">En `Program.Main`, llamar al nuevo método de extensión, `RunAsCustomService`, en lugar de `RunAsService`:</span><span class="sxs-lookup"><span data-stu-id="f034b-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f034b-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f034b-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f034b-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f034b-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

<span data-ttu-id="f034b-146">Si la opción de instalación `WebHostService` código requiere un servicio de inserción de dependencias (como un registrador), lo obtenga el `Services` propiedad de `IWebHost`:</span><span class="sxs-lookup"><span data-stu-id="f034b-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="acknowledgments"></a><span data-ttu-id="f034b-147">Confirmaciones</span><span class="sxs-lookup"><span data-stu-id="f034b-147">Acknowledgments</span></span>

<span data-ttu-id="f034b-148">En este artículo se escribió con la Ayuda de orígenes publicadas:</span><span class="sxs-lookup"><span data-stu-id="f034b-148">This article was written with the help of published sources:</span></span>

* [<span data-ttu-id="f034b-149">Hospedaje de ASP.NET Core como servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="f034b-149">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="f034b-150">Cómo hospedar el núcleo de ASP.NET en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="f034b-150">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
