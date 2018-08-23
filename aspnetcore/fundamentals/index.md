---
title: Conceptos básicos de ASP.NET Core
author: rick-anderson
description: Descubra los conceptos básicos para crear aplicaciones de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2018
uid: fundamentals/index
ms.openlocfilehash: 68760f179c4d6e806510b727e2284f8c2c4a4ff6
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "41746093"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="04efd-103">Conceptos básicos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04efd-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="04efd-104">Una aplicación de ASP.NET Core es una aplicación de consola que crea un servidor web en su método `Main`:</span><span class="sxs-lookup"><span data-stu-id="04efd-104">An ASP.NET Core app is a console app that creates a web server in its `Main` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="04efd-105">El método `Main` invoca [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), que sigue el [patrón del generador](https://wikipedia.org/wiki/Builder_pattern) para crear un host de web.</span><span class="sxs-lookup"><span data-stu-id="04efd-105">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="04efd-106">El generador tiene métodos que definen el servidor web (por ejemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) y la clase de inicio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="04efd-106">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="04efd-107">En el ejemplo anterior, se asigna automáticamente el servidor web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="04efd-107">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="04efd-108">El host web de ASP.NET Core intenta ejecutarse en IIS, si está disponible.</span><span class="sxs-lookup"><span data-stu-id="04efd-108">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="04efd-109">Otros servidores web, como [HTTP.sys](xref:fundamentals/servers/httpsys), se pueden usar al invocar el método de extensión adecuado.</span><span class="sxs-lookup"><span data-stu-id="04efd-109">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="04efd-110">`UseStartup` se explica en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="04efd-110">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="04efd-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, el tipo de valor devuelto de la invocación `WebHost.CreateDefaultBuilder`, proporciona muchos métodos opcionales.</span><span class="sxs-lookup"><span data-stu-id="04efd-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="04efd-112">Algunos de estos métodos incluyen `UseHttpSys` para hospedar la aplicación en HTTP.sys y <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> para especificar el directorio de contenido raíz.</span><span class="sxs-lookup"><span data-stu-id="04efd-112">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="04efd-113">Los métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> y <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> crean el objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda la aplicación y empieza a escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="04efd-113">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="04efd-114">El método `Main` usa <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, que sigue el [patrón del generador](https://wikipedia.org/wiki/Builder_pattern) para crear un host de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="04efd-114">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="04efd-115">El generador tiene métodos que definen el servidor web (por ejemplo, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) y la clase de inicio (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span><span class="sxs-lookup"><span data-stu-id="04efd-115">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="04efd-116">En el ejemplo anterior, se usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="04efd-116">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="04efd-117">Si se invoca el método de extensión adecuado, se pueden usar otros servidores web, como [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="04efd-117">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="04efd-118">`UseStartup` se explica en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="04efd-118">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="04efd-119">`WebHostBuilder` proporciona muchos métodos opcionales, incluido <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> para hospedar en IIS e IIS Express, y <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> para especificar el directorio de contenido raíz.</span><span class="sxs-lookup"><span data-stu-id="04efd-119">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="04efd-120">Los métodos <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> y <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> crean el objeto <xref:Microsoft.AspNetCore.Hosting.IWebHost> que hospeda la aplicación y empieza a escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="04efd-120">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="04efd-121">Inicio</span><span class="sxs-lookup"><span data-stu-id="04efd-121">Startup</span></span>

<span data-ttu-id="04efd-122">El método `UseStartup` de `WebHostBuilder` especifica la clase `Startup` para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="04efd-122">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="04efd-123">La clase `Startup` es donde se define la canalización de control de solicitudes y donde se configuran los servicios necesarios para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="04efd-123">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="04efd-124">La clase `Startup` debe ser pública y contener los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="04efd-124">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="04efd-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> define los [servicios](#dependency-injection-services) que usa la aplicación (por ejemplo, ASP.NET Core MVC, Entity Framework Core, Identity).</span><span class="sxs-lookup"><span data-stu-id="04efd-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="04efd-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> define el [software intermedio](xref:fundamentals/middleware/index) al que se llama en la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="04efd-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="04efd-127">Para obtener más información, vea <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="04efd-127">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="04efd-128">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="04efd-128">Content root</span></span>

<span data-ttu-id="04efd-129">La raíz del contenido es la ruta de acceso base a cualquier contenido que usa la aplicación, como [Razor Pages](xref:razor-pages/index), las vistas de MVC y los recursos estáticos.</span><span class="sxs-lookup"><span data-stu-id="04efd-129">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="04efd-130">De forma predeterminada, la raíz del contenido es la misma ubicación que la ruta de acceso base de la aplicación para el archivo ejecutable que hospeda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="04efd-130">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="04efd-131">Raíz web</span><span class="sxs-lookup"><span data-stu-id="04efd-131">Web root</span></span>

<span data-ttu-id="04efd-132">La raíz web de una aplicación es el directorio del proyecto que contiene recursos públicos y estáticos, como archivos de imagen, CSS y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="04efd-132">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="04efd-133">Inserción de dependencias (servicios)</span><span class="sxs-lookup"><span data-stu-id="04efd-133">Dependency injection (services)</span></span>

<span data-ttu-id="04efd-134">Un *servicio* es un componente que está pensado para su uso común en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="04efd-134">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="04efd-135">Los servicios se ponen a disposición a través de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="04efd-135">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="04efd-136">ASP.NET Core incluye un contenedor de inversión del control (IoC) nativo que admite la [inserción de constructores](xref:mvc/controllers/dependency-injection#constructor-injection) de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="04efd-136">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="04efd-137">Si lo prefiere, puede reemplazar el contenedor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="04efd-137">You can replace the default container if you wish.</span></span> <span data-ttu-id="04efd-138">Además de la [ventaja de acoplamiento flexible](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI hace que los servicios estén disponibles en toda la aplicación, por ejemplo, el [registro](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="04efd-138">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="04efd-139">Para obtener más información, vea <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="04efd-139">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="04efd-140">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="04efd-140">Middleware</span></span>

<span data-ttu-id="04efd-141">En ASP.NET Core, se crea la canalización de solicitudes mediante [software intermedio](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="04efd-141">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="04efd-142">El software intermedio de ASP.NET Core lleva a cabo las operaciones asincrónicas en un `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="04efd-142">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="04efd-143">Normalmente, se agrega un componente de software intermedio denominado "XYZ" a la canalización al invocar a un método de extensión `UseXYZ` en el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="04efd-143">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="04efd-144">ASP.NET Core incluye una gran variedad de software intermedio integrado. Además, puede escribir su propio software intermedio personalizado.</span><span class="sxs-lookup"><span data-stu-id="04efd-144">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="04efd-145">La [interfaz web abierta para .NET (OWIN)](xref:fundamentals/owin), que permite desacoplar las aplicaciones web de servidores web, es compatible con las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="04efd-145">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="04efd-146">Para obtener más información, consulte <xref:fundamentals/middleware/index> y <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="04efd-146">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="04efd-147">Inicio de solicitudes HTTP</span><span class="sxs-lookup"><span data-stu-id="04efd-147">Initiate HTTP requests</span></span>

<span data-ttu-id="04efd-148"><xref:System.Net.Http.IHttpClientFactory> está disponible para acceder a instancias de <xref:System.Net.Http.HttpClient> con el fin de realizar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="04efd-148"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="04efd-149">Para obtener más información, vea <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="04efd-149">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="04efd-150">Entornos</span><span class="sxs-lookup"><span data-stu-id="04efd-150">Environments</span></span>

<span data-ttu-id="04efd-151">Los entornos, como *Desarrollo* y *Producción*, son un concepto de primera clase en ASP.NET Core y se pueden establecer mediante una variable de entorno, un archivo de configuración o un argumento de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="04efd-151">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="04efd-152">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="04efd-152">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="04efd-153">Hospedaje</span><span class="sxs-lookup"><span data-stu-id="04efd-153">Hosting</span></span>

<span data-ttu-id="04efd-154">Las aplicaciones ASP.NET Core configuran e inician un *host*. Dicho host es responsable de la administración de inicio y duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="04efd-154">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="04efd-155">Para obtener más información, vea <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="04efd-155">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="04efd-156">Servidores</span><span class="sxs-lookup"><span data-stu-id="04efd-156">Servers</span></span>

<span data-ttu-id="04efd-157">El modelo de hospedaje de ASP.NET Core no escucha directamente las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="04efd-157">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="04efd-158">El modelo de hospedaje se basa en una implementación de servidor HTTP para reenviar la solicitud a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="04efd-158">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="04efd-159">La solicitud reenviada se empaqueta como un conjunto de objetos de característica al que se puede tener acceso a través de interfaces.</span><span class="sxs-lookup"><span data-stu-id="04efd-159">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="04efd-160">ASP.NET Core incluye un servidor web administrado multiplataforma, denominado [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="04efd-160">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="04efd-161">Kestrel suele ejecutarse tras un servidor web de producción, como [IIS](https://www.iis.net/) o [Nginx](http://nginx.org), en una configuración proxy invertida.</span><span class="sxs-lookup"><span data-stu-id="04efd-161">Kestrel is commonly run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org) in a reverse proxy configuration.</span></span> <span data-ttu-id="04efd-162">Kestrel también puede ejecutarse como servidor perimetral expuesto directamente a Internet en ASP.NET Core 2.0 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="04efd-162">Kestrel can also be run as an edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="04efd-163">Para obtener más información, vea <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="04efd-163">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="04efd-164">Configuración</span><span class="sxs-lookup"><span data-stu-id="04efd-164">Configuration</span></span>

<span data-ttu-id="04efd-165">ASP.NET Core usa un modelo de configuración basado en pares de nombre-valor.</span><span class="sxs-lookup"><span data-stu-id="04efd-165">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="04efd-166">El modelo de configuración no se basa en <xref:System.Configuration> o *web.config*. La configuración obtiene valores de un conjunto ordenado de proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="04efd-166">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="04efd-167">Los proveedores de configuración integrados admiten una gran variedad de formatos de archivo (XML, JSON, INI), variables de entorno y argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="04efd-167">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="04efd-168">También puede escribir sus propios proveedores de configuración personalizados.</span><span class="sxs-lookup"><span data-stu-id="04efd-168">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="04efd-169">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="04efd-169">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="04efd-170">Registro</span><span class="sxs-lookup"><span data-stu-id="04efd-170">Logging</span></span>

<span data-ttu-id="04efd-171">ASP.NET Core es compatible con una API de registro que funciona con una gran variedad de proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="04efd-171">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="04efd-172">Los proveedores integrados admiten el envío de registros a uno o varios destinos.</span><span class="sxs-lookup"><span data-stu-id="04efd-172">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="04efd-173">Se pueden usar plataformas de registro de terceros.</span><span class="sxs-lookup"><span data-stu-id="04efd-173">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="04efd-174">Para obtener más información, vea <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="04efd-174">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="04efd-175">Control de errores</span><span class="sxs-lookup"><span data-stu-id="04efd-175">Error handling</span></span>

<span data-ttu-id="04efd-176">ASP.NET Core tiene escenarios integrados para controlar los errores en las aplicaciones, incluidas una página de excepciones de desarrollador, páginas de errores personalizados, páginas de códigos de estado estáticos y control de excepciones de inicio.</span><span class="sxs-lookup"><span data-stu-id="04efd-176">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="04efd-177">Para obtener más información, vea <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="04efd-177">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="04efd-178">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="04efd-178">Routing</span></span>

<span data-ttu-id="04efd-179">ASP.NET Core ofrece casos para el enrutamiento de solicitudes de aplicación a los controladores de ruta.</span><span class="sxs-lookup"><span data-stu-id="04efd-179">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="04efd-180">Para obtener más información, vea <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="04efd-180">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="file-providers"></a><span data-ttu-id="04efd-181">Proveedores de archivos</span><span class="sxs-lookup"><span data-stu-id="04efd-181">File Providers</span></span>

<span data-ttu-id="04efd-182">ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos, lo que ofrece una interfaz común para trabajar con archivos entre plataformas.</span><span class="sxs-lookup"><span data-stu-id="04efd-182">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="04efd-183">Para obtener más información, vea <xref:fundamentals/file-providers>.</span><span class="sxs-lookup"><span data-stu-id="04efd-183">For more information, see <xref:fundamentals/file-providers>.</span></span>

## <a name="static-files"></a><span data-ttu-id="04efd-184">Archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="04efd-184">Static files</span></span>

<span data-ttu-id="04efd-185">El software intermedio de archivos estáticos trabaja con archivos estáticos, por ejemplo, HTML, CSS, imágenes y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="04efd-185">Static Files Middleware serves static files, such as HTML, CSS, image, and JavaScript files.</span></span>

<span data-ttu-id="04efd-186">Para obtener más información, vea <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="04efd-186">For more information, see <xref:fundamentals/static-files>.</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="04efd-187">Estado de sesión y aplicación</span><span class="sxs-lookup"><span data-stu-id="04efd-187">Session and app state</span></span>

<span data-ttu-id="04efd-188">ASP.NET Core ofrece varios enfoques para conservar el estado de sesión y aplicación mientras el usuario examina una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="04efd-188">ASP.NET Core offers several approaches to preserve session and app state while a user browses a web app.</span></span>

<span data-ttu-id="04efd-189">Para obtener más información, vea <xref:fundamentals/app-state>.</span><span class="sxs-lookup"><span data-stu-id="04efd-189">For more information, see <xref:fundamentals/app-state>.</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="04efd-190">Globalización y localización</span><span class="sxs-lookup"><span data-stu-id="04efd-190">Globalization and localization</span></span>

<span data-ttu-id="04efd-191">El hecho de crear un sitio web multilingüe con ASP.NET Core permite que este llegue a un público más amplio.</span><span class="sxs-lookup"><span data-stu-id="04efd-191">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="04efd-192">Además, proporciona servicios y software intermedio para la localización de contenido en diferentes idiomas y referencias culturales.</span><span class="sxs-lookup"><span data-stu-id="04efd-192">ASP.NET Core provides services and middleware for localizing content into different languages and cultures.</span></span>

<span data-ttu-id="04efd-193">Para obtener más información, vea <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="04efd-193">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="request-features"></a><span data-ttu-id="04efd-194">Características de la solicitud</span><span class="sxs-lookup"><span data-stu-id="04efd-194">Request features</span></span>

<span data-ttu-id="04efd-195">Los detalles de implementación del servidor web relacionados con las solicitudes HTTP y las respuestas se definen en las interfaces.</span><span class="sxs-lookup"><span data-stu-id="04efd-195">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="04efd-196">En las implementaciones del servidor web y el software intermedio se usan estas interfaces para crear y modificar la canalización de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="04efd-196">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="04efd-197">Para obtener más información, vea <xref:fundamentals/request-features>.</span><span class="sxs-lookup"><span data-stu-id="04efd-197">For more information, see <xref:fundamentals/request-features>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="04efd-198">Tareas en segundo plano</span><span class="sxs-lookup"><span data-stu-id="04efd-198">Background tasks</span></span>

<span data-ttu-id="04efd-199">Las tareas en segundo plano se implementan como *servicios hospedados*.</span><span class="sxs-lookup"><span data-stu-id="04efd-199">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="04efd-200">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="04efd-200">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="04efd-201">Para obtener más información, vea <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="04efd-201">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="04efd-202">Acceso a HttpContext</span><span class="sxs-lookup"><span data-stu-id="04efd-202">Access HttpContext</span></span>

<span data-ttu-id="04efd-203">`HttpContext` está disponible automáticamente al procesar las solicitudes con Razor Pages y MVC.</span><span class="sxs-lookup"><span data-stu-id="04efd-203">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="04efd-204">En los casos en los que `HttpContext` no esté disponible, podrá acceder a `HttpContext` a través de la interfaz de <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> y su implementación predeterminada, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span><span class="sxs-lookup"><span data-stu-id="04efd-204">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="04efd-205">Para obtener más información, vea <xref:fundamentals/httpcontext>.</span><span class="sxs-lookup"><span data-stu-id="04efd-205">For more information, see <xref:fundamentals/httpcontext>.</span></span>

## <a name="websockets"></a><span data-ttu-id="04efd-206">WebSockets</span><span class="sxs-lookup"><span data-stu-id="04efd-206">WebSockets</span></span>

<span data-ttu-id="04efd-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) es un protocolo que habilita canales de comunicación bidireccional persistentes a través de conexiones TCP.</span><span class="sxs-lookup"><span data-stu-id="04efd-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="04efd-208">Se usa para aplicaciones de chat, tableros de cotizaciones, juegos y donde se necesite funcionalidad en tiempo real en una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="04efd-208">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="04efd-209">ASP.NET Core es compatible con casos de socket web.</span><span class="sxs-lookup"><span data-stu-id="04efd-209">ASP.NET Core supports web socket scenarios.</span></span>

<span data-ttu-id="04efd-210">Para obtener más información, vea <xref:fundamentals/websockets>.</span><span class="sxs-lookup"><span data-stu-id="04efd-210">For more information, see <xref:fundamentals/websockets>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="04efd-211">Metapaquete Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="04efd-211">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="04efd-212">El metapaquete [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) simplifica la administración de metapaquetes.</span><span class="sxs-lookup"><span data-stu-id="04efd-212">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span>

<span data-ttu-id="04efd-213">Para obtener más información, vea <xref:fundamentals/metapackage-app>.</span><span class="sxs-lookup"><span data-stu-id="04efd-213">For more information, see <xref:fundamentals/metapackage-app>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="04efd-214">Metapaquete Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="04efd-214">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="04efd-215">El metapaquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="04efd-215">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="04efd-216">Todos los paquetes admitidos por el equipo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="04efd-216">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="04efd-217">Todos los paquetes admitidos por Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="04efd-217">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="04efd-218">Dependencias internas y de terceros usadas por ASP.NET Core y Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="04efd-218">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="04efd-219">Para obtener más información, vea <xref:fundamentals/metapackage>.</span><span class="sxs-lookup"><span data-stu-id="04efd-219">For more information, see <xref:fundamentals/metapackage>.</span></span>

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="04efd-220">Entorno de ejecución de .NET Core frente a .NET Framework</span><span class="sxs-lookup"><span data-stu-id="04efd-220">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="04efd-221">Una aplicación de ASP.NET Core puede tener como destino el entorno de ejecución de .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="04efd-221">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="04efd-222">Para más información, vea [Selección entre .NET Core y .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="04efd-222">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="04efd-223">Elección entre ASP.NET Core y ASP.NET</span><span class="sxs-lookup"><span data-stu-id="04efd-223">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="04efd-224">Para obtener más información sobre cómo elegir entre ASP.NET Core y ASP.NET, vea <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span><span class="sxs-lookup"><span data-stu-id="04efd-224">For more information on choosing between ASP.NET Core and ASP.NET, see <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span></span>
