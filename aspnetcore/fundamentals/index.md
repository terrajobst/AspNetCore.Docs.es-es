---
title: Conceptos básicos de ASP.NET Core
author: rick-anderson
description: Descubra los conceptos básicos para crear aplicaciones de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/06/2019
uid: fundamentals/index
ms.openlocfilehash: a56beebd796448705c7b84f47699e9739f451419
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099239"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="e29ed-103">Conceptos básicos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e29ed-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="e29ed-104">Una aplicación de ASP.NET Core es una aplicación de consola que crea un servidor web en su método `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="e29ed-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="e29ed-105">El método `Main` es el *punto de entrada administrado* de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="e29ed-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="e29ed-106">Host de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="e29ed-106">The .NET Core Host:</span></span>

* <span data-ttu-id="e29ed-107">Carga el [entorno de ejecución de .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="e29ed-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="e29ed-108">Usa el primer argumento de la línea de comandos como la ruta de acceso al binario administrado que contiene el punto de entrada (`Main`) e inicia la ejecución del código.</span><span class="sxs-lookup"><span data-stu-id="e29ed-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="e29ed-109">El método `Main` invoca [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), que sigue el [patrón del generador](https://wikipedia.org/wiki/Builder_pattern) para crear un host de web.</span><span class="sxs-lookup"><span data-stu-id="e29ed-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="e29ed-110">El generador tiene métodos que definen el servidor web (por ejemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) y la clase de inicio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="e29ed-110">The builder has methods that define a web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="e29ed-111">En el ejemplo anterior, se asigna automáticamente el servidor web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e29ed-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="e29ed-112">El host web de ASP.NET Core intenta ejecutarse en [Internet Information Services (IIS)](https://www.iis.net/), si está disponible.</span><span class="sxs-lookup"><span data-stu-id="e29ed-112">ASP.NET Core's web host attempts to run on [Internet Information Services (IIS)](https://www.iis.net/), if available.</span></span> <span data-ttu-id="e29ed-113">Otros servidores web, como [HTTP.sys](xref:fundamentals/servers/httpsys), se pueden usar al invocar el método de extensión adecuado.</span><span class="sxs-lookup"><span data-stu-id="e29ed-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="e29ed-114">`UseStartup` se explica con más detalle en la sección [Inicio](#startup).</span><span class="sxs-lookup"><span data-stu-id="e29ed-114">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="e29ed-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, el tipo de valor devuelto de la invocación `WebHost.CreateDefaultBuilder`, proporciona muchos métodos opcionales.</span><span class="sxs-lookup"><span data-stu-id="e29ed-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="e29ed-116">Algunos de estos métodos incluyen `UseHttpSys` para hospedar la aplicación en HTTP.sys y <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> para especificar el directorio de contenido raíz.</span><span class="sxs-lookup"><span data-stu-id="e29ed-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="e29ed-117">Los métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> y <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> crean el objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda la aplicación y empieza a escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="e29ed-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="e29ed-118">Host de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="e29ed-118">The .NET Core Host:</span></span>

* <span data-ttu-id="e29ed-119">Carga el [entorno de ejecución de .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="e29ed-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="e29ed-120">Usa el primer argumento de la línea de comandos como la ruta de acceso al binario administrado que contiene el punto de entrada (`Main`) e inicia la ejecución del código.</span><span class="sxs-lookup"><span data-stu-id="e29ed-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="e29ed-121">El método `Main` usa <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, que sigue el [patrón del generador](https://wikipedia.org/wiki/Builder_pattern) para crear un host de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="e29ed-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="e29ed-122">El generador tiene métodos que definen el servidor web (por ejemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) y la clase de inicio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="e29ed-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="e29ed-123">En el ejemplo anterior, se usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e29ed-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="e29ed-124">Otros servidores web, como [HTTP.sys](xref:fundamentals/servers/httpsys), se pueden usar al invocar el método de extensión adecuado.</span><span class="sxs-lookup"><span data-stu-id="e29ed-124">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="e29ed-125">`UseStartup` se explica con más detalle en la sección [Inicio](#startup).</span><span class="sxs-lookup"><span data-stu-id="e29ed-125">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="e29ed-126">`WebHostBuilder` proporciona muchos métodos opcionales, incluido <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> para hospedar en IIS e IIS Express, y <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> para especificar el directorio de contenido raíz.</span><span class="sxs-lookup"><span data-stu-id="e29ed-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="e29ed-127">Los métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> y <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> crean el objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda la aplicación y empieza a escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="e29ed-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="e29ed-128">Inicio</span><span class="sxs-lookup"><span data-stu-id="e29ed-128">Startup</span></span>

<span data-ttu-id="e29ed-129">El método `UseStartup` de `WebHostBuilder` especifica la clase `Startup` para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="e29ed-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="e29ed-130">La clase `Startup` es donde se configuran los servicios necesarios para la aplicación y donde se define la canalización de control de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="e29ed-130">The `Startup` class is where any services required by the app are configured and the request handling pipeline is defined.</span></span> <span data-ttu-id="e29ed-131">La clase `Startup` debe ser pública y normalmente contiene los siguientes métodos.</span><span class="sxs-lookup"><span data-stu-id="e29ed-131">The `Startup` class must be public and usually contains the following methods.</span></span> <span data-ttu-id="e29ed-132">`Startup.ConfigureServices` es opcional.</span><span class="sxs-lookup"><span data-stu-id="e29ed-132">`Startup.ConfigureServices` is optional.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="e29ed-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> define los [servicios](#dependency-injection-services) que usa la aplicación (por ejemplo, ASP.NET Core MVC, Entity Framework Core, Identity).</span><span class="sxs-lookup"><span data-stu-id="e29ed-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="e29ed-134"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> define el [software intermedio](xref:fundamentals/middleware/index) al que se llama en la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="e29ed-134"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="e29ed-135">Para obtener más información, vea <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-135">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="e29ed-136">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="e29ed-136">Content root</span></span>

<span data-ttu-id="e29ed-137">La raíz del contenido es la ruta de acceso base a cualquier contenido que usa la aplicación, como [Razor Pages](xref:razor-pages/index), las vistas de MVC y los recursos estáticos.</span><span class="sxs-lookup"><span data-stu-id="e29ed-137">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="e29ed-138">De forma predeterminada, la raíz del contenido es la misma ubicación que la ruta de acceso base de la aplicación para el archivo ejecutable que hospeda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e29ed-138">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="e29ed-139">Raíz web (webroot)</span><span class="sxs-lookup"><span data-stu-id="e29ed-139">Web root (webroot)</span></span>

<span data-ttu-id="e29ed-140">La raíz web de una aplicación es el directorio del proyecto que contiene recursos públicos y estáticos, como archivos de imagen, CSS y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e29ed-140">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="e29ed-141">De forma predeterminada, la raíz web es *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="e29ed-141">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="e29ed-142">Para los archivos de Razor (*.cshtml*), la virgulilla `~/` apunta a la raíz web.</span><span class="sxs-lookup"><span data-stu-id="e29ed-142">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="e29ed-143">Las rutas de acceso que empiezan por `~/` se conocen como rutas de acceso virtuales.</span><span class="sxs-lookup"><span data-stu-id="e29ed-143">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="e29ed-144">Inserción de dependencias (servicios)</span><span class="sxs-lookup"><span data-stu-id="e29ed-144">Dependency injection (services)</span></span>

<span data-ttu-id="e29ed-145">Un *servicio* es un componente que está pensado para su uso común en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="e29ed-145">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="e29ed-146">Los servicios se ponen a disposición a través de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e29ed-146">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e29ed-147">ASP.NET Core incluye un contenedor de inversión del control (IoC) nativo que admite la [inserción de constructores](xref:mvc/controllers/dependency-injection#constructor-injection) de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="e29ed-147">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="e29ed-148">Si lo prefiere, puede reemplazar el contenedor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="e29ed-148">You can replace the default container if you wish.</span></span> <span data-ttu-id="e29ed-149">Además de la [ventaja de acoplamiento flexible](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI hace que los servicios estén disponibles en toda la aplicación, por ejemplo, el [registro](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="e29ed-149">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="e29ed-150">Para obtener más información, vea <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-150">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="e29ed-151">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="e29ed-151">Middleware</span></span>

<span data-ttu-id="e29ed-152">En ASP.NET Core, se crea la canalización de solicitudes mediante [software intermedio](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="e29ed-152">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="e29ed-153">El software intermedio de ASP.NET Core lleva a cabo las operaciones asincrónicas en un `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="e29ed-153">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="e29ed-154">Normalmente, se agrega un componente de software intermedio denominado "XYZ" a la canalización al invocar a un método de extensión `UseXYZ` en el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="e29ed-154">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="e29ed-155">ASP.NET Core incluye una gran variedad de software intermedio integrado. Además, puede escribir su propio software intermedio personalizado.</span><span class="sxs-lookup"><span data-stu-id="e29ed-155">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="e29ed-156">La [interfaz web abierta para .NET (OWIN)](xref:fundamentals/owin), que permite desacoplar las aplicaciones web de servidores web, es compatible con las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e29ed-156">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="e29ed-157">Para obtener más información, vea <xref:fundamentals/middleware/index> y <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-157">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="e29ed-158">Inicio de solicitudes HTTP</span><span class="sxs-lookup"><span data-stu-id="e29ed-158">Initiate HTTP requests</span></span>

<span data-ttu-id="e29ed-159"><xref:System.Net.Http.IHttpClientFactory> está disponible para acceder a instancias de <xref:System.Net.Http.HttpClient> con el fin de realizar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="e29ed-159"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="e29ed-160">Para obtener más información, vea <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-160">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="e29ed-161">Entornos</span><span class="sxs-lookup"><span data-stu-id="e29ed-161">Environments</span></span>

<span data-ttu-id="e29ed-162">Los entornos, como *Desarrollo* y *Producción*, son un concepto de primera clase en ASP.NET Core y se pueden establecer mediante una variable de entorno, un archivo de configuración o un argumento de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="e29ed-162">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="e29ed-163">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-163">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="e29ed-164">Hospedaje</span><span class="sxs-lookup"><span data-stu-id="e29ed-164">Hosting</span></span>

<span data-ttu-id="e29ed-165">Las aplicaciones ASP.NET Core configuran e inician un *host*. Dicho host es responsable de la administración de inicio y duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e29ed-165">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="e29ed-166">Para obtener más información, vea <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-166">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="e29ed-167">Servidores</span><span class="sxs-lookup"><span data-stu-id="e29ed-167">Servers</span></span>

<span data-ttu-id="e29ed-168">El modelo de hospedaje de ASP.NET Core no escucha directamente las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="e29ed-168">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="e29ed-169">El modelo de hospedaje se basa en una implementación de servidor HTTP para reenviar la solicitud a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e29ed-169">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="e29ed-170">Windows</span><span class="sxs-lookup"><span data-stu-id="e29ed-170">Windows</span></span>](#tab/windows)

<span data-ttu-id="e29ed-171">ASP.NET Core proporciona las siguientes implementaciones de servidor:</span><span class="sxs-lookup"><span data-stu-id="e29ed-171">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="e29ed-172">El servidor [Kestrel](xref:fundamentals/servers/kestrel) es un servidor web multiplataforma administrado.</span><span class="sxs-lookup"><span data-stu-id="e29ed-172">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="e29ed-173">Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="e29ed-173">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="e29ed-174">Kestrel también puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="e29ed-174">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="e29ed-175">El servidor HTTP de IIS (`IISHttpServer`) es un [servidor en proceso](xref:fundamentals/servers/index#in-process-hosting-model) de IIS.</span><span class="sxs-lookup"><span data-stu-id="e29ed-175">IIS HTTP Server (`IISHttpServer`) is an [in-process server](xref:fundamentals/servers/index#in-process-hosting-model) for IIS.</span></span>
* <span data-ttu-id="e29ed-176">El servidor [HTTP.sys](xref:fundamentals/servers/httpsys) es un servidor web para ASP.NET Core en Windows.</span><span class="sxs-lookup"><span data-stu-id="e29ed-176">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="e29ed-177">macOS</span><span class="sxs-lookup"><span data-stu-id="e29ed-177">macOS</span></span>](#tab/macos)

<span data-ttu-id="e29ed-178">ASP.NET Core usa la implementación del servidor [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e29ed-178">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="e29ed-179">Kestrel es un servidor web multiplataforma administrado.</span><span class="sxs-lookup"><span data-stu-id="e29ed-179">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="e29ed-180">Kestrel también puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="e29ed-180">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="e29ed-181">Linux</span><span class="sxs-lookup"><span data-stu-id="e29ed-181">Linux</span></span>](#tab/linux)

<span data-ttu-id="e29ed-182">ASP.NET Core usa la implementación del servidor [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e29ed-182">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="e29ed-183">Kestrel es un servidor web multiplataforma administrado.</span><span class="sxs-lookup"><span data-stu-id="e29ed-183">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="e29ed-184">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="e29ed-184">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="e29ed-185">Kestrel también puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="e29ed-185">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="e29ed-186">Windows</span><span class="sxs-lookup"><span data-stu-id="e29ed-186">Windows</span></span>](#tab/windows)

<span data-ttu-id="e29ed-187">ASP.NET Core proporciona las siguientes implementaciones de servidor:</span><span class="sxs-lookup"><span data-stu-id="e29ed-187">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="e29ed-188">El servidor [Kestrel](xref:fundamentals/servers/kestrel) es un servidor web multiplataforma administrado.</span><span class="sxs-lookup"><span data-stu-id="e29ed-188">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="e29ed-189">Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="e29ed-189">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="e29ed-190">Kestrel también puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="e29ed-190">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="e29ed-191">El servidor [HTTP.sys](xref:fundamentals/servers/httpsys) es un servidor web para ASP.NET Core en Windows.</span><span class="sxs-lookup"><span data-stu-id="e29ed-191">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="e29ed-192">macOS</span><span class="sxs-lookup"><span data-stu-id="e29ed-192">macOS</span></span>](#tab/macos)

<span data-ttu-id="e29ed-193">ASP.NET Core usa la implementación del servidor [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e29ed-193">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="e29ed-194">Kestrel es un servidor web multiplataforma administrado.</span><span class="sxs-lookup"><span data-stu-id="e29ed-194">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="e29ed-195">Kestrel también puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="e29ed-195">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="e29ed-196">Linux</span><span class="sxs-lookup"><span data-stu-id="e29ed-196">Linux</span></span>](#tab/linux)

<span data-ttu-id="e29ed-197">ASP.NET Core usa la implementación del servidor [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="e29ed-197">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="e29ed-198">Kestrel es un servidor web multiplataforma administrado.</span><span class="sxs-lookup"><span data-stu-id="e29ed-198">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="e29ed-199">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="e29ed-199">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="e29ed-200">Kestrel también puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="e29ed-200">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

<span data-ttu-id="e29ed-201">Para obtener más información, vea <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-201">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="e29ed-202">Configuración</span><span class="sxs-lookup"><span data-stu-id="e29ed-202">Configuration</span></span>

<span data-ttu-id="e29ed-203">ASP.NET Core usa un modelo de configuración basado en pares de nombre-valor.</span><span class="sxs-lookup"><span data-stu-id="e29ed-203">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="e29ed-204">El modelo de configuración no se basa en <xref:System.Configuration> o *web.config*. La configuración obtiene valores de un conjunto ordenado de proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="e29ed-204">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="e29ed-205">Los proveedores de configuración integrados admiten una gran variedad de formatos de archivo (XML, JSON, INI), variables de entorno y argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="e29ed-205">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="e29ed-206">También puede escribir sus propios proveedores de configuración personalizados.</span><span class="sxs-lookup"><span data-stu-id="e29ed-206">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="e29ed-207">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-207">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="e29ed-208">Registro</span><span class="sxs-lookup"><span data-stu-id="e29ed-208">Logging</span></span>

<span data-ttu-id="e29ed-209">ASP.NET Core es compatible con una API de registro que funciona con una gran variedad de proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="e29ed-209">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="e29ed-210">Los proveedores integrados admiten el envío de registros a uno o varios destinos.</span><span class="sxs-lookup"><span data-stu-id="e29ed-210">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="e29ed-211">Se pueden usar plataformas de registro de terceros.</span><span class="sxs-lookup"><span data-stu-id="e29ed-211">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="e29ed-212">Para obtener más información, vea <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-212">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="e29ed-213">Control de errores</span><span class="sxs-lookup"><span data-stu-id="e29ed-213">Error handling</span></span>

<span data-ttu-id="e29ed-214">ASP.NET Core tiene escenarios integrados para controlar los errores en las aplicaciones, incluidas una página de excepciones de desarrollador, páginas de errores personalizados, páginas de códigos de estado estáticos y control de excepciones de inicio.</span><span class="sxs-lookup"><span data-stu-id="e29ed-214">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="e29ed-215">Para obtener más información, vea <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-215">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="e29ed-216">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="e29ed-216">Routing</span></span>

<span data-ttu-id="e29ed-217">ASP.NET Core ofrece casos para el enrutamiento de solicitudes de aplicación a los controladores de ruta.</span><span class="sxs-lookup"><span data-stu-id="e29ed-217">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="e29ed-218">Para obtener más información, vea <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-218">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="e29ed-219">Tareas en segundo plano</span><span class="sxs-lookup"><span data-stu-id="e29ed-219">Background tasks</span></span>

<span data-ttu-id="e29ed-220">Las tareas en segundo plano se implementan como *servicios hospedados*.</span><span class="sxs-lookup"><span data-stu-id="e29ed-220">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="e29ed-221">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-221">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="e29ed-222">Para obtener más información, vea <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-222">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="e29ed-223">Acceso a HttpContext</span><span class="sxs-lookup"><span data-stu-id="e29ed-223">Access HttpContext</span></span>

<span data-ttu-id="e29ed-224">`HttpContext` está disponible automáticamente al procesar las solicitudes con Razor Pages y MVC.</span><span class="sxs-lookup"><span data-stu-id="e29ed-224">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="e29ed-225">En los casos en los que `HttpContext` no esté disponible, podrá acceder a `HttpContext` a través de la interfaz de <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> y su implementación predeterminada, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-225">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="e29ed-226">Para obtener más información, vea <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="e29ed-226">For more information, see <xref:fundamentals/httpcontext>.</span></span>
