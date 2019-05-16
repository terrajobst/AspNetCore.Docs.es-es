---
title: Conceptos básicos de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre los conceptos básicos para crear aplicaciones de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: fundamentals/index
ms.openlocfilehash: 9c7bc25d813ad17825ef03f5176882993cc2dd63
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610328"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="5900f-103">Conceptos básicos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5900f-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="5900f-104">Este artículo es una introducción de los temas clave para entender cómo desarrollar aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5900f-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="5900f-105">Clase Startup</span><span class="sxs-lookup"><span data-stu-id="5900f-105">The Startup class</span></span>

<span data-ttu-id="5900f-106">La clase `Startup` es donde:</span><span class="sxs-lookup"><span data-stu-id="5900f-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="5900f-107">Se configuran los servicios requeridos por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5900f-107">Any services required by the app are configured.</span></span>
* <span data-ttu-id="5900f-108">Se define la solicitud de canalización.</span><span class="sxs-lookup"><span data-stu-id="5900f-108">The request handling pipeline is defined.</span></span>

* <span data-ttu-id="5900f-109">Se agrega al método `Startup.ConfigureServices` el código para configurar (o *registrar*) servicios.</span><span class="sxs-lookup"><span data-stu-id="5900f-109">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="5900f-110">Los *servicios* son componentes que usan la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5900f-110">*Services* are components that are used by the app.</span></span> <span data-ttu-id="5900f-111">Por ejemplo, un objeto de contexto de Entity Framework Core es un servicio.</span><span class="sxs-lookup"><span data-stu-id="5900f-111">For example, an Entity Framework Core context object is a service.</span></span>
* <span data-ttu-id="5900f-112">Se agrega al método `Startup.Configure` el código para configurar la canalización de control de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="5900f-112">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span> <span data-ttu-id="5900f-113">La canalización se compone de una serie de componentes de *software intermedio*.</span><span class="sxs-lookup"><span data-stu-id="5900f-113">The pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="5900f-114">Por ejemplo, un software intermedio podría controlar las solicitudes de archivos estáticos o redirigir las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5900f-114">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="5900f-115">Cada software intermedio lleva a cabo las operaciones asincrónicas en un contexto `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="5900f-115">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="5900f-116">Aquí tiene una clase `Startup` de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5900f-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="5900f-117">Para obtener más información, vea <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="5900f-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="5900f-118">Inserción de dependencias (servicios)</span><span class="sxs-lookup"><span data-stu-id="5900f-118">Dependency injection (services)</span></span>

<span data-ttu-id="5900f-119">ASP.NET Core tiene un marco de inserción de dependencias (DI) integrado que pone a disposición los servicios configurados para las clases de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="5900f-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="5900f-120">Una manera de obtener una instancia de un servicio en una clase es crear un constructor con un parámetro del tipo necesario.</span><span class="sxs-lookup"><span data-stu-id="5900f-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="5900f-121">El parámetro puede ser el tipo de servicio o una interfaz.</span><span class="sxs-lookup"><span data-stu-id="5900f-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="5900f-122">El sistema de DI proporciona el servicio en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="5900f-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="5900f-123">Aquí tiene una clase que usa DI para obtener un objeto de contexto de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="5900f-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="5900f-124">La línea resaltada es un ejemplo de inserción de constructor:</span><span class="sxs-lookup"><span data-stu-id="5900f-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="5900f-125">Aunque DI está integrada, está diseñada para permitirle conectar un contenedor de inversión de control (IoC) de terceros si lo prefiere.</span><span class="sxs-lookup"><span data-stu-id="5900f-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="5900f-126">Para obtener más información, vea <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="5900f-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="5900f-127">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="5900f-127">Middleware</span></span>

