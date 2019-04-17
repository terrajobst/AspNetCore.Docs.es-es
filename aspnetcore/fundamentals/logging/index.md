---
title: Registro en ASP.NET Core
author: tdykstra
description: Obtenga información sobre la plataforma de registro de ASP.NET Core. Descubra los proveedores de registro integrados y obtenga más información sobre proveedores de terceros conocidos.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/logging/index
ms.openlocfilehash: f0e4dbb6fda4f676ad8e769c71cc9548a4d61d66
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614440"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="59802-104">Registro en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="59802-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="59802-105">Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="59802-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="59802-106">ASP.NET Core es compatible con una API de registro que funciona con una gran variedad de proveedores de registro integrados y de terceros.</span><span class="sxs-lookup"><span data-stu-id="59802-106">ASP.NET Core supports a logging API that works with a variety of built-in and third-party logging providers.</span></span> <span data-ttu-id="59802-107">En este artículo se muestra cómo usar las API de registro con proveedores integrados.</span><span class="sxs-lookup"><span data-stu-id="59802-107">This article shows how to use the logging API with built-in providers.</span></span>

<span data-ttu-id="59802-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="59802-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="add-providers"></a><span data-ttu-id="59802-109">Incorporación de proveedores</span><span class="sxs-lookup"><span data-stu-id="59802-109">Add providers</span></span>

<span data-ttu-id="59802-110">Un proveedor de registro muestra o almacena registros.</span><span class="sxs-lookup"><span data-stu-id="59802-110">A logging provider displays or stores logs.</span></span> <span data-ttu-id="59802-111">Por ejemplo, el proveedor de consola muestra los registros en la consola y el proveedor de Azure Application Insights los almacena en Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="59802-111">For example, the Console provider displays logs on the console, and the Azure Application Insights provider stores them in Azure Application Insights.</span></span> <span data-ttu-id="59802-112">Los registros se pueden enviar a varios destinos mediante la incorporación de varios proveedores.</span><span class="sxs-lookup"><span data-stu-id="59802-112">Logs can be sent to multiple destinations by adding multiple providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="59802-113">Para usar un proveedor, llame al método de extensión `Add{provider name}` del proveedor en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="59802-113">To add a provider, call the provider's `Add{provider name}` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

<span data-ttu-id="59802-114">La plantilla de proyecto predeterminada llama a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, que agrega los siguientes proveedores de registro:</span><span class="sxs-lookup"><span data-stu-id="59802-114">The default project template calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>

* <span data-ttu-id="59802-115">Consola</span><span class="sxs-lookup"><span data-stu-id="59802-115">Console</span></span>
* <span data-ttu-id="59802-116">Depuración</span><span class="sxs-lookup"><span data-stu-id="59802-116">Debug</span></span>
* <span data-ttu-id="59802-117">EventSource (a partir de ASP.NET Core 2.2)</span><span class="sxs-lookup"><span data-stu-id="59802-117">EventSource (starting in ASP.NET Core 2.2)</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

<span data-ttu-id="59802-118">Si usa `CreateDefaultBuilder`, puede reemplazar los proveedores predeterminados por sus propios valores.</span><span class="sxs-lookup"><span data-stu-id="59802-118">If you use `CreateDefaultBuilder`, you can replace the default providers with your own choices.</span></span> <span data-ttu-id="59802-119">Llame a <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> y agregue los proveedores que desee.</span><span class="sxs-lookup"><span data-stu-id="59802-119">Call <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A>, and add the providers you want.</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="59802-120">Para usar un proveedor, instale su paquete NuGet y llame al método de extensión del proveedor en una instancia de <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span><span class="sxs-lookup"><span data-stu-id="59802-120">To use a provider, install its NuGet package and call the provider's extension method on an instance of <xref:Microsoft.Extensions.Logging.ILoggerFactory>:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="59802-121">La [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) de ASP.NET Core proporciona la instancia de `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="59802-121">ASP.NET Core [dependency injection (DI)](xref:fundamentals/dependency-injection) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="59802-122">Los métodos de extensión `AddConsole` y `AddDebug` se definen en los paquetes [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) y [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="59802-122">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="59802-123">Cada método de extensión llama al método `ILoggerFactory.AddProvider`, pasando una instancia del proveedor.</span><span class="sxs-lookup"><span data-stu-id="59802-123">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="59802-124">La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) agrega proveedores de registro en el método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="59802-124">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="59802-125">Para obtener la salida de registro de código que se ejecuta antes, agregue los proveedores de registro en el constructor de la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="59802-125">To obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="59802-126">Obtenga más información sobre los [proveedores de registro integrados](#built-in-logging-providers) y los [proveedores de registro de terceros](#third-party-logging-providers) más adelante en el artículo.</span><span class="sxs-lookup"><span data-stu-id="59802-126">Learn more about [built-in logging providers](#built-in-logging-providers) and [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="create-logs"></a><span data-ttu-id="59802-127">Creación de registros</span><span class="sxs-lookup"><span data-stu-id="59802-127">Create logs</span></span>

<span data-ttu-id="59802-128">Obtenga un objeto <xref:Microsoft.Extensions.Logging.ILogger%601> a partir de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="59802-128">Get an <xref:Microsoft.Extensions.Logging.ILogger%601> object from DI.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="59802-129">El siguiente ejemplo de controlador crea los registros `Information` y `Warning`.</span><span class="sxs-lookup"><span data-stu-id="59802-129">The following controller example creates `Information` and `Warning` logs.</span></span> <span data-ttu-id="59802-130">La *categoría* es `TodoApiSample.Controllers.TodoController` (el nombre de clase completo de `TodoController` en la aplicación de ejemplo):</span><span class="sxs-lookup"><span data-stu-id="59802-130">The *category* is `TodoApiSample.Controllers.TodoController` (the fully qualified class name of `TodoController` in the sample app):</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="59802-131">El siguiente ejemplo de Razor Pages crea registros con `Information` como el *nivel* y `TodoApiSample.Pages.AboutModel` como la *categoría*:</span><span class="sxs-lookup"><span data-stu-id="59802-131">The following Razor Pages example creates logs with `Information` as the *level* and `TodoApiSample.Pages.AboutModel` as the *category*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="59802-132">El ejemplo anterior crea registros con `Information` y `Warning` como el *nivel* y la clase `TodoController` como la *categoría*.</span><span class="sxs-lookup"><span data-stu-id="59802-132">The preceding example creates logs with `Information` and `Warning` as the *level* and `TodoController` class as the *category*.</span></span> 

::: moniker-end

