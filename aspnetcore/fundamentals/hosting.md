---
title: Hospedar en ASP.NET Core | Documentos de Microsoft
author: ardalis
description: "Introducción a los hosts de web en ASP.NET Core."
keywords: "Núcleo de ASP.NET, host de web, IWebHost"
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0725f3d2c130068094792e5ba9e17481155e4294
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-hosting-in-aspnet-core"></a><span data-ttu-id="0c831-104">Introducción a hospedar en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c831-104">Introduction to hosting in ASP.NET Core</span></span>

<span data-ttu-id="0c831-105">Por [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="0c831-105">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="0c831-106">Para ejecutar una aplicación de ASP.NET Core, debe configurar e iniciar un host con `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0c831-106">To run an ASP.NET Core app, you need to configure and launch a host using `WebHostBuilder`.</span></span>

## <a name="what-is-a-host"></a><span data-ttu-id="0c831-107">¿Qué es un Host?</span><span class="sxs-lookup"><span data-stu-id="0c831-107">What is a Host?</span></span>

<span data-ttu-id="0c831-108">Las aplicaciones de ASP.NET Core requieren un *host* en el que se va a ejecutar.</span><span class="sxs-lookup"><span data-stu-id="0c831-108">ASP.NET Core apps require a *host* in which to execute.</span></span> <span data-ttu-id="0c831-109">Un host debe implementar la `IWebHost` interfaz, que expone las colecciones de características y servicios, y un `Start` método.</span><span class="sxs-lookup"><span data-stu-id="0c831-109">A host must implement the `IWebHost` interface, which exposes collections of features and services, and a `Start` method.</span></span> <span data-ttu-id="0c831-110">El host se crea normalmente con una instancia de un `WebHostBuilder`, que genera y devuelve un `WebHost` instancia.</span><span class="sxs-lookup"><span data-stu-id="0c831-110">The host is typically created using an instance of a `WebHostBuilder`, which builds and returns a  `WebHost` instance.</span></span> <span data-ttu-id="0c831-111">El `WebHost` hace referencia al servidor que atenderá las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="0c831-111">The `WebHost` references the server that will handle requests.</span></span> <span data-ttu-id="0c831-112">Obtenga más información sobre [servidores](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="0c831-112">Learn more about [servers](servers/index.md).</span></span>

### <a name="what-is-the-difference-between-a-host-and-a-server"></a><span data-ttu-id="0c831-113">¿Cuál es la diferencia entre un host y un servidor?</span><span class="sxs-lookup"><span data-stu-id="0c831-113">What is the difference between a host and a server?</span></span>

<span data-ttu-id="0c831-114">El host es responsable de la administración de inicio y duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0c831-114">The host is responsible for application startup and lifetime management.</span></span> <span data-ttu-id="0c831-115">El servidor es responsable de aceptar solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c831-115">The server is responsible for accepting HTTP requests.</span></span> <span data-ttu-id="0c831-116">Parte de la responsabilidad del host incluye garantizar que Servicios de la aplicación y el servidor están disponible y configurado correctamente.</span><span class="sxs-lookup"><span data-stu-id="0c831-116">Part of the host's responsibility includes ensuring the application's services and the server are available and properly configured.</span></span> <span data-ttu-id="0c831-117">Se puede considerar el host como un contenedor alrededor del servidor.</span><span class="sxs-lookup"><span data-stu-id="0c831-117">You can think of the host as being a wrapper around the server.</span></span> <span data-ttu-id="0c831-118">El host está configurado para usar un servidor determinado; el servidor no es consciente de su host.</span><span class="sxs-lookup"><span data-stu-id="0c831-118">The host is configured to use a particular server; the server is unaware of its host.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="0c831-119">Configurar un Host</span><span class="sxs-lookup"><span data-stu-id="0c831-119">Setting up a Host</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0c831-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0c831-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0c831-121">Crear un host con una instancia de `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0c831-121">You create a host using an instance of `WebHostBuilder`.</span></span> <span data-ttu-id="0c831-122">Esto se hace normalmente en el punto de entrada de la aplicación: `public static void Main` (que en las plantillas de proyecto se encuentra en un *Program.cs* archivo).</span><span class="sxs-lookup"><span data-stu-id="0c831-122">This is typically done in your app's entry point: `public static void Main` (which in the project templates is located in a *Program.cs* file).</span></span> <span data-ttu-id="0c831-123">Una típica *Program.cs*, como se muestra a continuación, se muestra cómo utilizar un `WebHostBuilder` para crear un host.</span><span class="sxs-lookup"><span data-stu-id="0c831-123">A typical *Program.cs*, shown below, demonstrates how to use a `WebHostBuilder` to build a host.</span></span>

<span data-ttu-id="0c831-124">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]</span><span class="sxs-lookup"><span data-stu-id="0c831-124">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]</span></span>

