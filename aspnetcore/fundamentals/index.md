---
title: Conceptos básicos de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre los conceptos básicos para crear aplicaciones de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: fundamentals/index
ms.openlocfilehash: 7533242140c31a937f32cc9082d760103347ce25
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219186"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="a8206-103">Conceptos básicos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8206-103">ASP.NET Core fundamentals</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a8206-104">Este artículo es una introducción de los temas clave para entender cómo desarrollar aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8206-104">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="a8206-105">Clase Startup</span><span class="sxs-lookup"><span data-stu-id="a8206-105">The Startup class</span></span>

<span data-ttu-id="a8206-106">La clase `Startup` es donde:</span><span class="sxs-lookup"><span data-stu-id="a8206-106">The `Startup` class is where:</span></span>

* <span data-ttu-id="a8206-107">Se configuran los servicios requeridos por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8206-107">Services required by the app are configured.</span></span>
* <span data-ttu-id="a8206-108">Se define la solicitud de canalización.</span><span class="sxs-lookup"><span data-stu-id="a8206-108">The request handling pipeline is defined.</span></span>

<span data-ttu-id="a8206-109">Los *servicios* son componentes que usan la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8206-109">*Services* are components that are used by the app.</span></span> <span data-ttu-id="a8206-110">Por ejemplo, un componente de registro es un servicio.</span><span class="sxs-lookup"><span data-stu-id="a8206-110">For example, a logging component is a service.</span></span> <span data-ttu-id="a8206-111">Se agrega al método `Startup.ConfigureServices` el código para configurar (o *registrar*) servicios.</span><span class="sxs-lookup"><span data-stu-id="a8206-111">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="a8206-112">La canalización de control de solicitudes se compone de una serie de *componentes de software intermedio*.</span><span class="sxs-lookup"><span data-stu-id="a8206-112">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="a8206-113">Por ejemplo, un software intermedio podría controlar las solicitudes de archivos estáticos o redirigir las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a8206-113">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="a8206-114">Cada software intermedio lleva a cabo las operaciones asincrónicas en un contexto `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="a8206-114">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="a8206-115">Se agrega al método `Startup.Configure` el código para configurar la canalización de control de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="a8206-115">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="a8206-116">Aquí tiene una clase `Startup` de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a8206-116">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="a8206-117">Para obtener más información, vea <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="a8206-117">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="a8206-118">Inserción de dependencias (servicios)</span><span class="sxs-lookup"><span data-stu-id="a8206-118">Dependency injection (services)</span></span>

<span data-ttu-id="a8206-119">ASP.NET Core tiene un marco de inserción de dependencias (DI) integrado que pone a disposición los servicios configurados para las clases de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8206-119">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="a8206-120">Una manera de obtener una instancia de un servicio en una clase es crear un constructor con un parámetro del tipo necesario.</span><span class="sxs-lookup"><span data-stu-id="a8206-120">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="a8206-121">El parámetro puede ser el tipo de servicio o una interfaz.</span><span class="sxs-lookup"><span data-stu-id="a8206-121">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="a8206-122">El sistema de DI proporciona el servicio en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="a8206-122">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="a8206-123">Aquí tiene una clase que usa DI para obtener un objeto de contexto de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a8206-123">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="a8206-124">La línea resaltada es un ejemplo de inserción de constructor:</span><span class="sxs-lookup"><span data-stu-id="a8206-124">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="a8206-125">Aunque DI está integrada, está diseñada para permitirle conectar un contenedor de inversión de control (IoC) de terceros si lo prefiere.</span><span class="sxs-lookup"><span data-stu-id="a8206-125">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="a8206-126">Para obtener más información, vea <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="a8206-126">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="a8206-127">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="a8206-127">Middleware</span></span>

<span data-ttu-id="a8206-128">La canalización de control de solicitudes se compone de una serie de componentes de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="a8206-128">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="a8206-129">Cada componente lleva a cabo las operaciones asincrónicas en un contexto `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="a8206-129">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="a8206-130">Normalmente, se agrega un componente de software intermedio a la canalización al invocar su método de extensión `Use...` en el método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a8206-130">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="a8206-131">Por ejemplo, para habilitar la representación de los archivos estáticos, llame a `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="a8206-131">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="a8206-132">El código resaltado en el ejemplo siguiente configura la canalización de control de solicitudes:</span><span class="sxs-lookup"><span data-stu-id="a8206-132">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="a8206-133">ASP.NET Core incluye una gran variedad de software intermedio integrado. Además, puede escribir software intermedio personalizado.</span><span class="sxs-lookup"><span data-stu-id="a8206-133">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="a8206-134">Para obtener más información, vea <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="a8206-134">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="a8206-135">administrador de flujos de trabajo</span><span class="sxs-lookup"><span data-stu-id="a8206-135">Host</span></span>

<span data-ttu-id="a8206-136">Una aplicación de ASP.NET Core compila un *host* durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="a8206-136">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="a8206-137">El host es un objeto que encapsula todos los recursos de la aplicación, como:</span><span class="sxs-lookup"><span data-stu-id="a8206-137">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="a8206-138">Una implementación de servidor de HTTP</span><span class="sxs-lookup"><span data-stu-id="a8206-138">An HTTP server implementation</span></span>
* <span data-ttu-id="a8206-139">Componentes de software intermedio</span><span class="sxs-lookup"><span data-stu-id="a8206-139">Middleware components</span></span>
* <span data-ttu-id="a8206-140">Registro</span><span class="sxs-lookup"><span data-stu-id="a8206-140">Logging</span></span>
* <span data-ttu-id="a8206-141">DI</span><span class="sxs-lookup"><span data-stu-id="a8206-141">DI</span></span>
* <span data-ttu-id="a8206-142">Configuración</span><span class="sxs-lookup"><span data-stu-id="a8206-142">Configuration</span></span>

<span data-ttu-id="a8206-143">La razón principal para incluir todos los recursos interdependientes de la aplicación en un objeto es la administración de la duración: el control sobre el inicio de la aplicación y el apagado estable.</span><span class="sxs-lookup"><span data-stu-id="a8206-143">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="a8206-144">Hay dos hosts disponibles: el host genérico y el host web.</span><span class="sxs-lookup"><span data-stu-id="a8206-144">Two hosts are available: the Generic Host and the Web Host.</span></span> <span data-ttu-id="a8206-145">Se recomienda el host genérico, mientras que el host web está disponible solo para compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="a8206-145">The Generic Host is recommended, and the Web Host is available only for backwards compatibility.</span></span>