<span data-ttu-id="59802-133">El *nivel* de registro indica la gravedad del evento registrado.</span><span class="sxs-lookup"><span data-stu-id="59802-133">The Log *level* indicates the severity of the logged event.</span></span> <span data-ttu-id="59802-134">La *categoría* de registro es una cadena que está asociada con cada registro.</span><span class="sxs-lookup"><span data-stu-id="59802-134">The log *category* is a string that is associated with each log.</span></span> <span data-ttu-id="59802-135">La `ILogger<T>` instancia crea registros que tienen el nombre completo del tipo `T` como la categoría.</span><span class="sxs-lookup"><span data-stu-id="59802-135">The `ILogger<T>` instance creates logs that have the fully qualified name of type `T` as the category.</span></span> <span data-ttu-id="59802-136">Los [niveles](#log-level) y las [categorías](#log-category) se explican detalladamente más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="59802-136">[Levels](#log-level) and [categories](#log-category) are explained in more detail later in this article.</span></span> 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a><span data-ttu-id="59802-137">Creación de registros durante el inicio</span><span class="sxs-lookup"><span data-stu-id="59802-137">Create logs in Startup</span></span>

<span data-ttu-id="59802-138">Para escribir registros en la clase `Startup`, incluya un parámetro `ILogger` en la signatura de construcción:</span><span class="sxs-lookup"><span data-stu-id="59802-138">To write logs in the `Startup` class, include an `ILogger` parameter in the constructor signature:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a><span data-ttu-id="59802-139">Creación de registros en la clase Program</span><span class="sxs-lookup"><span data-stu-id="59802-139">Create logs in Program</span></span>

<span data-ttu-id="59802-140">Para escribir registros la clase `Program`, obtenga una instancia `ILogger` de inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="59802-140">To write logs in the `Program` class, get an `ILogger` instance from DI:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a><span data-ttu-id="59802-141">No hay métodos de registrador asincrónicos</span><span class="sxs-lookup"><span data-stu-id="59802-141">No asynchronous logger methods</span></span>

<span data-ttu-id="59802-142">El registro debe ser tan rápido que no merezca la pena el costo de rendimiento del código asincrónico.</span><span class="sxs-lookup"><span data-stu-id="59802-142">Logging should be so fast that it isn't worth the performance cost of asynchronous code.</span></span> <span data-ttu-id="59802-143">Si el almacén de datos de registro es lento, no escriba directamente en él.</span><span class="sxs-lookup"><span data-stu-id="59802-143">If your logging data store is slow, don't write to it directly.</span></span> <span data-ttu-id="59802-144">Considere la posibilidad de escribir los mensajes de registro en un almacén rápido inicialmente y luego moverlos a la tienda lenta.</span><span class="sxs-lookup"><span data-stu-id="59802-144">Consider writing the log messages to a fast store initially, then move them to the slow store later.</span></span> <span data-ttu-id="59802-145">Por ejemplo, si inicia sesión en SQL Server, no desea hacerlo directamente en un método `Log`, ya que los métodos `Log` son sincrónicos.</span><span class="sxs-lookup"><span data-stu-id="59802-145">For example, if you're logging to SQL Server, you don't want to do that directly in a `Log` method, since the `Log` methods are synchronous.</span></span> <span data-ttu-id="59802-146">En su lugar, agregue sincrónicamente mensajes de registro a una cola en memoria y haga que un trabajo en segundo plano extraiga los mensajes de la cola para realizar el trabajo asincrónico de insertar datos en SQL Server.</span><span class="sxs-lookup"><span data-stu-id="59802-146">Instead, synchronously add log messages to an in-memory queue and have a background worker pull the messages out of the queue to do the asynchronous work of pushing data to SQL Server.</span></span>

## <a name="configuration"></a><span data-ttu-id="59802-147">Configuración</span><span class="sxs-lookup"><span data-stu-id="59802-147">Configuration</span></span>

<span data-ttu-id="59802-148">Uno o varios proveedores de configuración proporcionan la configuración del proveedor de registro:</span><span class="sxs-lookup"><span data-stu-id="59802-148">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="59802-149">Formatos de archivo (INI, JSON y XML).</span><span class="sxs-lookup"><span data-stu-id="59802-149">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="59802-150">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="59802-150">Command-line arguments.</span></span>
* <span data-ttu-id="59802-151">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="59802-151">Environment variables.</span></span>
* <span data-ttu-id="59802-152">Objetos de .NET en memoria.</span><span class="sxs-lookup"><span data-stu-id="59802-152">In-memory .NET objects.</span></span>
* <span data-ttu-id="59802-153">El almacenamiento de [administrador secreto](xref:security/app-secrets) sin cifrar.</span><span class="sxs-lookup"><span data-stu-id="59802-153">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="59802-154">Un almacén de usuario cifrado, como [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="59802-154">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="59802-155">Proveedores personalizados (instalados o creados).</span><span class="sxs-lookup"><span data-stu-id="59802-155">Custom providers (installed or created).</span></span>

<span data-ttu-id="59802-156">Por ejemplo, la sección `Logging` de archivos de configuración de aplicación suele proporcionar la configuración de registro.</span><span class="sxs-lookup"><span data-stu-id="59802-156">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="59802-157">En el ejemplo siguiente se muestra el contenido de un archivo *appsettings.Development.json* típico:</span><span class="sxs-lookup"><span data-stu-id="59802-157">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": true
    }
  }
}
```

<span data-ttu-id="59802-158">La propiedad `Logging` puede tener `LogLevel` y propiedades del proveedor de registro (se muestra la consola).</span><span class="sxs-lookup"><span data-stu-id="59802-158">The `Logging` property can have `LogLevel` and log provider properties (Console is shown).</span></span>

<span data-ttu-id="59802-159">La propiedad `LogLevel` bajo `Logging` especifica el [nivel](#log-level) mínimo que se va a registrar para las categorías seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="59802-159">The `LogLevel` property under `Logging` specifies the minimum [level](#log-level) to log for selected categories.</span></span> <span data-ttu-id="59802-160">En el ejemplo, las categorías `System` y `Microsoft` se registran en el nivel `Information` y todas las demás se registran en el nivel `Debug`.</span><span class="sxs-lookup"><span data-stu-id="59802-160">In the example, `System` and `Microsoft` categories log at `Information` level, and all others log at `Debug` level.</span></span>

<span data-ttu-id="59802-161">Otras propiedades bajo `Logging` especifican proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="59802-161">Other properties under `Logging` specify logging providers.</span></span> <span data-ttu-id="59802-162">El ejemplo es para el proveedor de consola.</span><span class="sxs-lookup"><span data-stu-id="59802-162">The example is for the Console provider.</span></span> <span data-ttu-id="59802-163">Si un proveedor admite [ámbitos de registro](#log-scopes), `IncludeScopes` indica si están habilitados.</span><span class="sxs-lookup"><span data-stu-id="59802-163">If a provider supports [log scopes](#log-scopes), `IncludeScopes` indicates whether they're enabled.</span></span> <span data-ttu-id="59802-164">Una propiedad de proveedor (como `Console` en el ejemplo) también puede especificar una propiedad `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="59802-164">A provider property (such as `Console` in the example) may also specify a `LogLevel` property.</span></span> <span data-ttu-id="59802-165">`LogLevel` en un proveedor especifica niveles de registro para ese proveedor.</span><span class="sxs-lookup"><span data-stu-id="59802-165">`LogLevel` under a provider specifies levels to log for that provider.</span></span>

<span data-ttu-id="59802-166">Si los niveles se especifican en `Logging.{providername}.LogLevel`, invalidan todo lo establecido en `Logging.LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="59802-166">If levels are specified in `Logging.{providername}.LogLevel`, they override anything set in `Logging.LogLevel`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

<span data-ttu-id="59802-167">Las claves `LogLevel` representan los nombres de registro.</span><span class="sxs-lookup"><span data-stu-id="59802-167">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="59802-168">La clave `Default` se aplica a los registros que no se enumeran de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="59802-168">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="59802-169">El valor representa el [nivel de registro](#log-level) aplicado al registro determinado.</span><span class="sxs-lookup"><span data-stu-id="59802-169">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="59802-170">Para obtener información sobre cómo implementar proveedores de configuración, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="59802-170">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="59802-171">Salida de registro de ejemplo</span><span class="sxs-lookup"><span data-stu-id="59802-171">Sample logging output</span></span>

<span data-ttu-id="59802-172">Con el código de ejemplo que se muestra en la sección anterior, los registros aparecen en la consola cuando la aplicación se ejecuta desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="59802-172">With the sample code shown in the preceding section, logs appear in the console when the app is run from the command line.</span></span> <span data-ttu-id="59802-173">Este es un ejemplo de salida de la consola:</span><span class="sxs-lookup"><span data-stu-id="59802-173">Here's an example of console output:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

<span data-ttu-id="59802-174">Los registros anteriores se generaron mediante la realización de una solicitud HTTP GET a la aplicación de ejemplo en `http://localhost:5000/api/todo/0`.</span><span class="sxs-lookup"><span data-stu-id="59802-174">The preceding logs were generated by making an HTTP Get request to the sample app at `http://localhost:5000/api/todo/0`.</span></span>

<span data-ttu-id="59802-175">Este es un ejemplo de los mismos registros tal y como aparecen en la ventana de depuración cuando se ejecuta la aplicación de ejemplo en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="59802-175">Here's an example of the same logs as they appear in the Debug window when you run the sample app in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="59802-176">Los registros creados por las llamadas a `ILogger` que se muestran en la sección anterior comienzan por "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="59802-176">The logs that are created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="59802-177">Los registros que comienzan por categorías de "Microsoft" son del código de marco de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59802-177">The logs that begin with "Microsoft" categories are from ASP.NET Core framework code.</span></span> <span data-ttu-id="59802-178">ASP.NET Core y el código de la aplicación usan la misma API y los mismos proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="59802-178">ASP.NET Core and application code are using the same logging API and providers.</span></span>

<span data-ttu-id="59802-179">En el resto de este artículo se explican algunos detalles y opciones para el registro.</span><span class="sxs-lookup"><span data-stu-id="59802-179">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="59802-180">Paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="59802-180">NuGet packages</span></span>