<span data-ttu-id="0c831-125">El `WebHostBuilder` es responsable de crear el host que se arranca el servidor de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0c831-125">The `WebHostBuilder` is responsible for creating the host that will bootstrap the server for the app.</span></span> <span data-ttu-id="0c831-126">`WebHostBuilder`requiere que proporcione un servidor que implementa `IServer` (`UseKestrel` en el código anterior).</span><span class="sxs-lookup"><span data-stu-id="0c831-126">`WebHostBuilder` requires you provide a server that implements `IServer` (`UseKestrel` in the code above).</span></span> <span data-ttu-id="0c831-127">`UseKestrel`Especifica que el servidor Kestrel usará la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0c831-127">`UseKestrel` specifies the Kestrel server will be used by the app.</span></span>

<span data-ttu-id="0c831-128">El servidor *contenido raíz* determina donde busca los archivos de contenido, como los archivos de vista de MVC.</span><span class="sxs-lookup"><span data-stu-id="0c831-128">The server's *content root* determines where it searches for content files, like MVC View files.</span></span> <span data-ttu-id="0c831-129">La raíz de contenido de forma predeterminada es la carpeta desde la que se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0c831-129">The default content root is the folder from which the application is run.</span></span>

> [!NOTE]
> <span data-ttu-id="0c831-130">Especificar `Directory.GetCurrentDirectory` como la raíz de contenido utilizarán carpeta raíz del proyecto web como raíz del contenido de la aplicación cuando la aplicación se inicia desde esta carpeta (por ejemplo, al llamar a `dotnet run` desde la carpeta del proyecto web).</span><span class="sxs-lookup"><span data-stu-id="0c831-130">Specifying `Directory.GetCurrentDirectory` as the content root will use the web project's root folder as the app's content root when the app is started from this folder (for example, calling `dotnet run` from the web project folder).</span></span> <span data-ttu-id="0c831-131">Se trata de los valores predeterminados utilizados en Visual Studio y `dotnet new` plantillas.</span><span class="sxs-lookup"><span data-stu-id="0c831-131">This is the default used in Visual Studio and `dotnet new` templates.</span></span>

<span data-ttu-id="0c831-132">Para utilizar IIS como un proxy inverso, llame a `UseIISIntegration` como parte de la creación del host.</span><span class="sxs-lookup"><span data-stu-id="0c831-132">To use IIS as a reverse proxy, call `UseIISIntegration` as part of building the host.</span></span> 

<span data-ttu-id="0c831-133">Tenga en cuenta que `UseIISIntegration` no configura un *server*, como `UseKestrel` does.</span><span class="sxs-lookup"><span data-stu-id="0c831-133">Note that `UseIISIntegration` doesn't configure a *server*, like `UseKestrel` does.</span></span> <span data-ttu-id="0c831-134">Para usar IIS con ASP.NET Core, debe especificar tanto `UseKestrel` y `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="0c831-134">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="0c831-135">`UseKestrel`el servidor web se crea y hospeda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0c831-135">`UseKestrel` creates the web server and hosts the app.</span></span> <span data-ttu-id="0c831-136">`UseIISIntegration`examina las variables de entorno utilizadas por IIS/IISExpress y configura opciones como el puerto para escuchar en y los encabezados a la utilice.</span><span class="sxs-lookup"><span data-stu-id="0c831-136">`UseIISIntegration` examines environment variables used by IIS/IISExpress and configures settings such as the port to listen on and the headers to use.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0c831-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0c831-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0c831-138">Crear un host con una instancia de `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0c831-138">You create a host using an instance of `WebHostBuilder`.</span></span> <span data-ttu-id="0c831-139">Esto se hace normalmente en el punto de entrada de la aplicación: `public static void Main` (que en las plantillas de proyecto se encuentra en un *Program.cs* archivo).</span><span class="sxs-lookup"><span data-stu-id="0c831-139">This is typically done in your app's entry point: `public static void Main` (which in the project templates is located in a *Program.cs* file).</span></span> <span data-ttu-id="0c831-140">Una típica *Program.cs*, como se muestra a continuación, llama `CreateDefaultbuilder` para crear un host:</span><span class="sxs-lookup"><span data-stu-id="0c831-140">A typical *Program.cs*, shown below, calls `CreateDefaultbuilder` to build a host:</span></span>

