---
title: Conceptos básicos de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre los conceptos básicos para crear aplicaciones de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/index
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="2e11d-103">Conceptos básicos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e11d-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="2e11d-104">Este artículo es una introducción de los temas clave para entender cómo desarrollar aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2e11d-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="2e11d-105">Clase Startup</span><span class="sxs-lookup"><span data-stu-id="2e11d-105">The Startup class</span></span>

<span data-ttu-id="2e11d-106">La clase `Startup` es donde:</span><span class="sxs-lookup"><span data-stu-id="2e11d-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="2e11d-107">Se configuran los servicios requeridos por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2e11d-107">Any services required by the app are configured.</span></span>
* <span data-ttu-id="2e11d-108">Se define la solicitud de canalización.</span><span class="sxs-lookup"><span data-stu-id="2e11d-108">The request handling pipeline is defined.</span></span>

* <span data-ttu-id="2e11d-109">Se agrega al método `Startup.ConfigureServices` el código para configurar (o *registrar*) servicios.</span><span class="sxs-lookup"><span data-stu-id="2e11d-109">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="2e11d-110">Los *servicios* son componentes que usan la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2e11d-110">*Services* are components that are used by the app.</span></span> <span data-ttu-id="2e11d-111">Por ejemplo, un objeto de contexto de Entity Framework Core es un servicio.</span><span class="sxs-lookup"><span data-stu-id="2e11d-111">For example, an Entity Framework Core context object is a service.</span></span>
* <span data-ttu-id="2e11d-112">Se agrega al método `Startup.Configure` el código para configurar la canalización de control de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="2e11d-112">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span> <span data-ttu-id="2e11d-113">La canalización se compone de una serie de componentes de *software intermedio*.</span><span class="sxs-lookup"><span data-stu-id="2e11d-113">The pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="2e11d-114">Por ejemplo, un software intermedio podría controlar las solicitudes de archivos estáticos o redirigir las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2e11d-114">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="2e11d-115">Cada software intermedio lleva a cabo las operaciones asincrónicas en un contexto `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="2e11d-115">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2e11d-116">Aquí tiene una clase `Startup` de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2e11d-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

::: moniker-end