<span data-ttu-id="a8206-146">El código para crear un host está en `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="a8206-146">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

<span data-ttu-id="a8206-147">Los métodos `CreateDefaultBuilder` y `ConfigureWebHostDefaults` configuran un host con opciones de uso común, como las siguientes:</span><span class="sxs-lookup"><span data-stu-id="a8206-147">The `CreateDefaultBuilder` and `ConfigureWebHostDefaults` methods configure a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="a8206-148">Use [Kestrel](#servers) como servidor web y habilite la integración de IIS.</span><span class="sxs-lookup"><span data-stu-id="a8206-148">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="a8206-149">Cargue la configuración de *appsettings.json*, *appsettings.[nombre del entorno].json*, las variables de entorno, los argumentos de línea de comandos y otros orígenes de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8206-149">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="a8206-150">Envíe la salida de registro a la consola y los proveedores de depuración.</span><span class="sxs-lookup"><span data-stu-id="a8206-150">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="a8206-151">Para obtener más información, vea <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="a8206-151">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="a8206-152">Escenarios que no son web</span><span class="sxs-lookup"><span data-stu-id="a8206-152">Non-web scenarios</span></span>

<span data-ttu-id="a8206-153">El host genérico permite que otros tipos de aplicaciones usen las extensiones de marcos transversales, como el registro, la inserción de dependencias (DI), la configuración y la administración de la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8206-153">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="a8206-154">Para obtener más información, vea <xref:fundamentals/host/generic-host> y <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="a8206-154">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="a8206-155">Servidores</span><span class="sxs-lookup"><span data-stu-id="a8206-155">Servers</span></span>

<span data-ttu-id="a8206-156">Una aplicación ASP.NET Core usa una implementación de servidor HTTP para escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="a8206-156">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="a8206-157">El servidor expone las solicitudes a la aplicación como un conjunto de [características de solicitud](xref:fundamentals/request-features) integradas en un contexto `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="a8206-157">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

# <a name="windows"></a>[<span data-ttu-id="a8206-158">Windows</span><span class="sxs-lookup"><span data-stu-id="a8206-158">Windows</span></span>](#tab/windows)

