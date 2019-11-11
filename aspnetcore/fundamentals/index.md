---
title: Conceptos básicos de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre los conceptos básicos para crear aplicaciones de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/index
ms.openlocfilehash: 7173a732a04bf3e598adef298fa9120c15dd52fb
ms.sourcegitcommit: 67116718dc33a7a01696d41af38590fdbb58e014
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799370"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="39f02-103">Conceptos básicos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="39f02-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="39f02-104">Este artículo es una introducción de los temas clave para entender cómo desarrollar aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="39f02-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="39f02-105">Clase Startup</span><span class="sxs-lookup"><span data-stu-id="39f02-105">The Startup class</span></span>

<span data-ttu-id="39f02-106">La clase `Startup` es donde:</span><span class="sxs-lookup"><span data-stu-id="39f02-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="39f02-107">Se configuran los servicios requeridos por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="39f02-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="39f02-108">Se define la solicitud de canalización.</span><span class="sxs-lookup"><span data-stu-id="39f02-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="39f02-109">Los *servicios* son componentes que usan la aplicación.</span><span class="sxs-lookup"><span data-stu-id="39f02-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="39f02-110">Por ejemplo, un componente de registro es un servicio.</span><span class="sxs-lookup"><span data-stu-id="39f02-110">For example, a logging component is a service.</span></span> <span data-ttu-id="39f02-111">Se agrega al método `Startup.ConfigureServices` el código para configurar (o *registrar*) servicios.</span><span class="sxs-lookup"><span data-stu-id="39f02-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="39f02-112">La canalización de control de solicitudes se compone de una serie de *componentes de software intermedio*.</span><span class="sxs-lookup"><span data-stu-id="39f02-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="39f02-113">Por ejemplo, un software intermedio podría controlar las solicitudes de archivos estáticos o redirigir las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="39f02-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="39f02-114">Cada software intermedio lleva a cabo las operaciones asincrónicas en un contexto `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="39f02-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="39f02-115">Se agrega al método `Startup.Configure` el código para configurar la canalización de control de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="39f02-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="39f02-116">Aquí tiene una clase `Startup` de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="39f02-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="39f02-117">Para más información, consulte <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="39f02-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="39f02-118">Inserción de dependencias (servicios)</span><span class="sxs-lookup"><span data-stu-id="39f02-118">Dependency injection (services)</span></span>

<span data-ttu-id="39f02-119">ASP.NET Core tiene un marco de inserción de dependencias (DI) integrado que pone a disposición los servicios configurados para las clases de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="39f02-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="39f02-120">Una manera de obtener una instancia de un servicio en una clase es crear un constructor con un parámetro del tipo necesario.</span><span class="sxs-lookup"><span data-stu-id="39f02-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="39f02-121">El parámetro puede ser el tipo de servicio o una interfaz.</span><span class="sxs-lookup"><span data-stu-id="39f02-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="39f02-122">El sistema de DI proporciona el servicio en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="39f02-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="39f02-123">Aquí tiene una clase que usa DI para obtener un objeto de contexto de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="39f02-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="39f02-124">La línea resaltada es un ejemplo de inserción de constructor:</span><span class="sxs-lookup"><span data-stu-id="39f02-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="39f02-125">Aunque DI está integrada, está diseñada para permitirle conectar un contenedor de inversión de control (IoC) de terceros si lo prefiere.</span><span class="sxs-lookup"><span data-stu-id="39f02-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="39f02-126">Para más información, consulte <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="39f02-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="39f02-127">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="39f02-127">Middleware</span></span>