<span data-ttu-id="2e11d-117">Para obtener más información, vea [Inicio de la aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="2e11d-117">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="2e11d-118">Inserción de dependencias (servicios)</span><span class="sxs-lookup"><span data-stu-id="2e11d-118">Dependency injection (services)</span></span>

<span data-ttu-id="2e11d-119">ASP.NET Core tiene un marco de inserción de dependencias (DI) integrado que pone a disposición los servicios configurados para las clases de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="2e11d-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="2e11d-120">Una manera de obtener una instancia de un servicio en una clase es crear un constructor con un parámetro del tipo necesario.</span><span class="sxs-lookup"><span data-stu-id="2e11d-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="2e11d-121">El parámetro puede ser el tipo de servicio o una interfaz.</span><span class="sxs-lookup"><span data-stu-id="2e11d-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="2e11d-122">El sistema de DI proporciona el servicio en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="2e11d-122">The DI system provides the service at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2e11d-123">Aquí tiene una clase que usa DI para obtener un objeto de contexto de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="2e11d-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="2e11d-124">La línea resaltada es un ejemplo de inserción de constructor:</span><span class="sxs-lookup"><span data-stu-id="2e11d-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

::: moniker-end

<span data-ttu-id="2e11d-125">Aunque DI está integrada, está diseñada para permitirle conectar un contenedor de inversión de control (IoC) de terceros si lo prefiere.</span><span class="sxs-lookup"><span data-stu-id="2e11d-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="2e11d-126">Para obtener más información, consulte [Inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2e11d-126">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="2e11d-127">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="2e11d-127">Middleware</span></span>

<span data-ttu-id="2e11d-128">La canalización de control de solicitudes se compone de una serie de componentes de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="2e11d-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="2e11d-129">Cada componente lleva a cabo las operaciones asincrónicas en un contexto `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="2e11d-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="2e11d-130">Normalmente, se agrega un componente de software intermedio a la canalización al invocar su método de extensión `Use...` en el método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="2e11d-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="2e11d-131">Por ejemplo, para habilitar la representación de los archivos estáticos, llame a `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="2e11d-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2e11d-132">El código resaltado en el ejemplo siguiente configura la canalización de control de solicitudes:</span><span class="sxs-lookup"><span data-stu-id="2e11d-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

::: moniker-end

<span data-ttu-id="2e11d-133">ASP.NET Core incluye una gran variedad de software intermedio integrado. Además, puede escribir software intermedio personalizado.</span><span class="sxs-lookup"><span data-stu-id="2e11d-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="2e11d-134">Para obtener más información vea [Software intermedio](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="2e11d-134">For more information, see [Middleware](xref:fundamentals/middleware/index).</span></span>

<a id="host"/>

## <a name="the-host"></a><span data-ttu-id="2e11d-135">El host</span><span class="sxs-lookup"><span data-stu-id="2e11d-135">The host</span></span>

<span data-ttu-id="2e11d-136">Una aplicación de ASP.NET Core compila un *host* durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="2e11d-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="2e11d-137">El host es un objeto que encapsula todos los recursos de la aplicación, como:</span><span class="sxs-lookup"><span data-stu-id="2e11d-137">The host is an object that encapsulates all of the the app's resources, such as:</span></span>

* <span data-ttu-id="2e11d-138">Una implementación de servidor de HTTP</span><span class="sxs-lookup"><span data-stu-id="2e11d-138">An HTTP server implementation</span></span>
* <span data-ttu-id="2e11d-139">Componentes de software intermedio</span><span class="sxs-lookup"><span data-stu-id="2e11d-139">Middleware components</span></span>
* <span data-ttu-id="2e11d-140">Registro</span><span class="sxs-lookup"><span data-stu-id="2e11d-140">Logging</span></span>
* <span data-ttu-id="2e11d-141">DI</span><span class="sxs-lookup"><span data-stu-id="2e11d-141">DI</span></span>
* <span data-ttu-id="2e11d-142">Configuración</span><span class="sxs-lookup"><span data-stu-id="2e11d-142">Configuration</span></span>

<span data-ttu-id="2e11d-143">La razón principal para incluir todos los recursos interdependientes de la aplicación en un objeto es la administración de la duración: el control sobre el inicio de la aplicación y el apagado estable.</span><span class="sxs-lookup"><span data-stu-id="2e11d-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="2e11d-144">El código para crear un host se encuentra en `Program.Main` y sigue el [patrón de generador](https://wikipedia.org/wiki/Builder_pattern).</span><span class="sxs-lookup"><span data-stu-id="2e11d-144">The code to create a host is in `Program.Main` and follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern).</span></span> <span data-ttu-id="2e11d-145">Se llama a los métodos para configurar cada recurso que forma parte del host.</span><span class="sxs-lookup"><span data-stu-id="2e11d-145">Methods are called to configure each resource that is part of the host.</span></span> <span data-ttu-id="2e11d-146">Se llama a un método de generador para agrupar y crear una instancia del objeto host.</span><span class="sxs-lookup"><span data-stu-id="2e11d-146">A builder method is called to pull it all together and instantiate the host object.</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="2e11d-147">ASP.NET Core 2.x usa el host web (la clase `WebHost`) para las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="2e11d-147">ASP.NET Core 2.x uses Web Host (the `WebHost` class) for web apps.</span></span> <span data-ttu-id="2e11d-148">El marco proporciona métodos de extensión `CreateDefaultBuilder` que configuran un host con opciones de uso común, como las siguientes:</span><span class="sxs-lookup"><span data-stu-id="2e11d-148">The framework provides `CreateDefaultBuilder` extension methods that set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="2e11d-149">Use [Kestrel](#servers) como servidor web y habilite la integración de IIS.</span><span class="sxs-lookup"><span data-stu-id="2e11d-149">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="2e11d-150">Cargue la configuración de *appsettings.json*, las variables de entorno, los argumentos de línea de comandos y otros orígenes.</span><span class="sxs-lookup"><span data-stu-id="2e11d-150">Load configuration from *appsettings.json*, environment variables, command line arguments, and other sources.</span></span>
* <span data-ttu-id="2e11d-151">Envíe la salida de registro a la consola y los proveedores de depuración.</span><span class="sxs-lookup"><span data-stu-id="2e11d-151">Send logging output to the console and debug providers.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="2e11d-152">Aquí tiene un código de ejemplo que crea un host:</span><span class="sxs-lookup"><span data-stu-id="2e11d-152">Here's sample code that builds a host:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

<span data-ttu-id="2e11d-153">Para obtener más información, vea [Host web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="2e11d-153">For more information, see [Web Host](xref:fundamentals/host/web-host).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="2e11d-154">En ASP.NET Core 3.0, en una aplicación web se puede usar el host web (clase `WebHost`) o el host genérico (clase `Host`).</span><span class="sxs-lookup"><span data-stu-id="2e11d-154">In ASP.NET Core 3.0, Web Host (`WebHost` class) or Generic Host (`Host` class) can be used in a web app.</span></span> <span data-ttu-id="2e11d-155">Se recomienda el host genérico, mientras que el host web está disponible para compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="2e11d-155">Generic Host is recommended, and Web Host is available for backwards compatibility.</span></span>

<span data-ttu-id="2e11d-156">El marco proporciona métodos de extensión `CreateDefaultBuilder` y `ConfigureWebHostDefaults` que configuran un host con opciones de uso común, como las siguientes:</span><span class="sxs-lookup"><span data-stu-id="2e11d-156">The framework provides `CreateDefaultBuilder` and `ConfigureWebHostDefaults` extension methods that set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="2e11d-157">Use [Kestrel](#servers) como servidor web y habilite la integración de IIS.</span><span class="sxs-lookup"><span data-stu-id="2e11d-157">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="2e11d-158">Cargue la configuración de *appsettings.json*, *appsettings.[EnvironmentName].json*, las variables de entorno y los argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="2e11d-158">Load configuration from *appsettings.json*, *appsettings.[EnvironmentName].json*, environment variables, and command line arguments.</span></span>
* <span data-ttu-id="2e11d-159">Envíe la salida de registro a la consola y los proveedores de depuración.</span><span class="sxs-lookup"><span data-stu-id="2e11d-159">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="2e11d-160">Aquí tiene un código de ejemplo que crea un host.</span><span class="sxs-lookup"><span data-stu-id="2e11d-160">Here's sample code that builds a host.</span></span> <span data-ttu-id="2e11d-161">Se resaltan los métodos de extensión que configuran el host con las opciones usadas habitualmente.</span><span class="sxs-lookup"><span data-stu-id="2e11d-161">The extension methods that set up the host with commonly used options are highlighted.</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

<span data-ttu-id="2e11d-162">Para obtener más información, vea [Host genérico](xref:fundamentals/host/generic-host) y [Host web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="2e11d-162">For more information, see [Generic Host](xref:fundamentals/host/generic-host) and [Web Host](xref:fundamentals/host/web-host)</span></span>

::: moniker-end

### <a name="advanced-host-scenarios"></a><span data-ttu-id="2e11d-163">Escenarios de host avanzados</span><span class="sxs-lookup"><span data-stu-id="2e11d-163">Advanced host scenarios</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="2e11d-164">El host web está diseñado para incluir una implementación de servidor HTTP, que no es necesaria para otros tipos de aplicaciones. NET.</span><span class="sxs-lookup"><span data-stu-id="2e11d-164">Web Host is designed to include an HTTP server implementation, which isn't needed for other kinds of .NET apps.</span></span> <span data-ttu-id="2e11d-165">A partir de la versión 2.1, el host genérico (clase `Host`) está disponible para que lo use cualquier aplicación de .NET Core, no solo las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2e11d-165">Starting in 2.1, Generic Host (`Host` class) is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="2e11d-166">El host genérico permite usar funciones transversales como el registro, la inserción de dependencias, la configuración y la administración de la duración en otros tipos de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="2e11d-166">Generic Host lets you use cross-cutting features such as logging, DI, configuration, and app lifetime management in other types of apps.</span></span> <span data-ttu-id="2e11d-167">Para obtener más información, vea [Host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="2e11d-167">For more information, see [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="2e11d-168">El host genérico está disponible para que lo use cualquier aplicación de .NET Core, no solo las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2e11d-168">Generic Host is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="2e11d-169">El host genérico permite usar funciones transversales como el registro, la inserción de dependencias, la configuración y la administración de la duración en otros tipos de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="2e11d-169">Generic Host lets you use cross-cutting features such as logging, DI, configuration, and app lifetime management in other types of apps.</span></span> <span data-ttu-id="2e11d-170">Para obtener más información, vea [Host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="2e11d-170">For more information, see [Generic Host](xref:fundamentals/host/generic-host).</span></span>

::: moniker-end

<span data-ttu-id="2e11d-171">También puede usar el host para ejecutar tareas en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="2e11d-171">You can also use the host to run background tasks.</span></span> <span data-ttu-id="2e11d-172">Para obtener más información, vea [Tareas en segundo plano](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="2e11d-172">For more information, see [Background tasks](xref:fundamentals/host/hosted-services).</span></span>

## <a name="servers"></a><span data-ttu-id="2e11d-173">Servidores</span><span class="sxs-lookup"><span data-stu-id="2e11d-173">Servers</span></span>

<span data-ttu-id="2e11d-174">Una aplicación ASP.NET Core usa una implementación de servidor HTTP para escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="2e11d-174">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="2e11d-175">El servidor expone las solicitudes a la aplicación como un conjunto de [características de solicitud](xref:fundamentals/request-features) integradas en un contexto `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="2e11d-175">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="2e11d-176">Windows</span><span class="sxs-lookup"><span data-stu-id="2e11d-176">Windows</span></span>](#tab/windows)

<span data-ttu-id="2e11d-177">ASP.NET Core proporciona las siguientes implementaciones de servidor:</span><span class="sxs-lookup"><span data-stu-id="2e11d-177">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="2e11d-178">*Kestrel* es un servidor web multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="2e11d-178">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="2e11d-179">Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="2e11d-179">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="2e11d-180">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="2e11d-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="2e11d-181">Un *servidor HTTP de IIS* es un servidor para Windows que usa IIS.</span><span class="sxs-lookup"><span data-stu-id="2e11d-181">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="2e11d-182">Con este servidor, la aplicación de ASP.NET Core e IIS se ejecutan en el mismo proceso.</span><span class="sxs-lookup"><span data-stu-id="2e11d-182">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="2e11d-183">*HTTP.sys* es un servidor de Windows que no se usa con IIS.</span><span class="sxs-lookup"><span data-stu-id="2e11d-183">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="2e11d-184">macOS</span><span class="sxs-lookup"><span data-stu-id="2e11d-184">macOS</span></span>](#tab/macos)

<span data-ttu-id="2e11d-185">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="2e11d-185">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="2e11d-186">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="2e11d-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="2e11d-187">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="2e11d-187">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="2e11d-188">Linux</span><span class="sxs-lookup"><span data-stu-id="2e11d-188">Linux</span></span>](#tab/linux)

<span data-ttu-id="2e11d-189">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="2e11d-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="2e11d-190">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="2e11d-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="2e11d-191">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="2e11d-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="2e11d-192">Windows</span><span class="sxs-lookup"><span data-stu-id="2e11d-192">Windows</span></span>](#tab/windows)

<span data-ttu-id="2e11d-193">ASP.NET Core proporciona las siguientes implementaciones de servidor:</span><span class="sxs-lookup"><span data-stu-id="2e11d-193">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="2e11d-194">*Kestrel* es un servidor web multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="2e11d-194">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="2e11d-195">Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="2e11d-195">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="2e11d-196">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="2e11d-196">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="2e11d-197">*HTTP.sys* es un servidor de Windows que no se usa con IIS.</span><span class="sxs-lookup"><span data-stu-id="2e11d-197">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="2e11d-198">macOS</span><span class="sxs-lookup"><span data-stu-id="2e11d-198">macOS</span></span>](#tab/macos)

<span data-ttu-id="2e11d-199">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="2e11d-199">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="2e11d-200">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="2e11d-200">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="2e11d-201">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="2e11d-201">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="2e11d-202">Linux</span><span class="sxs-lookup"><span data-stu-id="2e11d-202">Linux</span></span>](#tab/linux)

<span data-ttu-id="2e11d-203">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="2e11d-203">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="2e11d-204">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="2e11d-204">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="2e11d-205">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="2e11d-205">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="2e11d-206">Para obtener más información, vea [Servidores](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="2e11d-206">For more information, see [Servers](xref:fundamentals/servers/index).</span></span>

## <a name="configuration"></a><span data-ttu-id="2e11d-207">Configuración</span><span class="sxs-lookup"><span data-stu-id="2e11d-207">Configuration</span></span>

<span data-ttu-id="2e11d-208">ASP.NET Core proporciona un marco de configuración que obtiene la configuración como pares nombre/valor de un conjunto ordenado de proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="2e11d-208">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="2e11d-209">Hay proveedores de configuración integrados para una gran variedad de orígenes, como archivos *.json* y *.xml*, variables de entorno y argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="2e11d-209">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="2e11d-210">También puede escribir proveedores de configuración personalizados.</span><span class="sxs-lookup"><span data-stu-id="2e11d-210">You can also write custom configuration providers.</span></span>

<span data-ttu-id="2e11d-211">Por ejemplo, puede especificar que la configuración procede de *appsettings.json* y las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="2e11d-211">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="2e11d-212">Después, cuando se solicite el valor de *ConnectionString*, el marco buscará primero en el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2e11d-212">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="2e11d-213">Si se encuentra el valor allí, pero también en una variable de entorno, el valor de la variable de entorno tendría prioridad.</span><span class="sxs-lookup"><span data-stu-id="2e11d-213">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="2e11d-214">Para administrar los datos de configuración confidencial como las contraseñas, ASP.NET Core proporciona una [herramienta de administrador secreto](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="2e11d-214">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="2e11d-215">Para los secretos de producción, se recomienda [Azure Key Vault](/aspnet/core/security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="2e11d-215">For production secrets, we recommend [Azure Key Vault](/aspnet/core/security/key-vault-configuration).</span></span>

<span data-ttu-id="2e11d-216">Para obtener más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="2e11d-216">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="options"></a><span data-ttu-id="2e11d-217">Opciones</span><span class="sxs-lookup"><span data-stu-id="2e11d-217">Options</span></span>

<span data-ttu-id="2e11d-218">Siempre que sea posible, ASP.NET Core sigue el *patrón de opciones* para almacenar y recuperar los valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="2e11d-218">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="2e11d-219">El patrón de opciones usa clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="2e11d-219">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="2e11d-220">Por ejemplo, el código siguiente define las opciones de WebSockets:</span><span class="sxs-lookup"><span data-stu-id="2e11d-220">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="2e11d-221">Para obtener más información, vea [Opciones](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="2e11d-221">For more information, see [Options](xref:fundamentals/configuration/options).</span></span>

## <a name="environments"></a><span data-ttu-id="2e11d-222">Entornos</span><span class="sxs-lookup"><span data-stu-id="2e11d-222">Environments</span></span>

<span data-ttu-id="2e11d-223">Los entornos de ejecución, como *desarrollo*, *almacenamiento provisional* y *Producción*, son un concepto de primera clase en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2e11d-223">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="2e11d-224">Puede especificar el entorno que ejecuta una aplicación estableciendo la variable de entorno `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="2e11d-224">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="2e11d-225">ASP.NET Core lee dicha variable de entorno al inicio de la aplicación y almacena el valor en una implementación `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="2e11d-225">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="2e11d-226">El objeto de entorno está disponible en cualquier parte de la aplicación a través de DI.</span><span class="sxs-lookup"><span data-stu-id="2e11d-226">The environment object is available anywhere in the app via DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2e11d-227">El siguiente ejemplo de código desde la clase `Startup` configura la aplicación para que proporcione información detallada del error solo cuando se ejecuta en el desarrollo:</span><span class="sxs-lookup"><span data-stu-id="2e11d-227">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

::: moniker-end

<span data-ttu-id="2e11d-228">Para obtener más información, vea [Entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="2e11d-228">For more information, see [Environments](xref:fundamentals/environments).</span></span>

## <a name="logging"></a><span data-ttu-id="2e11d-229">Registro</span><span class="sxs-lookup"><span data-stu-id="2e11d-229">Logging</span></span>

<span data-ttu-id="2e11d-230">ASP.NET Core es compatible con una API de registro que funciona con una gran variedad de proveedores de registro integrados y de terceros.</span><span class="sxs-lookup"><span data-stu-id="2e11d-230">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="2e11d-231">Entre los proveedores disponibles se incluyen los siguientes:</span><span class="sxs-lookup"><span data-stu-id="2e11d-231">Available providers include the following:</span></span>

* <span data-ttu-id="2e11d-232">Consola</span><span class="sxs-lookup"><span data-stu-id="2e11d-232">Console</span></span>
* <span data-ttu-id="2e11d-233">Depuración</span><span class="sxs-lookup"><span data-stu-id="2e11d-233">Debug</span></span>
* <span data-ttu-id="2e11d-234">Seguimiento de eventos en Windows</span><span class="sxs-lookup"><span data-stu-id="2e11d-234">Event Tracing on Windows</span></span>
* <span data-ttu-id="2e11d-235">Registro de errores de Windows</span><span class="sxs-lookup"><span data-stu-id="2e11d-235">Windows Event Log</span></span>
* <span data-ttu-id="2e11d-236">TraceSource</span><span class="sxs-lookup"><span data-stu-id="2e11d-236">TraceSource</span></span>
* <span data-ttu-id="2e11d-237">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2e11d-237">Azure App Service</span></span>
* <span data-ttu-id="2e11d-238">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="2e11d-238">Azure Application Insights</span></span>

<span data-ttu-id="2e11d-239">Escribir registros desde cualquier lugar en el código de una aplicación mediante la obtención de un objeto `ILogger` de DI y llamar a métodos de registro.</span><span class="sxs-lookup"><span data-stu-id="2e11d-239">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2e11d-240">Aquí tiene un código de ejemplo que usa un objeto `ILogger`, con la inserción del constructor y resaltadas las llamadas del método de registro.</span><span class="sxs-lookup"><span data-stu-id="2e11d-240">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

::: moniker-end

<span data-ttu-id="2e11d-241">La interfaz `ILogger` le permite pasar cualquier número de campos para el proveedor de registro.</span><span class="sxs-lookup"><span data-stu-id="2e11d-241">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="2e11d-242">Los campos habitualmente se usan para construir una cadena de mensaje, pero el proveedor también puede enviarlos como campos independientes o almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="2e11d-242">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="2e11d-243">Esta característica permite a los proveedores de registro implementar el [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="2e11d-243">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="2e11d-244">Para obtener más información, vea [Registro](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="2e11d-244">For more information, see [Logging](xref:fundamentals/logging/index).</span></span>

## <a name="routing"></a><span data-ttu-id="2e11d-245">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="2e11d-245">Routing</span></span>

<span data-ttu-id="2e11d-246">Una *ruta* es un patrón de dirección URL que se asigna a un controlador.</span><span class="sxs-lookup"><span data-stu-id="2e11d-246">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="2e11d-247">El controlador normalmente es una página de Razor, un método de acción en un controlador MVC o un software intermedio.</span><span class="sxs-lookup"><span data-stu-id="2e11d-247">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="2e11d-248">El enrutamiento de ASP.NET Core le permite controlar las direcciones URL usadas por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2e11d-248">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="2e11d-249">Para más información, vea [Enrutamiento](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="2e11d-249">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="error-handling"></a><span data-ttu-id="2e11d-250">Control de errores</span><span class="sxs-lookup"><span data-stu-id="2e11d-250">Error handling</span></span>

<span data-ttu-id="2e11d-251">ASP.NET Core tiene características integradas para controlar los errores, tales como:</span><span class="sxs-lookup"><span data-stu-id="2e11d-251">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="2e11d-252">Una página de excepciones para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="2e11d-252">A developer exception page</span></span>
* <span data-ttu-id="2e11d-253">Páginas de errores personalizados</span><span class="sxs-lookup"><span data-stu-id="2e11d-253">Custom error pages</span></span>
* <span data-ttu-id="2e11d-254">Páginas de códigos de estado estáticos</span><span class="sxs-lookup"><span data-stu-id="2e11d-254">Static status code pages</span></span>
* <span data-ttu-id="2e11d-255">Control de excepciones de inicio</span><span class="sxs-lookup"><span data-stu-id="2e11d-255">Startup exception handling</span></span>

<span data-ttu-id="2e11d-256">Para obtener más información, vea [Control de excepciones](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="2e11d-256">For more information, see [Error handling](xref:fundamentals/error-handling).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="make-http-requests"></a><span data-ttu-id="2e11d-257">Realización de solicitudes HTTP</span><span class="sxs-lookup"><span data-stu-id="2e11d-257">Make HTTP requests</span></span>

<span data-ttu-id="2e11d-258">Una implementación de `IHttpClientFactory` está disponible para crear instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="2e11d-258">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="2e11d-259">Este servicio:</span><span class="sxs-lookup"><span data-stu-id="2e11d-259">The factory:</span></span>

* <span data-ttu-id="2e11d-260">Proporciona una ubicación central para denominar y configurar instancias de `HttpClient` lógicas.</span><span class="sxs-lookup"><span data-stu-id="2e11d-260">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="2e11d-261">Así, por ejemplo, se puede registrar y configurar un cliente *github* para tener acceso a GitHub.</span><span class="sxs-lookup"><span data-stu-id="2e11d-261">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="2e11d-262">y, de igual modo, registrar otro cliente predeterminado para otros fines.</span><span class="sxs-lookup"><span data-stu-id="2e11d-262">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="2e11d-263">Admite el registro y encadenamiento de varios controladores de delegación para crear una canalización de middleware de solicitud saliente.</span><span class="sxs-lookup"><span data-stu-id="2e11d-263">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="2e11d-264">Este patrón es similar a la canalización de middleware de entrada de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2e11d-264">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="2e11d-265">Dicho patrón proporciona un mecanismo para administrar cuestiones transversales relativas a las solicitudes HTTP, como el almacenamiento en caché, el control de errores, la serialización y el registro.</span><span class="sxs-lookup"><span data-stu-id="2e11d-265">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="2e11d-266">Se integra con *Polly*, una conocida biblioteca de terceros para el control de errores transitorios.</span><span class="sxs-lookup"><span data-stu-id="2e11d-266">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="2e11d-267">Administra la agrupación y duración de las instancias de `HttpClientMessageHandler` subyacentes para evitar los problemas de DNS que suelen producirse al administrar las duraciones de `HttpClient` manualmente.</span><span class="sxs-lookup"><span data-stu-id="2e11d-267">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="2e11d-268">Agrega una experiencia de registro configurable (a través de *ILogger*) en todas las solicitudes enviadas a través de los clientes creados por Factory.</span><span class="sxs-lookup"><span data-stu-id="2e11d-268">Adds a configurable logging experience (via *ILogger*) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="2e11d-269">Para obtener más información, vea [Realización de solicitudes HTTP](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="2e11d-269">For more information, see [Make HTTP requests](xref:fundamentals/http-requests).</span></span>

::: moniker-end

## <a name="content-root"></a><span data-ttu-id="2e11d-270">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="2e11d-270">Content root</span></span>

<span data-ttu-id="2e11d-271">La raíz del contenido es la ruta de acceso base a cualquier contenido privado que usa la aplicación, como sus archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="2e11d-271">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="2e11d-272">De forma predeterminada, la raíz del contenido es la ruta de acceso base para el archivo ejecutable que hospeda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2e11d-272">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="2e11d-273">Se puede especificar una ubicación alternativa al [crear el host](#host).</span><span class="sxs-lookup"><span data-stu-id="2e11d-273">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="2e11d-274">Para obtener más información, vea [Raíz del contenido](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="2e11d-274">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="2e11d-275">Para obtener más información, vea [Raíz del contenido](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="2e11d-275">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="2e11d-276">Raíz web</span><span class="sxs-lookup"><span data-stu-id="2e11d-276">Web root</span></span>

<span data-ttu-id="2e11d-277">La raíz web (también conocida como *webroot*) es la ruta de acceso base a los recursos públicos y estáticos, como archivos de imágenes, CSS y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2e11d-277">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="2e11d-278">De forma predeterminada, el software intermedio de archivos estáticos solo ofrecerá archivos desde el directorio raíz web (y subdirectorios).</span><span class="sxs-lookup"><span data-stu-id="2e11d-278">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="2e11d-279">El valor predeterminado de la ruta de acceso web es *\<raíz del contenido>/wwwroot*, pero se puede especificar una ubicación diferente al [crear el host](#host).</span><span class="sxs-lookup"><span data-stu-id="2e11d-279">The web root path defaults to *\<content root>/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="2e11d-280">En los archivos de Razor (*.cshtml*), la virgulilla `~/` apunta a la raíz web.</span><span class="sxs-lookup"><span data-stu-id="2e11d-280">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="2e11d-281">Las rutas de acceso que empiezan por `~/` se conocen como rutas de acceso virtuales.</span><span class="sxs-lookup"><span data-stu-id="2e11d-281">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="2e11d-282">Para obtener más información, consulte [Archivos estáticos](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="2e11d-282">For more information, see [Static files](xref:fundamentals/static-files).</span></span>