<span data-ttu-id="0c831-141">[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="0c831-141">[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]</span></span>

<span data-ttu-id="0c831-142">`CreateDefaultbuilder`crea una instancia de `WebHostBuilder` para crear el host que ejecuta un bootstrap el servidor de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0c831-142">`CreateDefaultbuilder` creates an instance of `WebHostBuilder` to build the host that bootstraps the server for the app.</span></span> <span data-ttu-id="0c831-143">El host requiere una [server que implementa IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="0c831-143">The host requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="0c831-144">Los servidores integrados son [Kestrel](servers/kestrel.md) y [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` usar Kestrel de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0c831-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md); `CreateDefaultbuilder` use Kestrel by default.</span></span>

<span data-ttu-id="0c831-145">`CreateDefaultbuilder`realiza tareas de instalación además de configurar Kestrel como el servidor web:</span><span class="sxs-lookup"><span data-stu-id="0c831-145">`CreateDefaultbuilder` performs set-up tasks in addition to configuring Kestrel as the web server:</span></span>

* <span data-ttu-id="0c831-146">Establece la raíz del contenido en `Directory.GetCurrentDirectory`.</span><span class="sxs-lookup"><span data-stu-id="0c831-146">Sets the content root to `Directory.GetCurrentDirectory`.</span></span>
* <span data-ttu-id="0c831-147">Configuración de las cargas de:</span><span class="sxs-lookup"><span data-stu-id="0c831-147">Loads configuration from:</span></span>
  * <span data-ttu-id="0c831-148">*appSettings.JSON que se*</span><span class="sxs-lookup"><span data-stu-id="0c831-148">*appsettings.json*</span></span>
  * <span data-ttu-id="0c831-149">*appSettings. \<EnvironmentName > .json*.</span><span class="sxs-lookup"><span data-stu-id="0c831-149">*appsettings.\<EnvironmentName>.json*.</span></span>
  * <span data-ttu-id="0c831-150">secretos del usuario cuando la aplicación se ejecuta en el entorno de desarrollo</span><span class="sxs-lookup"><span data-stu-id="0c831-150">user secrets when the app runs in the Development environment</span></span>
  * <span data-ttu-id="0c831-151">variables de entorno</span><span class="sxs-lookup"><span data-stu-id="0c831-151">environment variables</span></span>
  * <span data-ttu-id="0c831-152">argumentos de línea de comandos proporcionados</span><span class="sxs-lookup"><span data-stu-id="0c831-152">supplied command line args</span></span>
* <span data-ttu-id="0c831-153">Configura el registro para la salida de consola y de depuración, con el filtrado de las reglas especificadas en una sección de configuración de registro.</span><span class="sxs-lookup"><span data-stu-id="0c831-153">Configures logging for console and debug output, with filtering rules specified in a Logging configuration section.</span></span>
* <span data-ttu-id="0c831-154">Habilita la integración de IIS.</span><span class="sxs-lookup"><span data-stu-id="0c831-154">Enables IIS integration.</span></span>
* <span data-ttu-id="0c831-155">Agrega la página de la excepción de desarrollador cuando la aplicación se ejecuta en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="0c831-155">Adds the developer exception page when the app runs in the Development environment.</span></span>

<span data-ttu-id="0c831-156">El servidor *contenido raíz* determina donde busca los archivos de contenido, como los archivos de vista de MVC.</span><span class="sxs-lookup"><span data-stu-id="0c831-156">The server's *content root* determines where it searches for content files, like MVC View files.</span></span> <span data-ttu-id="0c831-157">La raíz de contenido de forma predeterminada es la carpeta desde la que se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0c831-157">The default content root is the folder from which the application is run.</span></span>

> [!NOTE]
> <span data-ttu-id="0c831-158">Especificar `Directory.GetCurrentDirectory` como la raíz de contenido utilizarán carpeta raíz del proyecto web como raíz del contenido de la aplicación cuando la aplicación se inicia desde esta carpeta (por ejemplo, al llamar a `dotnet run` desde la carpeta del proyecto web).</span><span class="sxs-lookup"><span data-stu-id="0c831-158">Specifying `Directory.GetCurrentDirectory` as the content root will use the web project's root folder as the app's content root when the app is started from this folder (for example, calling `dotnet run` from the web project folder).</span></span> <span data-ttu-id="0c831-159">Se trata de los valores predeterminados utilizados en Visual Studio y `dotnet new` plantillas.</span><span class="sxs-lookup"><span data-stu-id="0c831-159">This is the default used in Visual Studio and `dotnet new` templates.</span></span>

<span data-ttu-id="0c831-160">Cuando se usa IIS como un proxy inverso, ASP.NET Core llama automáticamente a `UseIISIntegration` como parte de la creación del host.</span><span class="sxs-lookup"><span data-stu-id="0c831-160">When you use IIS as a reverse proxy, ASP.NET Core automatically calls `UseIISIntegration` as part of building the host.</span></span> <span data-ttu-id="0c831-161">Para obtener más información, consulte [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="0c831-161">For more information, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="0c831-162">Tenga en cuenta que `UseIISIntegration` no configura un *server*, como `UseKestrel` does.</span><span class="sxs-lookup"><span data-stu-id="0c831-162">Note that `UseIISIntegration` doesn't configure a *server*, like `UseKestrel` does.</span></span> <span data-ttu-id="0c831-163">`UseKestrel`el servidor web se crea y hospeda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0c831-163">`UseKestrel` creates the web server and hosts the app.</span></span> <span data-ttu-id="0c831-164">`UseIISIntegration`examina las variables de entorno utilizadas por IIS/IISExpress y configura opciones como el puerto para escuchar en y los encabezados a la utilice.</span><span class="sxs-lookup"><span data-stu-id="0c831-164">`UseIISIntegration` examines environment variables used by IIS/IISExpress and configures settings such as the port to listen on and the headers to use.</span></span>

---

<span data-ttu-id="0c831-165">Una implementación mínima de configuración de un host (y una aplicación de ASP.NET Core) incluye sólo un servidor y la configuración de canalización de solicitud de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="0c831-165">A minimal implementation of configuring a host (and an ASP.NET Core app) would include just a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
    })
    .Build();

