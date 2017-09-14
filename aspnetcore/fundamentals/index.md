---
title: "Conceptos básicos de ASP.NET Core"
author: rick-anderson
description: "En este artículo se proporciona información general de los conceptos fundamentales que deben conocerse al compilar aplicaciones de ASP.NET Core."
keywords: "ASP.NET Core, conceptos básicos, información general"
ms.author: riande
manager: wpickett
ms.date: 08/18/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5d8ca35b0e2e4b6e9b1ec745a3a7cf7c3983c461
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="aspnet-core-fundamentals-overview"></a><span data-ttu-id="69860-104">Información general de los conceptos básicos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="69860-104">ASP.NET Core fundamentals overview</span></span>

<span data-ttu-id="69860-105">Una aplicación de ASP.NET Core es una aplicación de consola que crea un servidor web en su método `Main`:</span><span class="sxs-lookup"><span data-stu-id="69860-105">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69860-106">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69860-106">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="69860-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span><span class="sxs-lookup"><span data-stu-id="69860-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span></span>

<span data-ttu-id="69860-108">El método `Main` invoca a `WebHost.CreateDefaultBuilder`, que sigue el patrón de generador para crear un host de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="69860-108">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="69860-109">El generador tiene métodos que definen el servidor web (por ejemplo, `UseKestrel`) y la clase de inicio (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="69860-109">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="69860-110">En el ejemplo anterior, se asigna automáticamente un servidor web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="69860-110">In the preceding example, a [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="69860-111">El host web de ASP.NET Core intentará ejecutarse en IIS, si está disponible.</span><span class="sxs-lookup"><span data-stu-id="69860-111">ASP.NET Core's web host will attempt to run on IIS, if it is available.</span></span> <span data-ttu-id="69860-112">Otros servidores web, como [HTTP.sys](xref:fundamentals/servers/httpsys), se pueden usar al invocar el método de extensión adecuado.</span><span class="sxs-lookup"><span data-stu-id="69860-112">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="69860-113">`UseStartup` se explica en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="69860-113">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="69860-114">`IWebHostBuilder`, el tipo de valor devuelto de la invocación `WebHost.CreateDefaultBuilder`, proporciona muchos métodos opcionales.</span><span class="sxs-lookup"><span data-stu-id="69860-114">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="69860-115">Algunos de estos métodos incluyen `UseHttpSys` para hospedar la aplicación en HTTP.sys y `UseContentRoot` para especificar el directorio de contenido raíz.</span><span class="sxs-lookup"><span data-stu-id="69860-115">Some of these methods include `UseHttpSys` for hosting the application in HTTP.sys, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="69860-116">Los métodos `Build` y `Run` crean el objeto `IWebHost` que hospedará la aplicación y empiezan a escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="69860-116">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69860-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69860-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="69860-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="69860-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span></span>

<span data-ttu-id="69860-119">El método `Main` usa `WebHostBuilder`, que sigue el patrón de generador para crear un host de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="69860-119">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="69860-120">El generador tiene métodos que definen el servidor web (por ejemplo, `UseKestrel`) y la clase de inicio (`UseStartup`).</span><span class="sxs-lookup"><span data-stu-id="69860-120">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="69860-121">En el ejemplo anterior, se usa el servidor web [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="69860-121">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="69860-122">Si se invoca el método de extensión adecuado, se pueden usar otros servidores web, como [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="69860-122">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="69860-123">`UseStartup` se explica en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="69860-123">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="69860-124">`WebHostBuilder` proporciona muchos métodos opcionales, incluidos `UseIISIntegration` para hospedar en IIS e IIS Express y `UseContentRoot` para especificar el directorio de contenido raíz.</span><span class="sxs-lookup"><span data-stu-id="69860-124">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="69860-125">Los métodos `Build` y `Run` crean el objeto `IWebHost` que hospedará la aplicación y empiezan a escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="69860-125">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="69860-126">Inicio</span><span class="sxs-lookup"><span data-stu-id="69860-126">Startup</span></span>

<span data-ttu-id="69860-127">El método `UseStartup` de `WebHostBuilder` especifica la clase `Startup` para la aplicación:</span><span class="sxs-lookup"><span data-stu-id="69860-127">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69860-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69860-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="69860-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="69860-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69860-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69860-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="69860-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="69860-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span></span>

---

<span data-ttu-id="69860-132">La clase `Startup` es donde se define la canalización de control de la solicitud y donde se configuran los servicios necesarios para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="69860-132">The `Startup` class is where you define the request handling pipeline and where any services needed by the application are configured.</span></span> <span data-ttu-id="69860-133">La clase `Startup` debe ser pública y contener los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="69860-133">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

* <span data-ttu-id="69860-134">`ConfigureServices` define los [Servicios](#services) usados por la aplicación (por ejemplo, ASP.NET Core MVC, Entity Framework Core, Identity, etc.).</span><span class="sxs-lookup"><span data-stu-id="69860-134">`ConfigureServices` defines the [Services](#services) used by your application (such as ASP.NET Core MVC, Entity Framework Core, Identity, etc.).</span></span>

* <span data-ttu-id="69860-135">`Configure` define el [software intermedio](xref:fundamentals/middleware) en la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="69860-135">`Configure` defines the [middleware](xref:fundamentals/middleware) in the request pipeline.</span></span>

<span data-ttu-id="69860-136">Para obtener más información, vea [Application startup](xref:fundamentals/startup) (Inicio de la aplicación).</span><span class="sxs-lookup"><span data-stu-id="69860-136">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="services"></a><span data-ttu-id="69860-137">Servicios</span><span class="sxs-lookup"><span data-stu-id="69860-137">Services</span></span>

<span data-ttu-id="69860-138">Un servicio es un componente que está pensado para su uso común en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="69860-138">A service is a component that is intended for common consumption in an application.</span></span> <span data-ttu-id="69860-139">Los servicios se ponen a disposición a través de la [inserción de dependencias](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="69860-139">Services are made available through [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="69860-140">ASP.NET Core incluye un controlador de inversión nativa del control (IoC) que admite la [inserción de constructores](xref:mvc/controllers/dependency-injection#constructor-injection) de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="69860-140">ASP.NET Core includes a native inversion of control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="69860-141">El contenedor nativo puede reemplazarse con el contenedor que quiera.</span><span class="sxs-lookup"><span data-stu-id="69860-141">The native container can be replaced with your container of choice.</span></span> <span data-ttu-id="69860-142">Además de la ventaja de acoplamiento flexible, DI hace que los servicios estén disponibles en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="69860-142">In addition to its loose coupling benefit, DI makes services available throughout your application.</span></span> <span data-ttu-id="69860-143">Por ejemplo, el [registro](xref:fundamentals/logging) está disponible en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="69860-143">For example, [logging](xref:fundamentals/logging) is available throughout your application.</span></span>

<span data-ttu-id="69860-144">Para obtener más información, consulte [Inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="69860-144">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="69860-145">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="69860-145">Middleware</span></span>

<span data-ttu-id="69860-146">En ASP.NET Core, se crea la canalización de solicitudes mediante [software intermedio](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="69860-146">In ASP.NET Core, you compose your request pipeline using [Middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="69860-147">El software intermedio de ASP.NET Core lleva a cabo la lógica asincrónica en `HttpContext` y después invoca al siguiente software intermedio de la secuencia o finaliza la solicitud directamente.</span><span class="sxs-lookup"><span data-stu-id="69860-147">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="69860-148">Se agrega un componente de software intermedio denominado "XYZ" al invocar a un método de extensión `UseXYZ` en el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="69860-148">A middleware component called "XYZ" is added by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="69860-149">ASP.NET Core incluye un amplio conjunto de software intermedio integrado:</span><span class="sxs-lookup"><span data-stu-id="69860-149">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="69860-150">Archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="69860-150">Static files</span></span>](xref:fundamentals/static-files)

* [<span data-ttu-id="69860-151">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="69860-151">Routing</span></span>](xref:fundamentals/routing)

* [<span data-ttu-id="69860-152">Autenticación</span><span class="sxs-lookup"><span data-stu-id="69860-152">Authentication</span></span>](xref:security/authentication/index)

<span data-ttu-id="69860-153">Puede usar cualquier software intermedio basado en [OWIN](http://owin.org) con ASP.NET Core y puede escribir su propio software intermedio personalizado.</span><span class="sxs-lookup"><span data-stu-id="69860-153">You can use any [OWIN](http://owin.org)-based middleware with ASP.NET Core, and you can write your own custom middleware.</span></span>

<span data-ttu-id="69860-154">Para obtener más información, consulte [Middleware](xref:fundamentals/middleware) (Software intermedio) y [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) [Interfaz web abierta para .NET (OWIN)].</span><span class="sxs-lookup"><span data-stu-id="69860-154">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="servers"></a><span data-ttu-id="69860-155">Servidores</span><span class="sxs-lookup"><span data-stu-id="69860-155">Servers</span></span>

<span data-ttu-id="69860-156">El modelo de hospedaje de ASP.NET Core no escucha directamente las solicitudes; en su lugar, se basa en una implementación de servidor HTTP para reenviar la solicitud a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="69860-156">The ASP.NET Core hosting model does not directly listen for requests; rather, it relies on an HTTP server implementation to forward the request to the application.</span></span> <span data-ttu-id="69860-157">La solicitud reenviada se empaqueta como un conjunto de objetos de característica al que se puede tener acceso a través de las interfaces.</span><span class="sxs-lookup"><span data-stu-id="69860-157">The forwarded request is wrapped as a set of feature objects that you can access through interfaces.</span></span> <span data-ttu-id="69860-158">La aplicación crea este conjunto en un `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="69860-158">The application composes this set into an `HttpContext`.</span></span> <span data-ttu-id="69860-159">ASP.NET Core incluye un servidor web administrado multiplataforma, denominado [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="69860-159">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="69860-160">Kestrel se suele ejecutar detrás de un servidor web de producción como [IIS](https://www.iis.net/) o [nginx](http://nginx.org).</span><span class="sxs-lookup"><span data-stu-id="69860-160">Kestrel is typically run behind a production web server like [IIS](https://www.iis.net/) or [nginx](http://nginx.org).</span></span>

<span data-ttu-id="69860-161">Para obtener más información, consulte [Servers](xref:fundamentals/servers/index) (Servidores) y [Hosting](xref:fundamentals/hosting) (Hospedaje).</span><span class="sxs-lookup"><span data-stu-id="69860-161">For more information, see [Servers](xref:fundamentals/servers/index) and [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="content-root"></a><span data-ttu-id="69860-162">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="69860-162">Content root</span></span>

<span data-ttu-id="69860-163">La raíz del contenido es la ruta de acceso base a cualquier contenido que usa la aplicación, como vistas, [páginas de Razor](xref:mvc/razor-pages/index) y activos estáticos.</span><span class="sxs-lookup"><span data-stu-id="69860-163">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="69860-164">De forma predeterminada, la raíz del contenido es la misma que la ruta de acceso base de la aplicación para el ejecutable que hospeda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="69860-164">By default, the content root is the same as application base path for the executable hosting the application.</span></span> <span data-ttu-id="69860-165">Con `WebHostBuilder` se especifica una ubicación alternativa para la raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="69860-165">An alternative location for content root is specified with `WebHostBuilder`.</span></span>

## <a name="web-root"></a><span data-ttu-id="69860-166">Raíz web</span><span class="sxs-lookup"><span data-stu-id="69860-166">Web root</span></span>

<span data-ttu-id="69860-167">La raíz web de una aplicación es el directorio en el proyecto que contiene recursos públicos y estáticos, como archivos de imagen, CSS y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="69860-167">The web root of an application is the directory in the project containing public, static resources like CSS, JavaScript, and image files.</span></span> <span data-ttu-id="69860-168">De forma predeterminada, el software intermedio de archivos estáticos solo ofrecerá archivos desde el directorio raíz web y sus subdirectorios.</span><span class="sxs-lookup"><span data-stu-id="69860-168">By default, the static files middleware will only serve files from the web root directory and its sub-directories.</span></span> <span data-ttu-id="69860-169">Consulte [Trabajar con archivos estáticos](xref:fundamentals/static-files) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="69860-169">See [working with static files](xref:fundamentals/static-files) for more info.</span></span> <span data-ttu-id="69860-170">El valor predeterminado de la ruta de acceso de raíz web es */wwwroot*, pero puede especificar una ubicación diferente mediante `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="69860-170">The web root path defaults to */wwwroot*, but you can specify a different location using the `WebHostBuilder`.</span></span>

## <a name="configuration"></a><span data-ttu-id="69860-171">Configuración</span><span class="sxs-lookup"><span data-stu-id="69860-171">Configuration</span></span>

<span data-ttu-id="69860-172">ASP.NET Core usa un nuevo modelo de configuración para administrar pares nombre-valor simples.</span><span class="sxs-lookup"><span data-stu-id="69860-172">ASP.NET Core uses a new configuration model for handling simple name-value pairs.</span></span> <span data-ttu-id="69860-173">El nuevo modelo de configuración no se basa en `System.Configuration` o *web.config*; en su lugar, extrae de un conjunto ordenado de proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="69860-173">The new configuration model is not based on `System.Configuration` or *web.config*; rather, it pulls from an ordered set of configuration providers.</span></span> <span data-ttu-id="69860-174">Los proveedores de configuración integrados admiten una variedad de formatos de archivo (XML, JSON, INI) y variables de entorno para habilitar la configuración basada en el entorno.</span><span class="sxs-lookup"><span data-stu-id="69860-174">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="69860-175">También puede escribir sus propios proveedores de configuración personalizados.</span><span class="sxs-lookup"><span data-stu-id="69860-175">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="69860-176">Para obtener más información, vea [Configuración](xref:fundamentals/configuration).</span><span class="sxs-lookup"><span data-stu-id="69860-176">For more information, see [Configuration](xref:fundamentals/configuration).</span></span>

## <a name="environments"></a><span data-ttu-id="69860-177">Entornos</span><span class="sxs-lookup"><span data-stu-id="69860-177">Environments</span></span>

<span data-ttu-id="69860-178">Los entornos, como "Desarrollo" y "Producción", son un concepto de primera clase en ASP.NET Core y se pueden establecer mediante variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="69860-178">Environments, like "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="69860-179">Para obtener más información, consulte [Trabajar con varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="69860-179">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="69860-180">Entorno de ejecución de .NET Core frente a .NET Framework</span><span class="sxs-lookup"><span data-stu-id="69860-180">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="69860-181">Una aplicación de ASP.NET Core puede tener como destino el entorno de ejecución de .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="69860-181">An ASP.NET Core application can target the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="69860-182">Para obtener más información, consulte [Selección entre .NET Core y .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="69860-182">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="additional-information"></a><span data-ttu-id="69860-183">Información adicional</span><span class="sxs-lookup"><span data-stu-id="69860-183">Additional information</span></span>

<span data-ttu-id="69860-184">También puede consultar los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="69860-184">See also the following topics:</span></span>

- [<span data-ttu-id="69860-185">Control de errores</span><span class="sxs-lookup"><span data-stu-id="69860-185">Error Handling</span></span>](xref:fundamentals/error-handling)
- [<span data-ttu-id="69860-186">Proveedores de archivos</span><span class="sxs-lookup"><span data-stu-id="69860-186">File Providers</span></span>](xref:fundamentals/file-providers)
- [<span data-ttu-id="69860-187">Globalización y localización</span><span class="sxs-lookup"><span data-stu-id="69860-187">Globalization and localization</span></span>](xref:fundamentals/localization)
- [<span data-ttu-id="69860-188">Registro</span><span class="sxs-lookup"><span data-stu-id="69860-188">Logging</span></span>](xref:fundamentals/logging)
- [<span data-ttu-id="69860-189">Administración del estado de la aplicación</span><span class="sxs-lookup"><span data-stu-id="69860-189">Managing Application State</span></span>](xref:fundamentals/app-state)