<span data-ttu-id="59802-181">Las interfaces `ILogger` e `ILoggerFactory` se encuentran en [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), y sus implementaciones predeterminadas en [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="59802-181">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="59802-182">Categoría de registro</span><span class="sxs-lookup"><span data-stu-id="59802-182">Log category</span></span>

<span data-ttu-id="59802-183">Cuando se crea un objeto `ILogger`, se ha especificado una *categoría* para él.</span><span class="sxs-lookup"><span data-stu-id="59802-183">When an `ILogger` object is created, a *category* is specified for it.</span></span> <span data-ttu-id="59802-184">Esa categoría se incluye con cada mensaje de registro creado por esa instancia de `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="59802-184">That category is included with each log message created by that instance of `ILogger`.</span></span> <span data-ttu-id="59802-185">La categoría puede ser cualquier cadena, pero la convención es usar el nombre de clase, como "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="59802-185">The category may be any string, but the convention is to use the class name, such as "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="59802-186">Use `ILogger<T>` para obtener una instancia `ILogger` que utiliza el nombre de tipo completo de `T` como la categoría:</span><span class="sxs-lookup"><span data-stu-id="59802-186">Use `ILogger<T>` to get an `ILogger` instance that uses the fully qualified type name of `T` as the category:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="59802-187">Para especificar explícitamente la categoría, llame a `ILoggerFactory.CreateLogger`:</span><span class="sxs-lookup"><span data-stu-id="59802-187">To explicitly specify the category, call `ILoggerFactory.CreateLogger`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="59802-188">`ILogger<T>` es equivale a llamar a `CreateLogger` con el nombre de tipo completo de `T`.</span><span class="sxs-lookup"><span data-stu-id="59802-188">`ILogger<T>` is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="59802-189">Nivel de registro</span><span class="sxs-lookup"><span data-stu-id="59802-189">Log level</span></span>

<span data-ttu-id="59802-190">Cara registro especifica un valor <xref:Microsoft.Extensions.Logging.LogLevel>.</span><span class="sxs-lookup"><span data-stu-id="59802-190">Every log specifies a <xref:Microsoft.Extensions.Logging.LogLevel> value.</span></span> <span data-ttu-id="59802-191">El nivel de registro indica la gravedad o importancia.</span><span class="sxs-lookup"><span data-stu-id="59802-191">The log level indicates the severity or importance.</span></span> <span data-ttu-id="59802-192">Por ejemplo, podría escribir un registro `Information` cuando un método termina con normalidad y un registro `Warning` cuando un método devuelve un código de estado *404 No encontrado*.</span><span class="sxs-lookup"><span data-stu-id="59802-192">For example, you might write an `Information` log when a method ends normally and a `Warning` log when a method returns a *404 Not Found* status code.</span></span>

<span data-ttu-id="59802-193">El siguiente código crea los registros `Information` y `Warning`:</span><span class="sxs-lookup"><span data-stu-id="59802-193">The following code creates `Information` and `Warning` logs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="59802-194">En el código anterior, el primer parámetro es el [id. de evento del registro](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="59802-194">In the preceding code, the first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="59802-195">El segundo parámetro es una plantilla de mensaje con marcadores de posición para los valores de argumento proporcionados por el resto de parámetros de método.</span><span class="sxs-lookup"><span data-stu-id="59802-195">The second parameter is a message template with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="59802-196">Los parámetros de método se explican detalladamente en la [sección de la plantilla de mensaje](#log-message-template) más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="59802-196">The method parameters are explained in the [message template section](#log-message-template) later in this article.</span></span>

<span data-ttu-id="59802-197">Los métodos de registro que incluyen el nivel en el nombre del método (por ejemplo `LogInformation` y `LogWarning`) son [métodos de extensión para ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span><span class="sxs-lookup"><span data-stu-id="59802-197">Log methods that include the level in the method name (for example, `LogInformation` and `LogWarning`) are [extension methods for ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions).</span></span> <span data-ttu-id="59802-198">Estos métodos llaman a un método `Log` que toma un parámetro `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="59802-198">These methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="59802-199">Puede llamar directamente al método `Log` en lugar de a uno de estos métodos de extensión, pero la sintaxis es relativamente complicada.</span><span class="sxs-lookup"><span data-stu-id="59802-199">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="59802-200">Para más información, vea la <xref:Microsoft.Extensions.Logging.ILogger> y el [código fuente de las extensiones de registrador](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="59802-200">For more information, see <xref:Microsoft.Extensions.Logging.ILogger> and the [logger extensions source code](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).</span></span>

<span data-ttu-id="59802-201">ASP.NET Core define los niveles de registro siguientes, que aquí se ordenan de menor a mayor gravedad.</span><span class="sxs-lookup"><span data-stu-id="59802-201">ASP.NET Core defines the following log levels, ordered here from lowest to highest severity.</span></span>

* <span data-ttu-id="59802-202">Seguimiento = 0</span><span class="sxs-lookup"><span data-stu-id="59802-202">Trace = 0</span></span>

  <span data-ttu-id="59802-203">Para información que normalmente solo es útil para la depuración.</span><span class="sxs-lookup"><span data-stu-id="59802-203">For information that's typically valuable only for debugging.</span></span> <span data-ttu-id="59802-204">Estos mensajes pueden contener datos confidenciales de la aplicación, por lo que no deben habilitarse en un entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="59802-204">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="59802-205">*Deshabilitado de forma predeterminada.*</span><span class="sxs-lookup"><span data-stu-id="59802-205">*Disabled by default.*</span></span>

* <span data-ttu-id="59802-206">Depurar = 1</span><span class="sxs-lookup"><span data-stu-id="59802-206">Debug = 1</span></span>

  <span data-ttu-id="59802-207">Para información que puede ser útil para el desarrollo y la depuración.</span><span class="sxs-lookup"><span data-stu-id="59802-207">For information that may be useful in development and debugging.</span></span> <span data-ttu-id="59802-208">Ejemplo: `Entering method Configure with flag set to true.` Habilite los registros de nivel `Debug` en producción cuando esté solucionando un problema, debido al elevado volumen de registros.</span><span class="sxs-lookup"><span data-stu-id="59802-208">Example: `Entering method Configure with flag set to true.` Enable `Debug` level logs in production only when troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="59802-209">Información = 2</span><span class="sxs-lookup"><span data-stu-id="59802-209">Information = 2</span></span>

  <span data-ttu-id="59802-210">Para realizar el seguimiento del flujo general de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59802-210">For tracking the general flow of the app.</span></span> <span data-ttu-id="59802-211">Estos registros suelen tener algún valor a largo plazo.</span><span class="sxs-lookup"><span data-stu-id="59802-211">These logs typically have some long-term value.</span></span> <span data-ttu-id="59802-212">Ejemplo: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="59802-212">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="59802-213">Advertencia = 3</span><span class="sxs-lookup"><span data-stu-id="59802-213">Warning = 3</span></span>

  <span data-ttu-id="59802-214">Para los eventos anómalos o inesperados en el flujo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59802-214">For abnormal or unexpected events in the app flow.</span></span> <span data-ttu-id="59802-215">Estos pueden incluir errores u otras condiciones que no hacen que la aplicación se detenga, pero que puede que sea necesario investigar.</span><span class="sxs-lookup"><span data-stu-id="59802-215">These may include errors or other conditions that don't cause the app to stop but might need to be investigated.</span></span> <span data-ttu-id="59802-216">Las excepciones controladas son un lugar común para usar el nivel de registro `Warning`.</span><span class="sxs-lookup"><span data-stu-id="59802-216">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="59802-217">Ejemplo: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="59802-217">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="59802-218">Error = 4</span><span class="sxs-lookup"><span data-stu-id="59802-218">Error = 4</span></span>

  <span data-ttu-id="59802-219">Para los errores y excepciones que no se pueden controlar.</span><span class="sxs-lookup"><span data-stu-id="59802-219">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="59802-220">Estos mensajes indican un error en la actividad u operación actual (por ejemplo, la solicitud HTTP actual), no un error de toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59802-220">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an app-wide failure.</span></span> <span data-ttu-id="59802-221">Mensaje de registro de ejemplo: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="59802-221">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="59802-222">Crítico = 5</span><span class="sxs-lookup"><span data-stu-id="59802-222">Critical = 5</span></span>

  <span data-ttu-id="59802-223">Para los errores que requieren atención inmediata.</span><span class="sxs-lookup"><span data-stu-id="59802-223">For failures that require immediate attention.</span></span> <span data-ttu-id="59802-224">Ejemplos: escenarios de pérdida de datos, espacio en disco insuficiente.</span><span class="sxs-lookup"><span data-stu-id="59802-224">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="59802-225">Use el nivel de registro para controlar la cantidad de salida del registro que se escribe en un medio de almacenamiento determinado o ventana de presentación.</span><span class="sxs-lookup"><span data-stu-id="59802-225">Use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="59802-226">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="59802-226">For example:</span></span>

* <span data-ttu-id="59802-227">En producción, envíe `Trace` a través del nivel `Information` a un almacén de datos de volumen.</span><span class="sxs-lookup"><span data-stu-id="59802-227">In production, send `Trace` through `Information` level to a volume data store.</span></span> <span data-ttu-id="59802-228">Envíe `Warning` a través de `Critical` a un almacén de datos de valor.</span><span class="sxs-lookup"><span data-stu-id="59802-228">Send `Warning` through `Critical` to a value data store.</span></span>
* <span data-ttu-id="59802-229">Durante el desarrollo, envíe `Warning` a través de `Critical` a la consola y agregue `Trace` a través de `Information` cuando solucione problemas.</span><span class="sxs-lookup"><span data-stu-id="59802-229">During development, send `Warning` through `Critical` to the console, and add `Trace` through `Information` when troubleshooting.</span></span>

<span data-ttu-id="59802-230">En la sección [Filtrado del registro](#log-filtering) de este artículo se explica cómo controlar los niveles de registro que controla un proveedor.</span><span class="sxs-lookup"><span data-stu-id="59802-230">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="59802-231">ASP.NET Core escribe registros de eventos de marco.</span><span class="sxs-lookup"><span data-stu-id="59802-231">ASP.NET Core writes logs for framework events.</span></span> <span data-ttu-id="59802-232">En los ejemplos de registro anteriores de este artículo se excluyeron los registros por debajo del nivel `Information`, por lo que no se crearon los registros de nivel `Debug` o `Trace`.</span><span class="sxs-lookup"><span data-stu-id="59802-232">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` or `Trace` level logs were created.</span></span> <span data-ttu-id="59802-233">Este es un ejemplo de registros de consola generados mediante la ejecución de la aplicación de ejemplo configurada para mostrar registros `Debug`:</span><span class="sxs-lookup"><span data-stu-id="59802-233">Here's an example of console logs produced by running the sample app configured to show `Debug` logs:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a><span data-ttu-id="59802-234">Id. de evento del registro</span><span class="sxs-lookup"><span data-stu-id="59802-234">Log event ID</span></span>

<span data-ttu-id="59802-235">Cada registro se puede especificar un *id. de evento*.</span><span class="sxs-lookup"><span data-stu-id="59802-235">Each log can specify an *event ID*.</span></span> <span data-ttu-id="59802-236">La aplicación de ejemplo lo hace mediante una clase `LoggingEvents` definida de forma local:</span><span class="sxs-lookup"><span data-stu-id="59802-236">The sample app does this by using a locally defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="59802-237">Un id. de evento asocia un conjunto de eventos.</span><span class="sxs-lookup"><span data-stu-id="59802-237">An event ID associates a set of events.</span></span> <span data-ttu-id="59802-238">Por ejemplo, todos los registros relacionados con la presentación de una lista de elementos en una página podrían ser 1001.</span><span class="sxs-lookup"><span data-stu-id="59802-238">For example, all logs related to displaying a list of items on a page might be 1001.</span></span>

<span data-ttu-id="59802-239">El proveedor de registro puede almacenar el id. de evento en un campo de identificador, en el mensaje de registro o no almacenarlo.</span><span class="sxs-lookup"><span data-stu-id="59802-239">The logging provider may store the event ID in an ID field, in the logging message, or not at all.</span></span> <span data-ttu-id="59802-240">El proveedor de depuración no muestra los identificadores de evento.</span><span class="sxs-lookup"><span data-stu-id="59802-240">The Debug provider doesn't show event IDs.</span></span> <span data-ttu-id="59802-241">El proveedor de consola muestra los identificadores de evento entre corchetes después de la categoría:</span><span class="sxs-lookup"><span data-stu-id="59802-241">The console provider shows event IDs in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="59802-242">Plantilla de mensaje de registro</span><span class="sxs-lookup"><span data-stu-id="59802-242">Log message template</span></span>

<span data-ttu-id="59802-243">Cada registro especifica una plantilla de mensaje.</span><span class="sxs-lookup"><span data-stu-id="59802-243">Each log specifies a message template.</span></span> <span data-ttu-id="59802-244">La plantilla de mensaje puede contener marcadores de posición para los que se proporcionan argumentos.</span><span class="sxs-lookup"><span data-stu-id="59802-244">The message template can contain placeholders for which arguments are provided.</span></span> <span data-ttu-id="59802-245">Use los nombres de los marcadores de posición, no números.</span><span class="sxs-lookup"><span data-stu-id="59802-245">Use names for the placeholders, not numbers.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="59802-246">El orden de los marcadores de posición, no sus nombres, determina qué parámetros se usan para proporcionar sus valores.</span><span class="sxs-lookup"><span data-stu-id="59802-246">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="59802-247">En el código siguiente, tenga en cuenta que los nombres de parámetro están fuera de la secuencia en la plantilla de mensaje:</span><span class="sxs-lookup"><span data-stu-id="59802-247">In the following code, notice that the parameter names are out of sequence in the message template:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="59802-248">Este código crea un mensaje de registro con los valores de parámetro en secuencia:</span><span class="sxs-lookup"><span data-stu-id="59802-248">This code creates a log message with the parameter values in sequence:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="59802-249">La plataforma de registro funciona de esta manera para que los proveedores de registro puedan implementar [el registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="59802-249">The logging framework works this way so that logging providers can implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="59802-250">Los propios argumentos se pasan al sistema de registro, no solo a la plantilla de mensaje con formato.</span><span class="sxs-lookup"><span data-stu-id="59802-250">The arguments themselves are passed to the logging system, not just the formatted message template.</span></span> <span data-ttu-id="59802-251">Esta información permite a los proveedores de registro almacenar los valores de parámetro como campos.</span><span class="sxs-lookup"><span data-stu-id="59802-251">This information enables logging providers to store the parameter values as fields.</span></span> <span data-ttu-id="59802-252">Por ejemplo, suponga que las llamadas del método del registrador tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="59802-252">For example, suppose logger method calls look like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="59802-253">Si envía los registros a Azure Table Storage, cada entidad de Azure Table puede tener propiedades `ID` y `RequestTime`, lo que simplifica las consultas en los datos de registro.</span><span class="sxs-lookup"><span data-stu-id="59802-253">If you're sending the logs to Azure Table Storage, each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="59802-254">Una consulta puede buscar todos los registros dentro de un intervalo `RequestTime` determinado sin analizar el tiempo de espera del mensaje de texto.</span><span class="sxs-lookup"><span data-stu-id="59802-254">A query can find all logs within a particular `RequestTime` range without parsing the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="59802-255">Excepciones de registro</span><span class="sxs-lookup"><span data-stu-id="59802-255">Logging exceptions</span></span>

<span data-ttu-id="59802-256">Los métodos de registrador tienen sobrecargas que le permiten pasar una excepción, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="59802-256">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="59802-257">Cada proveedor controla la información de la excepción de maneras diferentes.</span><span class="sxs-lookup"><span data-stu-id="59802-257">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="59802-258">Este es un ejemplo de salida del proveedor de depuración del código mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="59802-258">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="59802-259">Filtrado del registro</span><span class="sxs-lookup"><span data-stu-id="59802-259">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="59802-260">Puede especificar un nivel de registro mínimo para un proveedor y una categoría específicos, o para todos los proveedores o todas las categorías.</span><span class="sxs-lookup"><span data-stu-id="59802-260">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="59802-261">Los registros por debajo del nivel mínimo no se pasan a ese proveedor, de modo que no se muestran o almacenan.</span><span class="sxs-lookup"><span data-stu-id="59802-261">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="59802-262">Para suprimir todos los registros, especifique `LogLevel.None` como el nivel de registro mínimo.</span><span class="sxs-lookup"><span data-stu-id="59802-262">To suppress all logs, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="59802-263">El valor entero de `LogLevel.None` es 6, que es superior a `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="59802-263">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="59802-264">Creación de reglas de filtro en la configuración</span><span class="sxs-lookup"><span data-stu-id="59802-264">Create filter rules in configuration</span></span>

<span data-ttu-id="59802-265">El código de la plantilla de proyecto llama a `CreateDefaultBuilder` para configurar el registro para los proveedores de consola y de depuración.</span><span class="sxs-lookup"><span data-stu-id="59802-265">The project template code calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="59802-266">El método `CreateDefaultBuilder` también establece el registro para buscar la configuración en una sección `Logging`, mediante código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="59802-266">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

<span data-ttu-id="59802-267">Los datos de configuración especifican niveles de registro mínimo por proveedor y categoría, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="59802-267">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="59802-268">Este archivo JSON crea seis reglas de filtro, una para el proveedor de depuración, cuatro para el proveedor de la consola y una para todos los proveedores.</span><span class="sxs-lookup"><span data-stu-id="59802-268">This JSON creates six filter rules: one for the Debug provider, four for the Console provider, and one for all providers.</span></span> <span data-ttu-id="59802-269">Se elige una sola regla para cada proveedor cuando se crea un objeto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="59802-269">A single rule is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="59802-270">Reglas de filtro en el código</span><span class="sxs-lookup"><span data-stu-id="59802-270">Filter rules in code</span></span>

<span data-ttu-id="59802-271">En el siguiente ejemplo se muestra cómo registrar reglas de filtro en el código:</span><span class="sxs-lookup"><span data-stu-id="59802-271">The following example shows how to register filter rules in code:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="59802-272">El segundo `AddFilter` especifica el proveedor de depuración mediante su nombre de tipo.</span><span class="sxs-lookup"><span data-stu-id="59802-272">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="59802-273">El primer `AddFilter` se aplica a todos los proveedores, dado que no especifica un tipo de proveedor.</span><span class="sxs-lookup"><span data-stu-id="59802-273">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="59802-274">Cómo se aplican las reglas de filtro</span><span class="sxs-lookup"><span data-stu-id="59802-274">How filtering rules are applied</span></span>

<span data-ttu-id="59802-275">Los datos de configuración y el código de `AddFilter` que se muestran en los ejemplos anteriores crean las reglas que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="59802-275">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="59802-276">Las seis primeras proceden del ejemplo de configuración y las dos últimas del ejemplo de código.</span><span class="sxs-lookup"><span data-stu-id="59802-276">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="59802-277">número</span><span class="sxs-lookup"><span data-stu-id="59802-277">Number</span></span> | <span data-ttu-id="59802-278">Proveedor</span><span class="sxs-lookup"><span data-stu-id="59802-278">Provider</span></span>      | <span data-ttu-id="59802-279">Categorías que comienzan por...</span><span class="sxs-lookup"><span data-stu-id="59802-279">Categories that begin with ...</span></span>          | <span data-ttu-id="59802-280">Nivel de registro mínimo</span><span class="sxs-lookup"><span data-stu-id="59802-280">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="59802-281">1</span><span class="sxs-lookup"><span data-stu-id="59802-281">1</span></span>      | <span data-ttu-id="59802-282">Depuración</span><span class="sxs-lookup"><span data-stu-id="59802-282">Debug</span></span>         | <span data-ttu-id="59802-283">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="59802-283">All categories</span></span>                          | <span data-ttu-id="59802-284">Información</span><span class="sxs-lookup"><span data-stu-id="59802-284">Information</span></span>       |
| <span data-ttu-id="59802-285">2</span><span class="sxs-lookup"><span data-stu-id="59802-285">2</span></span>      | <span data-ttu-id="59802-286">Consola</span><span class="sxs-lookup"><span data-stu-id="59802-286">Console</span></span>       | <span data-ttu-id="59802-287">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="59802-287">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="59802-288">Advertencia</span><span class="sxs-lookup"><span data-stu-id="59802-288">Warning</span></span>           |
| <span data-ttu-id="59802-289">3</span><span class="sxs-lookup"><span data-stu-id="59802-289">3</span></span>      | <span data-ttu-id="59802-290">Consola</span><span class="sxs-lookup"><span data-stu-id="59802-290">Console</span></span>       | <span data-ttu-id="59802-291">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="59802-291">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="59802-292">Depuración</span><span class="sxs-lookup"><span data-stu-id="59802-292">Debug</span></span>             |
| <span data-ttu-id="59802-293">4</span><span class="sxs-lookup"><span data-stu-id="59802-293">4</span></span>      | <span data-ttu-id="59802-294">Consola</span><span class="sxs-lookup"><span data-stu-id="59802-294">Console</span></span>       | <span data-ttu-id="59802-295">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="59802-295">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="59802-296">Error</span><span class="sxs-lookup"><span data-stu-id="59802-296">Error</span></span>             |
| <span data-ttu-id="59802-297">5</span><span class="sxs-lookup"><span data-stu-id="59802-297">5</span></span>      | <span data-ttu-id="59802-298">Consola</span><span class="sxs-lookup"><span data-stu-id="59802-298">Console</span></span>       | <span data-ttu-id="59802-299">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="59802-299">All categories</span></span>                          | <span data-ttu-id="59802-300">Información</span><span class="sxs-lookup"><span data-stu-id="59802-300">Information</span></span>       |
| <span data-ttu-id="59802-301">6</span><span class="sxs-lookup"><span data-stu-id="59802-301">6</span></span>      | <span data-ttu-id="59802-302">Todos los proveedores</span><span class="sxs-lookup"><span data-stu-id="59802-302">All providers</span></span> | <span data-ttu-id="59802-303">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="59802-303">All categories</span></span>                          | <span data-ttu-id="59802-304">Depuración</span><span class="sxs-lookup"><span data-stu-id="59802-304">Debug</span></span>             |
| <span data-ttu-id="59802-305">7</span><span class="sxs-lookup"><span data-stu-id="59802-305">7</span></span>      | <span data-ttu-id="59802-306">Todos los proveedores</span><span class="sxs-lookup"><span data-stu-id="59802-306">All providers</span></span> | <span data-ttu-id="59802-307">Sistema</span><span class="sxs-lookup"><span data-stu-id="59802-307">System</span></span>                                  | <span data-ttu-id="59802-308">Depuración</span><span class="sxs-lookup"><span data-stu-id="59802-308">Debug</span></span>             |
| <span data-ttu-id="59802-309">8</span><span class="sxs-lookup"><span data-stu-id="59802-309">8</span></span>      | <span data-ttu-id="59802-310">Depuración</span><span class="sxs-lookup"><span data-stu-id="59802-310">Debug</span></span>         | <span data-ttu-id="59802-311">Microsoft</span><span class="sxs-lookup"><span data-stu-id="59802-311">Microsoft</span></span>                               | <span data-ttu-id="59802-312">Seguimiento</span><span class="sxs-lookup"><span data-stu-id="59802-312">Trace</span></span>             |

<span data-ttu-id="59802-313">Cuando se crea un objeto `ILogger`, el objeto `ILoggerFactory` selecciona una sola regla por proveedor para aplicar a ese registrador.</span><span class="sxs-lookup"><span data-stu-id="59802-313">When an `ILogger` object is created, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="59802-314">Todos los mensajes escritos por una instancia `ILogger` se filtran según las reglas seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="59802-314">All messages written by an `ILogger` instance are filtered based on the selected rules.</span></span> <span data-ttu-id="59802-315">De las reglas disponibles se selecciona la más específica posible para cada par de categoría y proveedor.</span><span class="sxs-lookup"><span data-stu-id="59802-315">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="59802-316">Cuando se crea un `ILogger` para una categoría determinada, se usa el algoritmo siguiente para cada proveedor:</span><span class="sxs-lookup"><span data-stu-id="59802-316">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="59802-317">Se seleccionan todas las reglas que coinciden con el proveedor o su alias.</span><span class="sxs-lookup"><span data-stu-id="59802-317">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="59802-318">Si no se encuentra ninguna coincidencia, se seleccionan todas las reglas con un proveedor vacío.</span><span class="sxs-lookup"><span data-stu-id="59802-318">If no match is found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="59802-319">Del resultado del paso anterior, se seleccionan las reglas con el prefijo de categoría coincidente más largo.</span><span class="sxs-lookup"><span data-stu-id="59802-319">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="59802-320">Si no se encuentra ninguna coincidencia, se seleccionan todas las reglas que no especifican una categoría.</span><span class="sxs-lookup"><span data-stu-id="59802-320">If no match is found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="59802-321">Si se seleccionan varias reglas, se toma la **última**.</span><span class="sxs-lookup"><span data-stu-id="59802-321">If multiple rules are selected, take the **last** one.</span></span>
* <span data-ttu-id="59802-322">Si no se selecciona ninguna regla, se usa `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="59802-322">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="59802-323">Con la lista de reglas anterior, supongamos que crea un objeto `ILogger` para la categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="59802-323">With the preceding list of rules, suppose you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="59802-324">Para el proveedor de depuración, se aplican las reglas 1, 6 y 8.</span><span class="sxs-lookup"><span data-stu-id="59802-324">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="59802-325">La regla 8 es la más específica, por lo que se selecciona.</span><span class="sxs-lookup"><span data-stu-id="59802-325">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="59802-326">Para el proveedor de la consola, se aplican las reglas 3, 4, 5 y 6.</span><span class="sxs-lookup"><span data-stu-id="59802-326">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="59802-327">La regla 3 es la más específica.</span><span class="sxs-lookup"><span data-stu-id="59802-327">Rule 3 is most specific.</span></span>

<span data-ttu-id="59802-328">La instancia `ILogger` resultante envía los registros de nivel `Trace` y superiores al proveedor de depuración.</span><span class="sxs-lookup"><span data-stu-id="59802-328">The resulting `ILogger` instance sends logs of `Trace` level and above to the Debug provider.</span></span> <span data-ttu-id="59802-329">Los registros de nivel `Debug` y superiores se envían al proveedor de consola.</span><span class="sxs-lookup"><span data-stu-id="59802-329">Logs of `Debug` level and above are sent to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="59802-330">Alias de proveedor</span><span class="sxs-lookup"><span data-stu-id="59802-330">Provider aliases</span></span>

<span data-ttu-id="59802-331">Cada proveedor define un *alias* que se puede utilizar en la configuración en lugar del nombre de tipo completo.</span><span class="sxs-lookup"><span data-stu-id="59802-331">Each provider defines an *alias* that can be used in configuration in place of the fully qualified type name.</span></span>  <span data-ttu-id="59802-332">Para los proveedores integrados, use los alias siguientes:</span><span class="sxs-lookup"><span data-stu-id="59802-332">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="59802-333">Consola</span><span class="sxs-lookup"><span data-stu-id="59802-333">Console</span></span>
* <span data-ttu-id="59802-334">Depuración</span><span class="sxs-lookup"><span data-stu-id="59802-334">Debug</span></span>
* <span data-ttu-id="59802-335">EventLog</span><span class="sxs-lookup"><span data-stu-id="59802-335">EventLog</span></span>
* <span data-ttu-id="59802-336">AzureAppServicesFile</span><span class="sxs-lookup"><span data-stu-id="59802-336">AzureAppServicesFile</span></span>
* <span data-ttu-id="59802-337">AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="59802-337">AzureAppServicesBlob</span></span>
* <span data-ttu-id="59802-338">TraceSource</span><span class="sxs-lookup"><span data-stu-id="59802-338">TraceSource</span></span>
* <span data-ttu-id="59802-339">EventSource</span><span class="sxs-lookup"><span data-stu-id="59802-339">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="59802-340">Nivel mínimo predeterminado</span><span class="sxs-lookup"><span data-stu-id="59802-340">Default minimum level</span></span>

<span data-ttu-id="59802-341">Hay una configuración de nivel mínimo que solo tiene efecto si no se aplica ninguna regla de configuración o código para un proveedor y una categoría determinados.</span><span class="sxs-lookup"><span data-stu-id="59802-341">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="59802-342">En el ejemplo siguiente se muestra cómo establecer el nivel mínimo:</span><span class="sxs-lookup"><span data-stu-id="59802-342">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="59802-343">Si no establece explícitamente el nivel mínimo, el valor predeterminado es `Information`, lo que significa que los registros `Trace` y `Debug` se omiten.</span><span class="sxs-lookup"><span data-stu-id="59802-343">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="59802-344">Funciones de filtro</span><span class="sxs-lookup"><span data-stu-id="59802-344">Filter functions</span></span>

<span data-ttu-id="59802-345">Se invoca una función de filtro para todos los proveedores y categorías que no tienen reglas asignadas mediante configuración o código.</span><span class="sxs-lookup"><span data-stu-id="59802-345">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="59802-346">El código de la función tiene acceso al tipo de proveedor, la categoría y el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="59802-346">Code in the function has access to the provider type, category, and log level.</span></span> <span data-ttu-id="59802-347">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="59802-347">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="59802-348">Algunos proveedores de registro permiten especificar cuándo deben escribirse los registros en un medio de almacenamiento o ignorarse en función de la categoría y el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="59802-348">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="59802-349">Los métodos de extensión `AddConsole` y `AddDebug` proporcionan sobrecargas que aceptan criterios de filtrado.</span><span class="sxs-lookup"><span data-stu-id="59802-349">The `AddConsole` and `AddDebug` extension methods provide overloads that accept filtering criteria.</span></span> <span data-ttu-id="59802-350">El código de ejemplo siguiente hace que el proveedor de la consola ignore los registros por debajo del nivel `Warning`, mientras que el proveedor de depuración omite los registros creados por la plataforma.</span><span class="sxs-lookup"><span data-stu-id="59802-350">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="59802-351">El método `AddEventLog` tiene una sobrecarga que toma una instancia de `EventLogSettings`, que puede contener una función de filtrado en su propiedad `Filter`.</span><span class="sxs-lookup"><span data-stu-id="59802-351">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="59802-352">El proveedor de TraceSource no proporciona ninguna de estas sobrecargas, dado que su nivel de registro y otros parámetros se basan en el `SourceSwitch` y `TraceListener` que usa.</span><span class="sxs-lookup"><span data-stu-id="59802-352">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="59802-353">Para establecer reglas de filtrado para todos los proveedores que están registrados con un instancia de `ILoggerFactory`, use el método de extensión `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="59802-353">To set filtering rules for all providers that are registered with an `ILoggerFactory` instance, use the `WithFilter` extension method.</span></span> <span data-ttu-id="59802-354">En el ejemplo siguiente se limitan los registros de la plataforma (la categoría comienza con "Microsoft" o "System") a las advertencias mientras los registros creados por el código de aplicación se registran en el nivel de depuración.</span><span class="sxs-lookup"><span data-stu-id="59802-354">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while logging at debug level for logs created by application code.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="59802-355">Para evitar que los registros se escriban, especifique `LogLevel.None` como el nivel de registro mínimo.</span><span class="sxs-lookup"><span data-stu-id="59802-355">To prevent any logs from being written, specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="59802-356">El valor entero de `LogLevel.None` es 6, que es superior a `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="59802-356">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="59802-357">El paquete NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) proporciona el método de extensión `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="59802-357">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="59802-358">El método devuelve una instancia nueva de `ILoggerFactory` que filtrará los mensajes de registro pasados a todos los proveedores de registrador registrados con ella.</span><span class="sxs-lookup"><span data-stu-id="59802-358">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="59802-359">No afecta a ninguna otra instancia de `ILoggerFactory`, incluida la instancia de `ILoggerFactory` original.</span><span class="sxs-lookup"><span data-stu-id="59802-359">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="system-categories-and-levels"></a><span data-ttu-id="59802-360">Niveles y categorías del sistema</span><span class="sxs-lookup"><span data-stu-id="59802-360">System categories and levels</span></span>

<span data-ttu-id="59802-361">Estas son algunas categorías que ASP.NET Core y Entity Framework Core usan, con notas sobre lo que los registros de espera de ellas:</span><span class="sxs-lookup"><span data-stu-id="59802-361">Here are some categories used by ASP.NET Core and Entity Framework Core, with notes about what logs to expect from them:</span></span>

| <span data-ttu-id="59802-362">Categoría</span><span class="sxs-lookup"><span data-stu-id="59802-362">Category</span></span>                            | <span data-ttu-id="59802-363">Notas</span><span class="sxs-lookup"><span data-stu-id="59802-363">Notes</span></span> |
| ----------------------------------- | ----- |
| <span data-ttu-id="59802-364">Microsoft.AspNetCore</span><span class="sxs-lookup"><span data-stu-id="59802-364">Microsoft.AspNetCore</span></span>                | <span data-ttu-id="59802-365">Diagnósticos generales de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59802-365">General ASP.NET Core diagnostics.</span></span> |
| <span data-ttu-id="59802-366">Microsoft.AspNetCore.DataProtection</span><span class="sxs-lookup"><span data-stu-id="59802-366">Microsoft.AspNetCore.DataProtection</span></span> | <span data-ttu-id="59802-367">Qué claves se tuvieron en cuenta, encontraron y usaron.</span><span class="sxs-lookup"><span data-stu-id="59802-367">Which keys were considered, found, and used.</span></span> |
| <span data-ttu-id="59802-368">Microsoft.AspNetCore.HostFiltering</span><span class="sxs-lookup"><span data-stu-id="59802-368">Microsoft.AspNetCore.HostFiltering</span></span>  | <span data-ttu-id="59802-369">Hosts permitidos.</span><span class="sxs-lookup"><span data-stu-id="59802-369">Hosts allowed.</span></span> |
| <span data-ttu-id="59802-370">Microsoft.AspNetCore.Hosting</span><span class="sxs-lookup"><span data-stu-id="59802-370">Microsoft.AspNetCore.Hosting</span></span>        | <span data-ttu-id="59802-371">Cuánto tiempo tardaron en completarse las solicitudes HTTP y a qué hora comenzaron.</span><span class="sxs-lookup"><span data-stu-id="59802-371">How long HTTP requests took to complete and what time they started.</span></span> <span data-ttu-id="59802-372">Qué ensamblados de inicio de hospedaje se cargaron.</span><span class="sxs-lookup"><span data-stu-id="59802-372">Which hosting startup assemblies were loaded.</span></span> |
| <span data-ttu-id="59802-373">Microsoft.AspNetCore.Mvc</span><span class="sxs-lookup"><span data-stu-id="59802-373">Microsoft.AspNetCore.Mvc</span></span>            | <span data-ttu-id="59802-374">Diagnósticos de MVC y Razor.</span><span class="sxs-lookup"><span data-stu-id="59802-374">MVC and Razor diagnostics.</span></span> <span data-ttu-id="59802-375">Enlace de modelos, ejecución de filtros, compilación de vistas y selección de acciones.</span><span class="sxs-lookup"><span data-stu-id="59802-375">Model binding, filter execution, view compilation, action selection.</span></span> |
| <span data-ttu-id="59802-376">Microsoft.AspNetCore.Routing</span><span class="sxs-lookup"><span data-stu-id="59802-376">Microsoft.AspNetCore.Routing</span></span>        | <span data-ttu-id="59802-377">Información de coincidencia de ruta.</span><span class="sxs-lookup"><span data-stu-id="59802-377">Route matching information.</span></span> |
| <span data-ttu-id="59802-378">Microsoft.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="59802-378">Microsoft.AspNetCore.Server</span></span>         | <span data-ttu-id="59802-379">Inicio y detención de conexión y mantener las respuestas activas.</span><span class="sxs-lookup"><span data-stu-id="59802-379">Connection start, stop, and keep alive responses.</span></span> <span data-ttu-id="59802-380">Información de certificado HTTPS.</span><span class="sxs-lookup"><span data-stu-id="59802-380">HTTPS certificate information.</span></span> |
| <span data-ttu-id="59802-381">Microsoft.AspNetCore.StaticFiles</span><span class="sxs-lookup"><span data-stu-id="59802-381">Microsoft.AspNetCore.StaticFiles</span></span>    | <span data-ttu-id="59802-382">Archivos servidos.</span><span class="sxs-lookup"><span data-stu-id="59802-382">Files served.</span></span> |
| <span data-ttu-id="59802-383">Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="59802-383">Microsoft.EntityFrameworkCore</span></span>       | <span data-ttu-id="59802-384">Diagnósticos generales de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="59802-384">General Entity Framework Core diagnostics.</span></span> <span data-ttu-id="59802-385">Actividad y la configuración de bases de datos, detección de cambios y migraciones.</span><span class="sxs-lookup"><span data-stu-id="59802-385">Database activity and configuration, change detection, migrations.</span></span> |

## <a name="log-scopes"></a><span data-ttu-id="59802-386">Ámbitos de registro</span><span class="sxs-lookup"><span data-stu-id="59802-386">Log scopes</span></span>

 <span data-ttu-id="59802-387">Un *ámbito* puede agrupar un conjunto de operaciones lógicas.</span><span class="sxs-lookup"><span data-stu-id="59802-387">A *scope* can group a set of logical operations.</span></span> <span data-ttu-id="59802-388">Esta agrupación se puede utilizar para adjuntar los mismos datos para cada registro que se crea como parte de un conjunto.</span><span class="sxs-lookup"><span data-stu-id="59802-388">This grouping can be used to attach the same data to each log that's created as part of a set.</span></span> <span data-ttu-id="59802-389">Por ejemplo, cada registro creado como parte del procesamiento de una transacción puede incluir el identificador de dicha transacción.</span><span class="sxs-lookup"><span data-stu-id="59802-389">For example, every log created as part of processing a transaction can include the transaction ID.</span></span>

<span data-ttu-id="59802-390">Un ámbito es un tipo `IDisposable` devuelto por el método <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> y se conserva hasta que se elimina.</span><span class="sxs-lookup"><span data-stu-id="59802-390">A scope is an `IDisposable` type that's returned by the <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*> method and lasts until it's disposed.</span></span> <span data-ttu-id="59802-391">Use un ámbito encapsulando las llamadas de registrador en un bloque `using`:</span><span class="sxs-lookup"><span data-stu-id="59802-391">Use a scope by wrapping logger calls in a `using` block:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="59802-392">El código siguiente permite ámbitos para el proveedor de la consola:</span><span class="sxs-lookup"><span data-stu-id="59802-392">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="59802-393">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="59802-393">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="59802-394">Es necesario configurar la opción del registrador de consola `IncludeScopes` para habilitar el registro basado en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="59802-394">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="59802-395">Para obtener información sobre la configuración, consulte la sección [Configuración](#configuration).</span><span class="sxs-lookup"><span data-stu-id="59802-395">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="59802-396">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="59802-396">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="59802-397">Es necesario configurar la opción del registrador de consola `IncludeScopes` para habilitar el registro basado en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="59802-397">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="59802-398">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="59802-398">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="59802-399">Cada mensaje de registro incluye la información de ámbito:</span><span class="sxs-lookup"><span data-stu-id="59802-399">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="59802-400">Proveedores de registro integrados</span><span class="sxs-lookup"><span data-stu-id="59802-400">Built-in logging providers</span></span>

<span data-ttu-id="59802-401">ASP.NET Core incluye los proveedores siguientes:</span><span class="sxs-lookup"><span data-stu-id="59802-401">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="59802-402">Consola</span><span class="sxs-lookup"><span data-stu-id="59802-402">Console</span></span>](#console-provider)
* [<span data-ttu-id="59802-403">Depurar</span><span class="sxs-lookup"><span data-stu-id="59802-403">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="59802-404">EventSource</span><span class="sxs-lookup"><span data-stu-id="59802-404">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="59802-405">EventLog</span><span class="sxs-lookup"><span data-stu-id="59802-405">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="59802-406">TraceSource</span><span class="sxs-lookup"><span data-stu-id="59802-406">TraceSource</span></span>](#tracesource-provider)

<span data-ttu-id="59802-407">Las opciones para el [registro en Azure](#logging-in-azure) se tratan más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="59802-407">Options for [Logging in Azure](#logging-in-azure) are covered later in this article.</span></span>

<span data-ttu-id="59802-408">Para información sobre el registro de stdout, consulte <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> y <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="59802-408">For information about stdout logging, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> and <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

### <a name="console-provider"></a><span data-ttu-id="59802-409">Proveedor de la consola</span><span class="sxs-lookup"><span data-stu-id="59802-409">Console provider</span></span>

<span data-ttu-id="59802-410">El paquete de proveedor [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envía la salida del registro a la consola.</span><span class="sxs-lookup"><span data-stu-id="59802-410">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="59802-411">[Las sobrecargas de AddConsole](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) permiten pasar un nivel de registro mínimo, una función de filtro y un valor booleano que indica si se admiten los ámbitos.</span><span class="sxs-lookup"><span data-stu-id="59802-411">[AddConsole overloads](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) let you pass in a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="59802-412">Otra opción consiste en pasar un objeto `IConfiguration`, que puede especificar niveles de registro y si se admiten los ámbitos.</span><span class="sxs-lookup"><span data-stu-id="59802-412">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="59802-413">El proveedor de consola tiene un impacto importante en el rendimiento y no suele ser adecuado para su uso en producción.</span><span class="sxs-lookup"><span data-stu-id="59802-413">The console provider has a significant impact on performance and is generally not appropriate for use in production.</span></span>

<span data-ttu-id="59802-414">Cuando se crea un proyecto en Visual Studio, el método `AddConsole` tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="59802-414">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="59802-415">Este código hace referencia a la sección `Logging` del archivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="59802-415">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

<span data-ttu-id="59802-416">La configuración que se muestra limita los registros de la plataforma a las advertencias mientras que permite a la aplicación registrar en el nivel de depuración, como se explica en la sección [Filtrado del registro](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="59802-416">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="59802-417">Para obtener más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="59802-417">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

<span data-ttu-id="59802-418">Para ver una salida de registro de la consola, abra un símbolo del sistema en la carpeta del proyecto y ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="59802-418">To see console logging output, open a command prompt in the project folder and run the following command:</span></span>

```console
dotnet run
```

### <a name="debug-provider"></a><span data-ttu-id="59802-419">Proveedor de depuración</span><span class="sxs-lookup"><span data-stu-id="59802-419">Debug provider</span></span>

<span data-ttu-id="59802-420">El paquete de proveedor [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) escribe la salida del registro mediante la clase [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (llamadas a métodos `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="59802-420">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="59802-421">En Linux, este proveedor escribe registros en */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="59802-421">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="59802-422">[Las sobrecargas de AddDebug](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) permiten pasar un nivel mínimo de registro o una función de filtro.</span><span class="sxs-lookup"><span data-stu-id="59802-422">[AddDebug overloads](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="59802-423">Proveedor EventSource</span><span class="sxs-lookup"><span data-stu-id="59802-423">EventSource provider</span></span>

<span data-ttu-id="59802-424">Para las aplicaciones que tengan como destino ASP.NET Core 1.1.0 o una versión posterior, el paquete de proveedor [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) puede implementar el seguimiento de eventos.</span><span class="sxs-lookup"><span data-stu-id="59802-424">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="59802-425">En Windows, usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="59802-425">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="59802-426">Es un proveedor multiplataforma, pero todavía no hay herramientas de recopilación y visualización de eventos para Linux o macOS.</span><span class="sxs-lookup"><span data-stu-id="59802-426">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

<span data-ttu-id="59802-427">Una buena manera de recopilar y ver los registros es usar la [utilidad PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="59802-427">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="59802-428">Hay otras herramientas para ver los registros ETW, pero PerfView proporciona la mejor experiencia para trabajar con los eventos ETW emitidos por ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="59802-428">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="59802-429">Para configurar PerfView para la recopilación de eventos registrados por este proveedor, agregue la cadena `*Microsoft-Extensions-Logging` a la lista **Proveedores adicionales**.</span><span class="sxs-lookup"><span data-stu-id="59802-429">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="59802-430">(No olvide el asterisco al principio de la cadena).</span><span class="sxs-lookup"><span data-stu-id="59802-430">(Don't miss the asterisk at the start of the string.)</span></span>

![Proveedores adicionales de Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="59802-432">Proveedor EventLog de Windows</span><span class="sxs-lookup"><span data-stu-id="59802-432">Windows EventLog provider</span></span>

<span data-ttu-id="59802-433">El paquete de proveedor [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envía la salida del registro al Registro de eventos de Windows.</span><span class="sxs-lookup"><span data-stu-id="59802-433">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="59802-434">[Las sobrecargas de AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) permiten pasar `EventLogSettings` o un nivel de registro mínimo.</span><span class="sxs-lookup"><span data-stu-id="59802-434">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="59802-435">Proveedor TraceSource</span><span class="sxs-lookup"><span data-stu-id="59802-435">TraceSource provider</span></span>

<span data-ttu-id="59802-436">El paquete de proveedor [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa las bibliotecas y proveedores de <xref:System.Diagnostics.TraceSource>.</span><span class="sxs-lookup"><span data-stu-id="59802-436">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the <xref:System.Diagnostics.TraceSource> libraries and providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

<span data-ttu-id="59802-437">[Las sobrecargas de AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) permiten pasar un modificador de origen y un agente de escucha de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="59802-437">[AddTraceSource overloads](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="59802-438">Para usar este proveedor, una aplicación debe ejecutarse en .NET Framework (en lugar de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="59802-438">To use this provider, an app has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="59802-439">El proveedor puede enrutar mensajes a una variedad de [agentes de escucha](/dotnet/framework/debug-trace-profile/trace-listeners), como <xref:System.Diagnostics.TextWriterTraceListener> que se usa en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="59802-439">The provider can route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the <xref:System.Diagnostics.TextWriterTraceListener> used in the sample app.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="59802-440">En el ejemplo siguiente se configura un proveedor `TraceSource` que registra mensajes `Warning` y superiores en la ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="59802-440">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

## <a name="logging-in-azure"></a><span data-ttu-id="59802-441">Registro en Azure</span><span class="sxs-lookup"><span data-stu-id="59802-441">Logging in Azure</span></span>

<span data-ttu-id="59802-442">Para información sobre el registro en Azure, consulte las secciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="59802-442">For information about logging in Azure, see the following sections:</span></span>

* [<span data-ttu-id="59802-443">Proveedor Azure App Service</span><span class="sxs-lookup"><span data-stu-id="59802-443">Azure App Service provider</span></span>](#azure-app-service-provider)
* [<span data-ttu-id="59802-444">Secuencias de registro de Azure</span><span class="sxs-lookup"><span data-stu-id="59802-444">Azure log streaming</span></span>](#azure-log-streaming)

::: moniker range=">= aspnetcore-1.1"

* [<span data-ttu-id="59802-445">Registro de seguimiento de Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="59802-445">Azure Application Insights trace logging</span></span>](#azure-application-insights-trace-logging)

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="59802-446">Proveedor Azure App Service</span><span class="sxs-lookup"><span data-stu-id="59802-446">Azure App Service provider</span></span>

<span data-ttu-id="59802-447">El paquete de proveedor [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) escribe los registros en archivos de texto en el sistema de archivos de una aplicación de Azure App Service y en [Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) en una cuenta de Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="59802-447">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="59802-448">El paquete de proveedor está disponible para aplicaciones cuyo destino sea .NET Core 1.1 o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="59802-448">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="59802-449">Si el destino es .NET Core, tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="59802-449">If targeting .NET Core, note the following points:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="59802-450">El paquete de proveedor está incluido en el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59802-450">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="59802-451">El paquete de proveedor no está incluido en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="59802-451">The provider package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="59802-452">Para usar el proveedor, instale el paquete.</span><span class="sxs-lookup"><span data-stu-id="59802-452">To use the provider, install the package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="59802-453">No llame explícitamente a <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>.</span><span class="sxs-lookup"><span data-stu-id="59802-453">Don't explicitly call <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*>.</span></span> <span data-ttu-id="59802-454">El proveedor está disponible automáticamente para la aplicación cuando esta se implementa en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="59802-454">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="59802-455">Si el destino es .NET Framework o hace referencia al metapaquete `Microsoft.AspNetCore.App`, agregue el paquete de proveedor al proyecto.</span><span class="sxs-lookup"><span data-stu-id="59802-455">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="59802-456">Invoke `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="59802-456">Invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="59802-457">Una sobrecarga <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> permite pasar <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span><span class="sxs-lookup"><span data-stu-id="59802-457">An <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> overload lets you pass in <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>.</span></span> <span data-ttu-id="59802-458">El objeto de configuración puede invalidar la configuración predeterminada, como la plantilla de salida de registro, el nombre de blob y el límite de tamaño de archivo.</span><span class="sxs-lookup"><span data-stu-id="59802-458">The settings object can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="59802-459">(La *plantilla salida* es una plantilla de mensaje que se aplica a todos los registros además de que se proporciona con una llamada de método `ILogger`).</span><span class="sxs-lookup"><span data-stu-id="59802-459">(*Output template* is a message template that's applied to all logs in addition to what's provided with an `ILogger` method call.)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="59802-460">Para configurar las opciones de proveedor, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> y <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, tal y como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="59802-460">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

<span data-ttu-id="59802-461">Cuando se implementa en una aplicación de App Service, la aplicación respeta la configuración de la sección [Registros de diagnóstico](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) de la página **App Service** de Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="59802-461">When you deploy to an App Service app, the application honors the settings in the [Diagnostic Logs](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="59802-462">Cuando se actualiza esta configuración, los cambios se aplican inmediatamente sin necesidad de reiniciar ni de volver a implementar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59802-462">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Configuración de los registros de Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="59802-464">La ubicación predeterminada de los archivos de registro es la carpeta *D:\\home\\LogFiles\\Application* y el nombre de archivo predeterminado es *diagnostics-aaaammdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="59802-464">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="59802-465">El límite de tamaño de archivo predeterminado es 10 MB, y el número máximo predeterminado de archivos que se conservan es 2.</span><span class="sxs-lookup"><span data-stu-id="59802-465">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="59802-466">El nombre de blob predeterminado es *{nombre-de-la-aplicación}{marca de tiempo}/aaaa/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="59802-466">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="59802-467">El proveedor solo funciona cuando el proyecto se ejecuta en el entorno de Azure.</span><span class="sxs-lookup"><span data-stu-id="59802-467">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="59802-468">No tiene ningún efecto cuando el proyecto se ejecuta de manera local (no escribe en los archivos locales ni en el almacenamiento de desarrollo local de blobs).</span><span class="sxs-lookup"><span data-stu-id="59802-468">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

### <a name="azure-log-streaming"></a><span data-ttu-id="59802-469">Secuencias de registro de Azure</span><span class="sxs-lookup"><span data-stu-id="59802-469">Azure log streaming</span></span>

<span data-ttu-id="59802-470">Las secuencias de registro de Azure permiten ver la actividad de registro en tiempo real desde:</span><span class="sxs-lookup"><span data-stu-id="59802-470">Azure log streaming lets you view log activity in real time from:</span></span>

* <span data-ttu-id="59802-471">El servidor de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="59802-471">The app server</span></span>
* <span data-ttu-id="59802-472">El servidor web</span><span class="sxs-lookup"><span data-stu-id="59802-472">The web server</span></span>
* <span data-ttu-id="59802-473">Error del seguimiento de solicitudes</span><span class="sxs-lookup"><span data-stu-id="59802-473">Failed request tracing</span></span>

<span data-ttu-id="59802-474">Para configurar las secuencias de registro de Azure:</span><span class="sxs-lookup"><span data-stu-id="59802-474">To configure Azure log streaming:</span></span>

* <span data-ttu-id="59802-475">Navegue hasta la página **Registros de diagnóstico** desde la página de portal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59802-475">Navigate to the **Diagnostics Logs** page from your app's portal page.</span></span>
* <span data-ttu-id="59802-476">Establezca **Registro de la aplicación (sistema de archivos)** en **Activado**.</span><span class="sxs-lookup"><span data-stu-id="59802-476">Set **Application Logging (Filesystem)** to **On**.</span></span>

![Página de registros de diagnóstico de Azure Portal](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="59802-478">Navegue hasta la página **Secuencias de registro** para ver los mensajes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="59802-478">Navigate to the **Log Streaming** page to view app messages.</span></span> <span data-ttu-id="59802-479">La aplicación los registra a través de la interfaz `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="59802-479">They're logged by the app through the `ILogger` interface.</span></span>

![Secuencias de registro de aplicación de Azure Portal](index/_static/azure-log-streaming.png)

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="59802-481">Registro de seguimiento de Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="59802-481">Azure Application Insights trace logging</span></span>

<span data-ttu-id="59802-482">El SDK de Application Insights puede recopilar y notificar los registros que la infraestructura de registro de ASP.NET Core genera.</span><span class="sxs-lookup"><span data-stu-id="59802-482">The Application Insights SDK can collect and report logs generated by the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="59802-483">Para obtener más información, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="59802-483">For more information, see the following resources:</span></span>

* [<span data-ttu-id="59802-484">Información general de Application Insights</span><span class="sxs-lookup"><span data-stu-id="59802-484">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
* [<span data-ttu-id="59802-485">Application Insights para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="59802-485">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* <span data-ttu-id="59802-486">[Adaptadores de registro de Application Insights](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md)</span><span class="sxs-lookup"><span data-stu-id="59802-486">[Application Insights logging adapters](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).</span></span>
* [<span data-ttu-id="59802-487">Muestras de implementación de Application Insights ILogger</span><span class="sxs-lookup"><span data-stu-id="59802-487">Application Insights ILogger implementation samples</span></span>](/azure/azure-monitor/app/ilogger)

::: moniker-end

## <a name="third-party-logging-providers"></a><span data-ttu-id="59802-488">Proveedores de registro de terceros</span><span class="sxs-lookup"><span data-stu-id="59802-488">Third-party logging providers</span></span>

<span data-ttu-id="59802-489">Plataformas de registro de terceros que funcionan con ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="59802-489">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="59802-490">[elmah.io](https://elmah.io/) ([repositorio de GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="59802-490">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="59802-491">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repositorio de GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="59802-491">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="59802-492">[JSNLog](http://jsnlog.com/) ([repositorio de GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="59802-492">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="59802-493">[KissLog.net](https://kisslog.net/) ([repositorio de GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="59802-493">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="59802-494">[Loggr](http://loggr.net/) ([repositorio de GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="59802-494">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="59802-495">[NLog](http://nlog-project.org/) ([repositorio de GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="59802-495">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="59802-496">[Sentry](https://sentry.io/welcome/) ([repositorio de GitHub](https://github.com/getsentry/sentry-dotnet))</span><span class="sxs-lookup"><span data-stu-id="59802-496">[Sentry](https://sentry.io/welcome/) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
* <span data-ttu-id="59802-497">[Serilog](https://serilog.net/) ([repositorio de GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="59802-497">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>
* <span data-ttu-id="59802-498">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([repositorio de GitHub](https://github.com/googleapis/google-cloud-dotnet))</span><span class="sxs-lookup"><span data-stu-id="59802-498">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="59802-499">Algunas plataformas de terceros pueden realizar [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="59802-499">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="59802-500">El uso de una plataforma de terceros es similar al uso de uno de los proveedores integrados:</span><span class="sxs-lookup"><span data-stu-id="59802-500">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="59802-501">Agregue un paquete NuGet al proyecto.</span><span class="sxs-lookup"><span data-stu-id="59802-501">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="59802-502">Llame a `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="59802-502">Call an `ILoggerFactory`.</span></span>

<span data-ttu-id="59802-503">Para más información, vea la documentación de cada proveedor.</span><span class="sxs-lookup"><span data-stu-id="59802-503">For more information, see each provider's documentation.</span></span> <span data-ttu-id="59802-504">Microsoft no admite los proveedores de registro de terceros.</span><span class="sxs-lookup"><span data-stu-id="59802-504">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="59802-505">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="59802-505">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