host.Run();
```

> [!NOTE]
> <span data-ttu-id="0c831-166">Al configurar un host, puede proporcionar `Configure` y `ConfigureServices` métodos, en lugar de o además de especificar un `Startup` clase (que también debe definir estos métodos: vea [inicio de la aplicación](startup.md)).</span><span class="sxs-lookup"><span data-stu-id="0c831-166">When setting up a host, you can provide `Configure` and `ConfigureServices` methods, instead of or in addition to specifying a `Startup` class (which must also define these methods - see [Application Startup](startup.md)).</span></span> <span data-ttu-id="0c831-167">Varias llamadas a `ConfigureServices` anexará entre sí; llama a `Configure` o `UseStartup` , reemplazará la configuración anterior.</span><span class="sxs-lookup"><span data-stu-id="0c831-167">Multiple calls to `ConfigureServices` will append to one another; calls to `Configure` or `UseStartup` will replace previous settings.</span></span>

## <a name="configuring-a-host"></a><span data-ttu-id="0c831-168">Configuración de un Host</span><span class="sxs-lookup"><span data-stu-id="0c831-168">Configuring a Host</span></span>

<span data-ttu-id="0c831-169">El `WebHostBuilder` proporciona métodos para establecer la mayoría de los valores de configuración disponibles para el host, que también se puede establecer directamente mediante `UseSetting` y la clave asociada.</span><span class="sxs-lookup"><span data-stu-id="0c831-169">The `WebHostBuilder` provides methods for setting most of the available configuration values for the host, which can also be set directly using `UseSetting` and associated key.</span></span>

### <a name="host-configuration-values"></a><span data-ttu-id="0c831-170">Valores de configuración de host</span><span class="sxs-lookup"><span data-stu-id="0c831-170">Host Configuration Values</span></span>

<span data-ttu-id="0c831-171">**Capturar errores de inicio**`bool`</span><span class="sxs-lookup"><span data-stu-id="0c831-171">**Capture Startup Errors** `bool`</span></span>

<span data-ttu-id="0c831-172">Clave: `captureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="0c831-172">Key: `captureStartupErrors`.</span></span> <span data-ttu-id="0c831-173">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="0c831-173">Defaults to `false`.</span></span> <span data-ttu-id="0c831-174">Cuando `false`, errores durante el resultado de inicio en el host de salir.</span><span class="sxs-lookup"><span data-stu-id="0c831-174">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="0c831-175">Cuando `true`, el host capturará las excepciones de la `Startup` clase e intentar iniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="0c831-175">When `true`, the host will capture any exceptions from the `Startup` class and attempt to start the server.</span></span> <span data-ttu-id="0c831-176">Se mostrará una página de error (genérico o detallada, según la configuración de errores detallados a continuación) para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="0c831-176">It will display an error page (generic, or detailed, based on the Detailed Errors setting, below) for every request.</span></span> <span data-ttu-id="0c831-177">Establecer mediante el `CaptureStartupErrors` método.</span><span class="sxs-lookup"><span data-stu-id="0c831-177">Set using the `CaptureStartupErrors` method.</span></span>

