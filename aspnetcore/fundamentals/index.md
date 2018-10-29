---
title: Conceptos básicos de ASP.NET Core
author: rick-anderson
description: Descubra los conceptos básicos para crear aplicaciones de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/index
ms.openlocfilehash: ab140051648c1640b3c4f382bfd8201c5c0c2039
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207477"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="c0e17-103">Conceptos básicos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c0e17-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="c0e17-104">Una aplicación de ASP.NET Core es una aplicación de consola que crea un servidor web en su método `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="c0e17-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="c0e17-105">El método `Main` es el *punto de entrada administrado* de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="c0e17-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="c0e17-106">Host de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="c0e17-106">The .NET Core Host:</span></span>

* <span data-ttu-id="c0e17-107">Carga el [entorno de ejecución de .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="c0e17-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="c0e17-108">Usa el primer argumento de la línea de comandos como la ruta de acceso al binario administrado que contiene el punto de entrada (`Main`) e inicia la ejecución del código.</span><span class="sxs-lookup"><span data-stu-id="c0e17-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="c0e17-109">El método `Main` invoca [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), que sigue el [patrón del generador](https://wikipedia.org/wiki/Builder_pattern) para crear un host de web.</span><span class="sxs-lookup"><span data-stu-id="c0e17-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="c0e17-110">El generador tiene métodos que definen el servidor web (por ejemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) y la clase de inicio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="c0e17-110">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="c0e17-111">En el ejemplo anterior, se asigna automáticamente el servidor web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="c0e17-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="c0e17-112">El host web de ASP.NET Core intenta ejecutarse en IIS, si está disponible.</span><span class="sxs-lookup"><span data-stu-id="c0e17-112">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="c0e17-113">Otros servidores web, como [HTTP.sys](xref:fundamentals/servers/httpsys), se pueden usar al invocar el método de extensión adecuado.</span><span class="sxs-lookup"><span data-stu-id="c0e17-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="c0e17-114">`UseStartup` se explica en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="c0e17-114">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="c0e17-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, el tipo de valor devuelto de la invocación `WebHost.CreateDefaultBuilder`, proporciona muchos métodos opcionales.</span><span class="sxs-lookup"><span data-stu-id="c0e17-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="c0e17-116">Algunos de estos métodos incluyen `UseHttpSys` para hospedar la aplicación en HTTP.sys y <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> para especificar el directorio de contenido raíz.</span><span class="sxs-lookup"><span data-stu-id="c0e17-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="c0e17-117">Los métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> y <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> crean el objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda la aplicación y empieza a escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0e17-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="c0e17-118">Host de .NET Core:</span><span class="sxs-lookup"><span data-stu-id="c0e17-118">The .NET Core Host:</span></span>

* <span data-ttu-id="c0e17-119">Carga el [entorno de ejecución de .NET Core](https://github.com/dotnet/coreclr).</span><span class="sxs-lookup"><span data-stu-id="c0e17-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="c0e17-120">Usa el primer argumento de la línea de comandos como la ruta de acceso al binario administrado que contiene el punto de entrada (`Main`) e inicia la ejecución del código.</span><span class="sxs-lookup"><span data-stu-id="c0e17-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="c0e17-121">El método `Main` usa <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, que sigue el [patrón del generador](https://wikipedia.org/wiki/Builder_pattern) para crear un host de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="c0e17-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="c0e17-122">El generador tiene métodos que definen el servidor web (por ejemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) y la clase de inicio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="c0e17-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="c0e17-123">En el ejemplo anterior, se usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="c0e17-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="c0e17-124">Si se invoca el método de extensión adecuado, se pueden usar otros servidores web, como [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="c0e17-124">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="c0e17-125">`UseStartup` se explica en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="c0e17-125">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="c0e17-126">`WebHostBuilder` proporciona muchos métodos opcionales, incluido <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> para hospedar en IIS e IIS Express, y <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> para especificar el directorio de contenido raíz.</span><span class="sxs-lookup"><span data-stu-id="c0e17-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="c0e17-127">Los métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> y <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> crean el objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda la aplicación y empieza a escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0e17-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="c0e17-128">Inicio</span><span class="sxs-lookup"><span data-stu-id="c0e17-128">Startup</span></span>

<span data-ttu-id="c0e17-129">El método `UseStartup` de `WebHostBuilder` especifica la clase `Startup` para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="c0e17-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="c0e17-130">La clase `Startup` es donde se define la canalización de control de solicitudes y donde se configuran los servicios necesarios para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c0e17-130">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="c0e17-131">La clase `Startup` debe ser pública y contener los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="c0e17-131">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="c0e17-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> define los [servicios](#dependency-injection-services) que usa la aplicación (por ejemplo, ASP.NET Core MVC, Entity Framework Core, Identity).</span><span class="sxs-lookup"><span data-stu-id="c0e17-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="c0e17-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> define el [software intermedio](xref:fundamentals/middleware/index) al que se llama en la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="c0e17-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="c0e17-134">Para obtener más información, vea <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-134">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="c0e17-135">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="c0e17-135">Content root</span></span>

<span data-ttu-id="c0e17-136">La raíz del contenido es la ruta de acceso base a cualquier contenido que usa la aplicación, como [Razor Pages](xref:razor-pages/index), las vistas de MVC y los recursos estáticos.</span><span class="sxs-lookup"><span data-stu-id="c0e17-136">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="c0e17-137">De forma predeterminada, la raíz del contenido es la misma ubicación que la ruta de acceso base de la aplicación para el archivo ejecutable que hospeda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c0e17-137">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="c0e17-138">Raíz web (webroot)</span><span class="sxs-lookup"><span data-stu-id="c0e17-138">Web root (webroot)</span></span>

<span data-ttu-id="c0e17-139">La raíz web de una aplicación es el directorio del proyecto que contiene recursos públicos y estáticos, como archivos de imagen, CSS y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c0e17-139">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="c0e17-140">De forma predeterminada, la raíz web es *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="c0e17-140">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="c0e17-141">Para los archivos de Razor (*.cshtml*), la virgulilla `~/` apunta a la raíz web.</span><span class="sxs-lookup"><span data-stu-id="c0e17-141">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="c0e17-142">Las rutas de acceso que empiezan por `~/` se conocen como rutas de acceso virtuales.</span><span class="sxs-lookup"><span data-stu-id="c0e17-142">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="c0e17-143">Inserción de dependencias (servicios)</span><span class="sxs-lookup"><span data-stu-id="c0e17-143">Dependency injection (services)</span></span>

<span data-ttu-id="c0e17-144">Un *servicio* es un componente que está pensado para su uso común en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="c0e17-144">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="c0e17-145">Los servicios se ponen a disposición a través de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c0e17-145">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c0e17-146">ASP.NET Core incluye un contenedor de inversión del control (IoC) nativo que admite la [inserción de constructores](xref:mvc/controllers/dependency-injection#constructor-injection) de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c0e17-146">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="c0e17-147">Si lo prefiere, puede reemplazar el contenedor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="c0e17-147">You can replace the default container if you wish.</span></span> <span data-ttu-id="c0e17-148">Además de la [ventaja de acoplamiento flexible](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI hace que los servicios estén disponibles en toda la aplicación, por ejemplo, el [registro](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="c0e17-148">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="c0e17-149">Para obtener más información, vea <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-149">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="c0e17-150">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="c0e17-150">Middleware</span></span>

<span data-ttu-id="c0e17-151">En ASP.NET Core, se crea la canalización de solicitudes mediante [software intermedio](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="c0e17-151">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="c0e17-152">El software intermedio de ASP.NET Core lleva a cabo las operaciones asincrónicas en un `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="c0e17-152">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="c0e17-153">Normalmente, se agrega un componente de software intermedio denominado "XYZ" a la canalización al invocar a un método de extensión `UseXYZ` en el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="c0e17-153">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="c0e17-154">ASP.NET Core incluye una gran variedad de software intermedio integrado. Además, puede escribir su propio software intermedio personalizado.</span><span class="sxs-lookup"><span data-stu-id="c0e17-154">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="c0e17-155">La [interfaz web abierta para .NET (OWIN)](xref:fundamentals/owin), que permite desacoplar las aplicaciones web de servidores web, es compatible con las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c0e17-155">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="c0e17-156">Para obtener más información, vea <xref:fundamentals/middleware/index> y <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-156">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="c0e17-157">Inicio de solicitudes HTTP</span><span class="sxs-lookup"><span data-stu-id="c0e17-157">Initiate HTTP requests</span></span>

<span data-ttu-id="c0e17-158"><xref:System.Net.Http.IHttpClientFactory> está disponible para acceder a instancias de <xref:System.Net.Http.HttpClient> con el fin de realizar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0e17-158"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="c0e17-159">Para obtener más información, vea <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-159">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="c0e17-160">Entornos</span><span class="sxs-lookup"><span data-stu-id="c0e17-160">Environments</span></span>

<span data-ttu-id="c0e17-161">Los entornos, como *Desarrollo* y *Producción*, son un concepto de primera clase en ASP.NET Core y se pueden establecer mediante una variable de entorno, un archivo de configuración o un argumento de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="c0e17-161">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="c0e17-162">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-162">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="c0e17-163">Hospedaje</span><span class="sxs-lookup"><span data-stu-id="c0e17-163">Hosting</span></span>

<span data-ttu-id="c0e17-164">Las aplicaciones ASP.NET Core configuran e inician un *host*. Dicho host es responsable de la administración de inicio y duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c0e17-164">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="c0e17-165">Para obtener más información, vea <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-165">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="c0e17-166">Servidores</span><span class="sxs-lookup"><span data-stu-id="c0e17-166">Servers</span></span>

<span data-ttu-id="c0e17-167">El modelo de hospedaje de ASP.NET Core no escucha directamente las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="c0e17-167">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="c0e17-168">El modelo de hospedaje se basa en una implementación de servidor HTTP para reenviar la solicitud a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c0e17-168">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="c0e17-169">La solicitud reenviada se empaqueta como un conjunto de objetos de característica al que se puede tener acceso a través de interfaces.</span><span class="sxs-lookup"><span data-stu-id="c0e17-169">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="c0e17-170">ASP.NET Core incluye un servidor web administrado multiplataforma, denominado [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="c0e17-170">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="c0e17-171">Kestrel suele ejecutarse tras un servidor web de producción, como [IIS](https://www.iis.net/) o [Nginx](http://nginx.org), en una configuración proxy invertida.</span><span class="sxs-lookup"><span data-stu-id="c0e17-171">Kestrel is commonly run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org) in a reverse proxy configuration.</span></span> <span data-ttu-id="c0e17-172">Kestrel también puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="c0e17-172">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="c0e17-173">Para obtener más información, vea <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-173">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="c0e17-174">Configuración</span><span class="sxs-lookup"><span data-stu-id="c0e17-174">Configuration</span></span>

<span data-ttu-id="c0e17-175">ASP.NET Core usa un modelo de configuración basado en pares de nombre-valor.</span><span class="sxs-lookup"><span data-stu-id="c0e17-175">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="c0e17-176">El modelo de configuración no se basa en <xref:System.Configuration> o *web.config*. La configuración obtiene valores de un conjunto ordenado de proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="c0e17-176">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="c0e17-177">Los proveedores de configuración integrados admiten una gran variedad de formatos de archivo (XML, JSON, INI), variables de entorno y argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="c0e17-177">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="c0e17-178">También puede escribir sus propios proveedores de configuración personalizados.</span><span class="sxs-lookup"><span data-stu-id="c0e17-178">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="c0e17-179">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-179">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="c0e17-180">Registro</span><span class="sxs-lookup"><span data-stu-id="c0e17-180">Logging</span></span>

<span data-ttu-id="c0e17-181">ASP.NET Core es compatible con una API de registro que funciona con una gran variedad de proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="c0e17-181">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="c0e17-182">Los proveedores integrados admiten el envío de registros a uno o varios destinos.</span><span class="sxs-lookup"><span data-stu-id="c0e17-182">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="c0e17-183">Se pueden usar plataformas de registro de terceros.</span><span class="sxs-lookup"><span data-stu-id="c0e17-183">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="c0e17-184">Para obtener más información, vea <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-184">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="c0e17-185">Control de errores</span><span class="sxs-lookup"><span data-stu-id="c0e17-185">Error handling</span></span>

<span data-ttu-id="c0e17-186">ASP.NET Core tiene escenarios integrados para controlar los errores en las aplicaciones, incluidas una página de excepciones de desarrollador, páginas de errores personalizados, páginas de códigos de estado estáticos y control de excepciones de inicio.</span><span class="sxs-lookup"><span data-stu-id="c0e17-186">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="c0e17-187">Para obtener más información, vea <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-187">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="c0e17-188">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="c0e17-188">Routing</span></span>

<span data-ttu-id="c0e17-189">ASP.NET Core ofrece casos para el enrutamiento de solicitudes de aplicación a los controladores de ruta.</span><span class="sxs-lookup"><span data-stu-id="c0e17-189">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="c0e17-190">Para obtener más información, vea <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-190">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="c0e17-191">Tareas en segundo plano</span><span class="sxs-lookup"><span data-stu-id="c0e17-191">Background tasks</span></span>

<span data-ttu-id="c0e17-192">Las tareas en segundo plano se implementan como *servicios hospedados*.</span><span class="sxs-lookup"><span data-stu-id="c0e17-192">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="c0e17-193">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-193">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="c0e17-194">Para obtener más información, vea <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-194">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="c0e17-195">Acceso a HttpContext</span><span class="sxs-lookup"><span data-stu-id="c0e17-195">Access HttpContext</span></span>

<span data-ttu-id="c0e17-196">`HttpContext` está disponible automáticamente al procesar las solicitudes con Razor Pages y MVC.</span><span class="sxs-lookup"><span data-stu-id="c0e17-196">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="c0e17-197">En los casos en los que `HttpContext` no esté disponible, podrá acceder a `HttpContext` a través de la interfaz de <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> y su implementación predeterminada, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-197">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="c0e17-198">Para obtener más información, vea <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="c0e17-198">For more information, see <xref:fundamentals/httpcontext>.</span></span>