<span data-ttu-id="a8206-159">ASP.NET Core proporciona las siguientes implementaciones de servidor:</span><span class="sxs-lookup"><span data-stu-id="a8206-159">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="a8206-160">*Kestrel* es un servidor web multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="a8206-160">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="a8206-161">Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="a8206-161">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="a8206-162">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="a8206-162">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="a8206-163">*Servidor HTTP de IIS* es un servidor para Windows que usa IIS.</span><span class="sxs-lookup"><span data-stu-id="a8206-163">*IIS HTTP Server* is a server for Windows that uses IIS.</span></span> <span data-ttu-id="a8206-164">Con este servidor, la aplicación de ASP.NET Core e IIS se ejecutan en el mismo proceso.</span><span class="sxs-lookup"><span data-stu-id="a8206-164">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="a8206-165">*HTTP.sys* es un servidor de Windows que no se usa con IIS.</span><span class="sxs-lookup"><span data-stu-id="a8206-165">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="a8206-166">macOS</span><span class="sxs-lookup"><span data-stu-id="a8206-166">macOS</span></span>](#tab/macos)

<span data-ttu-id="a8206-167">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="a8206-167">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="a8206-168">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="a8206-168">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="a8206-169">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a8206-169">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="a8206-170">Linux</span><span class="sxs-lookup"><span data-stu-id="a8206-170">Linux</span></span>](#tab/linux)

<span data-ttu-id="a8206-171">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="a8206-171">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="a8206-172">En ASP.NET Core 2.0 y versiones posteriores, Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="a8206-172">In ASP.NET Core 2.0 or later, Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="a8206-173">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a8206-173">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

<span data-ttu-id="a8206-174">Para obtener más información, vea <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="a8206-174">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="a8206-175">Configuración</span><span class="sxs-lookup"><span data-stu-id="a8206-175">Configuration</span></span>

<span data-ttu-id="a8206-176">ASP.NET Core proporciona un marco de configuración que obtiene la configuración como pares nombre/valor de un conjunto ordenado de proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8206-176">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="a8206-177">Hay proveedores de configuración integrados para una gran variedad de orígenes, como archivos *.json* y *.xml*, variables de entorno y argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="a8206-177">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="a8206-178">También puede escribir proveedores de configuración personalizados.</span><span class="sxs-lookup"><span data-stu-id="a8206-178">You can also write custom configuration providers.</span></span>

<span data-ttu-id="a8206-179">Por ejemplo, puede especificar que la configuración procede de *appsettings.json* y las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="a8206-179">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="a8206-180">Después, cuando se solicite el valor de *ConnectionString*, el marco buscará primero en el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8206-180">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="a8206-181">Si se encuentra el valor allí, pero también en una variable de entorno, el valor de la variable de entorno tendría prioridad.</span><span class="sxs-lookup"><span data-stu-id="a8206-181">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="a8206-182">Para administrar los datos de configuración confidencial como las contraseñas, ASP.NET Core proporciona una [herramienta de administrador secreto](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="a8206-182">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="a8206-183">Para los secretos de producción, se recomienda [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="a8206-183">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="a8206-184">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a8206-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="a8206-185">Opciones</span><span class="sxs-lookup"><span data-stu-id="a8206-185">Options</span></span>

<span data-ttu-id="a8206-186">Siempre que sea posible, ASP.NET Core sigue el *patrón de opciones* para almacenar y recuperar los valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8206-186">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="a8206-187">El patrón de opciones usa clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="a8206-187">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="a8206-188">Por ejemplo, el código siguiente define las opciones de WebSockets:</span><span class="sxs-lookup"><span data-stu-id="a8206-188">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="a8206-189">Para obtener más información, vea <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="a8206-189">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="a8206-190">Entornos</span><span class="sxs-lookup"><span data-stu-id="a8206-190">Environments</span></span>

<span data-ttu-id="a8206-191">Los entornos de ejecución, como *desarrollo*, *almacenamiento provisional* y *Producción*, son un concepto de primera clase en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8206-191">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="a8206-192">Puede especificar el entorno que ejecuta una aplicación estableciendo la variable de entorno `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="a8206-192">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="a8206-193">ASP.NET Core lee dicha variable de entorno al inicio de la aplicación y almacena el valor en una implementación `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="a8206-193">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="a8206-194">El objeto de entorno está disponible en cualquier parte de la aplicación a través de DI.</span><span class="sxs-lookup"><span data-stu-id="a8206-194">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="a8206-195">El siguiente ejemplo de código desde la clase `Startup` configura la aplicación para que proporcione información detallada del error solo cuando se ejecuta en el desarrollo:</span><span class="sxs-lookup"><span data-stu-id="a8206-195">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="a8206-196">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="a8206-196">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="a8206-197">Registro</span><span class="sxs-lookup"><span data-stu-id="a8206-197">Logging</span></span>

<span data-ttu-id="a8206-198">ASP.NET Core es compatible con una API de registro que funciona con una gran variedad de proveedores de registro integrados y de terceros.</span><span class="sxs-lookup"><span data-stu-id="a8206-198">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="a8206-199">Entre los proveedores disponibles se incluyen los siguientes:</span><span class="sxs-lookup"><span data-stu-id="a8206-199">Available providers include the following:</span></span>

* <span data-ttu-id="a8206-200">Consola</span><span class="sxs-lookup"><span data-stu-id="a8206-200">Console</span></span>
* <span data-ttu-id="a8206-201">Depuración</span><span class="sxs-lookup"><span data-stu-id="a8206-201">Debug</span></span>
* <span data-ttu-id="a8206-202">Seguimiento de eventos en Windows</span><span class="sxs-lookup"><span data-stu-id="a8206-202">Event Tracing on Windows</span></span>
* <span data-ttu-id="a8206-203">Registro de errores de Windows</span><span class="sxs-lookup"><span data-stu-id="a8206-203">Windows Event Log</span></span>
* <span data-ttu-id="a8206-204">TraceSource</span><span class="sxs-lookup"><span data-stu-id="a8206-204">TraceSource</span></span>
* <span data-ttu-id="a8206-205">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a8206-205">Azure App Service</span></span>
* <span data-ttu-id="a8206-206">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="a8206-206">Azure Application Insights</span></span>

<span data-ttu-id="a8206-207">Escribir registros desde cualquier lugar en el código de una aplicación mediante la obtención de un objeto `ILogger` de DI y llamar a métodos de registro.</span><span class="sxs-lookup"><span data-stu-id="a8206-207">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="a8206-208">Aquí tiene un código de ejemplo que usa un objeto `ILogger`, con la inserción del constructor y resaltadas las llamadas del método de registro.</span><span class="sxs-lookup"><span data-stu-id="a8206-208">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="a8206-209">La interfaz `ILogger` le permite pasar cualquier número de campos para el proveedor de registro.</span><span class="sxs-lookup"><span data-stu-id="a8206-209">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="a8206-210">Los campos habitualmente se usan para construir una cadena de mensaje, pero el proveedor también puede enviarlos como campos independientes o almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="a8206-210">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="a8206-211">Esta característica permite a los proveedores de registro implementar el [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="a8206-211">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="a8206-212">Para obtener más información, vea <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="a8206-212">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="a8206-213">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="a8206-213">Routing</span></span>

<span data-ttu-id="a8206-214">Una *ruta* es un patrón de dirección URL que se asigna a un controlador.</span><span class="sxs-lookup"><span data-stu-id="a8206-214">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="a8206-215">El controlador normalmente es una página de Razor, un método de acción en un controlador MVC o un software intermedio.</span><span class="sxs-lookup"><span data-stu-id="a8206-215">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="a8206-216">El enrutamiento de ASP.NET Core le permite controlar las direcciones URL usadas por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8206-216">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="a8206-217">Para obtener más información, vea <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="a8206-217">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="a8206-218">Control de errores</span><span class="sxs-lookup"><span data-stu-id="a8206-218">Error handling</span></span>

<span data-ttu-id="a8206-219">ASP.NET Core tiene características integradas para controlar los errores, tales como:</span><span class="sxs-lookup"><span data-stu-id="a8206-219">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="a8206-220">Una página de excepciones para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="a8206-220">A developer exception page</span></span>
* <span data-ttu-id="a8206-221">Páginas de errores personalizados</span><span class="sxs-lookup"><span data-stu-id="a8206-221">Custom error pages</span></span>
* <span data-ttu-id="a8206-222">Páginas de códigos de estado estáticos</span><span class="sxs-lookup"><span data-stu-id="a8206-222">Static status code pages</span></span>
* <span data-ttu-id="a8206-223">Control de excepciones de inicio</span><span class="sxs-lookup"><span data-stu-id="a8206-223">Startup exception handling</span></span>

<span data-ttu-id="a8206-224">Para obtener más información, vea <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="a8206-224">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="a8206-225">Realización de solicitudes HTTP</span><span class="sxs-lookup"><span data-stu-id="a8206-225">Make HTTP requests</span></span>

<span data-ttu-id="a8206-226">Una implementación de `IHttpClientFactory` está disponible para crear instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8206-226">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="a8206-227">Este servicio:</span><span class="sxs-lookup"><span data-stu-id="a8206-227">The factory:</span></span>

* <span data-ttu-id="a8206-228">Proporciona una ubicación central para denominar y configurar instancias de `HttpClient` lógicas.</span><span class="sxs-lookup"><span data-stu-id="a8206-228">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="a8206-229">Así, por ejemplo, se puede registrar y configurar un cliente *github* para tener acceso a GitHub.</span><span class="sxs-lookup"><span data-stu-id="a8206-229">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="a8206-230">y, de igual modo, registrar otro cliente predeterminado para otros fines.</span><span class="sxs-lookup"><span data-stu-id="a8206-230">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="a8206-231">Admite el registro y encadenamiento de varios controladores de delegación para crear una canalización de middleware de solicitud saliente.</span><span class="sxs-lookup"><span data-stu-id="a8206-231">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="a8206-232">Este patrón es similar a la canalización de middleware de entrada de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8206-232">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="a8206-233">Dicho patrón proporciona un mecanismo para administrar cuestiones transversales relativas a las solicitudes HTTP, como el almacenamiento en caché, el control de errores, la serialización y el registro.</span><span class="sxs-lookup"><span data-stu-id="a8206-233">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="a8206-234">Se integra con *Polly*, una conocida biblioteca de terceros para el control de errores transitorios.</span><span class="sxs-lookup"><span data-stu-id="a8206-234">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="a8206-235">Administra la agrupación y duración de las instancias de `HttpClientMessageHandler` subyacentes para evitar los problemas de DNS que suelen producirse al administrar las duraciones de `HttpClient` manualmente.</span><span class="sxs-lookup"><span data-stu-id="a8206-235">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="a8206-236">Agrega una experiencia de registro configurable (a través de `ILogger`) en todas las solicitudes enviadas a través de los clientes creados por Factory.</span><span class="sxs-lookup"><span data-stu-id="a8206-236">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="a8206-237">Para obtener más información, vea <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="a8206-237">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="a8206-238">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="a8206-238">Content root</span></span>

<span data-ttu-id="a8206-239">La raíz del contenido es la ruta de acceso base a:</span><span class="sxs-lookup"><span data-stu-id="a8206-239">The content root is the base path to the:</span></span>

* <span data-ttu-id="a8206-240">El archivo ejecutable que hospeda la aplicación ( *.exe*).</span><span class="sxs-lookup"><span data-stu-id="a8206-240">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="a8206-241">Los ensamblados compilados que componen la aplicación ( *.dll*).</span><span class="sxs-lookup"><span data-stu-id="a8206-241">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="a8206-242">Los archivos de contenido que no son de código usados por la aplicación, como:</span><span class="sxs-lookup"><span data-stu-id="a8206-242">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="a8206-243">Archivos de Razor ( *.cshtml*, *.razor*)</span><span class="sxs-lookup"><span data-stu-id="a8206-243">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="a8206-244">Archivos de configuración ( *.json*, *.xml*)</span><span class="sxs-lookup"><span data-stu-id="a8206-244">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="a8206-245">Archivos de datos ( *.db*)</span><span class="sxs-lookup"><span data-stu-id="a8206-245">Data files (*.db*)</span></span>
* <span data-ttu-id="a8206-246">[Raíz web](#web-root), normalmente la carpeta *wwwroot* publicada.</span><span class="sxs-lookup"><span data-stu-id="a8206-246">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="a8206-247">Durante el desarrollo:</span><span class="sxs-lookup"><span data-stu-id="a8206-247">During development:</span></span>

* <span data-ttu-id="a8206-248">La raíz del contenido tiene como valor predeterminado el directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a8206-248">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="a8206-249">El directorio raíz del proyecto se usa para crear:</span><span class="sxs-lookup"><span data-stu-id="a8206-249">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="a8206-250">La ruta de acceso a los archivos de contenido que no son de código de la aplicación en el directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a8206-250">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="a8206-251">La [raíz web](#web-root), normalmente la carpeta *wwwroot* en el directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a8206-251">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="a8206-252">Se puede especificar una ruta raíz de contenido alternativa al [crear el host](#host).</span><span class="sxs-lookup"><span data-stu-id="a8206-252">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="a8206-253">Para obtener más información, vea <xref:fundamentals/host/generic-host#contentrootpath>.</span><span class="sxs-lookup"><span data-stu-id="a8206-253">For more information, see <xref:fundamentals/host/generic-host#contentrootpath>.</span></span>

## <a name="web-root"></a><span data-ttu-id="a8206-254">Raíz web</span><span class="sxs-lookup"><span data-stu-id="a8206-254">Web root</span></span>

<span data-ttu-id="a8206-255">La raíz web es la ruta de acceso base a archivos de recursos públicos, que no son de código y estáticos, como:</span><span class="sxs-lookup"><span data-stu-id="a8206-255">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="a8206-256">Hojas de estilo ( *.css*)</span><span class="sxs-lookup"><span data-stu-id="a8206-256">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="a8206-257">JavaScript ( *.js*)</span><span class="sxs-lookup"><span data-stu-id="a8206-257">JavaScript (*.js*)</span></span>
* <span data-ttu-id="a8206-258">Imágenes ( *.png*, *.jpg*)</span><span class="sxs-lookup"><span data-stu-id="a8206-258">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="a8206-259">De forma predeterminada, los archivos estáticos se atienden solo desde el directorio raíz web (y los subdirectorios).</span><span class="sxs-lookup"><span data-stu-id="a8206-259">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="a8206-260">El valor predeterminado de la ruta de acceso de la raíz web es *{raíz del contenido}/wwwroot*, pero se puede especificar una raíz web diferente [al crear el host](#host).</span><span class="sxs-lookup"><span data-stu-id="a8206-260">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="a8206-261">Para obtener más información, vea <xref:fundamentals/host/generic-host#webroot>.</span><span class="sxs-lookup"><span data-stu-id="a8206-261">For more information, see <xref:fundamentals/host/generic-host#webroot>.</span></span>

<span data-ttu-id="a8206-262">Evite la publicación de archivos en *wwwroot* con el [\<Content> elemento de proyecto](/visualstudio/msbuild/common-msbuild-project-items#content) en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="a8206-262">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="a8206-263">En el ejemplo siguiente se impide la publicación de contenido en el directorio *wwwroot/local* y en los subdirectorios:</span><span class="sxs-lookup"><span data-stu-id="a8206-263">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="a8206-264">Para evitar la publicación de recursos de identidad estáticos en la raíz web, consulte <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="a8206-264">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

<span data-ttu-id="a8206-265">En los archivos de Razor ( *.cshtml*), la virgulilla `~/` apunta a la raíz web.</span><span class="sxs-lookup"><span data-stu-id="a8206-265">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="a8206-266">Una ruta de acceso que empieza por `~/` se conoce como *ruta de acceso virtual*.</span><span class="sxs-lookup"><span data-stu-id="a8206-266">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="a8206-267">Para obtener más información, vea <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="a8206-267">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a8206-268">Este artículo es una introducción de los temas clave para entender cómo desarrollar aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8206-268">This article is an overview of key topics for understanding how to develop ASP.NET Core apps.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="a8206-269">Clase Startup</span><span class="sxs-lookup"><span data-stu-id="a8206-269">The Startup class</span></span>

<span data-ttu-id="a8206-270">La clase `Startup` es donde:</span><span class="sxs-lookup"><span data-stu-id="a8206-270">The `Startup` class is where:</span></span>

* <span data-ttu-id="a8206-271">Se configuran los servicios requeridos por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8206-271">Services required by the app are configured.</span></span>
* <span data-ttu-id="a8206-272">Se define la solicitud de canalización.</span><span class="sxs-lookup"><span data-stu-id="a8206-272">The request handling pipeline is defined.</span></span>

<span data-ttu-id="a8206-273">Los *servicios* son componentes que usan la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8206-273">*Services* are components that are used by the app.</span></span> <span data-ttu-id="a8206-274">Por ejemplo, un componente de registro es un servicio.</span><span class="sxs-lookup"><span data-stu-id="a8206-274">For example, a logging component is a service.</span></span> <span data-ttu-id="a8206-275">Se agrega al método `Startup.ConfigureServices` el código para configurar (o *registrar*) servicios.</span><span class="sxs-lookup"><span data-stu-id="a8206-275">Code to configure (or *register*) services is added to the `Startup.ConfigureServices` method.</span></span>

<span data-ttu-id="a8206-276">La canalización de control de solicitudes se compone de una serie de *componentes de software intermedio*.</span><span class="sxs-lookup"><span data-stu-id="a8206-276">The request handling pipeline is composed as a series of *middleware* components.</span></span> <span data-ttu-id="a8206-277">Por ejemplo, un software intermedio podría controlar las solicitudes de archivos estáticos o redirigir las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a8206-277">For example, a middleware might handle requests for static files or redirect HTTP requests to HTTPS.</span></span> <span data-ttu-id="a8206-278">Cada software intermedio lleva a cabo las operaciones asincrónicas en un contexto `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="a8206-278">Each middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span> <span data-ttu-id="a8206-279">Se agrega al método `Startup.Configure` el código para configurar la canalización de control de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="a8206-279">Code to configure the request handling pipeline is added to the `Startup.Configure` method.</span></span>

<span data-ttu-id="a8206-280">Aquí tiene una clase `Startup` de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a8206-280">Here's a sample `Startup` class:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

<span data-ttu-id="a8206-281">Para obtener más información, vea <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="a8206-281">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="a8206-282">Inserción de dependencias (servicios)</span><span class="sxs-lookup"><span data-stu-id="a8206-282">Dependency injection (services)</span></span>

<span data-ttu-id="a8206-283">ASP.NET Core tiene un marco de inserción de dependencias (DI) integrado que pone a disposición los servicios configurados para las clases de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8206-283">ASP.NET Core has a built-in dependency injection (DI) framework that makes configured services available to an app's classes.</span></span> <span data-ttu-id="a8206-284">Una manera de obtener una instancia de un servicio en una clase es crear un constructor con un parámetro del tipo necesario.</span><span class="sxs-lookup"><span data-stu-id="a8206-284">One way to get an instance of a service in a class is to create a constructor with a parameter of the required type.</span></span> <span data-ttu-id="a8206-285">El parámetro puede ser el tipo de servicio o una interfaz.</span><span class="sxs-lookup"><span data-stu-id="a8206-285">The parameter can be the service type or an interface.</span></span> <span data-ttu-id="a8206-286">El sistema de DI proporciona el servicio en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="a8206-286">The DI system provides the service at runtime.</span></span>

<span data-ttu-id="a8206-287">Aquí tiene una clase que usa DI para obtener un objeto de contexto de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a8206-287">Here's a class that uses DI to get an Entity Framework Core context object.</span></span> <span data-ttu-id="a8206-288">La línea resaltada es un ejemplo de inserción de constructor:</span><span class="sxs-lookup"><span data-stu-id="a8206-288">The highlighted line is an example of constructor injection:</span></span>

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

<span data-ttu-id="a8206-289">Aunque DI está integrada, está diseñada para permitirle conectar un contenedor de inversión de control (IoC) de terceros si lo prefiere.</span><span class="sxs-lookup"><span data-stu-id="a8206-289">While DI is built in, it's designed to let you plug in a third-party Inversion of Control (IoC) container if you prefer.</span></span>

<span data-ttu-id="a8206-290">Para obtener más información, vea <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="a8206-290">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="a8206-291">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="a8206-291">Middleware</span></span>

<span data-ttu-id="a8206-292">La canalización de control de solicitudes se compone de una serie de componentes de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="a8206-292">The request handling pipeline is composed as a series of middleware components.</span></span> <span data-ttu-id="a8206-293">Cada componente lleva a cabo las operaciones asincrónicas en un contexto `HttpContext` y, después, invoca el siguiente software intermedio de la canalización o finaliza la solicitud.</span><span class="sxs-lookup"><span data-stu-id="a8206-293">Each component performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="a8206-294">Normalmente, se agrega un componente de software intermedio a la canalización al invocar su método de extensión `Use...` en el método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a8206-294">By convention, a middleware component is added to the pipeline by invoking its `Use...` extension method in the `Startup.Configure` method.</span></span> <span data-ttu-id="a8206-295">Por ejemplo, para habilitar la representación de los archivos estáticos, llame a `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="a8206-295">For example, to enable rendering of static files, call `UseStaticFiles`.</span></span>

<span data-ttu-id="a8206-296">El código resaltado en el ejemplo siguiente configura la canalización de control de solicitudes:</span><span class="sxs-lookup"><span data-stu-id="a8206-296">The highlighted code in the following example configures the request handling pipeline:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

<span data-ttu-id="a8206-297">ASP.NET Core incluye una gran variedad de software intermedio integrado. Además, puede escribir software intermedio personalizado.</span><span class="sxs-lookup"><span data-stu-id="a8206-297">ASP.NET Core includes a rich set of built-in middleware, and you can write custom middleware.</span></span>

<span data-ttu-id="a8206-298">Para obtener más información, vea <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="a8206-298">For more information, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="host"></a><span data-ttu-id="a8206-299">administrador de flujos de trabajo</span><span class="sxs-lookup"><span data-stu-id="a8206-299">Host</span></span>

<span data-ttu-id="a8206-300">Una aplicación de ASP.NET Core compila un *host* durante el inicio.</span><span class="sxs-lookup"><span data-stu-id="a8206-300">An ASP.NET Core app builds a *host* on startup.</span></span> <span data-ttu-id="a8206-301">El host es un objeto que encapsula todos los recursos de la aplicación, como:</span><span class="sxs-lookup"><span data-stu-id="a8206-301">The host is an object that encapsulates all of the app's resources, such as:</span></span>

* <span data-ttu-id="a8206-302">Una implementación de servidor de HTTP</span><span class="sxs-lookup"><span data-stu-id="a8206-302">An HTTP server implementation</span></span>
* <span data-ttu-id="a8206-303">Componentes de software intermedio</span><span class="sxs-lookup"><span data-stu-id="a8206-303">Middleware components</span></span>
* <span data-ttu-id="a8206-304">Registro</span><span class="sxs-lookup"><span data-stu-id="a8206-304">Logging</span></span>
* <span data-ttu-id="a8206-305">DI</span><span class="sxs-lookup"><span data-stu-id="a8206-305">DI</span></span>
* <span data-ttu-id="a8206-306">Configuración</span><span class="sxs-lookup"><span data-stu-id="a8206-306">Configuration</span></span>

<span data-ttu-id="a8206-307">La razón principal para incluir todos los recursos interdependientes de la aplicación en un objeto es la administración de la duración: el control sobre el inicio de la aplicación y el apagado estable.</span><span class="sxs-lookup"><span data-stu-id="a8206-307">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="a8206-308">Hay dos hosts disponibles: el host web y el host genérico.</span><span class="sxs-lookup"><span data-stu-id="a8206-308">Two hosts are available: the Web Host and the Generic Host.</span></span> <span data-ttu-id="a8206-309">En ASP.NET Core 2.x, el host genérico es solo para escenarios que no son web.</span><span class="sxs-lookup"><span data-stu-id="a8206-309">In ASP.NET Core 2.x, the Generic Host is only for non-web scenarios.</span></span>

<span data-ttu-id="a8206-310">El código para crear un host está en `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="a8206-310">The code to create a host is in `Program.Main`:</span></span>

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

<span data-ttu-id="a8206-311">El métodos `CreateDefaultBuilder` configura un host con opciones de uso común, como las siguientes:</span><span class="sxs-lookup"><span data-stu-id="a8206-311">The `CreateDefaultBuilder` method configures a host with commonly used options, such as the following:</span></span>

* <span data-ttu-id="a8206-312">Use [Kestrel](#servers) como servidor web y habilite la integración de IIS.</span><span class="sxs-lookup"><span data-stu-id="a8206-312">Use [Kestrel](#servers) as the web server and enable IIS integration.</span></span>
* <span data-ttu-id="a8206-313">Cargue la configuración de *appsettings.json*, *appsettings.[nombre del entorno].json*, las variables de entorno, los argumentos de línea de comandos y otros orígenes de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8206-313">Load configuration from *appsettings.json*, *appsettings.{Environment Name}.json*, environment variables, command line arguments, and other configuration sources.</span></span>
* <span data-ttu-id="a8206-314">Envíe la salida de registro a la consola y los proveedores de depuración.</span><span class="sxs-lookup"><span data-stu-id="a8206-314">Send logging output to the console and debug providers.</span></span>

<span data-ttu-id="a8206-315">Para obtener más información, vea <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="a8206-315">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="non-web-scenarios"></a><span data-ttu-id="a8206-316">Escenarios que no son web</span><span class="sxs-lookup"><span data-stu-id="a8206-316">Non-web scenarios</span></span>

<span data-ttu-id="a8206-317">El host genérico permite que otros tipos de aplicaciones usen las extensiones de marcos transversales, como el registro, la inserción de dependencias (DI), la configuración y la administración de la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8206-317">The Generic Host allows other types of apps to use cross-cutting framework extensions, such as logging, dependency injection (DI), configuration, and app lifetime management.</span></span> <span data-ttu-id="a8206-318">Para obtener más información, vea <xref:fundamentals/host/generic-host> y <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="a8206-318">For more information, see <xref:fundamentals/host/generic-host> and <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="servers"></a><span data-ttu-id="a8206-319">Servidores</span><span class="sxs-lookup"><span data-stu-id="a8206-319">Servers</span></span>

<span data-ttu-id="a8206-320">Una aplicación ASP.NET Core usa una implementación de servidor HTTP para escuchar las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="a8206-320">An ASP.NET Core app uses an HTTP server implementation to listen for HTTP requests.</span></span> <span data-ttu-id="a8206-321">El servidor expone las solicitudes a la aplicación como un conjunto de [características de solicitud](xref:fundamentals/request-features) integradas en un contexto `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="a8206-321">The server surfaces requests to the app as a set of [request features](xref:fundamentals/request-features) composed into an `HttpContext`.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="a8206-322">Windows</span><span class="sxs-lookup"><span data-stu-id="a8206-322">Windows</span></span>](#tab/windows)

<span data-ttu-id="a8206-323">ASP.NET Core proporciona las siguientes implementaciones de servidor:</span><span class="sxs-lookup"><span data-stu-id="a8206-323">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="a8206-324">*Kestrel* es un servidor web multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="a8206-324">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="a8206-325">Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="a8206-325">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="a8206-326">Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="a8206-326">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="a8206-327">Un *servidor HTTP de IIS* es un servidor para Windows que usa IIS.</span><span class="sxs-lookup"><span data-stu-id="a8206-327">*IIS HTTP Server* is a server for windows that uses IIS.</span></span> <span data-ttu-id="a8206-328">Con este servidor, la aplicación de ASP.NET Core e IIS se ejecutan en el mismo proceso.</span><span class="sxs-lookup"><span data-stu-id="a8206-328">With this server, the ASP.NET Core app and IIS run in the same process.</span></span>
* <span data-ttu-id="a8206-329">*HTTP.sys* es un servidor de Windows que no se usa con IIS.</span><span class="sxs-lookup"><span data-stu-id="a8206-329">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="a8206-330">macOS</span><span class="sxs-lookup"><span data-stu-id="a8206-330">macOS</span></span>](#tab/macos)

<span data-ttu-id="a8206-331">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="a8206-331">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="a8206-332">Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="a8206-332">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="a8206-333">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a8206-333">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="a8206-334">Linux</span><span class="sxs-lookup"><span data-stu-id="a8206-334">Linux</span></span>](#tab/linux)

<span data-ttu-id="a8206-335">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="a8206-335">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="a8206-336">Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="a8206-336">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="a8206-337">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a8206-337">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windows"></a>[<span data-ttu-id="a8206-338">Windows</span><span class="sxs-lookup"><span data-stu-id="a8206-338">Windows</span></span>](#tab/windows)

<span data-ttu-id="a8206-339">ASP.NET Core proporciona las siguientes implementaciones de servidor:</span><span class="sxs-lookup"><span data-stu-id="a8206-339">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="a8206-340">*Kestrel* es un servidor web multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="a8206-340">*Kestrel* is a cross-platform web server.</span></span> <span data-ttu-id="a8206-341">Kestrel se suele ejecutar en una configuración de proxy inverso con [IIS](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="a8206-341">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="a8206-342">Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="a8206-342">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span>
* <span data-ttu-id="a8206-343">*HTTP.sys* es un servidor de Windows que no se usa con IIS.</span><span class="sxs-lookup"><span data-stu-id="a8206-343">*HTTP.sys* is a server for Windows that isn't used with IIS.</span></span>

# <a name="macos"></a>[<span data-ttu-id="a8206-344">macOS</span><span class="sxs-lookup"><span data-stu-id="a8206-344">macOS</span></span>](#tab/macos)

<span data-ttu-id="a8206-345">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="a8206-345">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="a8206-346">Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="a8206-346">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="a8206-347">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a8206-347">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

# <a name="linux"></a>[<span data-ttu-id="a8206-348">Linux</span><span class="sxs-lookup"><span data-stu-id="a8206-348">Linux</span></span>](#tab/linux)

<span data-ttu-id="a8206-349">ASP.NET Core proporciona la implementación de servidor multiplataforma *Kestrel*.</span><span class="sxs-lookup"><span data-stu-id="a8206-349">ASP.NET Core provides the *Kestrel* cross-platform server implementation.</span></span> <span data-ttu-id="a8206-350">Kestrel puede ejecutarse como servidor perimetral de acceso público expuesto directamente a Internet.</span><span class="sxs-lookup"><span data-stu-id="a8206-350">Kestrel can be run as a public-facing edge server exposed directly to the Internet.</span></span> <span data-ttu-id="a8206-351">Kestrel se suele ejecutar en una configuración de proxy inverso con [Nginx](https://nginx.org) o [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a8206-351">Kestrel is often run in a reverse proxy configuration with [Nginx](https://nginx.org) or [Apache](https://httpd.apache.org/).</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a8206-352">Para obtener más información, vea <xref:fundamentals/servers/index>.</span><span class="sxs-lookup"><span data-stu-id="a8206-352">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="a8206-353">Configuración</span><span class="sxs-lookup"><span data-stu-id="a8206-353">Configuration</span></span>

<span data-ttu-id="a8206-354">ASP.NET Core proporciona un marco de configuración que obtiene la configuración como pares nombre/valor de un conjunto ordenado de proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8206-354">ASP.NET Core provides a configuration framework that gets settings as name-value pairs from an ordered set of configuration providers.</span></span> <span data-ttu-id="a8206-355">Hay proveedores de configuración integrados para una gran variedad de orígenes, como archivos *.json* y *.xml*, variables de entorno y argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="a8206-355">There are built-in configuration providers for a variety of sources, such as *.json* files, *.xml* files, environment variables, and command-line arguments.</span></span> <span data-ttu-id="a8206-356">También puede escribir proveedores de configuración personalizados.</span><span class="sxs-lookup"><span data-stu-id="a8206-356">You can also write custom configuration providers.</span></span>

<span data-ttu-id="a8206-357">Por ejemplo, puede especificar que la configuración procede de *appsettings.json* y las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="a8206-357">For example, you could specify that configuration comes from *appsettings.json* and environment variables.</span></span> <span data-ttu-id="a8206-358">Después, cuando se solicite el valor de *ConnectionString*, el marco buscará primero en el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8206-358">Then when the value of *ConnectionString* is requested, the framework looks first in the *appsettings.json* file.</span></span> <span data-ttu-id="a8206-359">Si se encuentra el valor allí, pero también en una variable de entorno, el valor de la variable de entorno tendría prioridad.</span><span class="sxs-lookup"><span data-stu-id="a8206-359">If the value is found there but also in an environment variable, the value from the environment variable would take precedence.</span></span>

<span data-ttu-id="a8206-360">Para administrar los datos de configuración confidencial como las contraseñas, ASP.NET Core proporciona una [herramienta de administrador secreto](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="a8206-360">For managing confidential configuration data such as passwords, ASP.NET Core provides a [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="a8206-361">Para los secretos de producción, se recomienda [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="a8206-361">For production secrets, we recommend [Azure Key Vault](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="a8206-362">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a8206-362">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="options"></a><span data-ttu-id="a8206-363">Opciones</span><span class="sxs-lookup"><span data-stu-id="a8206-363">Options</span></span>

<span data-ttu-id="a8206-364">Siempre que sea posible, ASP.NET Core sigue el *patrón de opciones* para almacenar y recuperar los valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8206-364">Where possible, ASP.NET Core follows the *options pattern* for storing and retrieving configuration values.</span></span> <span data-ttu-id="a8206-365">El patrón de opciones usa clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="a8206-365">The options pattern uses classes to represent groups of related settings.</span></span>

<span data-ttu-id="a8206-366">Por ejemplo, el código siguiente define las opciones de WebSockets:</span><span class="sxs-lookup"><span data-stu-id="a8206-366">For example, the following code sets WebSockets options:</span></span>

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

<span data-ttu-id="a8206-367">Para obtener más información, vea <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="a8206-367">For more information, see <xref:fundamentals/configuration/options>.</span></span>

## <a name="environments"></a><span data-ttu-id="a8206-368">Entornos</span><span class="sxs-lookup"><span data-stu-id="a8206-368">Environments</span></span>

<span data-ttu-id="a8206-369">Los entornos de ejecución, como *desarrollo*, *almacenamiento provisional* y *Producción*, son un concepto de primera clase en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8206-369">Execution environments, such as *Development*, *Staging*, and *Production*, are a first-class notion in ASP.NET Core.</span></span> <span data-ttu-id="a8206-370">Puede especificar el entorno que ejecuta una aplicación estableciendo la variable de entorno `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="a8206-370">You can specify the environment an app is running in by setting the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="a8206-371">ASP.NET Core lee dicha variable de entorno al inicio de la aplicación y almacena el valor en una implementación `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="a8206-371">ASP.NET Core reads that environment variable at app startup and stores the value in an `IHostingEnvironment` implementation.</span></span> <span data-ttu-id="a8206-372">El objeto de entorno está disponible en cualquier parte de la aplicación a través de DI.</span><span class="sxs-lookup"><span data-stu-id="a8206-372">The environment object is available anywhere in the app via DI.</span></span>

<span data-ttu-id="a8206-373">El siguiente ejemplo de código desde la clase `Startup` configura la aplicación para que proporcione información detallada del error solo cuando se ejecuta en el desarrollo:</span><span class="sxs-lookup"><span data-stu-id="a8206-373">The following sample code from the `Startup` class configures the app to provide detailed error information only when it runs in development:</span></span>

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

<span data-ttu-id="a8206-374">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="a8206-374">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="logging"></a><span data-ttu-id="a8206-375">Registro</span><span class="sxs-lookup"><span data-stu-id="a8206-375">Logging</span></span>

<span data-ttu-id="a8206-376">ASP.NET Core es compatible con una API de registro que funciona con una gran variedad de proveedores de registro integrados y de terceros.</span><span class="sxs-lookup"><span data-stu-id="a8206-376">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="a8206-377">Entre los proveedores disponibles se incluyen los siguientes:</span><span class="sxs-lookup"><span data-stu-id="a8206-377">Available providers include the following:</span></span>

* <span data-ttu-id="a8206-378">Consola</span><span class="sxs-lookup"><span data-stu-id="a8206-378">Console</span></span>
* <span data-ttu-id="a8206-379">Depuración</span><span class="sxs-lookup"><span data-stu-id="a8206-379">Debug</span></span>
* <span data-ttu-id="a8206-380">Seguimiento de eventos en Windows</span><span class="sxs-lookup"><span data-stu-id="a8206-380">Event Tracing on Windows</span></span>
* <span data-ttu-id="a8206-381">Registro de errores de Windows</span><span class="sxs-lookup"><span data-stu-id="a8206-381">Windows Event Log</span></span>
* <span data-ttu-id="a8206-382">TraceSource</span><span class="sxs-lookup"><span data-stu-id="a8206-382">TraceSource</span></span>
* <span data-ttu-id="a8206-383">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a8206-383">Azure App Service</span></span>
* <span data-ttu-id="a8206-384">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="a8206-384">Azure Application Insights</span></span>

<span data-ttu-id="a8206-385">Escribir registros desde cualquier lugar en el código de una aplicación mediante la obtención de un objeto `ILogger` de DI y llamar a métodos de registro.</span><span class="sxs-lookup"><span data-stu-id="a8206-385">Write logs from anywhere in an app's code by getting an `ILogger` object from DI and calling log methods.</span></span>

<span data-ttu-id="a8206-386">Aquí tiene un código de ejemplo que usa un objeto `ILogger`, con la inserción del constructor y resaltadas las llamadas del método de registro.</span><span class="sxs-lookup"><span data-stu-id="a8206-386">Here's sample code that uses an `ILogger` object, with constructor injection and the logging method calls highlighted.</span></span>

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

<span data-ttu-id="a8206-387">La interfaz `ILogger` le permite pasar cualquier número de campos para el proveedor de registro.</span><span class="sxs-lookup"><span data-stu-id="a8206-387">The `ILogger` interface lets you pass any number of fields to the logging provider.</span></span> <span data-ttu-id="a8206-388">Los campos habitualmente se usan para construir una cadena de mensaje, pero el proveedor también puede enviarlos como campos independientes o almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="a8206-388">The fields are commonly used to construct a message string, but the provider can also send them as separate fields to a data store.</span></span> <span data-ttu-id="a8206-389">Esta característica permite a los proveedores de registro implementar el [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="a8206-389">This feature makes it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="a8206-390">Para obtener más información, vea <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="a8206-390">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="routing"></a><span data-ttu-id="a8206-391">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="a8206-391">Routing</span></span>

<span data-ttu-id="a8206-392">Una *ruta* es un patrón de dirección URL que se asigna a un controlador.</span><span class="sxs-lookup"><span data-stu-id="a8206-392">A *route* is a URL pattern that is mapped to a handler.</span></span> <span data-ttu-id="a8206-393">El controlador normalmente es una página de Razor, un método de acción en un controlador MVC o un software intermedio.</span><span class="sxs-lookup"><span data-stu-id="a8206-393">The handler is typically a Razor page, an action method in an MVC controller, or a middleware.</span></span> <span data-ttu-id="a8206-394">El enrutamiento de ASP.NET Core le permite controlar las direcciones URL usadas por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8206-394">ASP.NET Core routing gives you control over the URLs used by your app.</span></span>

<span data-ttu-id="a8206-395">Para obtener más información, vea <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="a8206-395">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="a8206-396">Control de errores</span><span class="sxs-lookup"><span data-stu-id="a8206-396">Error handling</span></span>

<span data-ttu-id="a8206-397">ASP.NET Core tiene características integradas para controlar los errores, tales como:</span><span class="sxs-lookup"><span data-stu-id="a8206-397">ASP.NET Core has built-in features for handling errors, such as:</span></span>

* <span data-ttu-id="a8206-398">Una página de excepciones para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="a8206-398">A developer exception page</span></span>
* <span data-ttu-id="a8206-399">Páginas de errores personalizados</span><span class="sxs-lookup"><span data-stu-id="a8206-399">Custom error pages</span></span>
* <span data-ttu-id="a8206-400">Páginas de códigos de estado estáticos</span><span class="sxs-lookup"><span data-stu-id="a8206-400">Static status code pages</span></span>
* <span data-ttu-id="a8206-401">Control de excepciones de inicio</span><span class="sxs-lookup"><span data-stu-id="a8206-401">Startup exception handling</span></span>

<span data-ttu-id="a8206-402">Para obtener más información, vea <xref:fundamentals/error-handling>.</span><span class="sxs-lookup"><span data-stu-id="a8206-402">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="make-http-requests"></a><span data-ttu-id="a8206-403">Realización de solicitudes HTTP</span><span class="sxs-lookup"><span data-stu-id="a8206-403">Make HTTP requests</span></span>

<span data-ttu-id="a8206-404">Una implementación de `IHttpClientFactory` está disponible para crear instancias de `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="a8206-404">An implementation of `IHttpClientFactory` is available for creating `HttpClient` instances.</span></span> <span data-ttu-id="a8206-405">Este servicio:</span><span class="sxs-lookup"><span data-stu-id="a8206-405">The factory:</span></span>

* <span data-ttu-id="a8206-406">Proporciona una ubicación central para denominar y configurar instancias de `HttpClient` lógicas.</span><span class="sxs-lookup"><span data-stu-id="a8206-406">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="a8206-407">Así, por ejemplo, se puede registrar y configurar un cliente *github* para tener acceso a GitHub.</span><span class="sxs-lookup"><span data-stu-id="a8206-407">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="a8206-408">y, de igual modo, registrar otro cliente predeterminado para otros fines.</span><span class="sxs-lookup"><span data-stu-id="a8206-408">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="a8206-409">Admite el registro y encadenamiento de varios controladores de delegación para crear una canalización de middleware de solicitud saliente.</span><span class="sxs-lookup"><span data-stu-id="a8206-409">Supports registration and chaining of multiple delegating handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="a8206-410">Este patrón es similar a la canalización de middleware de entrada de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8206-410">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="a8206-411">Dicho patrón proporciona un mecanismo para administrar cuestiones transversales relativas a las solicitudes HTTP, como el almacenamiento en caché, el control de errores, la serialización y el registro.</span><span class="sxs-lookup"><span data-stu-id="a8206-411">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>
* <span data-ttu-id="a8206-412">Se integra con *Polly*, una conocida biblioteca de terceros para el control de errores transitorios.</span><span class="sxs-lookup"><span data-stu-id="a8206-412">Integrates with *Polly*, a popular third-party library for transient fault handling.</span></span>
* <span data-ttu-id="a8206-413">Administra la agrupación y duración de las instancias de `HttpClientMessageHandler` subyacentes para evitar los problemas de DNS que suelen producirse al administrar las duraciones de `HttpClient` manualmente.</span><span class="sxs-lookup"><span data-stu-id="a8206-413">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="a8206-414">Agrega una experiencia de registro configurable (a través de `ILogger`) en todas las solicitudes enviadas a través de los clientes creados por Factory.</span><span class="sxs-lookup"><span data-stu-id="a8206-414">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="a8206-415">Para obtener más información, vea <xref:fundamentals/http-requests>.</span><span class="sxs-lookup"><span data-stu-id="a8206-415">For more information, see <xref:fundamentals/http-requests>.</span></span>

## <a name="content-root"></a><span data-ttu-id="a8206-416">Raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="a8206-416">Content root</span></span>

<span data-ttu-id="a8206-417">La raíz del contenido es la ruta de acceso base a:</span><span class="sxs-lookup"><span data-stu-id="a8206-417">The content root is the base path to the:</span></span>

* <span data-ttu-id="a8206-418">El archivo ejecutable que hospeda la aplicación ( *.exe*).</span><span class="sxs-lookup"><span data-stu-id="a8206-418">Executable hosting the app (*.exe*).</span></span>
* <span data-ttu-id="a8206-419">Los ensamblados compilados que componen la aplicación ( *.dll*).</span><span class="sxs-lookup"><span data-stu-id="a8206-419">Compiled assemblies that make up the app (*.dll*).</span></span>
* <span data-ttu-id="a8206-420">Los archivos de contenido que no son de código usados por la aplicación, como:</span><span class="sxs-lookup"><span data-stu-id="a8206-420">Non-code content files used by the app, such as:</span></span>
  * <span data-ttu-id="a8206-421">Archivos de Razor ( *.cshtml*, *.razor*)</span><span class="sxs-lookup"><span data-stu-id="a8206-421">Razor files (*.cshtml*, *.razor*)</span></span>
  * <span data-ttu-id="a8206-422">Archivos de configuración ( *.json*, *.xml*)</span><span class="sxs-lookup"><span data-stu-id="a8206-422">Configuration files (*.json*, *.xml*)</span></span>
  * <span data-ttu-id="a8206-423">Archivos de datos ( *.db*)</span><span class="sxs-lookup"><span data-stu-id="a8206-423">Data files (*.db*)</span></span>
* <span data-ttu-id="a8206-424">[Raíz web](#web-root), normalmente la carpeta *wwwroot* publicada.</span><span class="sxs-lookup"><span data-stu-id="a8206-424">[Web root](#web-root), typically the published *wwwroot* folder.</span></span>

<span data-ttu-id="a8206-425">Durante el desarrollo:</span><span class="sxs-lookup"><span data-stu-id="a8206-425">During development:</span></span>

* <span data-ttu-id="a8206-426">La raíz del contenido tiene como valor predeterminado el directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a8206-426">The content root defaults to the project's root directory.</span></span>
* <span data-ttu-id="a8206-427">El directorio raíz del proyecto se usa para crear:</span><span class="sxs-lookup"><span data-stu-id="a8206-427">The project's root directory is used to create the:</span></span>
  * <span data-ttu-id="a8206-428">La ruta de acceso a los archivos de contenido que no son de código de la aplicación en el directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a8206-428">Path to the app's non-code content files in the project's root directory.</span></span>
  * <span data-ttu-id="a8206-429">La [raíz web](#web-root), normalmente la carpeta *wwwroot* en el directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a8206-429">[Web root](#web-root), typically the *wwwroot* folder in the project's root directory.</span></span>

<span data-ttu-id="a8206-430">Se puede especificar una ruta raíz de contenido alternativa al [crear el host](#host).</span><span class="sxs-lookup"><span data-stu-id="a8206-430">An alternative content root path can be specified when [building the host](#host).</span></span> <span data-ttu-id="a8206-431">Para obtener más información, vea <xref:fundamentals/host/web-host#content-root>.</span><span class="sxs-lookup"><span data-stu-id="a8206-431">For more information, see <xref:fundamentals/host/web-host#content-root>.</span></span>

## <a name="web-root"></a><span data-ttu-id="a8206-432">Raíz web</span><span class="sxs-lookup"><span data-stu-id="a8206-432">Web root</span></span>

<span data-ttu-id="a8206-433">La raíz web es la ruta de acceso base a archivos de recursos públicos, que no son de código y estáticos, como:</span><span class="sxs-lookup"><span data-stu-id="a8206-433">The web root is the base path to public, non-code, static resource files, such as:</span></span>

* <span data-ttu-id="a8206-434">Hojas de estilo ( *.css*)</span><span class="sxs-lookup"><span data-stu-id="a8206-434">Stylesheets (*.css*)</span></span>
* <span data-ttu-id="a8206-435">JavaScript ( *.js*)</span><span class="sxs-lookup"><span data-stu-id="a8206-435">JavaScript (*.js*)</span></span>
* <span data-ttu-id="a8206-436">Imágenes ( *.png*, *.jpg*)</span><span class="sxs-lookup"><span data-stu-id="a8206-436">Images (*.png*, *.jpg*)</span></span>

<span data-ttu-id="a8206-437">De forma predeterminada, los archivos estáticos se atienden solo desde el directorio raíz web (y los subdirectorios).</span><span class="sxs-lookup"><span data-stu-id="a8206-437">Static files are only served by default from the web root directory (and sub-directories).</span></span>

<span data-ttu-id="a8206-438">El valor predeterminado de la ruta de acceso de la raíz web es *{raíz del contenido}/wwwroot*, pero se puede especificar una raíz web diferente [al crear el host](#host).</span><span class="sxs-lookup"><span data-stu-id="a8206-438">The web root path defaults to *{content root}/wwwroot*, but a different web root can be specified when [building the host](#host).</span></span> <span data-ttu-id="a8206-439">Para obtener más información, vea [Raíz web](xref:fundamentals/host/web-host#web-root).</span><span class="sxs-lookup"><span data-stu-id="a8206-439">For more information, see [Web root](xref:fundamentals/host/web-host#web-root).</span></span>

<span data-ttu-id="a8206-440">Evite la publicación de archivos en *wwwroot* con el [\<Content> elemento de proyecto](/visualstudio/msbuild/common-msbuild-project-items#content) en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="a8206-440">Prevent publishing files in *wwwroot* with the [\<Content> project item](/visualstudio/msbuild/common-msbuild-project-items#content) in the project file.</span></span> <span data-ttu-id="a8206-441">En el ejemplo siguiente se impide la publicación de contenido en el directorio *wwwroot/local* y en los subdirectorios:</span><span class="sxs-lookup"><span data-stu-id="a8206-441">The following example prevents publishing content in the *wwwroot/local* directory and sub-directories:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="a8206-442">En los archivos de Razor ( *.cshtml*), la virgulilla `~/` apunta a la raíz web.</span><span class="sxs-lookup"><span data-stu-id="a8206-442">In Razor (*.cshtml*) files, the tilde-slash (`~/`) points to the web root.</span></span> <span data-ttu-id="a8206-443">Una ruta de acceso que empieza por `~/` se conoce como *ruta de acceso virtual*.</span><span class="sxs-lookup"><span data-stu-id="a8206-443">A path beginning with `~/` is referred to as a *virtual path*.</span></span>

<span data-ttu-id="a8206-444">Para obtener más información, vea <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="a8206-444">For more information, see <xref:fundamentals/static-files>.</span></span>

::: moniker-end