<span data-ttu-id="0c831-178">Nota: Cuando la aplicación se ejecuta con Kestrel e IIS, el comportamiento predeterminado es capturar los errores de inicio.</span><span class="sxs-lookup"><span data-stu-id="0c831-178">Note: When your app runs with Kestrel and IIS, the default behavior is to capture startup errors.</span></span> 

```csharp
new WebHostBuilder()
    .CaptureStartupErrors(true)
   ```

<span data-ttu-id="0c831-179">**Contenido raíz**`string`</span><span class="sxs-lookup"><span data-stu-id="0c831-179">**Content Root** `string`</span></span>

<span data-ttu-id="0c831-180">Clave: `contentRoot`.</span><span class="sxs-lookup"><span data-stu-id="0c831-180">Key: `contentRoot`.</span></span> <span data-ttu-id="0c831-181">El valor predeterminado es la carpeta donde reside el ensamblado de aplicación (para Kestrel; IIS utilizará la raíz del proyecto web de forma predeterminada).</span><span class="sxs-lookup"><span data-stu-id="0c831-181">Defaults to the folder where the application assembly resides (for Kestrel; IIS will use the web project root by default).</span></span> <span data-ttu-id="0c831-182">Esta configuración determina donde se iniciará la búsqueda de archivos de contenido, como las vistas de MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c831-182">This setting determines where ASP.NET Core will begin searching for content files, such as MVC Views.</span></span> <span data-ttu-id="0c831-183">También se utiliza como la ruta de acceso base para la [configuración de la raíz de Web](#web-root-setting).</span><span class="sxs-lookup"><span data-stu-id="0c831-183">Also used as the base path for the [Web Root Setting](#web-root-setting).</span></span> <span data-ttu-id="0c831-184">Establecer mediante el `UseContentRoot` método.</span><span class="sxs-lookup"><span data-stu-id="0c831-184">Set using the `UseContentRoot` method.</span></span> <span data-ttu-id="0c831-185">Ruta de acceso debe existir o se producirá un error de host iniciar.</span><span class="sxs-lookup"><span data-stu-id="0c831-185">Path must exist, or host will fail to start.</span></span>

```csharp
new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
   ```

<span data-ttu-id="0c831-186">**Errores detallados**`bool`</span><span class="sxs-lookup"><span data-stu-id="0c831-186">**Detailed Errors** `bool`</span></span>

<span data-ttu-id="0c831-187">Clave: `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="0c831-187">Key: `detailedErrors`.</span></span> <span data-ttu-id="0c831-188">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="0c831-188">Defaults to `false`.</span></span> <span data-ttu-id="0c831-189">Cuando `true` (o cuando el entorno está configurado para "Desarrollo"), la aplicación mostrará detalles de excepciones de inicio, en lugar de una página de error genérica.</span><span class="sxs-lookup"><span data-stu-id="0c831-189">When `true` (or when Environment is set to "Development"), the app will display details of startup exceptions, instead of just a generic error page.</span></span> <span data-ttu-id="0c831-190">Establecer mediante `UseSetting`.</span><span class="sxs-lookup"><span data-stu-id="0c831-190">Set using `UseSetting`.</span></span>

```csharp
new WebHostBuilder()
    .UseSetting("detailedErrors", "true")