<span data-ttu-id="39f02-128">La canalización de control de solicitudes se compone de una serie de componentes de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="39f02-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="39f02-129">Cada componente lleva a cabo las operaciones asincrónicas en un contexto `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="39f02-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="39f02-130">Normalmente, se agrega un componente de software intermedio a la canalización al invocar su método de extensión `Use...` en el método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="39f02-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="39f02-131">Por ejemplo, para habilitar la representación de los archivos estáticos, llame a `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="39f02-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="39f02-132">El código resaltado en el ejemplo siguiente configura la canalización de control de solicitudes:</span><span class="sxs-lookup"><span data-stu-id="39f02-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="39f02-133">ASP.NET Core incluye una gran variedad de software intermedio integrado. Además, puede escribir software intermedio personalizado.</span><span class="sxs-lookup"><span data-stu-id="39f02-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="39f02-134">Para más información, consulte <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="39f02-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="39f02-135">administrador de flujos de trabajo</span><span class="sxs-lookup"><span data-stu-id="39f02-135">Host</span></span>

<span data-ttu-id="39f02-136">Una aplicación de ASP.NET Core compila un *host* durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="39f02-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="39f02-137">El host es un objeto que encapsula todos los recursos de la aplicación, como:</span><span class="sxs-lookup"><span data-stu-id="39f02-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="39f02-138">Una implementación de servidor de HTTP</span><span class="sxs-lookup"><span data-stu-id="39f02-138">An HTTP server implementation</span></span>
* <span data-ttu-id="39f02-139">Componentes de software intermedio</span><span class="sxs-lookup"><span data-stu-id="39f02-139">Middleware components</span></span>
* <span data-ttu-id="39f02-140">Registro</span><span class="sxs-lookup"><span data-stu-id="39f02-140">Logging</span></span>
* <span data-ttu-id="39f02-141">DI</span><span class="sxs-lookup"><span data-stu-id="39f02-141">DI</span></span>
* <span data-ttu-id="39f02-142">Configuración</span><span class="sxs-lookup"><span data-stu-id="39f02-142">Configuration</span></span>

<span data-ttu-id="39f02-143">La razón principal para incluir todos los recursos interdependientes de la aplicación en un objeto es la administración de la duración: el control sobre el inicio de la aplicación y el apagado estable.</span><span class="sxs-lookup"><span data-stu-id="39f02-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="39f02-144">Hay dos hosts disponibles: el host genérico y el host web.</span><span class="sxs-lookup"><span data-stu-id="39f02-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="39f02-145">Se recomienda el host genérico, mientras que el host web está disponible solo para compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="39f02-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="39f02-146">El código para crear un host está en `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="39f02-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="39f02-147">Los métodos `CreateDefaultBuilder` y `ConfigureWebHostDefaults` configuran un host con opciones de uso común, como las siguientes:</span><span class="sxs-lookup"><span data-stu-id="39f02-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="39f02-148">Use [Kestrel](#servers) como servidor web y habilite la integración de IIS.</span><span class="sxs-lookup"><span data-stu-id="39f02-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="39f02-149">Cargue la configuración de *appsettings.json*, *appsettings.[nombre del entorno].json*, las variables de entorno, los argumentos de línea de comandos y otros orígenes de configuración.</span><span class="sxs-lookup"><span data-stu-id="39f02-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="39f02-150">Envíe la salida de registro a la consola y los proveedores de depuración.</span><span class="sxs-lookup"><span data-stu-id="39f02-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="39f02-151">Para más información, consulte <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="39f02-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="39f02-152">Hay dos hosts disponibles: el host web y el host genérico.</span><span class="sxs-lookup"><span data-stu-id="39f02-152">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="39f02-153">En ASP.NET Core 2.x, el host genérico es solo para escenarios que no son web.</span><span class="sxs-lookup"><span data-stu-id="39f02-153">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="39f02-154">El código para crear un host está en `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="39f02-154">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="39f02-155">El métodos `CreateDefaultBuilder` configura un host con opciones de uso común, como las siguientes:</span><span class="sxs-lookup"><span data-stu-id="39f02-155">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="39f02-156">Use [Kestrel](#servers) como servidor web y habilite la integración de IIS.</span><span class="sxs-lookup"><span data-stu-id="39f02-156">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="39f02-157">Cargue la configuración de *appsettings.json*, *appsettings.[nombre del entorno].json*, las variables de entorno, los argumentos de línea de comandos y otros orígenes de configuración.</span><span class="sxs-lookup"><span data-stu-id="39f02-157">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="39f02-158">Envíe la salida de registro a la consola y los proveedores de depuración.</span><span class="sxs-lookup"><span data-stu-id="39f02-158">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="39f02-159">Para más información, consulte <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="39f02-159">For more information, see <xref:fundamentals/host/web-host>.</span></span>