<span data-ttu-id="5900f-128">La canalización de control de solicitudes se compone de una serie de componentes de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="5900f-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="5900f-129">Cada componente lleva a cabo las operaciones asincrónicas en un contexto `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="5900f-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="5900f-130">Normalmente, se agrega un componente de software intermedio a la canalización al invocar su método de extensión `Use...` en el método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="5900f-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="5900f-131">Por ejemplo, para habilitar la representación de los archivos estáticos, llame a `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="5900f-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="5900f-132">El código resaltado en el ejemplo siguiente configura la canalización de control de solicitudes:</span><span class="sxs-lookup"><span data-stu-id="5900f-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="5900f-133">ASP.NET Core incluye una gran variedad de software intermedio integrado. Además, puede escribir software intermedio personalizado.</span><span class="sxs-lookup"><span data-stu-id="5900f-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="5900f-134">Para obtener más información, vea <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="5900f-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<a id="host"/>

## <a name="the-host"></a><span data-ttu-id="5900f-135">El host</span><span class="sxs-lookup"><span data-stu-id="5900f-135">The host</span></span>

<span data-ttu-id="5900f-136">Una aplicación de ASP.NET Core compila un *host* durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="5900f-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="5900f-137">El host es un objeto que encapsula todos los recursos de la aplicación, como:</span><span class="sxs-lookup"><span data-stu-id="5900f-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="5900f-138">Una implementación de servidor de HTTP</span><span class="sxs-lookup"><span data-stu-id="5900f-138">An HTTP server implementation</span></span>
* <span data-ttu-id="5900f-139">Componentes de software intermedio</span><span class="sxs-lookup"><span data-stu-id="5900f-139">Middleware components</span></span>
* <span data-ttu-id="5900f-140">Registro</span><span class="sxs-lookup"><span data-stu-id="5900f-140">Logging</span></span>
* <span data-ttu-id="5900f-141">DI</span><span class="sxs-lookup"><span data-stu-id="5900f-141">DI</span></span>
* <span data-ttu-id="5900f-142">Configuración</span><span class="sxs-lookup"><span data-stu-id="5900f-142">Configuration</span></span>