```

<span data-ttu-id="0c831-191">Cuando los errores detallados se establece en `false` y capturar errores de inicio es `true`, se muestra una página de error genérico en respuesta a todas las solicitudes al servidor.</span><span class="sxs-lookup"><span data-stu-id="0c831-191">When Detailed Errors is set to `false` and Capture Startup Errors is `true`, a generic error page is displayed in response to every request to the server.</span></span>

![Página de error genérico](hosting/_static/generic-error-page.png)

<span data-ttu-id="0c831-193">Cuando los errores detallados se establece en `true` y capturar errores de inicio es `true`, se muestra una página de error detallado en respuesta a todas las solicitudes al servidor.</span><span class="sxs-lookup"><span data-stu-id="0c831-193">When Detailed Errors is set to `true` and Capture Startup Errors is `true`, a detailed error page is displayed in response to every request to the server.</span></span>

![Página de error detallados](hosting/_static/detailed-error-page.png)

<span data-ttu-id="0c831-195">**Entorno**`string`</span><span class="sxs-lookup"><span data-stu-id="0c831-195">**Environment** `string`</span></span>

<span data-ttu-id="0c831-196">Clave: `environment`.</span><span class="sxs-lookup"><span data-stu-id="0c831-196">Key: `environment`.</span></span> <span data-ttu-id="0c831-197">El valor predeterminado es "Producción".</span><span class="sxs-lookup"><span data-stu-id="0c831-197">Defaults to "Production".</span></span> <span data-ttu-id="0c831-198">Puede establecerse en cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="0c831-198">May be set to any value.</span></span> <span data-ttu-id="0c831-199">Los valores definidos por el marco de trabajo incluyen "Desarrollo", "Ensayo" y "Production".</span><span class="sxs-lookup"><span data-stu-id="0c831-199">Framework-defined values include "Development", "Staging", and "Production".</span></span> <span data-ttu-id="0c831-200">Valores no distinguen entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="0c831-200">Values are not case sensitive.</span></span> <span data-ttu-id="0c831-201">Vea [trabajar con varios entornos](environments.md).</span><span class="sxs-lookup"><span data-stu-id="0c831-201">See [Working with Multiple Environments](environments.md).</span></span> <span data-ttu-id="0c831-202">Establecer mediante el `UseEnvironment` método.</span><span class="sxs-lookup"><span data-stu-id="0c831-202">Set using the `UseEnvironment` method.</span></span>

```csharp
new WebHostBuilder()
    .UseEnvironment("Development")
```

> [!NOTE]
> <span data-ttu-id="0c831-203">De forma predeterminada, el entorno se lee desde el `ASPNETCORE_ENVIRONMENT` variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="0c831-203">By default, the environment is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="0c831-204">Cuando se utiliza Visual Studio, las variables de entorno pueden establecerse el *launchSettings.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="0c831-204">When using Visual Studio, environment variables may be set in the *launchSettings.json* file.</span></span>

<a id="server-urls"></a>

<span data-ttu-id="0c831-205">**Direcciones URL del servidor**`string`</span><span class="sxs-lookup"><span data-stu-id="0c831-205">**Server URLs** `string`</span></span>

<span data-ttu-id="0c831-206">Clave: `urls`.</span><span class="sxs-lookup"><span data-stu-id="0c831-206">Key: `urls`.</span></span> <span data-ttu-id="0c831-207">Establecido en un punto y coma (;) separados de los prefijos de lista de direcciones URL que debe responder el servidor.</span><span class="sxs-lookup"><span data-stu-id="0c831-207">Set to a semicolon (;) separated list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="0c831-208">Por ejemplo: `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="0c831-208">For example, `http://localhost:123`.</span></span> <span data-ttu-id="0c831-209">El nombre de dominio/host puede reemplazarse por "\*" para indicar el servidor debe escuchar las solicitudes en cualquier dirección IP o host que usa el puerto especificado y el protocolo (por ejemplo, `http://*:5000` o `https://*:5001`).</span><span class="sxs-lookup"><span data-stu-id="0c831-209">The domain/host name can be replaced with "\*" to indicate the server should listen to requests on any IP address or host using the specified port and protocol (for example, `http://*:5000` or `https://*:5001`).</span></span> <span data-ttu-id="0c831-210">El protocolo (`http://` o `https://`) debe incluirse en cada dirección URL.</span><span class="sxs-lookup"><span data-stu-id="0c831-210">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="0c831-211">Los prefijos son interpretados por el servidor configurado; los formatos admitidos varían entre los servidores.</span><span class="sxs-lookup"><span data-stu-id="0c831-211">The prefixes are interpreted by the configured server; supported formats will vary between servers.</span></span>

```csharp
new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="0c831-212">En ASP.NET 2.0 de núcleo, Kestrel tiene su propia API de configuración de punto de conexión y no admite `https://` en el `urls` cadena.</span><span class="sxs-lookup"><span data-stu-id="0c831-212">In ASP.NET Core 2.0, Kestrel has its own endpoint configuration API and does not support `https://` in the `urls` string.</span></span> <span data-ttu-id="0c831-213">Para obtener más información, consulte [Introducción a Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="0c831-213">For more information, see [Introduction to Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

<span data-ttu-id="0c831-214">**Ensamblado de inicio**`string`</span><span class="sxs-lookup"><span data-stu-id="0c831-214">**Startup Assembly** `string`</span></span>

