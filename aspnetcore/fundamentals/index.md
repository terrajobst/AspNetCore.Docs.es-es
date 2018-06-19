---
title: Conceptos básicos de ASP.NET Core
author: rick-anderson
description: Descubra los conceptos básicos para crear aplicaciones ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: 97c0b289b259332d57f8175e05020fe03d505723
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233315"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="dceaf-103">Conceptos básicos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dceaf-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="dceaf-104">Una aplicación de ASP.NET Core es una aplicación de consola que crea un servidor web en su método `Main`:</span><span class="sxs-lookup"><span data-stu-id="dceaf-104">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dceaf-105">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dceaf-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="dceaf-106">El método `Main` invoca a `WebHost.CreateDefaultBuilder`, que sigue el patrón de generador para crear un host de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dceaf-106">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="dceaf-107">El generador tiene métodos que definen el servidor web (por ejemplo, `UseKestrel`) y la clase de inicio (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="dceaf-107">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="dceaf-108">En el ejemplo anterior, se asigna automáticamente el servidor web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="dceaf-108">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="dceaf-109">El host web de ASP.NET Core intenta ejecutarse en IIS, si está disponible.</span><span class="sxs-lookup"><span data-stu-id="dceaf-109">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="dceaf-110">Otros servidores web, como [HTTP.sys](xref:fundamentals/servers/httpsys), se pueden usar al invocar el método de extensión adecuado.</span><span class="sxs-lookup"><span data-stu-id="dceaf-110">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="dceaf-111">`UseStartup` se explica en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="dceaf-111">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="dceaf-112">`IWebHostBuilder`, el tipo de valor devuelto de la invocación `WebHost.CreateDefaultBuilder`, proporciona muchos métodos opcionales.</span><span class="sxs-lookup"><span data-stu-id="dceaf-112">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="dceaf-113">Algunos de estos métodos incluyen `UseHttpSys` para hospedar la aplicación en HTTP.sys y `UseContentRoot` para especificar el directorio de contenido raíz.</span><span class="sxs-lookup"><span data-stu-id="dceaf-113">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="dceaf-114">Los métodos `Build` y `Run` crean el objeto `IWebHost` que hospeda la aplicación y empieza a escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="dceaf-114">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dceaf-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dceaf-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="dceaf-116">El método `Main` usa `WebHostBuilder`, que sigue el patrón de generador para crear un host de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dceaf-116">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="dceaf-117">El generador tiene métodos que definen el servidor web (por ejemplo, `UseKestrel`) y la clase de inicio (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="dceaf-117">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="dceaf-118">En el ejemplo anterior, se usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="dceaf-118">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="dceaf-119">Si se invoca el método de extensión adecuado, se pueden usar otros servidores web, como [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="dceaf-119">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="dceaf-120">`UseStartup` se explica en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="dceaf-120">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="dceaf-121">`WebHostBuilder` proporciona muchos métodos opcionales, incluido `UseIISIntegration` para hospedar en IIS e IIS Express, y `UseContentRoot` para especificar el directorio de contenido raíz.</span><span class="sxs-lookup"><span data-stu-id="dceaf-121">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="dceaf-122">Los métodos `Build` y `Run` crean el objeto `IWebHost` que hospeda la aplicación y empieza a escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="dceaf-122">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="dceaf-123">Inicio</span><span class="sxs-lookup"><span data-stu-id="dceaf-123">Startup</span></span>

<span data-ttu-id="dceaf-124">El método `UseStartup` de `WebHostBuilder` especifica la clase `Startup` para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="dceaf-124">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dceaf-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dceaf-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dceaf-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dceaf-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="dceaf-127">La clase `Startup` es donde se define la canalización de control de solicitudes y donde se configuran los servicios necesarios para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dceaf-127">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="dceaf-128">La clase `Startup` debe ser pública y contener los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="dceaf-128">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

<span data-ttu-id="dceaf-129">`ConfigureServices` define los [servicios](#dependency-injection-services) que usa la aplicación (por ejemplo, ASP.NET Core MVC, Entity Framework Core, Identity).</span><span class="sxs-lookup"><span data-stu-id="dceaf-129">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="dceaf-130">`Configure` define el [software intermedio](xref:fundamentals/middleware/index) en la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="dceaf-130">`Configure` defines the [middleware](xref:fundamentals/middleware/index) for the request pipeline.</span></span>

<span data-ttu-id="dceaf-131">Para obtener más información, vea [Application startup](xref:fundamentals/startup) (Inicio de la aplicación).</span><span class="sxs-lookup"><span data-stu-id="dceaf-131">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="dceaf-132">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="dceaf-132">Content root</span></span>

<span data-ttu-id="dceaf-133">La raíz del contenido es la ruta de acceso base a cualquier contenido que usa la aplicación, como vistas, [páginas de Razor](xref:mvc/razor-pages/index) y activos estáticos.</span><span class="sxs-lookup"><span data-stu-id="dceaf-133">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="dceaf-134">De forma predeterminada, la raíz del contenido es la misma que la ruta de acceso base de la aplicación para el archivo ejecutable que hospeda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dceaf-134">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="dceaf-135">Raíz web</span><span class="sxs-lookup"><span data-stu-id="dceaf-135">Web root</span></span>

<span data-ttu-id="dceaf-136">La raíz web de una aplicación es el directorio del proyecto que contiene recursos públicos y estáticos, como archivos de imagen, CSS y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dceaf-136">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="dceaf-137">Inserción de dependencias (servicios)</span><span class="sxs-lookup"><span data-stu-id="dceaf-137">Dependency injection (services)</span></span>

<span data-ttu-id="dceaf-138">Un servicio es un componente que está pensado para su uso común en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="dceaf-138">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="dceaf-139">Los servicios se ponen a disposición a través de la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="dceaf-139">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="dceaf-140">ASP.NET Core incluye un contenedor **d**e **i**nversión del **c**ontrol (IoC) nativo que admite la [inserción de constructores](xref:mvc/controllers/dependency-injection#constructor-injection) de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="dceaf-140">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="dceaf-141">Si quiere, puede reemplazar el contenedor nativo predeterminado.</span><span class="sxs-lookup"><span data-stu-id="dceaf-141">You can replace the default native container if you wish.</span></span> <span data-ttu-id="dceaf-142">Además de la ventaja de acoplamiento flexible, DI hace que los servicios estén disponibles en toda la aplicación (por ejemplo, el [registro](xref:fundamentals/logging/index)).</span><span class="sxs-lookup"><span data-stu-id="dceaf-142">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="dceaf-143">Para obtener más información, consulte [Inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="dceaf-143">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="dceaf-144">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="dceaf-144">Middleware</span></span>

<span data-ttu-id="dceaf-145">En ASP.NET Core, se crea la canalización de solicitudes mediante [software intermedio](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="dceaf-145">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="dceaf-146">El software intermedio de ASP.NET Core lleva a cabo la lógica asincrónica en `HttpContext` y después invoca al siguiente software intermedio de la secuencia o finaliza la solicitud directamente.</span><span class="sxs-lookup"><span data-stu-id="dceaf-146">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="dceaf-147">Se agrega un componente de software intermedio denominado "XYZ" al invocar un método de extensión `UseXYZ` en el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="dceaf-147">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="dceaf-148">ASP.NET Core incluye un amplio conjunto de middleware integrado:</span><span class="sxs-lookup"><span data-stu-id="dceaf-148">ASP.NET Core includes a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="dceaf-149">Archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="dceaf-149">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="dceaf-150">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="dceaf-150">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="dceaf-151">Autenticación</span><span class="sxs-lookup"><span data-stu-id="dceaf-151">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="dceaf-152">Middleware de compresión de respuestas</span><span class="sxs-lookup"><span data-stu-id="dceaf-152">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="dceaf-153">Middleware de reescritura de dirección URL</span><span class="sxs-lookup"><span data-stu-id="dceaf-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="dceaf-154">El software intermedio basado en [OWIN](http://owin.org) está disponible para aplicaciones ASP.NET Core y puede escribir su propio software intermedio personalizado.</span><span class="sxs-lookup"><span data-stu-id="dceaf-154">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="dceaf-155">Para obtener más información, consulte [Middleware](xref:fundamentals/middleware/index) (Software intermedio) y [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) [Interfaz web abierta para .NET (OWIN)].</span><span class="sxs-lookup"><span data-stu-id="dceaf-155">For more information, see [Middleware](xref:fundamentals/middleware/index) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="initiate-http-requests"></a><span data-ttu-id="dceaf-156">Inicio de solicitudes HTTP</span><span class="sxs-lookup"><span data-stu-id="dceaf-156">Initiate HTTP requests</span></span>

<span data-ttu-id="dceaf-157">Para obtener información sobre el uso de `IHttpClientFactory` para acceder a instancias `HttpClient` a fin de realizar solicitudes HTTP, vea [Inicio de solicitudes HTTP](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="dceaf-157">For information about using `IHttpClientFactory` to access `HttpClient` instances to make HTTP requests, see [Initiate HTTP requests](xref:fundamentals/http-requests).</span></span>

## <a name="environments"></a><span data-ttu-id="dceaf-158">Entornos</span><span class="sxs-lookup"><span data-stu-id="dceaf-158">Environments</span></span>

<span data-ttu-id="dceaf-159">Los entornos, como "Desarrollo" y "Producción", son un concepto de primera clase en ASP.NET Core y se pueden establecer mediante variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="dceaf-159">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="dceaf-160">Para obtener más información, consulte [Uso de varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="dceaf-160">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="dceaf-161">Configuración</span><span class="sxs-lookup"><span data-stu-id="dceaf-161">Configuration</span></span>

<span data-ttu-id="dceaf-162">ASP.NET Core usa un modelo de configuración basado en pares de nombre-valor.</span><span class="sxs-lookup"><span data-stu-id="dceaf-162">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="dceaf-163">El modelo de configuración no se basa en `System.Configuration` o *web.config*. La configuración obtiene valores de un conjunto ordenado de proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="dceaf-163">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="dceaf-164">Los proveedores de configuración integrados admiten una variedad de formatos de archivo (XML, JSON, INI) y variables de entorno para habilitar la configuración basada en el entorno.</span><span class="sxs-lookup"><span data-stu-id="dceaf-164">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="dceaf-165">También puede escribir sus propios proveedores de configuración personalizados.</span><span class="sxs-lookup"><span data-stu-id="dceaf-165">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="dceaf-166">Para obtener más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="dceaf-166">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="dceaf-167">Registro</span><span class="sxs-lookup"><span data-stu-id="dceaf-167">Logging</span></span>

<span data-ttu-id="dceaf-168">ASP.NET Core es compatible con una API de registro que funciona con una variedad de proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="dceaf-168">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="dceaf-169">Los proveedores integrados admiten el envío de registros a uno o varios destinos.</span><span class="sxs-lookup"><span data-stu-id="dceaf-169">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="dceaf-170">Se pueden usar plataformas de registro de terceros.</span><span class="sxs-lookup"><span data-stu-id="dceaf-170">Third-party logging frameworks can be used.</span></span>

[<span data-ttu-id="dceaf-171">Registro</span><span class="sxs-lookup"><span data-stu-id="dceaf-171">Logging</span></span>](xref:fundamentals/logging/index)

## <a name="error-handling"></a><span data-ttu-id="dceaf-172">Control de errores</span><span class="sxs-lookup"><span data-stu-id="dceaf-172">Error handling</span></span>

<span data-ttu-id="dceaf-173">ASP.NET Core tiene características integradas para controlar los errores en las aplicaciones, incluida una página de excepciones de desarrollador, páginas de errores personalizados, páginas de códigos de estado estáticos y control de excepciones de inicio.</span><span class="sxs-lookup"><span data-stu-id="dceaf-173">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="dceaf-174">Para más información, vea [Introducción al control de errores](xref:fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="dceaf-174">For more information, see [how to handle errors](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="dceaf-175">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="dceaf-175">Routing</span></span>

<span data-ttu-id="dceaf-176">ASP.NET Core ofrece características para el enrutamiento de solicitudes de aplicación a los controladores de ruta.</span><span class="sxs-lookup"><span data-stu-id="dceaf-176">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="dceaf-177">Para más información, vea [Enrutamiento](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="dceaf-177">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="dceaf-178">Proveedores de archivos</span><span class="sxs-lookup"><span data-stu-id="dceaf-178">File providers</span></span>

<span data-ttu-id="dceaf-179">ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos, lo que ofrece una interfaz común para trabajar con archivos entre plataformas.</span><span class="sxs-lookup"><span data-stu-id="dceaf-179">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="dceaf-180">Para más información, vea [Proveedores de archivos](xref:fundamentals/file-providers).</span><span class="sxs-lookup"><span data-stu-id="dceaf-180">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="dceaf-181">Archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="dceaf-181">Static files</span></span>

<span data-ttu-id="dceaf-182">El software intermedio de archivos estáticos trabaja con archivos estáticos (por ejemplo, HTML, CSS, imágenes y JavaScript).</span><span class="sxs-lookup"><span data-stu-id="dceaf-182">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="dceaf-183">Para obtener más información, consulte [Archivos estáticos](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="dceaf-183">For more information, see [Static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="dceaf-184">Hospedaje</span><span class="sxs-lookup"><span data-stu-id="dceaf-184">Hosting</span></span>

<span data-ttu-id="dceaf-185">Las aplicaciones ASP.NET Core configuran e inician un *host*. Dicho host es responsable de la administración de inicio y duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dceaf-185">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="dceaf-186">Para obtener más información, consulte [Hospedaje en ASP.NET Core](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="dceaf-186">For more information, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

## <a name="session-and-application-state"></a><span data-ttu-id="dceaf-187">Estado de sesión y aplicación</span><span class="sxs-lookup"><span data-stu-id="dceaf-187">Session and application state</span></span>

<span data-ttu-id="dceaf-188">El estado de sesión es una característica de ASP.NET Core que se puede usar para guardar y almacenar datos de usuario mientras el usuario explora la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dceaf-188">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span>

<span data-ttu-id="dceaf-189">Para más información, vea [Estado de sesión y aplicación](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="dceaf-189">For more information, see [Session and application state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="dceaf-190">Servidores</span><span class="sxs-lookup"><span data-stu-id="dceaf-190">Servers</span></span>

<span data-ttu-id="dceaf-191">El modelo de hospedaje de ASP.NET Core no escucha directamente las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="dceaf-191">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="dceaf-192">El modelo de hospedaje se basa en una implementación de servidor HTTP para reenviar la solicitud a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dceaf-192">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="dceaf-193">La solicitud reenviada se empaqueta como un conjunto de objetos de característica al que se puede tener acceso a través de interfaces.</span><span class="sxs-lookup"><span data-stu-id="dceaf-193">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="dceaf-194">ASP.NET Core incluye un servidor web administrado multiplataforma, denominado [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="dceaf-194">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="dceaf-195">Kestrel se suele ejecutar detrás de un servidor web de producción como [IIS](https://www.iis.net/) o [Nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="dceaf-195">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org).</span></span> <span data-ttu-id="dceaf-196">Kestrel se puede ejecutar como un servidor perimetral.</span><span class="sxs-lookup"><span data-stu-id="dceaf-196">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="dceaf-197">Para más información, vea [Servidores](xref:fundamentals/servers/index) y los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="dceaf-197">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="dceaf-198">Kestrel</span><span class="sxs-lookup"><span data-stu-id="dceaf-198">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="dceaf-199">Módulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dceaf-199">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="dceaf-200">[HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominado [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="dceaf-200">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="dceaf-201">Globalización y localización</span><span class="sxs-lookup"><span data-stu-id="dceaf-201">Globalization and localization</span></span>

<span data-ttu-id="dceaf-202">El hecho de crear un sitio web multilingüe con ASP.NET Core permite que este llegue a un público más amplio.</span><span class="sxs-lookup"><span data-stu-id="dceaf-202">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="dceaf-203">ASP.NET Core proporciona servicios y software intermedio para la localización en diferentes idiomas y referencias culturales.</span><span class="sxs-lookup"><span data-stu-id="dceaf-203">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="dceaf-204">Para más información, vea [Globalización y localización](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="dceaf-204">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="dceaf-205">Características de la solicitud</span><span class="sxs-lookup"><span data-stu-id="dceaf-205">Request features</span></span>

<span data-ttu-id="dceaf-206">Los detalles de implementación del servidor web relacionados con las solicitudes HTTP y las respuestas se definen en las interfaces.</span><span class="sxs-lookup"><span data-stu-id="dceaf-206">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="dceaf-207">En las implementaciones del servidor web y el software intermedio se usan estas interfaces para crear y modificar la canalización de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dceaf-207">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="dceaf-208">Para más información, vea [Características de la solicitud](xref:fundamentals/request-features).</span><span class="sxs-lookup"><span data-stu-id="dceaf-208">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="background-tasks"></a><span data-ttu-id="dceaf-209">Tareas en segundo plano</span><span class="sxs-lookup"><span data-stu-id="dceaf-209">Background tasks</span></span>

<span data-ttu-id="dceaf-210">Las tareas en segundo plano se implementan como *servicios hospedados*.</span><span class="sxs-lookup"><span data-stu-id="dceaf-210">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="dceaf-211">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="dceaf-211">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span>

<span data-ttu-id="dceaf-212">Para más información, consulte [Tareas en segundo plano con servicios hospedados](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="dceaf-212">For more information, see [Background tasks with hosted services](xref:fundamentals/host/hosted-services).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="dceaf-213">Interfaz web abierta para .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="dceaf-213">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="dceaf-214">ASP.NET Core es compatible con la interfaz web abierta para .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="dceaf-214">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="dceaf-215">OWIN permite que las aplicaciones web se desacoplen de los servidores web.</span><span class="sxs-lookup"><span data-stu-id="dceaf-215">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="dceaf-216">Para más información, vea [Interfaz web abierta para .NET (OWIN)](xref:fundamentals/owin).</span><span class="sxs-lookup"><span data-stu-id="dceaf-216">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="dceaf-217">WebSockets</span><span class="sxs-lookup"><span data-stu-id="dceaf-217">WebSockets</span></span>

<span data-ttu-id="dceaf-218">[WebSocket](https://wikipedia.org/wiki/WebSocket) es un protocolo que habilita canales de comunicación bidireccional persistentes a través de conexiones TCP.</span><span class="sxs-lookup"><span data-stu-id="dceaf-218">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="dceaf-219">Se usa para aplicaciones de chat, tableros de cotizaciones, juegos y donde se necesite funcionalidad en tiempo real en una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dceaf-219">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="dceaf-220">ASP.NET Core es compatible con características de socket web.</span><span class="sxs-lookup"><span data-stu-id="dceaf-220">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="dceaf-221">Para más información, vea [WebSockets](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="dceaf-221">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="dceaf-222">Metapaquete Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="dceaf-222">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="dceaf-223">El metapaquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="dceaf-223">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="dceaf-224">Todos los paquetes admitidos por el equipo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dceaf-224">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="dceaf-225">Todos los paquetes admitidos por Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="dceaf-225">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="dceaf-226">Dependencias internas y de terceros usadas por ASP.NET Core y Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="dceaf-226">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="dceaf-227">Para más información, vea [Metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="dceaf-227">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="dceaf-228">Entorno de ejecución de .NET Core frente a .NET Framework</span><span class="sxs-lookup"><span data-stu-id="dceaf-228">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="dceaf-229">Una aplicación de ASP.NET Core puede tener como destino el entorno de ejecución de .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="dceaf-229">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="dceaf-230">Para más información, vea [Selección entre .NET Core y .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="dceaf-230">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="dceaf-231">Elección entre ASP.NET Core y ASP.NET</span><span class="sxs-lookup"><span data-stu-id="dceaf-231">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="dceaf-232">Para más información sobre cómo elegir entre ASP.NET Core y ASP.NET, vea [Elección entre ASP.NET Core y ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="dceaf-232">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