::: moniker-end

### <a name="non-web-scenarios"></a><span data-ttu-id="39f02-160">Escenarios que no son web</span><span class="sxs-lookup"><span data-stu-id="39f02-160">Non-web scenarios</span></span>

<span data-ttu-id="39f02-161">El host genérico permite que otros tipos de aplicaciones usen las extensiones de marcos transversales, como el registro, la inserción de dependencias (DI), la configuración y la administración de la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="39f02-161">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="39f02-162">Para obtener más información, vea <xref:fundamentals/host/generic-host> y <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="39f02-162">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="39f02-163">Servidores</span><span class="sxs-lookup"><span data-stu-id="39f02-163">Servers</span></span>

<span data-ttu-id="39f02-164">Una aplicación ASP.NET Core usa una implementación de servidor HTTP para escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="39f02-164">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="39f02-165">El servidor expone las solicitudes a la aplicación como un conjunto de [características de solicitud](xref:fundamentals/request-features) integradas en un contexto `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="39f02-165">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="39f02-166">Windows</span><span class="sxs-lookup"><span data-stu-id="39f02-166">Windows</span></span>](#tab/windows)

<span data-ttu-id="39f02-167">ASP.NET Core proporciona las siguientes implementaciones de servidor:</span><span class="sxs-lookup"><span data-stu-id="39f02-167">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="39f02-168">*Kestrel* es un servidor web multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="39f02-168">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="39f02-169">Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="39f02-169">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="39f02-170">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="39f02-170">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="39f02-171">Un *servidor HTTP de IIS* es un servidor para Windows que usa IIS.</span><span class="sxs-lookup"><span data-stu-id="39f02-171">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="39f02-172">Con este servidor, la aplicación de ASP.NET Core e IIS se ejecutan en el mismo proceso.</span><span class="sxs-lookup"><span data-stu-id="39f02-172">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="39f02-173">*HTTP.sys* es un servidor de Windows que no se usa con IIS.</span><span class="sxs-lookup"><span data-stu-id="39f02-173">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="39f02-174">macOS</span><span class="sxs-lookup"><span data-stu-id="39f02-174">macOS</span></span>](#tab/macos)

<span data-ttu-id="39f02-175">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="39f02-175">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="39f02-176">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="39f02-176">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="39f02-177">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="39f02-177">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="39f02-178">Linux</span><span class="sxs-lookup"><span data-stu-id="39f02-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="39f02-179">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="39f02-179">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="39f02-180">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="39f02-180">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="39f02-181">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="39f02-181">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="39f02-182">Windows</span><span class="sxs-lookup"><span data-stu-id="39f02-182">Windows</span></span>](#tab/windows)

<span data-ttu-id="39f02-183">ASP.NET Core proporciona las siguientes implementaciones de servidor:</span><span class="sxs-lookup"><span data-stu-id="39f02-183">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="39f02-184">*Kestrel* es un servidor web multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="39f02-184">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="39f02-185">Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="39f02-185">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="39f02-186">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="39f02-186">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="39f02-187">*HTTP.sys* es un servidor de Windows que no se usa con IIS.</span><span class="sxs-lookup"><span data-stu-id="39f02-187">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="39f02-188">macOS</span><span class="sxs-lookup"><span data-stu-id="39f02-188">macOS</span></span>](#tab/macos)

<span data-ttu-id="39f02-189">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="39f02-189">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="39f02-190">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="39f02-190">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="39f02-191">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="39f02-191">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="39f02-192">Linux</span><span class="sxs-lookup"><span data-stu-id="39f02-192">Linux</span></span>](#tab/linux)

<span data-ttu-id="39f02-193">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="39f02-193">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="39f02-194">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="39f02-194">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="39f02-195">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="39f02-195">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

<span data-ttu-id="39f02-196">Para más información, consulte <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="39f02-196">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="39f02-197">Configuración</span><span class="sxs-lookup"><span data-stu-id="39f02-197">Configuration</span></span>

<span data-ttu-id="39f02-198">ASP.NET Core proporciona un marco de configuración que obtiene la configuración como pares nombre/valor de un conjunto ordenado de proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="39f02-198">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="39f02-199">Hay proveedores de configuración integrados para una gran variedad de orígenes, como archivos *.json* y *.xml*, variables de entorno y argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="39f02-199">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="39f02-200">También puede escribir proveedores de configuración personalizados.</span><span class="sxs-lookup"><span data-stu-id="39f02-200">You can also write custom configuration providers.</span></span>

<span data-ttu-id="39f02-201">Por ejemplo, puede especificar que la configuración procede de *appsettings.json* y las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="39f02-201">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="39f02-202">Después, cuando se solicite el valor de *ConnectionString*, el marco buscará primero en el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="39f02-202">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="39f02-203">Si se encuentra el valor allí, pero también en una variable de entorno, el valor de la variable de entorno tendría prioridad.</span><span class="sxs-lookup"><span data-stu-id="39f02-203">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="39f02-204">Para administrar los datos de configuración confidencial como las contraseñas, ASP.NET Core proporciona una [herramienta de administrador secreto](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="39f02-204">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="39f02-205">Para los secretos de producción, se recomienda [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="39f02-205">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="39f02-206">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="39f02-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="39f02-207">Opciones</span><span class="sxs-lookup"><span data-stu-id="39f02-207">Options</span></span>

<span data-ttu-id="39f02-208">Siempre que sea posible, ASP.NET Core sigue el *patrón de opciones* para almacenar y recuperar los valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="39f02-208">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="39f02-209">El patrón de opciones usa clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="39f02-209">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="39f02-210">Por ejemplo, el código siguiente define las opciones de WebSockets:</span><span class="sxs-lookup"><span data-stu-id="39f02-210">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="39f02-211">Para más información, consulte <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="39f02-211">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="39f02-212">Entornos</span><span class="sxs-lookup"><span data-stu-id="39f02-212">Environments</span></span>

<span data-ttu-id="39f02-213">Los entornos de ejecución, como *desarrollo*, *almacenamiento provisional* y *Producción*, son un concepto de primera clase en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="39f02-213">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="39f02-214">Puede especificar el entorno que ejecuta una aplicación estableciendo la variable de entorno `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="39f02-214">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="39f02-215">ASP.NET Core lee dicha variable de entorno al inicio de la aplicación y almacena el valor en una implementación `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="39f02-215">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="39f02-216">El objeto de entorno está disponible en cualquier parte de la aplicación a través de DI.</span><span class="sxs-lookup"><span data-stu-id="39f02-216">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="39f02-217">El siguiente ejemplo de código desde la clase `Startup` configura la aplicación para que proporcione información detallada del error solo cuando se ejecuta en el desarrollo:</span><span class="sxs-lookup"><span data-stu-id="39f02-217">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="39f02-218">Para más información, consulte <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="39f02-218">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="39f02-219">Registro</span><span class="sxs-lookup"><span data-stu-id="39f02-219">Logging</span></span>

<span data-ttu-id="39f02-220">ASP.NET Core es compatible con una API de registro que funciona con una gran variedad de proveedores de registro integrados y de terceros.</span><span class="sxs-lookup"><span data-stu-id="39f02-220">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="39f02-221">Entre los proveedores disponibles se incluyen los siguientes:</span><span class="sxs-lookup"><span data-stu-id="39f02-221">Available providers include the following:</span></span>

* <span data-ttu-id="39f02-222">Consola</span><span class="sxs-lookup"><span data-stu-id="39f02-222">Console</span></span>
* <span data-ttu-id="39f02-223">Depuración</span><span class="sxs-lookup"><span data-stu-id="39f02-223">Debug</span></span>
* <span data-ttu-id="39f02-224">Seguimiento de eventos en Windows</span><span class="sxs-lookup"><span data-stu-id="39f02-224">Event Tracing on Windows</span></span>
* <span data-ttu-id="39f02-225">Registro de errores de Windows</span><span class="sxs-lookup"><span data-stu-id="39f02-225">Windows Event Log</span></span>
* <span data-ttu-id="39f02-226">TraceSource</span><span class="sxs-lookup"><span data-stu-id="39f02-226">TraceSource</span></span>
* <span data-ttu-id="39f02-227">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="39f02-227">Azure App Service</span></span>
* <span data-ttu-id="39f02-228">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="39f02-228">Azure Application Insights</span></span>

<span data-ttu-id="39f02-229">Escribir registros desde cualquier lugar en el código de una aplicación mediante la obtención de un objeto `ILogger` de DI y llamar a métodos de registro.</span><span class="sxs-lookup"><span data-stu-id="39f02-229">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="39f02-230">Aquí tiene un código de ejemplo que usa un objeto `ILogger`, con la inserción del constructor y resaltadas las llamadas del método de registro.</span><span class="sxs-lookup"><span data-stu-id="39f02-230">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="39f02-231">La interfaz `ILogger` le permite pasar cualquier número de campos para el proveedor de registro.</span><span class="sxs-lookup"><span data-stu-id="39f02-231">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="39f02-232">Los campos habitualmente se usan para construir una cadena de mensaje, pero el proveedor también puede enviarlos como campos independientes o almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="39f02-232">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="39f02-233">Esta característica permite a los proveedores de registro implementar el [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="39f02-233">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="39f02-234">Para más información, consulte <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="39f02-234">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="39f02-235">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="39f02-235">Routing</span></span>

<span data-ttu-id="39f02-236">Una *ruta* es un patrón de dirección URL que se asigna a un controlador.</span><span class="sxs-lookup"><span data-stu-id="39f02-236">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="39f02-237">El controlador normalmente es una página de Razor, un método de acción en un controlador MVC o un software intermedio.</span><span class="sxs-lookup"><span data-stu-id="39f02-237">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="39f02-238">El enrutamiento de ASP.NET Core le permite controlar las direcciones URL usadas por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="39f02-238">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="39f02-239">Para más información, consulte <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="39f02-239">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="39f02-240">Control de errores</span><span class="sxs-lookup"><span data-stu-id="39f02-240">Error handling</span></span>

<span data-ttu-id="39f02-241">ASP.NET Core tiene características integradas para controlar los errores, tales como:</span><span class="sxs-lookup"><span data-stu-id="39f02-241">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="39f02-242">Una página de excepciones para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="39f02-242">A developer exception page</span></span>
* <span data-ttu-id="39f02-243">Páginas de errores personalizados</span><span class="sxs-lookup"><span data-stu-id="39f02-243">Custom error pages</span></span>
* <span data-ttu-id="39f02-244">Páginas de códigos de estado estáticos</span><span class="sxs-lookup"><span data-stu-id="39f02-244">Static status code pages</span></span>
* <span data-ttu-id="39f02-245">Control de excepciones de inicio</span><span class="sxs-lookup"><span data-stu-id="39f02-245">Startup exception handling</span></span>

<span data-ttu-id="39f02-246">Para más información, consulte <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="39f02-246">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="39f02-247">Realización de solicitudes HTTP</span><span class="sxs-lookup"><span data-stu-id="39f02-247">Make HTTP requests</span></span>

<span data-ttu-id="39f02-248">Una implementación de `IHttpClientFactory` está disponible para crear instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="39f02-248">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="39f02-249">Este servicio:</span><span class="sxs-lookup"><span data-stu-id="39f02-249">The factory:</span></span>

* <span data-ttu-id="39f02-250">Proporciona una ubicación central para denominar y configurar instancias de `HttpClient` lógicas.</span><span class="sxs-lookup"><span data-stu-id="39f02-250">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="39f02-251">Así, por ejemplo, se puede registrar y configurar un cliente *github* para tener acceso a GitHub.</span><span class="sxs-lookup"><span data-stu-id="39f02-251">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="39f02-252">y, de igual modo, registrar otro cliente predeterminado para otros fines.</span><span class="sxs-lookup"><span data-stu-id="39f02-252">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="39f02-253">Admite el registro y encadenamiento de varios controladores de delegación para crear una canalización de middleware de solicitud saliente.</span><span class="sxs-lookup"><span data-stu-id="39f02-253">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="39f02-254">Este patrón es similar a la canalización de middleware de entrada de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="39f02-254">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="39f02-255">Dicho patrón proporciona un mecanismo para administrar cuestiones transversales relativas a las solicitudes HTTP, como el almacenamiento en caché, el control de errores, la serialización y el registro.</span><span class="sxs-lookup"><span data-stu-id="39f02-255">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="39f02-256">Se integra con *Polly*, una conocida biblioteca de terceros para el control de errores transitorios.</span><span class="sxs-lookup"><span data-stu-id="39f02-256">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="39f02-257">Administra la agrupación y duración de las instancias de `HttpClientMessageHandler` subyacentes para evitar los problemas de DNS que suelen producirse al administrar las duraciones de `HttpClient` manualmente.</span><span class="sxs-lookup"><span data-stu-id="39f02-257">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="39f02-258">Agrega una experiencia de registro configurable (a través de `ILogger`) en todas las solicitudes enviadas a través de los clientes creados por Factory.</span><span class="sxs-lookup"><span data-stu-id="39f02-258">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="39f02-259">Para más información, consulte <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="39f02-259">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="39f02-260">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="39f02-260">Content root</span></span>

<span data-ttu-id="39f02-261">La raíz del contenido es la ruta de acceso base a:</span><span class="sxs-lookup"><span data-stu-id="39f02-261">The content root is the base path to the:</span></span>

* <span data-ttu-id="39f02-262">El archivo ejecutable que hospeda la aplicación ( *.exe*).</span><span class="sxs-lookup"><span data-stu-id="39f02-262">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="39f02-263">Los ensamblados compilados que componen la aplicación ( *.dll*).</span><span class="sxs-lookup"><span data-stu-id="39f02-263">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="39f02-264">Los archivos de contenido que no son de código usados por la aplicación, como:</span><span class="sxs-lookup"><span data-stu-id="39f02-264">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="39f02-265">Archivos de Razor ( *.cshtml*, *.razor*)</span><span class="sxs-lookup"><span data-stu-id="39f02-265">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="39f02-266">Archivos de configuración ( *.json*, *.xml*)</span><span class="sxs-lookup"><span data-stu-id="39f02-266">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="39f02-267">Archivos de datos ( *.db*)</span><span class="sxs-lookup"><span data-stu-id="39f02-267">Data files (*.db*)</span></span>
* <span data-ttu-id="39f02-268">[Raíz web](#web-root), normalmente la carpeta *wwwroot* publicada.</span><span class="sxs-lookup"><span data-stu-id="39f02-268">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="39f02-269">Durante el desarrollo:</span><span class="sxs-lookup"><span data-stu-id="39f02-269">During development:</span></span>

* <span data-ttu-id="39f02-270">La raíz del contenido tiene como valor predeterminado el directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="39f02-270">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="39f02-271">El directorio raíz del proyecto se usa para crear:</span><span class="sxs-lookup"><span data-stu-id="39f02-271">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="39f02-272">La ruta de acceso a los archivos de contenido que no son de código de la aplicación en el directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="39f02-272">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="39f02-273">La [raíz web](#web-root), normalmente la carpeta *wwwroot* en el directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="39f02-273">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="39f02-274">Se puede especificar una ruta raíz de contenido alternativa al [crear el host](#host).</span><span class="sxs-lookup"><span data-stu-id="39f02-274">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="39f02-275">Para más información, consulte <xref:fundamentals/host/generic-host#contentrootpath>.</span><span class="sxs-lookup"><span data-stu-id="39f02-275">For more information, see <xref:fundamentals/host/generic-host#contentrootpath>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="39f02-276">Se puede especificar una ruta raíz de contenido alternativa al [crear el host](#host).</span><span class="sxs-lookup"><span data-stu-id="39f02-276">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="39f02-277">Para más información, consulte <xref:fundamentals/host/web-host#content-root>.</span><span class="sxs-lookup"><span data-stu-id="39f02-277">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

::: moniker-end

## <a name="web-root"></a><span data-ttu-id="39f02-278">Raíz web</span><span class="sxs-lookup"><span data-stu-id="39f02-278">Web root</span></span>

<span data-ttu-id="39f02-279">La raíz web es la ruta de acceso base a archivos de recursos públicos, que no son de código y estáticos, como:</span><span class="sxs-lookup"><span data-stu-id="39f02-279">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="39f02-280">Hojas de estilo ( *.css*)</span><span class="sxs-lookup"><span data-stu-id="39f02-280">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="39f02-281">JavaScript ( *.js*)</span><span class="sxs-lookup"><span data-stu-id="39f02-281">JavaScript (*.js*)</span></span>
* <span data-ttu-id="39f02-282">Imágenes ( *.png*, *.jpg*)</span><span class="sxs-lookup"><span data-stu-id="39f02-282">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="39f02-283">De forma predeterminada, los archivos estáticos se atienden solo desde el directorio raíz web (y los subdirectorios).</span><span class="sxs-lookup"><span data-stu-id="39f02-283">Static files are only served by default from the web root directory (and sub-directories).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="39f02-284">El valor predeterminado de la ruta de acceso de la raíz web es *{raíz del contenido}/wwwroot*, pero se puede especificar una raíz web diferente [al crear el host](#host).</span><span class="sxs-lookup"><span data-stu-id="39f02-284">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="39f02-285">Para más información, consulte <xref:fundamentals/host/generic-host#webroot>.</span><span class="sxs-lookup"><span data-stu-id="39f02-285">For more information, see <xref:fundamentals/host/generic-host#webroot>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="39f02-286">El valor predeterminado de la ruta de acceso de la raíz web es *{raíz del contenido}/wwwroot*, pero se puede especificar una raíz web diferente [al crear el host](#host).</span><span class="sxs-lookup"><span data-stu-id="39f02-286">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="39f02-287">Para obtener más información, vea [Raíz web](xref:fundamentals/host/web-host#web-root).</span><span class="sxs-lookup"><span data-stu-id="39f02-287">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

::: moniker-end

<span data-ttu-id="39f02-288">Evite la publicación de archivos en *wwwroot* con el [\<Content> elemento de proyecto](/visualstudio/msbuild/common-msbuild-project-items#content) en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="39f02-288">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="39f02-289">En el ejemplo siguiente se impide la publicación de contenido en el directorio *wwwroot/local* y en los subdirectorios:</span><span class="sxs-lookup"><span data-stu-id="39f02-289">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="39f02-290">En los archivos de Razor ( *.cshtml*), la virgulilla `~/` apunta a la raíz web.</span><span class="sxs-lookup"><span data-stu-id="39f02-290">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="39f02-291">Una ruta de acceso que empieza por `~/` se conoce como *ruta de acceso virtual*.</span><span class="sxs-lookup"><span data-stu-id="39f02-291">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="39f02-292">Para más información, consulte <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="39f02-292">For more information, see <xref:fundamentals/static-files>.</span></span>