<span data-ttu-id="0c831-215">Clave: `startupAssembly`.</span><span class="sxs-lookup"><span data-stu-id="0c831-215">Key: `startupAssembly`.</span></span> <span data-ttu-id="0c831-216">Determina el ensamblado que se va a buscar la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="0c831-216">Determines the assembly to search for the `Startup` class.</span></span> <span data-ttu-id="0c831-217">Establecer mediante el `UseStartup` método.</span><span class="sxs-lookup"><span data-stu-id="0c831-217">Set using the `UseStartup` method.</span></span> <span data-ttu-id="0c831-218">En su lugar puede hacer referencia a un tipo específico mediante `WebHostBuilder.UseStartup<StartupType>`.</span><span class="sxs-lookup"><span data-stu-id="0c831-218">May instead reference specific type using `WebHostBuilder.UseStartup<StartupType>`.</span></span> <span data-ttu-id="0c831-219">Si hay varios `UseStartup` se llaman a métodos, la última de ellas tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="0c831-219">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

<a name=web-root-setting></a>

<span data-ttu-id="0c831-220">**Web raíz**`string`</span><span class="sxs-lookup"><span data-stu-id="0c831-220">**Web Root** `string`</span></span>

<span data-ttu-id="0c831-221">Clave: `webroot`.</span><span class="sxs-lookup"><span data-stu-id="0c831-221">Key: `webroot`.</span></span> <span data-ttu-id="0c831-222">Si no se especifica el valor predeterminado es `(Content Root Path)\wwwroot`, si existe.</span><span class="sxs-lookup"><span data-stu-id="0c831-222">If not specified the default is `(Content Root Path)\wwwroot`, if it exists.</span></span> <span data-ttu-id="0c831-223">Si esta ruta de acceso no existe, se utiliza un proveedor de archivos de la operación inefectiva.</span><span class="sxs-lookup"><span data-stu-id="0c831-223">If this path doesn't exist, then a no-op file provider is used.</span></span> <span data-ttu-id="0c831-224">Establecer mediante `UseWebRoot`.</span><span class="sxs-lookup"><span data-stu-id="0c831-224">Set using `UseWebRoot`.</span></span>

```csharp
new WebHostBuilder()
    .UseWebRoot("public")
```

### <a name="overriding-configuration"></a><span data-ttu-id="0c831-225">Reemplazar configuración</span><span class="sxs-lookup"><span data-stu-id="0c831-225">Overriding Configuration</span></span>

