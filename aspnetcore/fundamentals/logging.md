---
title: "Inicio de sesión principal de ASP.NET"
author: ardalis
description: "Presenta el marco de trabajo de registro de ASP.NET Core. Incluye una sección para cada proveedor de registro integrados y vínculos a algunos proveedores de terceros populares."
keywords: "Establece el ámbito de ASP.NET Core, registro, proveedores de registro, Microsoft.Extensions.Logging, ILogger, ILoggerFactory, LogLevel, WithFilter, TraceSource, EventLog, EventSource,"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ac27ac68-d76a-4f8e-b8ab-ea045803e5f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30e00e2a442225bbe04be0d343f7048efe484477
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2017
---
# <a name="introduction-to-logging-in-aspnet-core"></a><span data-ttu-id="7b530-105">Introducción al inicio de sesión principal de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7b530-105">Introduction to Logging in ASP.NET Core</span></span>

<span data-ttu-id="7b530-106">Por [Steve Smith](http://ardalis.com) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7b530-106">By [Steve Smith](http://ardalis.com) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="7b530-107">ASP.NET Core es compatible con una API de registro que funciona con una variedad de proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="7b530-107">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="7b530-108">Proveedores integrados permiten enviar registros a uno o varios destinos, y puede conectar un marco de trabajo de registro de aplicaciones de terceros.</span><span class="sxs-lookup"><span data-stu-id="7b530-108">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="7b530-109">Este artículo muestra cómo usar las API de registro integrados y los proveedores en el código.</span><span class="sxs-lookup"><span data-stu-id="7b530-109">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7b530-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7b530-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[<span data-ttu-id="7b530-111">Ver o descargar el código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="7b530-111">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7b530-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7b530-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[<span data-ttu-id="7b530-113">Ver o descargar el código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="7b530-113">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample)

---

## <a name="how-to-create-logs"></a><span data-ttu-id="7b530-114">Cómo crear registros</span><span class="sxs-lookup"><span data-stu-id="7b530-114">How to create logs</span></span>

<span data-ttu-id="7b530-115">Para crear registros, obtener una `ILogger` objeto desde el [inyección de dependencia](dependency-injection.md) contenedor:</span><span class="sxs-lookup"><span data-stu-id="7b530-115">To create logs, get an `ILogger` object from the [dependency injection](dependency-injection.md) container:</span></span>

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="7b530-116">A continuación, llamar a métodos de registro de ese objeto de registrador:</span><span class="sxs-lookup"><span data-stu-id="7b530-116">Then call logging methods on that logger object:</span></span>

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="7b530-117">Este ejemplo crea registros con la `TodoController` de clase como el *categoría*.</span><span class="sxs-lookup"><span data-stu-id="7b530-117">This example creates logs with the `TodoController` class as the *category*.</span></span>  <span data-ttu-id="7b530-118">Categorías se explican [más adelante en este artículo](#log-category).</span><span class="sxs-lookup"><span data-stu-id="7b530-118">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="7b530-119">ASP.NET Core no proporciona async registrador métodos porque el registro debe ser tan rápido que no vale la pena el costo de usar async.</span><span class="sxs-lookup"><span data-stu-id="7b530-119">ASP.NET Core does not provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="7b530-120">Si se encuentra en una situación en los que no es así, considere la posibilidad de cambiar el modo de que iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="7b530-120">If you're in a situation where that's not true, consider changing the way you log.</span></span>  <span data-ttu-id="7b530-121">Si el almacén de datos es lento, escribir los mensajes de registro a un almacén rápido en primer lugar, a continuación, moverlos a un almacén de baja velocidad.</span><span class="sxs-lookup"><span data-stu-id="7b530-121">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="7b530-122">Por ejemplo, inicie sesión en una cola de mensajes que se lee y se conservan en almacenamiento lento por otro proceso.</span><span class="sxs-lookup"><span data-stu-id="7b530-122">For example, log to a message queue that is read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="7b530-123">Cómo agregar proveedores</span><span class="sxs-lookup"><span data-stu-id="7b530-123">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7b530-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7b530-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7b530-125">Un proveedor de registro toma los mensajes que se crean con un `ILogger` de objeto y muestra o almacenarlos.</span><span class="sxs-lookup"><span data-stu-id="7b530-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="7b530-126">Por ejemplo, el proveedor de la consola muestra mensajes en la consola y el proveedor de servicios de aplicación de Azure puede almacenar en el almacenamiento de blobs de Azure.</span><span class="sxs-lookup"><span data-stu-id="7b530-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="7b530-127">Para usar un proveedor, póngase en contacto el proveedor `Add<ProviderName>` método de extensión en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7b530-127">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="7b530-128">La plantilla de proyecto predeterminada establece la creación de registros de la manera en que puede ver en el código anterior, pero la `ConfigureLogging` llamada se realiza mediante el `CreateDefaultBuilder` método.</span><span class="sxs-lookup"><span data-stu-id="7b530-128">The default project template sets up logging the way you see it in the preceding code, but the `ConfigureLogging` call is done by the `CreateDefaultBuilder` method.</span></span> <span data-ttu-id="7b530-129">Este es el código *Program.cs* que se crean mediante plantillas de proyecto:</span><span class="sxs-lookup"><span data-stu-id="7b530-129">Here's the code in *Program.cs* that is created by project templates:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7b530-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7b530-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7b530-131">Un proveedor de registro toma los mensajes que se crean con un `ILogger` de objeto y muestra o almacenarlos.</span><span class="sxs-lookup"><span data-stu-id="7b530-131">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="7b530-132">Por ejemplo, el proveedor de la consola muestra mensajes en la consola y el proveedor de servicios de aplicación de Azure puede almacenar en el almacenamiento de blobs de Azure.</span><span class="sxs-lookup"><span data-stu-id="7b530-132">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="7b530-133">Para usar un proveedor, instale el paquete de NuGet y llame al método de extensión del proveedor en una instancia de `ILoggerFactory`, como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="7b530-133">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](logging/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="7b530-134">ASP.NET Core [inyección de dependencia](dependency-injection.md) (DI) proporciona el `ILoggerFactory` instancia.</span><span class="sxs-lookup"><span data-stu-id="7b530-134">ASP.NET Core [dependency injection](dependency-injection.md) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="7b530-135">El `AddConsole` y `AddDebug` métodos de extensión se definen en el [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) y [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) paquetes.</span><span class="sxs-lookup"><span data-stu-id="7b530-135">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="7b530-136">Llama a cada método de extensión el `ILoggerFactory.AddProvider` método, pasando una instancia del proveedor.</span><span class="sxs-lookup"><span data-stu-id="7b530-136">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="7b530-137">La aplicación de ejemplo de este artículo agrega proveedores de registro en el `Configure` método de la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="7b530-137">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="7b530-138">Si desea obtener la salida de registro del código que se ejecuta en versiones anteriores, agregar proveedores de registro en el `Startup` constructor de la clase en su lugar.</span><span class="sxs-lookup"><span data-stu-id="7b530-138">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="7b530-139">Puede encontrar información sobre cada [proveedor de registro integrados](#built-in-logging-providers) y vínculos a [proveedores de registro de aplicaciones de terceros](#third-party-logging-providers) más adelante en el artículo.</span><span class="sxs-lookup"><span data-stu-id="7b530-139">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="7b530-140">Resultados del registro de ejemplo</span><span class="sxs-lookup"><span data-stu-id="7b530-140">Sample logging output</span></span>

<span data-ttu-id="7b530-141">Con el código de ejemplo que se muestra en la sección anterior, verá los registros en la consola cuando se ejecuta desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="7b530-141">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="7b530-142">Este es un ejemplo de salida de la consola:</span><span class="sxs-lookup"><span data-stu-id="7b530-142">Here's an example of console output:</span></span>

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
 
<span data-ttu-id="7b530-143">Estos registros se crearon yendo a `http://localhost:5000/api/todo/0`, lo que desencadena la ejecución de ambos `ILogger` llamadas se muestra en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="7b530-143">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="7b530-144">Este es un ejemplo de los mismos registros que aparecen en la ventana de depuración cuando se ejecute la aplicación de ejemplo en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="7b530-144">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="7b530-145">Los registros creados por el `ILogger` llamadas se muestra en la sección anterior comienzan por "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="7b530-145">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="7b530-146">Los registros que comienzan por categorías de "Microsoft" son de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7b530-146">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="7b530-147">ASP.NET Core propio y el código de aplicación están utilizando las mismas API de registro y los mismos proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="7b530-147">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="7b530-148">El resto de este artículo explica algunos detalles y las opciones para el registro.</span><span class="sxs-lookup"><span data-stu-id="7b530-148">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="7b530-149">Paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="7b530-149">NuGet packages</span></span>

<span data-ttu-id="7b530-150">El `ILogger` y `ILoggerFactory` interfaces se encuentran en [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), y las implementaciones predeterminadas para ellos en [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span><span class="sxs-lookup"><span data-stu-id="7b530-150">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="7b530-151">Categoría de registro</span><span class="sxs-lookup"><span data-stu-id="7b530-151">Log category</span></span>

<span data-ttu-id="7b530-152">A *categoría* se incluye con cada registro creados por usted.</span><span class="sxs-lookup"><span data-stu-id="7b530-152">A *category* is included with each log that you create.</span></span>  <span data-ttu-id="7b530-153">Especifica la categoría cuando se crea un `ILogger` objeto.</span><span class="sxs-lookup"><span data-stu-id="7b530-153">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="7b530-154">La categoría puede ser cualquier cadena, pero una convención consiste en utilizar el nombre completo de la clase desde el que se escriben los registros.</span><span class="sxs-lookup"><span data-stu-id="7b530-154">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span>  <span data-ttu-id="7b530-155">Por ejemplo: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="7b530-155">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="7b530-156">Puede especificar la categoría como una cadena o utilice un método de extensión que se deriva la categoría del tipo.</span><span class="sxs-lookup"><span data-stu-id="7b530-156">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="7b530-157">Para especificar la categoría como una cadena, llame a `CreateLogger` en un `ILoggerFactory` de instancia, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="7b530-157">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="7b530-158">La mayoría de los casos, le resultará más fácil usar `ILogger<T>`, como en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="7b530-158">Most of the time, it will be easier to use  `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="7b530-159">Esto equivale a llamar a `CreateLogger` con el nombre de tipo completo de `T`.</span><span class="sxs-lookup"><span data-stu-id="7b530-159">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="7b530-160">Nivel de registro</span><span class="sxs-lookup"><span data-stu-id="7b530-160">Log level</span></span>

<span data-ttu-id="7b530-161">Cada vez que se escribe un registro, especifique su [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="7b530-161">Each time you write a log, you specify its [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="7b530-162">El nivel de registro indica el grado de gravedad o importancia.</span><span class="sxs-lookup"><span data-stu-id="7b530-162">The log level indicates the degree of severity or importance.</span></span>  <span data-ttu-id="7b530-163">Por ejemplo, puede escribir una `Information` iniciar sesión cuando un método finaliza normalmente, un `Warning` cuando un método devuelve un código de retorno de un error 404 y un registro `Error` registrar cuando se detecta una excepción inesperada.</span><span class="sxs-lookup"><span data-stu-id="7b530-163">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="7b530-164">En el ejemplo de código siguiente, los nombres de los métodos (por ejemplo, `LogWarning`) especificar el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="7b530-164">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span>  <span data-ttu-id="7b530-165">El primer parámetro es el [Id. de evento de registro](#log-event-id) (se explica más adelante en este artículo).</span><span class="sxs-lookup"><span data-stu-id="7b530-165">The first parameter is the [Log event ID](#log-event-id) (explained later in this article).</span></span>  <span data-ttu-id="7b530-166">Los parámetros restantes construcción una cadena de mensaje de registro.</span><span class="sxs-lookup"><span data-stu-id="7b530-166">The remaining parameters construct a log message string.</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="7b530-167">Los métodos de registro que incluyen el nivel en el nombre del método están [métodos de extensión para ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="7b530-167">Log methods that include the level in the method name are [extension methods for ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="7b530-168">En segundo plano, llame estos métodos una `Log` método que toma un `LogLevel` parámetro.</span><span class="sxs-lookup"><span data-stu-id="7b530-168">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="7b530-169">Puede llamar a la `Log` (método) directamente en lugar de uno de estos métodos de extensión, pero la sintaxis es relativamente complicado.</span><span class="sxs-lookup"><span data-stu-id="7b530-169">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="7b530-170">Para obtener más información, consulte el [interfaz ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) y [código fuente de las extensiones de registrador](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="7b530-170">For more information, see the [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="7b530-171">ASP.NET Core define las siguientes [niveles de registro](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), aquí ordenados de menor a mayor grado de gravedad.</span><span class="sxs-lookup"><span data-stu-id="7b530-171">ASP.NET Core defines the following [log levels](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="7b530-172">Seguimiento = 0</span><span class="sxs-lookup"><span data-stu-id="7b530-172">Trace = 0</span></span>

  <span data-ttu-id="7b530-173">Para obtener información que es útil sólo para un desarrollador depurar un problema.</span><span class="sxs-lookup"><span data-stu-id="7b530-173">For information that is valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="7b530-174">Estos mensajes pueden contener datos confidenciales de la aplicación y por lo que no deben habilitarse en un entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="7b530-174">These messages may contain sensitive application data and so should not be enabled in a production environment.</span></span> <span data-ttu-id="7b530-175">*Deshabilitada de forma predeterminada.*</span><span class="sxs-lookup"><span data-stu-id="7b530-175">*Disabled by default.*</span></span> <span data-ttu-id="7b530-176">Ejemplo: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="7b530-176">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="7b530-177">Depurar = 1</span><span class="sxs-lookup"><span data-stu-id="7b530-177">Debug = 1</span></span>

  <span data-ttu-id="7b530-178">Para obtener información que tiene la utilidad a corto plazo durante el desarrollo y depuración.</span><span class="sxs-lookup"><span data-stu-id="7b530-178">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="7b530-179">Ejemplo: `Entering method Configure with flag set to true.` normalmente no se debería habilitar `Debug` nivel se registra en producción, a menos que esté solucionando, debido al elevado volumen de registros.</span><span class="sxs-lookup"><span data-stu-id="7b530-179">Example: `Entering method Configure with flag set to true.` You typically would not enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="7b530-180">Información = 2</span><span class="sxs-lookup"><span data-stu-id="7b530-180">Information = 2</span></span>

  <span data-ttu-id="7b530-181">Para supervisar el flujo general de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7b530-181">For tracking the general flow of the application.</span></span> <span data-ttu-id="7b530-182">Estos registros suelen tengan algún valor a largo plazo.</span><span class="sxs-lookup"><span data-stu-id="7b530-182">These logs typically have some long-term value.</span></span> <span data-ttu-id="7b530-183">Ejemplo: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="7b530-183">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="7b530-184">Advertencia = 3</span><span class="sxs-lookup"><span data-stu-id="7b530-184">Warning = 3</span></span>

  <span data-ttu-id="7b530-185">Para los eventos anómalos o inesperados en el flujo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7b530-185">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="7b530-186">Estos pueden incluir errores u otras condiciones que hacen que la aplicación detener, pero puede que deba investigar.</span><span class="sxs-lookup"><span data-stu-id="7b530-186">These may include errors or other conditions that do not cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="7b530-187">Las excepciones administradas son un lugar común para utilizar el `Warning` nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="7b530-187">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="7b530-188">Ejemplo: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="7b530-188">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="7b530-189">Error = 4</span><span class="sxs-lookup"><span data-stu-id="7b530-189">Error = 4</span></span>

  <span data-ttu-id="7b530-190">Para los errores y excepciones que no se puede controlar.</span><span class="sxs-lookup"><span data-stu-id="7b530-190">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="7b530-191">Estos mensajes indican un error en la actividad actual o la operación (por ejemplo, la solicitud HTTP actual), no un error de toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7b530-191">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="7b530-192">Mensaje de registro de ejemplo:`Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="7b530-192">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="7b530-193">Crítico = 5</span><span class="sxs-lookup"><span data-stu-id="7b530-193">Critical = 5</span></span>

  <span data-ttu-id="7b530-194">Para los errores que requieren atención inmediata.</span><span class="sxs-lookup"><span data-stu-id="7b530-194">For failures that require immediate attention.</span></span> <span data-ttu-id="7b530-195">Ejemplos: escenarios de pérdida de datos de espacio en disco insuficiente.</span><span class="sxs-lookup"><span data-stu-id="7b530-195">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="7b530-196">Puede utilizar el nivel de registro para controlar cuántos resultados del registro se escriben en un medio de almacenamiento determinado o mostrar la ventana.</span><span class="sxs-lookup"><span data-stu-id="7b530-196">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="7b530-197">Por ejemplo, en producción podría interesar todos los registros de `Information` nivel y reducir para ir a un almacén de datos de volumen y todos los registros de `Warning` nivel y versiones posteriores para ir a un almacén de datos de valor.</span><span class="sxs-lookup"><span data-stu-id="7b530-197">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="7b530-198">Durante el desarrollo, normalmente puede enviar registros de `Warning` o en la consola de mayor gravedad.</span><span class="sxs-lookup"><span data-stu-id="7b530-198">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="7b530-199">A continuación si necesita solucionar problemas, puede agregar `Debug` nivel.</span><span class="sxs-lookup"><span data-stu-id="7b530-199">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="7b530-200">El [filtrado del registro](#log-filtering) sección más adelante en este artículo explica cómo controlar qué niveles de registro que se trata de un proveedor.</span><span class="sxs-lookup"><span data-stu-id="7b530-200">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="7b530-201">Escribe el marco de trabajo de ASP.NET Core `Debug` nivel registros de eventos de framework.</span><span class="sxs-lookup"><span data-stu-id="7b530-201">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="7b530-202">Los ejemplos de registro anteriormente en este artículo excluyen registros siguiente `Information` nivel, por lo que no `Debug` se mostraron registros de nivel.</span><span class="sxs-lookup"><span data-stu-id="7b530-202">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="7b530-203">Este es un ejemplo de registros de la consola si ejecuta la aplicación de ejemplo configurada para mostrar `Debug` y registros superior para el proveedor de la consola.</span><span class="sxs-lookup"><span data-stu-id="7b530-203">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="7b530-204">Id. de evento de registro</span><span class="sxs-lookup"><span data-stu-id="7b530-204">Log event ID</span></span>

<span data-ttu-id="7b530-205">Cada vez que se escribe un registro, puede especificar un *Id. de evento*.</span><span class="sxs-lookup"><span data-stu-id="7b530-205">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="7b530-206">La aplicación de ejemplo lo hace al utilizar un definido localmente `LoggingEvents` clase:</span><span class="sxs-lookup"><span data-stu-id="7b530-206">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](logging/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="7b530-207">Un identificador de evento es un valor entero que puede utilizar para asociar un conjunto de eventos registrados entre sí.</span><span class="sxs-lookup"><span data-stu-id="7b530-207">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="7b530-208">Por ejemplo, un registro para agregar un elemento a un carro de la compra podría ser el identificador de evento 1000 y un registro para completar una compra podría ser 1001 de Id. de evento.</span><span class="sxs-lookup"><span data-stu-id="7b530-208">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="7b530-209">En los resultados del registro, el identificador de evento podría estar almacenado en un campo o incluido en el mensaje de texto, en función del proveedor.</span><span class="sxs-lookup"><span data-stu-id="7b530-209">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span>  <span data-ttu-id="7b530-210">El proveedor de depuración no muestra el Id. de evento, pero el proveedor de la consola muestra entre paréntesis después de la categoría:</span><span class="sxs-lookup"><span data-stu-id="7b530-210">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-format-string"></a><span data-ttu-id="7b530-211">Cadena de formato de mensaje de registro</span><span class="sxs-lookup"><span data-stu-id="7b530-211">Log message format string</span></span>

<span data-ttu-id="7b530-212">Cada vez que escriba un registro, proporcionar un mensaje de texto.</span><span class="sxs-lookup"><span data-stu-id="7b530-212">Each time you write a log, you provide a text message.</span></span> <span data-ttu-id="7b530-213">La cadena de mensaje puede contener marcadores de posición con nombre en el argumento que se colocan valores, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7b530-213">The message string can contain named placeholders into which argument values are placed, as in the following example:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="7b530-214">El orden de los marcadores de posición, no por sus nombres, determina qué parámetros se utilizan para ellos.</span><span class="sxs-lookup"><span data-stu-id="7b530-214">The order of placeholders, not their names, determines which parameters are used for them.</span></span> <span data-ttu-id="7b530-215">Por ejemplo, si tiene el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="7b530-215">For example, if you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="7b530-216">El mensaje del registro resultante sería similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="7b530-216">The resulting log message would look like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="7b530-217">El marco de trabajo de registro de mensajes de formato de esta manera para que sea posible para los proveedores de registro implementar [registro semántica, también conocido como registro estructurado](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="7b530-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="7b530-218">Dado que los argumentos se pasan al sistema de registro, no solo la cadena de mensaje con formato, los proveedores de registro pueden almacenar los valores de parámetro como campos además de la cadena de mensaje.</span><span class="sxs-lookup"><span data-stu-id="7b530-218">Because the arguments themselves are passed to the logging system, not just the formatted message string, logging providers can store the parameter values as fields in addition to the message string.</span></span> <span data-ttu-id="7b530-219">Por ejemplo, si dirigen el registro de salida en el almacenamiento de tabla de Azure y la llamada al método de registrador tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="7b530-219">For example, if you are directing your log output to Azure Table Storage, and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="7b530-220">Cada entidad de tabla de Azure podría tener `ID` y `RequestTime` propiedades, que se simplifican las consultas en los datos de registro.</span><span class="sxs-lookup"><span data-stu-id="7b530-220">Each Azure Table entity could have `ID` and `RequestTime` properties, which would simplify queries on log data.</span></span> <span data-ttu-id="7b530-221">Puede buscar todos los registros dentro de un determinado `RequestTime` intervalo, sin tener que analizar el tiempo de espera del mensaje de texto.</span><span class="sxs-lookup"><span data-stu-id="7b530-221">You could find all logs within a particular `RequestTime` range, without having to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="7b530-222">Registro de excepciones</span><span class="sxs-lookup"><span data-stu-id="7b530-222">Logging exceptions</span></span>

<span data-ttu-id="7b530-223">Los métodos de registrador tienen sobrecargas que le permiten pasar una excepción, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7b530-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="7b530-224">Diferentes proveedores de controlan la información de excepción de maneras diferentes.</span><span class="sxs-lookup"><span data-stu-id="7b530-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="7b530-225">Este es un ejemplo de salida de proveedor de depuración desde el código mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="7b530-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="7b530-226">Filtrado del registro</span><span class="sxs-lookup"><span data-stu-id="7b530-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7b530-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7b530-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7b530-228">Puede especificar un nivel de registro mínimo de un proveedor específico y una categoría o para todos los proveedores o todas las categorías.</span><span class="sxs-lookup"><span data-stu-id="7b530-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span>  <span data-ttu-id="7b530-229">Los registros por debajo del nivel mínimo no se pasan a ese proveedor, de modo que no obtener mostrados o almacenados.</span><span class="sxs-lookup"><span data-stu-id="7b530-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="7b530-230">Si desea suprimir todos los registros, puede especificar `LogLevel.None` como el nivel de registro mínimo.</span><span class="sxs-lookup"><span data-stu-id="7b530-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="7b530-231">El valor del entero de `LogLevel.None` es 6, que son superiores a `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="7b530-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="7b530-232">**Crear reglas de filtro en la configuración**</span><span class="sxs-lookup"><span data-stu-id="7b530-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="7b530-233">Las plantillas de proyecto crean código que llama a `CreateDefaultBuilder` para configurar el registro para los proveedores de consola y de depuración.</span><span class="sxs-lookup"><span data-stu-id="7b530-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="7b530-234">El `CreateDefaultBuilder` método también establece el registro para buscar la configuración en un `Logging` sección, mediante código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="7b530-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="7b530-235">Los datos de configuración especifican niveles de registro mínimo al proveedor y categoría, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7b530-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](logging/sample2/appsettings.json)]

<span data-ttu-id="7b530-236">Este archivo JSON crea seis reglas de filtro, uno para el proveedor de depuración, 4 para el proveedor de la consola y que se aplica a todos los proveedores.</span><span class="sxs-lookup"><span data-stu-id="7b530-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="7b530-237">Podrá ver más adelante cómo solamente una de estas reglas se elige para cada proveedor cuando un `ILogger` se crea el objeto.</span><span class="sxs-lookup"><span data-stu-id="7b530-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="7b530-238">**Reglas de filtro en el código**</span><span class="sxs-lookup"><span data-stu-id="7b530-238">**Filter rules in code**</span></span>

<span data-ttu-id="7b530-239">Puede registrar las reglas de filtro en el código, tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7b530-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="7b530-240">El segundo `AddFilter` especifica el proveedor de depuración mediante su nombre de tipo.</span><span class="sxs-lookup"><span data-stu-id="7b530-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="7b530-241">La primera `AddFilter` se aplica a todos los proveedores, dado que no especifica un tipo de proveedor.</span><span class="sxs-lookup"><span data-stu-id="7b530-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="7b530-242">**Cómo filtrar las reglas se aplican**</span><span class="sxs-lookup"><span data-stu-id="7b530-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="7b530-243">Los datos de configuración y la `AddFilter` código que se muestra en los ejemplos anteriores crear las reglas que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="7b530-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="7b530-244">Los seis primeros proceden del ejemplo de configuración y las dos últimas proceden del ejemplo de código.</span><span class="sxs-lookup"><span data-stu-id="7b530-244">The first six come from the configuration example and the last two come from the code example.</span></span>

<span data-ttu-id="7b530-245">Número</span><span class="sxs-lookup"><span data-stu-id="7b530-245">Number</span></span>|<span data-ttu-id="7b530-246">Proveedor</span><span class="sxs-lookup"><span data-stu-id="7b530-246">Provider</span></span>|<span data-ttu-id="7b530-247">Categorías que comienzan por</span><span class="sxs-lookup"><span data-stu-id="7b530-247">Categories that begin with</span></span>|<span data-ttu-id="7b530-248">Nivel de registro mínimo</span><span class="sxs-lookup"><span data-stu-id="7b530-248">Minimum log level</span></span>|
------|--------|--------------------------|-----------------|
<span data-ttu-id="7b530-249">1</span><span class="sxs-lookup"><span data-stu-id="7b530-249">1</span></span>|<span data-ttu-id="7b530-250">Depuración</span><span class="sxs-lookup"><span data-stu-id="7b530-250">Debug</span></span>|<span data-ttu-id="7b530-251">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="7b530-251">All categories</span></span>|<span data-ttu-id="7b530-252">Información</span><span class="sxs-lookup"><span data-stu-id="7b530-252">Information</span></span>|
<span data-ttu-id="7b530-253">2</span><span class="sxs-lookup"><span data-stu-id="7b530-253">2</span></span>|<span data-ttu-id="7b530-254">Consola</span><span class="sxs-lookup"><span data-stu-id="7b530-254">Console</span></span>|<span data-ttu-id="7b530-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="7b530-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span>|<span data-ttu-id="7b530-256">Advertencia</span><span class="sxs-lookup"><span data-stu-id="7b530-256">Warning</span></span>|
<span data-ttu-id="7b530-257">3</span><span class="sxs-lookup"><span data-stu-id="7b530-257">3</span></span>|<span data-ttu-id="7b530-258">Consola</span><span class="sxs-lookup"><span data-stu-id="7b530-258">Console</span></span>|<span data-ttu-id="7b530-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="7b530-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>|<span data-ttu-id="7b530-260">Depuración</span><span class="sxs-lookup"><span data-stu-id="7b530-260">Debug</span></span>|
<span data-ttu-id="7b530-261">4</span><span class="sxs-lookup"><span data-stu-id="7b530-261">4</span></span>|<span data-ttu-id="7b530-262">Consola</span><span class="sxs-lookup"><span data-stu-id="7b530-262">Console</span></span>|<span data-ttu-id="7b530-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="7b530-263">Microsoft.AspNetCore.Mvc.Razor</span></span>|<span data-ttu-id="7b530-264">Error</span><span class="sxs-lookup"><span data-stu-id="7b530-264">Error</span></span>|
<span data-ttu-id="7b530-265">5</span><span class="sxs-lookup"><span data-stu-id="7b530-265">5</span></span>|<span data-ttu-id="7b530-266">Consola</span><span class="sxs-lookup"><span data-stu-id="7b530-266">Console</span></span>|<span data-ttu-id="7b530-267">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="7b530-267">All categories</span></span>|<span data-ttu-id="7b530-268">Información</span><span class="sxs-lookup"><span data-stu-id="7b530-268">Information</span></span>|
<span data-ttu-id="7b530-269">6</span><span class="sxs-lookup"><span data-stu-id="7b530-269">6</span></span>|<span data-ttu-id="7b530-270">Todos los proveedores</span><span class="sxs-lookup"><span data-stu-id="7b530-270">All providers</span></span>|<span data-ttu-id="7b530-271">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="7b530-271">All categories</span></span>|<span data-ttu-id="7b530-272">Advertencia</span><span class="sxs-lookup"><span data-stu-id="7b530-272">Warning</span></span>
<span data-ttu-id="7b530-273">7</span><span class="sxs-lookup"><span data-stu-id="7b530-273">7</span></span>|<span data-ttu-id="7b530-274">Todos los proveedores</span><span class="sxs-lookup"><span data-stu-id="7b530-274">All providers</span></span>|<span data-ttu-id="7b530-275">Sistema</span><span class="sxs-lookup"><span data-stu-id="7b530-275">System</span></span>|<span data-ttu-id="7b530-276">Depuración</span><span class="sxs-lookup"><span data-stu-id="7b530-276">Debug</span></span>
<span data-ttu-id="7b530-277">8</span><span class="sxs-lookup"><span data-stu-id="7b530-277">8</span></span>|<span data-ttu-id="7b530-278">Depuración</span><span class="sxs-lookup"><span data-stu-id="7b530-278">Debug</span></span>|<span data-ttu-id="7b530-279">Microsoft</span><span class="sxs-lookup"><span data-stu-id="7b530-279">Microsoft</span></span>|<span data-ttu-id="7b530-280">Seguimiento</span><span class="sxs-lookup"><span data-stu-id="7b530-280">Trace</span></span>

<span data-ttu-id="7b530-281">Cuando se crea un `ILogger` objeto para escribir los registros, la `ILoggerFactory` objeto selecciona una sola regla por proveedor para aplicar a dicho registrador.</span><span class="sxs-lookup"><span data-stu-id="7b530-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="7b530-282">Todos los mensajes escritos por esa `ILogger` objeto se filtra según las reglas seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="7b530-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="7b530-283">Se selecciona la regla más específica posible para cada par de categoría y el proveedor de las reglas disponibles.</span><span class="sxs-lookup"><span data-stu-id="7b530-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="7b530-284">Se utiliza el algoritmo siguiente para cada proveedor cuando un `ILogger` se crea para una categoría determinada:</span><span class="sxs-lookup"><span data-stu-id="7b530-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="7b530-285">Seleccionar todas las reglas que coinciden con el proveedor o su alias.</span><span class="sxs-lookup"><span data-stu-id="7b530-285">Select all rules that match the provider or its alias.</span></span>  <span data-ttu-id="7b530-286">Si no se encuentra ninguno, seleccione todas las reglas con un proveedor vacío.</span><span class="sxs-lookup"><span data-stu-id="7b530-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="7b530-287">Desde el resultado del paso anterior, seleccione las reglas con mayor tiempo coincidencia de prefijo de categoría.</span><span class="sxs-lookup"><span data-stu-id="7b530-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="7b530-288">Si no se encuentra ninguno, seleccione todas las reglas que no se especifican una categoría.</span><span class="sxs-lookup"><span data-stu-id="7b530-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="7b530-289">Si se seleccionan varias reglas de tomar la **última** uno.</span><span class="sxs-lookup"><span data-stu-id="7b530-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="7b530-290">Si no se selecciona ninguna regla, use `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="7b530-290">If no rules are selected, use `MinimumLevel`.</span></span>
 
<span data-ttu-id="7b530-291">Por ejemplo, supongamos que tiene la lista de reglas anterior y se crea un `ILogger` objeto de categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="7b530-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="7b530-292">Para el proveedor de depuración, se aplican las reglas 1, 6 y 8.</span><span class="sxs-lookup"><span data-stu-id="7b530-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="7b530-293">Regla 8 es más específico, por lo que está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="7b530-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="7b530-294">Para el proveedor de la consola, se aplican las reglas de 3, 4, 5 y 6.</span><span class="sxs-lookup"><span data-stu-id="7b530-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="7b530-295">La regla 3 es más específica.</span><span class="sxs-lookup"><span data-stu-id="7b530-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="7b530-296">Cuando crea registros con un `ILogger` para la categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", los registros de `Trace` nivel y versiones posteriores será dirigido a los proveedores de depuración y registros de `Debug` nivel y versiones posteriores pasa al proveedor de consola.</span><span class="sxs-lookup"><span data-stu-id="7b530-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="7b530-297">**Alias de proveedor**</span><span class="sxs-lookup"><span data-stu-id="7b530-297">**Provider aliases**</span></span>

<span data-ttu-id="7b530-298">Puede usar el nombre de tipo para especificar un proveedor de configuración, pero cada proveedor define una menor *alias* que es más fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="7b530-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that is easier to use.</span></span> <span data-ttu-id="7b530-299">Para los proveedores integrados, use los siguientes alias:</span><span class="sxs-lookup"><span data-stu-id="7b530-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="7b530-300">Consola</span><span class="sxs-lookup"><span data-stu-id="7b530-300">Console</span></span>
- <span data-ttu-id="7b530-301">Depuración</span><span class="sxs-lookup"><span data-stu-id="7b530-301">Debug</span></span>
- <span data-ttu-id="7b530-302">Registro de eventos</span><span class="sxs-lookup"><span data-stu-id="7b530-302">EventLog</span></span>
- <span data-ttu-id="7b530-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="7b530-303">AzureAppServices</span></span>
- <span data-ttu-id="7b530-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="7b530-304">TraceSource</span></span>
- <span data-ttu-id="7b530-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="7b530-305">EventSource</span></span>

<span data-ttu-id="7b530-306">**Nivel mínimo de manera predeterminada**</span><span class="sxs-lookup"><span data-stu-id="7b530-306">**Default minimum level**</span></span>

<span data-ttu-id="7b530-307">No hay una configuración de nivel mínima que solo tiene efecto si se aplica ninguna regla de configuración o código de un proveedor determinado y una categoría.</span><span class="sxs-lookup"><span data-stu-id="7b530-307">There is a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="7b530-308">En el ejemplo siguiente se muestra cómo establecer el nivel mínimo:</span><span class="sxs-lookup"><span data-stu-id="7b530-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="7b530-309">Si no establece explícitamente el nivel mínimo, el valor predeterminado es `Information`, lo que significa que `Trace` y `Debug` se omiten los registros.</span><span class="sxs-lookup"><span data-stu-id="7b530-309">IF you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="7b530-310">**Funciones de filtro**</span><span class="sxs-lookup"><span data-stu-id="7b530-310">**Filter functions**</span></span>

<span data-ttu-id="7b530-311">Puede escribir código en una función de filtro para aplicar las reglas de filtrado.</span><span class="sxs-lookup"><span data-stu-id="7b530-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="7b530-312">Se invoca una función de filtro para todos los proveedores y las categorías que no tiene reglas asignadas mediante configuración o código.</span><span class="sxs-lookup"><span data-stu-id="7b530-312">A filter function is invoked for all providers and categories that do not have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="7b530-313">Código de la función tiene acceso al tipo de proveedor, categoría y nivel de registro para decidir si se debe registrar un mensaje.</span><span class="sxs-lookup"><span data-stu-id="7b530-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="7b530-314">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7b530-314">For example:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7b530-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7b530-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7b530-316">Algunos proveedores de registro permiten especificar cuando los registros se deben escritos en un medio de almacenamiento o pasa por alto en función de categoría y nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="7b530-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="7b530-317">El `AddConsole` y `AddDebug` métodos de extensión proporcionan sobrecargas que le permiten pasar en criterios de filtrado.</span><span class="sxs-lookup"><span data-stu-id="7b530-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="7b530-318">El código de ejemplo siguiente hace que el proveedor de la consola pasar por alto los registros siguientes `Warning` nivel, mientras que el proveedor de depuración omite registros creados por el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="7b530-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="7b530-319">El `AddEventLog` método tiene una sobrecarga que toma un `EventLogSettings` instancia, lo que puede contener una función de filtrado en su `Filter` propiedad.</span><span class="sxs-lookup"><span data-stu-id="7b530-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="7b530-320">El proveedor de TraceSource no proporciona ninguna de dichas sobrecargas, puesto que su nivel de registro y otros parámetros se basan en el `SourceSwitch` y `TraceListener` utiliza.</span><span class="sxs-lookup"><span data-stu-id="7b530-320">The TraceSource provider does not provide any of those overloads, since its logging level and other parameters are based on the  `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="7b530-321">Puede establecer reglas de filtrado para todos los proveedores que están registrados con un `ILoggerFactory` instancia mediante el uso de la `WithFilter` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="7b530-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="7b530-322">En el ejemplo siguiente limita los registros de framework (categoría comienza con "Microsoft" o "System") para las advertencias al permitir que el registro de aplicación en el nivel de depuración.</span><span class="sxs-lookup"><span data-stu-id="7b530-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="7b530-323">Si desea usar el filtrado para impedir que todos los registros que se escriben para una categoría concreta, puede especificar `LogLevel.None` como el nivel de registro mínimo de esa categoría.</span><span class="sxs-lookup"><span data-stu-id="7b530-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="7b530-324">El valor del entero de `LogLevel.None` es 6, que son superiores a `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="7b530-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="7b530-325">El `WithFilter` se proporciona el método de extensión mediante la [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="7b530-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="7b530-326">El método devuelve un nuevo `ILoggerFactory` instancia que se filtrará los mensajes de registro pasados a todos los proveedores de registrador registrados con él.</span><span class="sxs-lookup"><span data-stu-id="7b530-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="7b530-327">No no afecta a ningún otro `ILoggerFactory` instancias, incluido el original `ILoggerFactory` instancia.</span><span class="sxs-lookup"><span data-stu-id="7b530-327">It does not affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="7b530-328">Ámbitos de registro</span><span class="sxs-lookup"><span data-stu-id="7b530-328">Log scopes</span></span>

<span data-ttu-id="7b530-329">Puede agrupar un conjunto de operaciones lógicas dentro de un *ámbito* para adjuntar los mismos datos para cada registro que se crea como parte de dicho conjunto.</span><span class="sxs-lookup"><span data-stu-id="7b530-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that is created as part of that set.</span></span>  <span data-ttu-id="7b530-330">Por ejemplo, podría desea que cada registro creado como parte del procesamiento de transacciones para incluir el identificador de transacción.</span><span class="sxs-lookup"><span data-stu-id="7b530-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="7b530-331">Un ámbito es un `IDisposable` tipo devuelto por la `ILogger.BeginScope<TState>` método y permanecen activas hasta que se elimina.</span><span class="sxs-lookup"><span data-stu-id="7b530-331">A scope is an `IDisposable` type that is returned by the `ILogger.BeginScope<TState>` method and lasts until it is disposed.</span></span> <span data-ttu-id="7b530-332">Para utilizar un ámbito, ajuste su registrador llama en un `using` bloquear, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="7b530-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="7b530-333">El código siguiente permite ámbitos para el proveedor de la consola:</span><span class="sxs-lookup"><span data-stu-id="7b530-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7b530-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7b530-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7b530-335">En *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7b530-335">In *Program.cs*:</span></span>

[!code-csharp[](logging/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7b530-336">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7b530-336">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7b530-337">En *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7b530-337">In *Startup.cs*:</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="7b530-338">Cada mensaje del registro incluye la información de ámbito:</span><span class="sxs-lookup"><span data-stu-id="7b530-338">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="7b530-339">Proveedores de registro integrados</span><span class="sxs-lookup"><span data-stu-id="7b530-339">Built-in logging providers</span></span>

<span data-ttu-id="7b530-340">ASP.NET Core incluye los proveedores siguientes:</span><span class="sxs-lookup"><span data-stu-id="7b530-340">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="7b530-341">Consola</span><span class="sxs-lookup"><span data-stu-id="7b530-341">Console</span></span>](#console)
* [<span data-ttu-id="7b530-342">Depurar</span><span class="sxs-lookup"><span data-stu-id="7b530-342">Debug</span></span>](#debug)
* [<span data-ttu-id="7b530-343">EventSource</span><span class="sxs-lookup"><span data-stu-id="7b530-343">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="7b530-344">EventLog</span><span class="sxs-lookup"><span data-stu-id="7b530-344">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="7b530-345">TraceSource</span><span class="sxs-lookup"><span data-stu-id="7b530-345">TraceSource</span></span>](#tracesource)
* [<span data-ttu-id="7b530-346">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="7b530-346">Azure App Service</span></span>](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="7b530-347">El proveedor de la consola</span><span class="sxs-lookup"><span data-stu-id="7b530-347">The console provider</span></span>

<span data-ttu-id="7b530-348">El [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) el paquete de proveedor envía los resultados de registro en la consola.</span><span class="sxs-lookup"><span data-stu-id="7b530-348">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7b530-349">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7b530-349">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7b530-350">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7b530-350">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="7b530-351">[Las sobrecargas de AddConsole](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) permiten pasar en un un nivel de registro mínimo, una función de filtro y un valor booleano que indica si se admiten los ámbitos.</span><span class="sxs-lookup"><span data-stu-id="7b530-351">[AddConsole overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span>  <span data-ttu-id="7b530-352">Otra opción consiste en pasar un `IConfiguration` objeto, que puede especificar niveles de registro y de soporte técnico de ámbitos.</span><span class="sxs-lookup"><span data-stu-id="7b530-352">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="7b530-353">Si está pensando en la consola de proveedor para su uso en producción, tenga en cuenta que tiene un impacto significativo en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="7b530-353">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="7b530-354">Cuando crea un nuevo proyecto en Visual Studio, el `AddConsole` método tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="7b530-354">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="7b530-355">Este código hace referencia a la `Logging` sección de la *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="7b530-355">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](logging/sample//appsettings.json)]

<span data-ttu-id="7b530-356">La configuración que muestra registros de framework de límite para las advertencias mientras que permite a la aplicación para iniciar sesión en el nivel de depuración, como se explica en la [filtrado del registro](#log-filtering) sección.</span><span class="sxs-lookup"><span data-stu-id="7b530-356">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="7b530-357">Para obtener más información, consulte [configuración](configuration.md).</span><span class="sxs-lookup"><span data-stu-id="7b530-357">For more information, see [Configuration](configuration.md).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="7b530-358">El proveedor de depuración</span><span class="sxs-lookup"><span data-stu-id="7b530-358">The Debug provider</span></span>

<span data-ttu-id="7b530-359">El [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) el paquete de proveedor escribe la salida del registro mediante el [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) clase (`Debug.WriteLine` llamadas a métodos).</span><span class="sxs-lookup"><span data-stu-id="7b530-359">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="7b530-360">En Linux, este proveedor escribe registros en */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="7b530-360">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7b530-361">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7b530-361">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7b530-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7b530-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="7b530-363">[Las sobrecargas de AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) permiten pasar en un nivel mínimo de registro o una función de filtro.</span><span class="sxs-lookup"><span data-stu-id="7b530-363">[AddDebug overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="7b530-364">El proveedor EventSource</span><span class="sxs-lookup"><span data-stu-id="7b530-364">The EventSource provider</span></span>

<span data-ttu-id="7b530-365">Para las aplicaciones que tienen como destino ASP.NET Core 1.1.0 o superior, el [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) el paquete de proveedor puede implementar el seguimiento de eventos.</span><span class="sxs-lookup"><span data-stu-id="7b530-365">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="7b530-366">En Windows, usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="7b530-366">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="7b530-367">El proveedor es entre plataformas, pero no hay ningún evento herramientas de recopilación y visualización aún para Linux o Mac OS.</span><span class="sxs-lookup"><span data-stu-id="7b530-367">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7b530-368">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7b530-368">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7b530-369">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7b530-369">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="7b530-370">Una buena manera de recopilar y ver los registros es usar la [PerfView utilidad](https://www.microsoft.com/download/details.aspx?id=28567).</span><span class="sxs-lookup"><span data-stu-id="7b530-370">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="7b530-371">Hay otras herramientas para ver los registros ETW, pero PerfView proporciona la mejor experiencia para trabajar con los eventos ETW emitidos por ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7b530-371">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="7b530-372">Para configurar PerfView para recopilar eventos registrados por este proveedor, agregue la cadena `*Microsoft-Extensions-Logging` a la **proveedores adicionales** lista.</span><span class="sxs-lookup"><span data-stu-id="7b530-372">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="7b530-373">(No se pierda el asterisco al principio de la cadena.)</span><span class="sxs-lookup"><span data-stu-id="7b530-373">(Don't miss the asterisk at the start of the string.)</span></span>

![Proveedores de Perfview adicionales](logging/_static/perfview-additional-providers.png)

<span data-ttu-id="7b530-375">Captura de eventos en Nano Server, requiere alguna configuración adicional:</span><span class="sxs-lookup"><span data-stu-id="7b530-375">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="7b530-376">Conectar la comunicación remota de PowerShell con Nano Server:</span><span class="sxs-lookup"><span data-stu-id="7b530-376">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="7b530-377">Crear una sesión de ETW:</span><span class="sxs-lookup"><span data-stu-id="7b530-377">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="7b530-378">Agregar proveedores ETW para [CLR](https://msdn.microsoft.com/library/ff357718), ASP.NET Core y otros elementos según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="7b530-378">Add ETW providers for [CLR](https://msdn.microsoft.com/library/ff357718), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="7b530-379">El proveedor de ASP.NET Core GUID es `3ac73b97-af73-50e9-0822-5da4367920d0`.</span><span class="sxs-lookup"><span data-stu-id="7b530-379">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="7b530-380">Ejecute el sitio Web y realice las acciones que desee obtener información de seguimiento para.</span><span class="sxs-lookup"><span data-stu-id="7b530-380">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="7b530-381">Detener la sesión de seguimiento cuando haya terminado:</span><span class="sxs-lookup"><span data-stu-id="7b530-381">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="7b530-382">Resultante *C:\trace.etl* archivo se pueden analizar con PerfView como en otras ediciones de Windows.</span><span class="sxs-lookup"><span data-stu-id="7b530-382">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="7b530-383">El proveedor de registro de eventos de Windows</span><span class="sxs-lookup"><span data-stu-id="7b530-383">The Windows EventLog provider</span></span>

<span data-ttu-id="7b530-384">El [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) el paquete de proveedor envía la salida de registro en el registro de eventos de Windows.</span><span class="sxs-lookup"><span data-stu-id="7b530-384">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7b530-385">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7b530-385">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7b530-386">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7b530-386">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="7b530-387">[Las sobrecargas de método AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) permiten pasar en `EventLogSettings` o un nivel de registro mínimo.</span><span class="sxs-lookup"><span data-stu-id="7b530-387">[AddEventLog overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="7b530-388">El proveedor de TraceSource</span><span class="sxs-lookup"><span data-stu-id="7b530-388">The TraceSource provider</span></span>

<span data-ttu-id="7b530-389">El [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) proveedor paquete utiliza la [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) bibliotecas y los proveedores.</span><span class="sxs-lookup"><span data-stu-id="7b530-389">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7b530-390">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7b530-390">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7b530-391">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7b530-391">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="7b530-392">[Las sobrecargas de AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) permiten pasar en un modificador de origen y un agente de escucha de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="7b530-392">[AddTraceSource overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="7b530-393">Para utilizar este proveedor, una aplicación debe ejecutarse en el .NET Framework (en lugar de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="7b530-393">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="7b530-394">El proveedor permite enrutar mensajes a una variedad de [los agentes de escucha](https://msdn.microsoft.com/library/4y5y10s7), como el [TextWriterTraceListener](https://msdn.microsoft.com/library/system.diagnostics.textwritertracelistener) usado en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="7b530-394">The provider lets you route messages to a variety of [listeners](https://msdn.microsoft.com/library/4y5y10s7), such as the [TextWriterTraceListener](https://msdn.microsoft.com/library/system.diagnostics.textwritertracelistener) used in the sample application.</span></span>

<span data-ttu-id="7b530-395">En el ejemplo siguiente se configura un `TraceSource` proveedor que registra `Warning` y mensajes superior en la ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="7b530-395">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](logging/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="7b530-396">El proveedor de servicios de aplicación de Azure</span><span class="sxs-lookup"><span data-stu-id="7b530-396">The Azure App Service provider</span></span>

<span data-ttu-id="7b530-397">El [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) el paquete de proveedor escribe registros en archivos de texto en el sistema de archivos de una aplicación de servicio de aplicaciones de Azure y al [almacenamiento de blobs](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) en una cuenta de almacenamiento de Azure.</span><span class="sxs-lookup"><span data-stu-id="7b530-397">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="7b530-398">El proveedor está disponible únicamente para las aplicaciones que tienen como destino ASP.NET Core 1.1.0 o superior.</span><span class="sxs-lookup"><span data-stu-id="7b530-398">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7b530-399">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7b530-399">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

> [!NOTE]
> <span data-ttu-id="7b530-400">Núcleo de ASP.NET 2.0 está en vista previa.</span><span class="sxs-lookup"><span data-stu-id="7b530-400">ASP.NET Core 2.0 is in preview.</span></span>  <span data-ttu-id="7b530-401">No pueden ejecutar las aplicaciones creadas con la versión preliminar más reciente cuando se implementa en el servicio de aplicaciones de Azure.</span><span class="sxs-lookup"><span data-stu-id="7b530-401">Apps created with the latest preview release may not run when deployed to Azure App Service.</span></span> <span data-ttu-id="7b530-402">Cuando se suelta el núcleo de ASP.NET 2.0, el servicio de aplicaciones de Azure se ejecutará 2.0 aplicaciones y el servicio de aplicaciones de Azure proveedor funcionará como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="7b530-402">When ASP.NET Core 2.0 is released, Azure App Service will run 2.0 apps, and the Azure App Service provider will work as indicated here.</span></span>

<span data-ttu-id="7b530-403">No tienes que instalar el paquete de proveedor o llame a la `AddAzureWebAppDiagnostics` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="7b530-403">You don't have to install the provider package or call the `AddAzureWebAppDiagnostics` extension method.</span></span>  <span data-ttu-id="7b530-404">El proveedor está disponible automáticamente para la aplicación al implementar la aplicación en el servicio de aplicaciones de Azure.</span><span class="sxs-lookup"><span data-stu-id="7b530-404">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7b530-405">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7b530-405">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="7b530-406">Un `AddAzureWebAppDiagnostics` sobrecarga permite pasar en [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), con el que puede invalidar la configuración predeterminada, como la plantilla de registro de salida, el nombre de blob y el límite de tamaño de archivo.</span><span class="sxs-lookup"><span data-stu-id="7b530-406">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="7b530-407">(*Plantilla salida* es una cadena de formato de mensaje que se aplica a todos los registros, en la parte superior que proporciona cuando se llama a un `ILogger` método.)</span><span class="sxs-lookup"><span data-stu-id="7b530-407">(*Output template* is a message format string that is applied to all logs, on top of the one that you provide when you call an `ILogger` method.)</span></span>  

---

<span data-ttu-id="7b530-408">Cuando se implementa en una aplicación de servicio de aplicaciones, la aplicación respeta la configuración de la [registros de diagnóstico](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) sección de la **servicio de aplicaciones** página del portal de Azure.</span><span class="sxs-lookup"><span data-stu-id="7b530-408">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="7b530-409">Cuando cambia esta configuración, los cambios surten efecto inmediatamente sin necesidad de reiniciar la aplicación ni de volver a implementar código en él.</span><span class="sxs-lookup"><span data-stu-id="7b530-409">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Configuración de registro de Azure](logging/_static/azure-logging-settings.png)

<span data-ttu-id="7b530-411">La ubicación predeterminada de los archivos de registro está en la *D:\\principal\\LogFiles\\aplicación* carpeta y el nombre de archivo predeterminado es *yyyymmdd.txt diagnósticos*.</span><span class="sxs-lookup"><span data-stu-id="7b530-411">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="7b530-412">El límite de tamaño de archivo predeterminado es 10 MB, y el número máximo predeterminado de archivos conserva es 2.</span><span class="sxs-lookup"><span data-stu-id="7b530-412">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="7b530-413">El nombre de blob predeterminado es *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="7b530-413">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="7b530-414">Para obtener más información sobre el comportamiento predeterminado, consulte [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span><span class="sxs-lookup"><span data-stu-id="7b530-414">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="7b530-415">El proveedor solo funciona cuando se ejecuta el proyecto en el entorno de Azure.</span><span class="sxs-lookup"><span data-stu-id="7b530-415">The provider only works when your project runs in the Azure environment.</span></span>  <span data-ttu-id="7b530-416">No tiene ningún efecto cuando se ejecuta localmente &mdash; no los escribe en archivos locales o almacenamiento de desarrollo local para los blobs.</span><span class="sxs-lookup"><span data-stu-id="7b530-416">It has no effect when you run locally &mdash; it does not write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="7b530-417">Proveedores de registro de aplicaciones de terceros</span><span class="sxs-lookup"><span data-stu-id="7b530-417">Third-party logging providers</span></span>

<span data-ttu-id="7b530-418">Estos son algunos marcos de registro de aplicaciones de terceros que funcionan con ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7b530-418">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="7b530-419">[ELMAH.IO](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -proveedor para el servicio Elmah.Io</span><span class="sxs-lookup"><span data-stu-id="7b530-419">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="7b530-420">[JSNLog](http://jsnlog.com) -registra las excepciones de JavaScript y otros eventos de cliente en el registro de servidor.</span><span class="sxs-lookup"><span data-stu-id="7b530-420">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="7b530-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -proveedor para el servicio Loggr</span><span class="sxs-lookup"><span data-stu-id="7b530-421">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="7b530-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) -proveedor para la biblioteca de NLog</span><span class="sxs-lookup"><span data-stu-id="7b530-422">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="7b530-423">[Serilog](https://github.com/serilog/serilog-framework-logging) -proveedor para la biblioteca de Serilog</span><span class="sxs-lookup"><span data-stu-id="7b530-423">[Serilog](https://github.com/serilog/serilog-framework-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="7b530-424">Algunos marcos de terceros pueden hacer [registro semántica, también conocido como registro estructurado](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="7b530-424">Some third-party frameworks can do [semantic logging, also known as structured logging](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="7b530-425">Con el marco de terceros es similar al uso de uno de los proveedores integrados: agregar un paquete de NuGet al proyecto y llame a un método de extensión en `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="7b530-425">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="7b530-426">Para obtener más información, consulte la documentación de cada marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="7b530-426">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="7b530-427">Puede crear sus propios proveedores personalizados, para admitir otros marcos de registro o sus propios requisitos de registro.</span><span class="sxs-lookup"><span data-stu-id="7b530-427">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>
