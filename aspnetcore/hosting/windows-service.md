---
title: Host en un servicio de Windows
author: tdykstra
description: "Obtenga información acerca de cómo hospedar una aplicación de ASP.NET Core en un servicio de Windows."
keywords: Hospedaje de ASP.NET Core, servicio de Windows,
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: 33a4eca48a04f9b29c60a446f4191d39d21e7e7d
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="9dc72-104">Hospedar una aplicación ASP.NET básica en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="9dc72-104">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="9dc72-105">Por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9dc72-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="9dc72-106">Es la manera recomendada para hospedar una aplicación de ASP.NET Core en Windows si no usa IIS para que se ejecute un [servicio de Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="9dc72-106">The recommended way to host an ASP.NET Core app on Windows when you don't use IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="9dc72-107">De este modo puede iniciar automáticamente después de reiniciar el equipo y se bloquea, sin esperar a que alguien inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="9dc72-107">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="9dc72-108">[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) consulte la [pasos](#next-steps) sección para obtener instrucciones sobre cómo ejecutarlo.</span><span class="sxs-lookup"><span data-stu-id="9dc72-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9dc72-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="9dc72-109">Prerequisites</span></span>

* <span data-ttu-id="9dc72-110">Debe ejecutar la aplicación en el tiempo de ejecución de .NET framework.</span><span class="sxs-lookup"><span data-stu-id="9dc72-110">The app must run on the .NET framework runtime.</span></span>  <span data-ttu-id="9dc72-111">En el *.csproj* de archivos, especifique los valores adecuados para [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) y [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="9dc72-111">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="9dc72-112">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9dc72-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="9dc72-113">Al crear un proyecto en Visual Studio, utilice el **aplicación de ASP.NET Core (.NET Framework)** plantilla.</span><span class="sxs-lookup"><span data-stu-id="9dc72-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="9dc72-114">Si la aplicación obtendrá las solicitudes de internet (no solo desde una red interna), debe utilizar el [WebListener](xref:fundamentals/servers/weblistener) servidor web en lugar de [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="9dc72-114">If the app will get requests from the internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="9dc72-115">Kestrel debe utilizarse con IIS para las implementaciones de borde.</span><span class="sxs-lookup"><span data-stu-id="9dc72-115">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="9dc72-116">Para obtener más información, consulte [cuándo utilizar Kestrel con un proxy inverso](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="9dc72-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="9dc72-117">Introducción</span><span class="sxs-lookup"><span data-stu-id="9dc72-117">Getting started</span></span>

<span data-ttu-id="9dc72-118">En esta sección se explica los cambios mínimos necesarios para configurar un proyecto de ASP.NET Core existente para que se ejecute en un servicio.</span><span class="sxs-lookup"><span data-stu-id="9dc72-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="9dc72-119">Instale el paquete NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="9dc72-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="9dc72-120">Realice los cambios siguientes en `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="9dc72-120">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="9dc72-121">Llame a `host.RunAsService` en lugar de `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="9dc72-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="9dc72-122">Si el código llama `UseContentRoot`, use una ruta de acceso a la ubicación de publicación en lugar de`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="9dc72-122">If your code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="9dc72-123">Publicar la aplicación en una carpeta.</span><span class="sxs-lookup"><span data-stu-id="9dc72-123">Publish the application to a folder.</span></span>

  <span data-ttu-id="9dc72-124">Use [publicar dotnet](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) o un [perfil de publicación de Visual Studio](xref:publishing/web-publishing-vs) que publica en una carpeta.</span><span class="sxs-lookup"><span data-stu-id="9dc72-124">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:publishing/web-publishing-vs) that publishes to a folder.</span></span>

* <span data-ttu-id="9dc72-125">Probar al crear e iniciar el servicio.</span><span class="sxs-lookup"><span data-stu-id="9dc72-125">Test by creating and starting the service.</span></span>

  <span data-ttu-id="9dc72-126">Abra una ventana de símbolo del sistema de administrador para usar la [sc.exe](https://technet.microsoft.com/library/bb490995) herramienta de línea de comandos para crear e iniciar un servicio.</span><span class="sxs-lookup"><span data-stu-id="9dc72-126">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="9dc72-127">Si asigna un nombre al servicio MyService, publicar la aplicación en `c:\svc`y la propia aplicación se denomina AspNetCoreService, los comandos sería similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="9dc72-127">If you name your service MyService, you publish your app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  <span data-ttu-id="9dc72-128">El `binPath` valor es la ruta de acceso al ejecutable de la aplicación, incluido el nombre de archivo ejecutable propio.</span><span class="sxs-lookup"><span data-stu-id="9dc72-128">The `binPath` value is the path to your app's executable, including the executable filename itself.</span></span>

  ![Ventana de la consola crear e iniciar el ejemplo](windows-service/_static/create-start.png)

  <span data-ttu-id="9dc72-130">Cuando termine de estos comandos, que puede ir a la misma ruta que cuando ejecuta como una aplicación de consola (de forma predeterminada, `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="9dc72-130">When these commands finish, you can browse to the same path as when you run as a console app (by default, `http://localhost:5000`)</span></span>

  ![Ejecución en un servicio](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="9dc72-132">Proporcionan una manera para que se ejecute fuera de un servicio</span><span class="sxs-lookup"><span data-stu-id="9dc72-132">Provide a way to run outside of a service</span></span>

<span data-ttu-id="9dc72-133">Es más fácil de probar y depurar cuando se ejecuta fuera de un servicio, por lo que es habitual para agregar código que llama a `host.RunAsService` únicamente bajo ciertas condiciones.</span><span class="sxs-lookup"><span data-stu-id="9dc72-133">It's easier to test and debug when you're running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="9dc72-134">Por ejemplo, podría ejecutar una aplicación de consola como si se produce un `--console` argumento de línea de comandos o si el depurador se adjunta.</span><span class="sxs-lookup"><span data-stu-id="9dc72-134">For example, you could run as a console app if you get a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="9dc72-135">Controlar detener e iniciar los eventos</span><span class="sxs-lookup"><span data-stu-id="9dc72-135">Handle stopping and starting events</span></span>

<span data-ttu-id="9dc72-136">Si desea controlar `OnStarting`, `OnStarted`, y `OnStopping` eventos, realice los siguientes cambios adicionales:</span><span class="sxs-lookup"><span data-stu-id="9dc72-136">If you want to handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="9dc72-137">Cree una clase que derive de `WebHostService`.</span><span class="sxs-lookup"><span data-stu-id="9dc72-137">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="9dc72-138">Crear un método de extensión para `IWebHost` que pasa personalizado `WebHostService` a `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="9dc72-138">Create an extension method for `IWebHost` that passes your custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="9dc72-139">En `Program.Main` cambio llamar al nuevo método de extensión en lugar de `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="9dc72-139">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="9dc72-140">Si su personalizado `WebHostService` código necesita obtener un servicio de inserción de dependencias (como un registrador), puede obtener desde la `Services` propiedad de `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="9dc72-140">If your custom `WebHostService` code needs to get a service from dependency injection (such as a logger), you can get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="9dc72-141">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="9dc72-141">Next steps</span></span>

<span data-ttu-id="9dc72-142">El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) que muestra en este artículo es una aplicación web MVC sencilla que se ha modificado como se muestra en los anteriores ejemplos de código.</span><span class="sxs-lookup"><span data-stu-id="9dc72-142">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="9dc72-143">Para ejecutarlo en un servicio, realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="9dc72-143">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="9dc72-144">Publicar en *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="9dc72-144">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="9dc72-145">Abra una ventana de administrador.</span><span class="sxs-lookup"><span data-stu-id="9dc72-145">Open an administrator window.</span></span>

* <span data-ttu-id="9dc72-146">Escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="9dc72-146">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="9dc72-147">En un explorador, vaya a http://localhost: 5000 para comprobar que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="9dc72-147">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="9dc72-148">Si la aplicación no se inicia como se esperaba cuando se ejecuta en un servicio, es una forma rápida de hacer accesibles los mensajes de error agregar un proveedor de registro como el [proveedor de registro de eventos de Windows](xref:fundamentals/logging#eventlog).</span><span class="sxs-lookup"><span data-stu-id="9dc72-148">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="9dc72-149">Confirmaciones</span><span class="sxs-lookup"><span data-stu-id="9dc72-149">Acknowledgments</span></span>

<span data-ttu-id="9dc72-150">En este artículo se escribió con la Ayuda de los orígenes que ya se han publicado.</span><span class="sxs-lookup"><span data-stu-id="9dc72-150">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="9dc72-151">La primera y más útiles de ellos fueron estos:</span><span class="sxs-lookup"><span data-stu-id="9dc72-151">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="9dc72-152">Hospedaje de ASP.NET Core como servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="9dc72-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="9dc72-153">Cómo hospedar el núcleo de ASP.NET en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="9dc72-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