<span data-ttu-id="5900f-143">La razón principal para incluir todos los recursos interdependientes de la aplicación en un objeto es la administración de la duración: el control sobre el inicio de la aplicación y el apagado estable.</span><span class="sxs-lookup"><span data-stu-id="5900f-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="5900f-144">El código para crear un host se encuentra en `Program.Main` y sigue el [patrón de generador](https://wikipedia.org/wiki/Builder_pattern).</span><span class="sxs-lookup"><span data-stu-id="5900f-144">The code to create a host is in `Program.Main` and follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern).</span></span> <span data-ttu-id="5900f-145">Se llama a los métodos para configurar cada recurso que forma parte del host.</span><span class="sxs-lookup"><span data-stu-id="5900f-145">Methods are called to configure each resource that is part of the host.</span></span> <span data-ttu-id="5900f-146">Se llama a un método de generador para agrupar y crear una instancia del objeto host.</span><span class="sxs-lookup"><span data-stu-id="5900f-146">A builder method is called to pull it all together and instantiate the host object.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5900f-147">`CreateHostBuilder` es un nombre especial que identifica el método de generador para los componentes externos, como [Entity Framework](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="5900f-147">`CreateHostBuilder` is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="5900f-148">En ASP.NET Core 3.0 o versiones posteriores, en una aplicación web se puede usar el host genérico (clase `Host`) o el host web (clase `WebHost`).</span><span class="sxs-lookup"><span data-stu-id="5900f-148">In ASP.NET Core 3.0 or later, Generic Host (`Host` class) or Web Host (`WebHost` class) can be used in a web app.</span></span> <span data-ttu-id="5900f-149">Se recomienda el host genérico, mientras que el host web está disponible para compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="5900f-149">Generic Host is recommended, and Web Host is available for backwards compatibility.</span></span>

<span data-ttu-id="5900f-150">El marco proporciona los métodos `CreateDefaultBuilder` y `ConfigureWebHostDefaults` para configurar un host con opciones de uso común, como las siguientes:</span><span class="sxs-lookup"><span data-stu-id="5900f-150">The framework provides the `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="5900f-151">Use [Kestrel](#servers) como servidor web y habilite la integración de IIS.</span><span class="sxs-lookup"><span data-stu-id="5900f-151">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="5900f-152">Cargue la configuración de *appsettings.json*, *appsettings.[nombre del entorno].json*, las variables de entorno, los argumentos de línea de comandos y otros orígenes de configuración.</span><span class="sxs-lookup"><span data-stu-id="5900f-152">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="5900f-153">Envíe la salida de registro a la consola y los proveedores de depuración.</span><span class="sxs-lookup"><span data-stu-id="5900f-153">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="5900f-154">Aquí tiene un código de ejemplo que crea un host.</span><span class="sxs-lookup"><span data-stu-id="5900f-154">Here's sample code that builds a host.</span></span> <span data-ttu-id="5900f-155">Se resaltan los métodos que configuran el host con las opciones usadas habitualmente:</span><span class="sxs-lookup"><span data-stu-id="5900f-155">The methods that set up the host with commonly used options are highlighted:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

<span data-ttu-id="5900f-156">Para obtener más información, vea <xref:fundamentals/host/generic-host> y <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="5900f-156">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5900f-157">`CreateWebHostBuilder` es un nombre especial que identifica el método de generador para los componentes externos, como [Entity Framework](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="5900f-157">`CreateWebHostBuilder` is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="5900f-158">ASP.NET Core 2.x usa el host web (clase `WebHost`) para las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="5900f-158">ASP.NET Core 2.x uses Web Host (`WebHost` class) for web apps.</span></span> <span data-ttu-id="5900f-159">El marco proporciona `CreateDefaultBuilder` para configurar un host con opciones de uso común, como las siguientes:</span><span class="sxs-lookup"><span data-stu-id="5900f-159">The framework provides `CreateDefaultBuilder` to set up a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="5900f-160">Use [Kestrel](#servers) como servidor web y habilite la integración de IIS.</span><span class="sxs-lookup"><span data-stu-id="5900f-160">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="5900f-161">Cargue la configuración de *appsettings.json*, *appsettings.[nombre del entorno].json*, las variables de entorno, los argumentos de línea de comandos y otros orígenes de configuración.</span><span class="sxs-lookup"><span data-stu-id="5900f-161">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="5900f-162">Envíe la salida de registro a la consola y los proveedores de depuración.</span><span class="sxs-lookup"><span data-stu-id="5900f-162">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="5900f-163">Aquí tiene un código de ejemplo que crea un host:</span><span class="sxs-lookup"><span data-stu-id="5900f-163">Here's sample code that builds a host:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

<span data-ttu-id="5900f-164">Para obtener más información, vea <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="5900f-164">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="advanced-host-scenarios"></a><span data-ttu-id="5900f-165">Escenarios de host avanzados</span><span class="sxs-lookup"><span data-stu-id="5900f-165">Advanced host scenarios</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5900f-166">El host genérico está disponible para que lo use cualquier aplicación de .NET Core, no solo las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5900f-166">Generic Host is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="5900f-167">El host genérico (clase `Host`) permite que otros tipos de aplicaciones usen las extensiones de marcos transversales, como el registro, la inserción de dependencias, la configuración y la administración de la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5900f-167">Generic Host (`Host` class) allows other types of apps to use cross-cutting framework extensions, such as logging, DI, configuration, and app lifetime management.</span></span> <span data-ttu-id="5900f-168">Para obtener más información, vea <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="5900f-168">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5900f-169">El host web está diseñado para incluir una implementación de servidor HTTP, que no es necesaria para otros tipos de aplicaciones. NET.</span><span class="sxs-lookup"><span data-stu-id="5900f-169">Web Host is designed to include an HTTP server implementation, which isn't required for other kinds of .NET apps.</span></span> <span data-ttu-id="5900f-170">A partir de la versión 2.1 de ASP.NET Core, el host genérico (clase `Host`) está disponible para que lo use cualquier aplicación de .NET Core&mdash;no solo las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5900f-170">Starting in ASP.NET Core 2.1, the Generic Host (`Host` class) is available for any .NET Core app to use&mdash;not just ASP.NET Core apps.</span></span> <span data-ttu-id="5900f-171">El host genérico permite que otros tipos de aplicaciones usen las extensiones de marcos transversales, como el registro, la inserción de dependencias, la configuración y la administración de la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5900f-171">Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, DI, configuration, and app lifetime management.</span></span> <span data-ttu-id="5900f-172">Para obtener más información, vea <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="5900f-172">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

<span data-ttu-id="5900f-173">También puede usar el host para ejecutar tareas en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="5900f-173">You can also use the host to run background tasks.</span></span> <span data-ttu-id="5900f-174">Para obtener más información, vea <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="5900f-174">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="5900f-175">Servidores</span><span class="sxs-lookup"><span data-stu-id="5900f-175">Servers</span></span>

<span data-ttu-id="5900f-176">Una aplicación ASP.NET Core usa una implementación de servidor HTTP para escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="5900f-176">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="5900f-177">El servidor expone las solicitudes a la aplicación como un conjunto de [características de solicitud](xref:fundamentals/request-features) integradas en un contexto `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="5900f-177">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="5900f-178">Windows</span><span class="sxs-lookup"><span data-stu-id="5900f-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="5900f-179">ASP.NET Core proporciona las siguientes implementaciones de servidor:</span><span class="sxs-lookup"><span data-stu-id="5900f-179">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="5900f-180">*Kestrel* es un servidor web multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="5900f-180">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="5900f-181">Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="5900f-181">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="5900f-182">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="5900f-182">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="5900f-183">Un *servidor HTTP de IIS* es un servidor para Windows que usa IIS.</span><span class="sxs-lookup"><span data-stu-id="5900f-183">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="5900f-184">Con este servidor, la aplicación de ASP.NET Core e IIS se ejecutan en el mismo proceso.</span><span class="sxs-lookup"><span data-stu-id="5900f-184">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="5900f-185">*HTTP.sys* es un servidor de Windows que no se usa con IIS.</span><span class="sxs-lookup"><span data-stu-id="5900f-185">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="5900f-186">macOS</span><span class="sxs-lookup"><span data-stu-id="5900f-186">macOS</span></span>](#tab/macos)

<span data-ttu-id="5900f-187">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="5900f-187">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="5900f-188">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="5900f-188">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="5900f-189">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="5900f-189">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="5900f-190">Linux</span><span class="sxs-lookup"><span data-stu-id="5900f-190">Linux</span></span>](#tab/linux)

<span data-ttu-id="5900f-191">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="5900f-191">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="5900f-192">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="5900f-192">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="5900f-193">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="5900f-193">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="5900f-194">Windows</span><span class="sxs-lookup"><span data-stu-id="5900f-194">Windows</span></span>](#tab/windows)

<span data-ttu-id="5900f-195">ASP.NET Core proporciona las siguientes implementaciones de servidor:</span><span class="sxs-lookup"><span data-stu-id="5900f-195">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="5900f-196">*Kestrel* es un servidor web multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="5900f-196">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="5900f-197">Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="5900f-197">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="5900f-198">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="5900f-198">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="5900f-199">*HTTP.sys* es un servidor de Windows que no se usa con IIS.</span><span class="sxs-lookup"><span data-stu-id="5900f-199">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="5900f-200">macOS</span><span class="sxs-lookup"><span data-stu-id="5900f-200">macOS</span></span>](#tab/macos)

<span data-ttu-id="5900f-201">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="5900f-201">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="5900f-202">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="5900f-202">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="5900f-203">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="5900f-203">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="5900f-204">Linux</span><span class="sxs-lookup"><span data-stu-id="5900f-204">Linux</span></span>](#tab/linux)

<span data-ttu-id="5900f-205">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="5900f-205">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="5900f-206">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="5900f-206">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="5900f-207">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](http://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="5900f-207">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="5900f-208">Para obtener más información, vea <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="5900f-208">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="5900f-209">Configuración</span><span class="sxs-lookup"><span data-stu-id="5900f-209">Configuration</span></span>

<span data-ttu-id="5900f-210">ASP.NET Core proporciona un marco de configuración que obtiene la configuración como pares nombre/valor de un conjunto ordenado de proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="5900f-210">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="5900f-211">Hay proveedores de configuración integrados para una gran variedad de orígenes, como archivos *.json* y *.xml*, variables de entorno y argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="5900f-211">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="5900f-212">También puede escribir proveedores de configuración personalizados.</span><span class="sxs-lookup"><span data-stu-id="5900f-212">You can also write custom configuration providers.</span></span>

<span data-ttu-id="5900f-213">Por ejemplo, puede especificar que la configuración procede de *appsettings.json* y las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="5900f-213">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="5900f-214">Después, cuando se solicite el valor de *ConnectionString*, el marco buscará primero en el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5900f-214">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="5900f-215">Si se encuentra el valor allí, pero también en una variable de entorno, el valor de la variable de entorno tendría prioridad.</span><span class="sxs-lookup"><span data-stu-id="5900f-215">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="5900f-216">Para administrar los datos de configuración confidencial como las contraseñas, ASP.NET Core proporciona una [herramienta de administrador secreto](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="5900f-216">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="5900f-217">Para los secretos de producción, se recomienda [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="5900f-217">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="5900f-218">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="5900f-218">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="5900f-219">Opciones</span><span class="sxs-lookup"><span data-stu-id="5900f-219">Options</span></span>

<span data-ttu-id="5900f-220">Siempre que sea posible, ASP.NET Core sigue el *patrón de opciones* para almacenar y recuperar los valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="5900f-220">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="5900f-221">El patrón de opciones usa clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="5900f-221">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="5900f-222">Por ejemplo, el código siguiente define las opciones de WebSockets:</span><span class="sxs-lookup"><span data-stu-id="5900f-222">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="5900f-223">Para obtener más información, vea <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="5900f-223">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="5900f-224">Entornos</span><span class="sxs-lookup"><span data-stu-id="5900f-224">Environments</span></span>

<span data-ttu-id="5900f-225">Los entornos de ejecución, como *desarrollo*, *almacenamiento provisional* y *Producción*, son un concepto de primera clase en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5900f-225">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="5900f-226">Puede especificar el entorno que ejecuta una aplicación estableciendo la variable de entorno `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="5900f-226">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="5900f-227">ASP.NET Core lee dicha variable de entorno al inicio de la aplicación y almacena el valor en una implementación `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="5900f-227">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="5900f-228">El objeto de entorno está disponible en cualquier parte de la aplicación a través de DI.</span><span class="sxs-lookup"><span data-stu-id="5900f-228">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="5900f-229">El siguiente ejemplo de código desde la clase `Startup` configura la aplicación para que proporcione información detallada del error solo cuando se ejecuta en el desarrollo:</span><span class="sxs-lookup"><span data-stu-id="5900f-229">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="5900f-230">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="5900f-230">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="5900f-231">Registro</span><span class="sxs-lookup"><span data-stu-id="5900f-231">Logging</span></span>

<span data-ttu-id="5900f-232">ASP.NET Core es compatible con una API de registro que funciona con una gran variedad de proveedores de registro integrados y de terceros.</span><span class="sxs-lookup"><span data-stu-id="5900f-232">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="5900f-233">Entre los proveedores disponibles se incluyen los siguientes:</span><span class="sxs-lookup"><span data-stu-id="5900f-233">Available providers include the following:</span></span>

* <span data-ttu-id="5900f-234">Consola</span><span class="sxs-lookup"><span data-stu-id="5900f-234">Console</span></span>
* <span data-ttu-id="5900f-235">Depuración</span><span class="sxs-lookup"><span data-stu-id="5900f-235">Debug</span></span>
* <span data-ttu-id="5900f-236">Seguimiento de eventos en Windows</span><span class="sxs-lookup"><span data-stu-id="5900f-236">Event Tracing on Windows</span></span>
* <span data-ttu-id="5900f-237">Registro de errores de Windows</span><span class="sxs-lookup"><span data-stu-id="5900f-237">Windows Event Log</span></span>
* <span data-ttu-id="5900f-238">TraceSource</span><span class="sxs-lookup"><span data-stu-id="5900f-238">TraceSource</span></span>
* <span data-ttu-id="5900f-239">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5900f-239">Azure App Service</span></span>
* <span data-ttu-id="5900f-240">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="5900f-240">Azure Application Insights</span></span>

<span data-ttu-id="5900f-241">Escribir registros desde cualquier lugar en el código de una aplicación mediante la obtención de un objeto `ILogger` de DI y llamar a métodos de registro.</span><span class="sxs-lookup"><span data-stu-id="5900f-241">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="5900f-242">Aquí tiene un código de ejemplo que usa un objeto `ILogger`, con la inserción del constructor y resaltadas las llamadas del método de registro.</span><span class="sxs-lookup"><span data-stu-id="5900f-242">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="5900f-243">La interfaz `ILogger` le permite pasar cualquier número de campos para el proveedor de registro.</span><span class="sxs-lookup"><span data-stu-id="5900f-243">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="5900f-244">Los campos habitualmente se usan para construir una cadena de mensaje, pero el proveedor también puede enviarlos como campos independientes o almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="5900f-244">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="5900f-245">Esta característica permite a los proveedores de registro implementar el [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="5900f-245">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="5900f-246">Para obtener más información, vea <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="5900f-246">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="5900f-247">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="5900f-247">Routing</span></span>

<span data-ttu-id="5900f-248">Una *ruta* es un patrón de dirección URL que se asigna a un controlador.</span><span class="sxs-lookup"><span data-stu-id="5900f-248">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="5900f-249">El controlador normalmente es una página de Razor, un método de acción en un controlador MVC o un software intermedio.</span><span class="sxs-lookup"><span data-stu-id="5900f-249">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="5900f-250">El enrutamiento de ASP.NET Core le permite controlar las direcciones URL usadas por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5900f-250">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="5900f-251">Para obtener más información, vea <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="5900f-251">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="5900f-252">Control de errores</span><span class="sxs-lookup"><span data-stu-id="5900f-252">Error handling</span></span>

<span data-ttu-id="5900f-253">ASP.NET Core tiene características integradas para controlar los errores, tales como:</span><span class="sxs-lookup"><span data-stu-id="5900f-253">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="5900f-254">Una página de excepciones para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="5900f-254">A developer exception page</span></span>
* <span data-ttu-id="5900f-255">Páginas de errores personalizados</span><span class="sxs-lookup"><span data-stu-id="5900f-255">Custom error pages</span></span>
* <span data-ttu-id="5900f-256">Páginas de códigos de estado estáticos</span><span class="sxs-lookup"><span data-stu-id="5900f-256">Static status code pages</span></span>
* <span data-ttu-id="5900f-257">Control de excepciones de inicio</span><span class="sxs-lookup"><span data-stu-id="5900f-257">Startup exception handling</span></span>

<span data-ttu-id="5900f-258">Para obtener más información, vea <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="5900f-258">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="5900f-259">Realización de solicitudes HTTP</span><span class="sxs-lookup"><span data-stu-id="5900f-259">Make HTTP requests</span></span>

<span data-ttu-id="5900f-260">Una implementación de `IHttpClientFactory` está disponible para crear instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="5900f-260">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="5900f-261">Este servicio:</span><span class="sxs-lookup"><span data-stu-id="5900f-261">The factory:</span></span>

* <span data-ttu-id="5900f-262">Proporciona una ubicación central para denominar y configurar instancias de `HttpClient` lógicas.</span><span class="sxs-lookup"><span data-stu-id="5900f-262">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="5900f-263">Así, por ejemplo, se puede registrar y configurar un cliente *github* para tener acceso a GitHub.</span><span class="sxs-lookup"><span data-stu-id="5900f-263">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="5900f-264">y, de igual modo, registrar otro cliente predeterminado para otros fines.</span><span class="sxs-lookup"><span data-stu-id="5900f-264">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="5900f-265">Admite el registro y encadenamiento de varios controladores de delegación para crear una canalización de middleware de solicitud saliente.</span><span class="sxs-lookup"><span data-stu-id="5900f-265">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="5900f-266">Este patrón es similar a la canalización de middleware de entrada de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5900f-266">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="5900f-267">Dicho patrón proporciona un mecanismo para administrar cuestiones transversales relativas a las solicitudes HTTP, como el almacenamiento en caché, el control de errores, la serialización y el registro.</span><span class="sxs-lookup"><span data-stu-id="5900f-267">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="5900f-268">Se integra con *Polly*, una conocida biblioteca de terceros para el control de errores transitorios.</span><span class="sxs-lookup"><span data-stu-id="5900f-268">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="5900f-269">Administra la agrupación y duración de las instancias de `HttpClientMessageHandler` subyacentes para evitar los problemas de DNS que suelen producirse al administrar las duraciones de `HttpClient` manualmente.</span><span class="sxs-lookup"><span data-stu-id="5900f-269">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="5900f-270">Agrega una experiencia de registro configurable (a través de `ILogger`) en todas las solicitudes enviadas a través de los clientes creados por Factory.</span><span class="sxs-lookup"><span data-stu-id="5900f-270">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="5900f-271">Para obtener más información, vea <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="5900f-271">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="5900f-272">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="5900f-272">Content root</span></span>

<span data-ttu-id="5900f-273">La raíz del contenido es la ruta de acceso base a cualquier contenido privado que usa la aplicación, como sus archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="5900f-273">The content root is the base path to any private content used by the app, such as its Razor files.</span></span> <span data-ttu-id="5900f-274">De forma predeterminada, la raíz del contenido es la ruta de acceso base para el archivo ejecutable que hospeda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5900f-274">By default, the content root is the base path for the executable hosting the app.</span></span> <span data-ttu-id="5900f-275">Se puede especificar una ubicación alternativa al [crear el host](#host).</span><span class="sxs-lookup"><span data-stu-id="5900f-275">An alternative location can be specified when [building the host](#host).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5900f-276">Para obtener más información, vea [Raíz del contenido](xref:fundamentals/host/generic-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="5900f-276">For more information, see [Content root](xref:fundamentals/host/generic-host#content-root).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5900f-277">Para obtener más información, vea [Raíz del contenido](xref:fundamentals/host/web-host#content-root).</span><span class="sxs-lookup"><span data-stu-id="5900f-277">For more information, see [Content root](xref:fundamentals/host/web-host#content-root).</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="5900f-278">Raíz web</span><span class="sxs-lookup"><span data-stu-id="5900f-278">Web root</span></span>

<span data-ttu-id="5900f-279">La raíz web (también conocida como *webroot*) es la ruta de acceso base a los recursos públicos y estáticos, como archivos de imágenes, CSS y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5900f-279">The web root (also known as *webroot*) is the base path to public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="5900f-280">De forma predeterminada, el software intermedio de archivos estáticos solo ofrecerá archivos desde el directorio raíz web (y subdirectorios).</span><span class="sxs-lookup"><span data-stu-id="5900f-280">The static files middleware will only serve files from the web root directory (and sub-directories) by default.</span></span> <span data-ttu-id="5900f-281">El valor predeterminado de la ruta de acceso web es *{raíz del contenido}/wwwroot*, pero se puede especificar una ubicación diferente [al crear el host](#host).</span><span class="sxs-lookup"><span data-stu-id="5900f-281">The web root path defaults to *{Content Root}/wwwroot*, but a different location can be specified when [building the host](#host).</span></span>

<span data-ttu-id="5900f-282">En los archivos de Razor (*.cshtml*), la virgulilla `~/` apunta a la raíz web.</span><span class="sxs-lookup"><span data-stu-id="5900f-282">In Razor (*.cshtml*) files, the tilde-slash `~/` points to the web root.</span></span> <span data-ttu-id="5900f-283">Las rutas de acceso que empiezan por `~/` se conocen como rutas de acceso virtuales.</span><span class="sxs-lookup"><span data-stu-id="5900f-283">Paths beginning with `~/` are referred to as virtual paths.</span></span>

<span data-ttu-id="5900f-284">Para obtener más información, vea <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="5900f-284">For more information, see <xref:fundamentals/static-files>.</span></span>