<span data-ttu-id="0c831-226">Use [configuración](configuration.md) para establecer los valores de configuración que va a usar el host.</span><span class="sxs-lookup"><span data-stu-id="0c831-226">Use [Configuration](configuration.md) to set configuration values to be used by the host.</span></span> <span data-ttu-id="0c831-227">Estos valores pueden ser reemplazados posteriormente.</span><span class="sxs-lookup"><span data-stu-id="0c831-227">These values may be subsequently overridden.</span></span> <span data-ttu-id="0c831-228">Esto se especifica con `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="0c831-228">This is specified using `UseConfiguration`.</span></span>

```csharp
public static void Main(string[] args)
{
    var config = new ConfigurationBuilder()
        .AddJsonFile("hosting.json", optional: true)
        .AddCommandLine(args)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .Configure(app =>
        {
            app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
        })
        .Build();

    host.Run();
}
```

<span data-ttu-id="0c831-229">En el ejemplo anterior, se pueden pasar argumentos de línea de comandos de para configurar el host y, opcionalmente, se pueden especificar valores de configuración en un *hosting.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="0c831-229">In the example above, command-line arguments may be passed in to configure the host, or configuration settings may optionally be specified in a *hosting.json* file.</span></span> <span data-ttu-id="0c831-230">Para especificar el host que se ejecutan en una dirección URL determinada, podría pasar el valor deseado desde un símbolo del sistema:</span><span class="sxs-lookup"><span data-stu-id="0c831-230">To specify the host run on a particular URL, you could pass in the desired value from a command prompt:</span></span>

```console
dotnet run --urls "http://*:5000"
```

<span data-ttu-id="0c831-231">El `Run` método inicia la aplicación web y bloquea el subproceso que realiza la llamada hasta que el host está apagado.</span><span class="sxs-lookup"><span data-stu-id="0c831-231">The `Run` method starts the web app and blocks the calling thread until the host is shutdown.</span></span>

```csharp
host.Run();
```

<span data-ttu-id="0c831-232">Puede ejecutar el host de una manera sin bloqueo mediante una llamada a su `Start` método:</span><span class="sxs-lookup"><span data-stu-id="0c831-232">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="0c831-233">Pase una lista de direcciones URL para el `Start` método y escuche en las direcciones URL especificadas:</span><span class="sxs-lookup"><span data-stu-id="0c831-233">Pass a list of URLs to the `Start` method and it will listen on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="0c831-234">Dependen de los formatos de dirección URL aquí son válidos en el servidor que está usando.</span><span class="sxs-lookup"><span data-stu-id="0c831-234">The URL formats that are valid here depend on the server you're using.</span></span> <span data-ttu-id="0c831-235">Para obtener más información, consulte [direcciones URL del servidor](#server-urls) anteriormente en este artículo.</span><span class="sxs-lookup"><span data-stu-id="0c831-235">For more information, see [Server URLs](#server-urls) earlier in this article.</span></span>

> [!NOTE]
> <span data-ttu-id="0c831-236">El `UseConfiguration` no está actualmente capaz de analizar una sección de configuración devuelta por el método de extensión `GetSection` (por ejemplo, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="0c831-236">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="0c831-237">El `GetSection` método filtra las claves de configuración a la sección solicitado, pero deja el nombre de sección de las claves (por ejemplo, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="0c831-237">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="0c831-238">El `UseConfiguration` método espera las claves para que coincida con el `WebHostBuilder` claves (por ejemplo, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="0c831-238">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="0c831-239">La presencia del nombre de sección de las claves evita que los valores de la sección de configuración del host.</span><span class="sxs-lookup"><span data-stu-id="0c831-239">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="0c831-240">Este problema se corregirá en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="0c831-240">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="0c831-241">Para obtener más información y soluciones alternativas, consulte [pasar la sección de configuración a WebHostBuilder.UseConfiguration utiliza claves completas](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="0c831-241">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="ordering-importance"></a><span data-ttu-id="0c831-242">Orden de importancia</span><span class="sxs-lookup"><span data-stu-id="0c831-242">Ordering Importance</span></span>

<span data-ttu-id="0c831-243">`WebHostBuilder`configuración primero se lee desde determinadas variables de entorno, si establece.</span><span class="sxs-lookup"><span data-stu-id="0c831-243">`WebHostBuilder` settings are first read from certain environment variables, if set.</span></span> <span data-ttu-id="0c831-244">Estas variables de entorno deben tener el formato `ASPNETCORE_{configurationKey}`, por lo que para que el ejemplo para establecer las direcciones URL que el servidor escuchará en de forma predeterminada, se establecería `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="0c831-244">These environment variables must use the format `ASPNETCORE_{configurationKey}`, so for example to set the URLs the server will listen on by default, you would set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="0c831-245">Puede invalidar cualquiera de estos valores de variable de entorno mediante la especificación de configuración (mediante `UseConfiguration`) o estableciendo el valor explícitamente (mediante `UseUrls` para la instancia).</span><span class="sxs-lookup"><span data-stu-id="0c831-245">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseUrls` for instance).</span></span> <span data-ttu-id="0c831-246">El host usará independientemente de la opción establece el valor de la última.</span><span class="sxs-lookup"><span data-stu-id="0c831-246">The host will use whichever option sets the value last.</span></span> <span data-ttu-id="0c831-247">Por esta razón, `UseIISIntegration` debe aparecer después de `UseUrls`, dado que reemplaza la dirección URL con una dinámicamente proporcionada por IIS.</span><span class="sxs-lookup"><span data-stu-id="0c831-247">For this reason, `UseIISIntegration` must appear after `UseUrls`, because it replaces the URL with one dynamically provided by IIS.</span></span> <span data-ttu-id="0c831-248">Si desea mediante programación establece la dirección URL predeterminada en un valor, pero permite que se reemplazará por la configuración, puede configurar el host como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="0c831-248">If you want to programmatically set the default URL to one value, but allow it to be overridden with configuration, you could configure the host as follows:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseUrls("http://*:1000") // default URL
    .UseConfiguration(config) // override from command line
    .UseKestrel()
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="0c831-249">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0c831-249">Additional resources</span></span>

* [<span data-ttu-id="0c831-250">Publicar en Windows que usan IIS</span><span class="sxs-lookup"><span data-stu-id="0c831-250">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="0c831-251">Publicar en Linux con Nginx</span><span class="sxs-lookup"><span data-stu-id="0c831-251">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="0c831-252">Publicar en Linux con Apache</span><span class="sxs-lookup"><span data-stu-id="0c831-252">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="0c831-253">Host en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="0c831-253">Host in a Windows Service</span></span>](xref:hosting/windows-service)

