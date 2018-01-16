---
title: Registro en ASP.NET Core
author: ardalis
description: "Obtenga información sobre la plataforma de registro de ASP.NET Core. Descubra los proveedores de registro integrados y obtenga más información sobre proveedores de terceros conocidos."
keywords: "ASP.NET Core, registro, proveedores de registro, Microsoft.Extensions.Logging, ILogger, ILoggerFactory, LogLevel, WithFilter, TraceSource, EventLog, EventSource, ámbitos"
ms.author: tdykstra
manager: wpickett
ms.date: 12/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging/index
ms.openlocfilehash: 3eb167c961b8d089d508ef5622db6ae1cdd99088
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2018
---
# <a name="introduction-to-logging-in-aspnet-core"></a><span data-ttu-id="3547b-105">Introducción al registro en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3547b-105">Introduction to logging in ASP.NET Core</span></span>

<span data-ttu-id="3547b-106">Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="3547b-106">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="3547b-107">ASP.NET Core es compatible con una API de registro que funciona con una variedad de proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="3547b-107">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="3547b-108">Los proveedores integrados permiten enviar registros a uno o varios destinos, y se puede conectar una plataforma de registro de terceros.</span><span class="sxs-lookup"><span data-stu-id="3547b-108">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="3547b-109">En este artículo se muestra cómo usar las API y los proveedores de registro integrados en el código.</span><span class="sxs-lookup"><span data-stu-id="3547b-109">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3547b-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3547b-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3547b-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3547b-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3547b-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3547b-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3547b-113">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3547b-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="how-to-create-logs"></a><span data-ttu-id="3547b-114">Cómo crear registros</span><span class="sxs-lookup"><span data-stu-id="3547b-114">How to create logs</span></span>

<span data-ttu-id="3547b-115">Para crear registros, obtenga un objeto `ILogger` desde el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="3547b-115">To create logs, get an `ILogger` object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="3547b-116">Después, llame a los métodos de registro de ese objeto de registrador:</span><span class="sxs-lookup"><span data-stu-id="3547b-116">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="3547b-117">En este ejemplo se crean registros con la clase `TodoController` como *categoría*.</span><span class="sxs-lookup"><span data-stu-id="3547b-117">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="3547b-118">Las categorías se explican [más adelante en este artículo](#log-category).</span><span class="sxs-lookup"><span data-stu-id="3547b-118">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="3547b-119">ASP.NET Core no proporciona métodos de registrador asincrónicos porque el registro debe ser tan rápido que el costo de usarlos no vale la pena.</span><span class="sxs-lookup"><span data-stu-id="3547b-119">ASP.NET Core does not provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="3547b-120">Si se encuentra en una situación en la que no sea así, considere la posibilidad de cambiar el modo de registro.</span><span class="sxs-lookup"><span data-stu-id="3547b-120">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="3547b-121">Si el almacén de datos es lento, escriba primero los mensajes de registro en un almacén rápido y, después, muévalos a un almacén de baja velocidad.</span><span class="sxs-lookup"><span data-stu-id="3547b-121">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="3547b-122">Por ejemplo, realice el registro en una cola de mensajes que otro proceso lea y conserve en almacenamiento lento.</span><span class="sxs-lookup"><span data-stu-id="3547b-122">For example, log to a message queue that is read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="3547b-123">Cómo agregar proveedores</span><span class="sxs-lookup"><span data-stu-id="3547b-123">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3547b-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3547b-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3547b-125">Un proveedor de registro toma los mensajes que se crean con un objeto `ILogger` y los muestra o almacena.</span><span class="sxs-lookup"><span data-stu-id="3547b-125">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="3547b-126">Por ejemplo, el proveedor de la consola muestra mensajes en la consola y el proveedor de Azure App Service puede almacenarlos en Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="3547b-126">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="3547b-127">Para usar un proveedor, llame al método de extensión `Add<ProviderName>` del proveedor en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="3547b-127">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="3547b-128">La plantilla de proyecto predeterminada permite el registro con el método [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___):</span><span class="sxs-lookup"><span data-stu-id="3547b-128">The default project template enables logging with the [CreateDefaultBuilder](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3547b-129">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3547b-129">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3547b-130">Un proveedor de registro toma los mensajes que se crean con un objeto `ILogger` y los muestra o almacena.</span><span class="sxs-lookup"><span data-stu-id="3547b-130">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="3547b-131">Por ejemplo, el proveedor de la consola muestra mensajes en la consola y el proveedor de Azure App Service puede almacenarlos en Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="3547b-131">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="3547b-132">Para usar un proveedor, instale su paquete NuGet y llame al método de extensión del proveedor en una instancia de `ILoggerFactory`, como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="3547b-132">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="3547b-133">La [inserción de dependencias](xref:fundamentals/dependency-injection) (DI) de ASP.NET Core proporciona la instancia de `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="3547b-133">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="3547b-134">Los métodos de extensión `AddConsole` y `AddDebug` se definen en los paquetes [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) y [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="3547b-134">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="3547b-135">Cada método de extensión llama al método `ILoggerFactory.AddProvider`, pasando una instancia del proveedor.</span><span class="sxs-lookup"><span data-stu-id="3547b-135">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="3547b-136">En la aplicación de ejemplo de este artículo se agregan proveedores de registro al método `Configure` de la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="3547b-136">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="3547b-137">Si quiere obtener la salida de registro de código que se ejecuta antes, agregue los proveedores de registro en el constructor de la clase `Startup` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="3547b-137">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="3547b-138">Puede encontrar información sobre cada [proveedor de registro integrado](#built-in-logging-providers) y vínculos a [proveedores de registro de terceros](#third-party-logging-providers) más adelante en el artículo.</span><span class="sxs-lookup"><span data-stu-id="3547b-138">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="3547b-139">Salida de registro de ejemplo</span><span class="sxs-lookup"><span data-stu-id="3547b-139">Sample logging output</span></span>

<span data-ttu-id="3547b-140">Con el código de ejemplo que se muestra en la sección anterior, verá los registros en la consola cuando se ejecute desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="3547b-140">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="3547b-141">Este es un ejemplo de salida de la consola:</span><span class="sxs-lookup"><span data-stu-id="3547b-141">Here's an example of console output:</span></span>

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
 
<span data-ttu-id="3547b-142">Estos registros se crearon yendo a `http://localhost:5000/api/todo/0`, lo que desencadena la ejecución de las dos llamadas a `ILogger` que se muestran en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="3547b-142">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="3547b-143">Este es un ejemplo de los mismos registros tal y como aparecen en la ventana de depuración cuando se ejecuta la aplicación de ejemplo en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="3547b-143">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="3547b-144">Los registros creados por las llamadas a `ILogger` que se muestran en la sección anterior comienzan por "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="3547b-144">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="3547b-145">Los registros que comienzan por categorías de "Microsoft" son de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3547b-145">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="3547b-146">El propio ASP.NET Core y el código de la aplicación usan la misma API y los mismos proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="3547b-146">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="3547b-147">En el resto de este artículo se explican algunos detalles y opciones para el registro.</span><span class="sxs-lookup"><span data-stu-id="3547b-147">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="3547b-148">Paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="3547b-148">NuGet packages</span></span>

<span data-ttu-id="3547b-149">Las interfaces `ILogger` e `ILoggerFactory` se encuentran en [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), y sus implementaciones predeterminadas en [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span><span class="sxs-lookup"><span data-stu-id="3547b-149">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="3547b-150">Categoría de registro</span><span class="sxs-lookup"><span data-stu-id="3547b-150">Log category</span></span>

<span data-ttu-id="3547b-151">Con cada registro que cree se incluye una *categoría*.</span><span class="sxs-lookup"><span data-stu-id="3547b-151">A *category* is included with each log that you create.</span></span> <span data-ttu-id="3547b-152">La categoría se especifica cuando se crea un objeto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="3547b-152">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="3547b-153">La categoría puede ser cualquier cadena, pero una convención consiste en usar el nombre completo de la clase desde la que se escriben los registros.</span><span class="sxs-lookup"><span data-stu-id="3547b-153">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="3547b-154">Por ejemplo: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="3547b-154">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="3547b-155">Puede especificar la categoría como una cadena o usar un método de extensión que derive la categoría del tipo.</span><span class="sxs-lookup"><span data-stu-id="3547b-155">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="3547b-156">Para especificar la categoría como una cadena, llame a `CreateLogger` en una instancia de `ILoggerFactory`, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3547b-156">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="3547b-157">En la mayoría de los casos, será más fácil usar `ILogger<T>`, como en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="3547b-157">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="3547b-158">Esto equivale a llamar a `CreateLogger` con el nombre de tipo completo de `T`.</span><span class="sxs-lookup"><span data-stu-id="3547b-158">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="3547b-159">Nivel de registro</span><span class="sxs-lookup"><span data-stu-id="3547b-159">Log level</span></span>

<span data-ttu-id="3547b-160">Cada vez que se escribe un registro, se especifica su [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="3547b-160">Each time you write a log, you specify its [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="3547b-161">El nivel de registro indica el grado de gravedad o importancia.</span><span class="sxs-lookup"><span data-stu-id="3547b-161">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="3547b-162">Por ejemplo, es posible que escriba un registro `Information` cuando un método finaliza normalmente, un registro `Warning` cuando un método devuelve un código de retorno 404 y un registro `Error` cuando capture una excepción inesperada.</span><span class="sxs-lookup"><span data-stu-id="3547b-162">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="3547b-163">En el ejemplo de código siguiente, los nombres de los métodos (por ejemplo, `LogWarning`) especifican el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="3547b-163">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="3547b-164">El primer parámetro es el [Id. del evento del Registro](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="3547b-164">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="3547b-165">El segundo parámetro es una [plantilla de mensaje](#log-message-template) con marcadores de posición para los valores de argumento proporcionados por el resto de parámetros de método.</span><span class="sxs-lookup"><span data-stu-id="3547b-165">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="3547b-166">Los parámetros de método se explican detalladamente más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="3547b-166">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="3547b-167">Los métodos de registro que incluyen el nivel en el nombre del método son [métodos de extensión para ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="3547b-167">Log methods that include the level in the method name are [extension methods for ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="3547b-168">En segundo plano, estos métodos llaman a un método `Log` que toma un parámetro `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="3547b-168">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="3547b-169">Puede llamar directamente al método `Log` en lugar de a uno de estos métodos de extensión, pero la sintaxis es relativamente complicada.</span><span class="sxs-lookup"><span data-stu-id="3547b-169">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="3547b-170">Para más información, vea la [interfaz ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) y el [código fuente de las extensiones de registrador](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="3547b-170">For more information, see the [ILogger interface](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="3547b-171">ASP.NET Core define los [niveles de registro](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel) siguientes, que aquí se ordenan de menor a mayor gravedad.</span><span class="sxs-lookup"><span data-stu-id="3547b-171">ASP.NET Core defines the following [log levels](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="3547b-172">Seguimiento = 0</span><span class="sxs-lookup"><span data-stu-id="3547b-172">Trace = 0</span></span>

  <span data-ttu-id="3547b-173">Para la información que solo es útil para un desarrollador que depura un problema.</span><span class="sxs-lookup"><span data-stu-id="3547b-173">For information that is valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="3547b-174">Estos mensajes pueden contener datos confidenciales de la aplicación por lo que no deben habilitarse en un entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="3547b-174">These messages may contain sensitive application data and so should not be enabled in a production environment.</span></span> <span data-ttu-id="3547b-175">*Deshabilitado de forma predeterminada.*</span><span class="sxs-lookup"><span data-stu-id="3547b-175">*Disabled by default.*</span></span> <span data-ttu-id="3547b-176">Ejemplo: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="3547b-176">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="3547b-177">Depurar = 1</span><span class="sxs-lookup"><span data-stu-id="3547b-177">Debug = 1</span></span>

  <span data-ttu-id="3547b-178">Para la información que tiene utilidad a corto plazo durante el desarrollo y la depuración.</span><span class="sxs-lookup"><span data-stu-id="3547b-178">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="3547b-179">Ejemplo: `Entering method Configure with flag set to true.` Normalmente los registros de nivel `Debug` no se habilitarían en producción, a menos que se esté solucionando un problema, debido al elevado volumen de registros.</span><span class="sxs-lookup"><span data-stu-id="3547b-179">Example: `Entering method Configure with flag set to true.` You typically would not enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="3547b-180">Información = 2</span><span class="sxs-lookup"><span data-stu-id="3547b-180">Information = 2</span></span>

  <span data-ttu-id="3547b-181">Para realizar el seguimiento del flujo general de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3547b-181">For tracking the general flow of the application.</span></span> <span data-ttu-id="3547b-182">Estos registros suelen tener algún valor a largo plazo.</span><span class="sxs-lookup"><span data-stu-id="3547b-182">These logs typically have some long-term value.</span></span> <span data-ttu-id="3547b-183">Ejemplo: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="3547b-183">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="3547b-184">Advertencia = 3</span><span class="sxs-lookup"><span data-stu-id="3547b-184">Warning = 3</span></span>

  <span data-ttu-id="3547b-185">Para los eventos anómalos o inesperados en el flujo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3547b-185">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="3547b-186">Estos pueden incluir errores u otras condiciones que no hacen que la aplicación se detenga, pero que puede que sea necesario investigar.</span><span class="sxs-lookup"><span data-stu-id="3547b-186">These may include errors or other conditions that do not cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="3547b-187">Las excepciones controladas son un lugar común para usar el nivel de registro `Warning`.</span><span class="sxs-lookup"><span data-stu-id="3547b-187">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="3547b-188">Ejemplo: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="3547b-188">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="3547b-189">Error = 4</span><span class="sxs-lookup"><span data-stu-id="3547b-189">Error = 4</span></span>

  <span data-ttu-id="3547b-190">Para los errores y excepciones que no se pueden controlar.</span><span class="sxs-lookup"><span data-stu-id="3547b-190">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="3547b-191">Estos mensajes indican un error en la actividad u operación actual (por ejemplo, la solicitud HTTP actual), no un error de toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3547b-191">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="3547b-192">Mensaje de registro de ejemplo: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="3547b-192">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="3547b-193">Crítico = 5</span><span class="sxs-lookup"><span data-stu-id="3547b-193">Critical = 5</span></span>

  <span data-ttu-id="3547b-194">Para los errores que requieren atención inmediata.</span><span class="sxs-lookup"><span data-stu-id="3547b-194">For failures that require immediate attention.</span></span> <span data-ttu-id="3547b-195">Ejemplos: escenarios de pérdida de datos, espacio en disco insuficiente.</span><span class="sxs-lookup"><span data-stu-id="3547b-195">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="3547b-196">Puede usar el nivel de registro para controlar la cantidad de salida del registro que se escribe en un medio de almacenamiento determinado o ventana de presentación.</span><span class="sxs-lookup"><span data-stu-id="3547b-196">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="3547b-197">Por ejemplo, en producción es posible que quiera que todos los registros de nivel `Information` e inferiores vayan a un almacén de datos de volumen y que todos los registros de nivel `Warning` y superiores vayan un almacén de datos de valor.</span><span class="sxs-lookup"><span data-stu-id="3547b-197">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="3547b-198">Durante el desarrollo, es posible que normalmente envíe los registros de gravedad `Warning` o superior a la consola.</span><span class="sxs-lookup"><span data-stu-id="3547b-198">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="3547b-199">Después, si tiene que solucionar problemas, puede agregar el nivel `Debug`.</span><span class="sxs-lookup"><span data-stu-id="3547b-199">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="3547b-200">En la sección [Filtrado del registro](#log-filtering) de este artículo se explica cómo controlar los niveles de registro que controla un proveedor.</span><span class="sxs-lookup"><span data-stu-id="3547b-200">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="3547b-201">La plataforma ASP.NET Core escribe registros de nivel `Debug` para los eventos de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="3547b-201">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="3547b-202">En los ejemplos de registro anteriores de este artículo se excluyeron los registros por debajo del nivel `Information`, por lo que no se mostraron los registros de nivel `Debug`.</span><span class="sxs-lookup"><span data-stu-id="3547b-202">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="3547b-203">Este es un ejemplo de registros de consola si ejecuta la aplicación de ejemplo configurada para mostrar `Debug` y registros superiores para el proveedor de la consola.</span><span class="sxs-lookup"><span data-stu-id="3547b-203">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="3547b-204">Id. de evento del registro</span><span class="sxs-lookup"><span data-stu-id="3547b-204">Log event ID</span></span>

<span data-ttu-id="3547b-205">Cada vez que se escribe un registro, puede especificar un *Id. de evento*.</span><span class="sxs-lookup"><span data-stu-id="3547b-205">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="3547b-206">La aplicación de ejemplo lo hace mediante una clase `LoggingEvents` definida de forma local:</span><span class="sxs-lookup"><span data-stu-id="3547b-206">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="3547b-207">Un identificador de evento es un valor entero que se puede usar para asociar un conjunto de eventos registrados entre sí.</span><span class="sxs-lookup"><span data-stu-id="3547b-207">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="3547b-208">Por ejemplo, un registro para agregar un elemento a un carro de la compra podría tener el identificador de evento 1000 y un registro para completar una compra podría tener el identificador de evento 1001.</span><span class="sxs-lookup"><span data-stu-id="3547b-208">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="3547b-209">En la salida del registro, el identificador de evento podría estar almacenado en un campo o incluido en el mensaje de texto, en función del proveedor.</span><span class="sxs-lookup"><span data-stu-id="3547b-209">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="3547b-210">El proveedor de depuración no muestra los identificadores de evento, pero el proveedor de la consola los muestra entre paréntesis después de la categoría:</span><span class="sxs-lookup"><span data-stu-id="3547b-210">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="3547b-211">Plantilla de mensaje de registro</span><span class="sxs-lookup"><span data-stu-id="3547b-211">Log message template</span></span>

<span data-ttu-id="3547b-212">Cada vez que se escribe un mensaje de registro, se proporciona una plantilla de mensaje.</span><span class="sxs-lookup"><span data-stu-id="3547b-212">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="3547b-213">La plantilla de mensaje puede ser una cadena o puede contener marcadores de posición con nombre en los que se colocan los valores de argumento.</span><span class="sxs-lookup"><span data-stu-id="3547b-213">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="3547b-214">La plantilla no es una cadena de formato y los marcadores de posición deben tener un nombre, no un número.</span><span class="sxs-lookup"><span data-stu-id="3547b-214">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="3547b-215">El orden de los marcadores de posición, no sus nombres, determina qué parámetros se usan para proporcionar sus valores.</span><span class="sxs-lookup"><span data-stu-id="3547b-215">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="3547b-216">Si tiene el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="3547b-216">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="3547b-217">El mensaje del registro resultante tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="3547b-217">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="3547b-218">La plataforma de registro aplica este formato de mensajes para que los proveedores de registro puedan implementar el [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="3547b-218">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="3547b-219">Dado que los propios argumentos se pasan al sistema de registro, no solo la plantilla de mensaje con formato, los proveedores de registro pueden almacenar los valores de parámetros como campos además de la plantilla de mensaje.</span><span class="sxs-lookup"><span data-stu-id="3547b-219">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="3547b-220">Si va a dirigir la salida del registro a Azure Table Storage y la llamada al método de registrador tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="3547b-220">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="3547b-221">Cada entidad de la Tabla de Azure puede tener propiedades `ID` y `RequestTime`, lo que simplifica las consultas en los datos de registro.</span><span class="sxs-lookup"><span data-stu-id="3547b-221">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="3547b-222">Puede buscar todos los registros dentro de un intervalo `RequestTime` determinado sin necesidad de analizar el tiempo de espera del mensaje de texto.</span><span class="sxs-lookup"><span data-stu-id="3547b-222">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="3547b-223">Excepciones de registro</span><span class="sxs-lookup"><span data-stu-id="3547b-223">Logging exceptions</span></span>

<span data-ttu-id="3547b-224">Los métodos de registrador tienen sobrecargas que le permiten pasar una excepción, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3547b-224">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="3547b-225">Cada proveedor controla la información de la excepción de maneras diferentes.</span><span class="sxs-lookup"><span data-stu-id="3547b-225">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="3547b-226">Este es un ejemplo de salida del proveedor de depuración del código mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="3547b-226">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="3547b-227">Filtrado del registro</span><span class="sxs-lookup"><span data-stu-id="3547b-227">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3547b-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3547b-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3547b-229">Puede especificar un nivel de registro mínimo para un proveedor y una categoría específicos, o para todos los proveedores o todas las categorías.</span><span class="sxs-lookup"><span data-stu-id="3547b-229">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="3547b-230">Los registros por debajo del nivel mínimo no se pasan a ese proveedor, de modo que no se muestran o almacenan.</span><span class="sxs-lookup"><span data-stu-id="3547b-230">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="3547b-231">Si quiere suprimir todos los registros, puede especificar `LogLevel.None` como el nivel de registro mínimo.</span><span class="sxs-lookup"><span data-stu-id="3547b-231">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="3547b-232">El valor entero de `LogLevel.None` es 6, que es superior a `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="3547b-232">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="3547b-233">**Crear reglas de filtro en la configuración**</span><span class="sxs-lookup"><span data-stu-id="3547b-233">**Create filter rules in configuration**</span></span>

<span data-ttu-id="3547b-234">Las plantillas de proyecto crean código que llama a `CreateDefaultBuilder` para configurar el registro para los proveedores de consola y de depuración.</span><span class="sxs-lookup"><span data-stu-id="3547b-234">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="3547b-235">El método `CreateDefaultBuilder` también establece el registro para buscar la configuración en una sección `Logging`, mediante código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="3547b-235">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="3547b-236">Los datos de configuración especifican niveles de registro mínimo por proveedor y categoría, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3547b-236">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="3547b-237">Este archivo JSON crea seis reglas de filtro, una para el proveedor de depuración, cuatro para el proveedor de la consola y una que se aplica a todos los proveedores.</span><span class="sxs-lookup"><span data-stu-id="3547b-237">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="3547b-238">Más adelante verá que solo una de estas reglas se elige para cada proveedor cuando se crea un objeto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="3547b-238">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="3547b-239">**Reglas de filtro en el código**</span><span class="sxs-lookup"><span data-stu-id="3547b-239">**Filter rules in code**</span></span>

<span data-ttu-id="3547b-240">Puede registrar las reglas de filtro en el código, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3547b-240">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="3547b-241">El segundo `AddFilter` especifica el proveedor de depuración mediante su nombre de tipo.</span><span class="sxs-lookup"><span data-stu-id="3547b-241">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="3547b-242">El primer `AddFilter` se aplica a todos los proveedores, dado que no especifica un tipo de proveedor.</span><span class="sxs-lookup"><span data-stu-id="3547b-242">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="3547b-243">**Cómo se aplican las reglas de filtro**</span><span class="sxs-lookup"><span data-stu-id="3547b-243">**How filtering rules are applied**</span></span>

<span data-ttu-id="3547b-244">Los datos de configuración y el código de `AddFilter` que se muestran en los ejemplos anteriores crean las reglas que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="3547b-244">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="3547b-245">Las seis primeras proceden del ejemplo de configuración y las dos últimas del ejemplo de código.</span><span class="sxs-lookup"><span data-stu-id="3547b-245">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="3547b-246">número</span><span class="sxs-lookup"><span data-stu-id="3547b-246">Number</span></span> | <span data-ttu-id="3547b-247">Proveedor</span><span class="sxs-lookup"><span data-stu-id="3547b-247">Provider</span></span>      | <span data-ttu-id="3547b-248">Categorías que comienzan por...</span><span class="sxs-lookup"><span data-stu-id="3547b-248">Categories that begin with ...</span></span>          | <span data-ttu-id="3547b-249">Nivel de registro mínimo</span><span class="sxs-lookup"><span data-stu-id="3547b-249">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="3547b-250">1</span><span class="sxs-lookup"><span data-stu-id="3547b-250">1</span></span>      | <span data-ttu-id="3547b-251">Depuración</span><span class="sxs-lookup"><span data-stu-id="3547b-251">Debug</span></span>         | <span data-ttu-id="3547b-252">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="3547b-252">All categories</span></span>                          | <span data-ttu-id="3547b-253">Información</span><span class="sxs-lookup"><span data-stu-id="3547b-253">Information</span></span>       |
| <span data-ttu-id="3547b-254">2</span><span class="sxs-lookup"><span data-stu-id="3547b-254">2</span></span>      | <span data-ttu-id="3547b-255">Consola</span><span class="sxs-lookup"><span data-stu-id="3547b-255">Console</span></span>       | <span data-ttu-id="3547b-256">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="3547b-256">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="3547b-257">Advertencia</span><span class="sxs-lookup"><span data-stu-id="3547b-257">Warning</span></span>           |
| <span data-ttu-id="3547b-258">3</span><span class="sxs-lookup"><span data-stu-id="3547b-258">3</span></span>      | <span data-ttu-id="3547b-259">Consola</span><span class="sxs-lookup"><span data-stu-id="3547b-259">Console</span></span>       | <span data-ttu-id="3547b-260">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="3547b-260">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="3547b-261">Depuración</span><span class="sxs-lookup"><span data-stu-id="3547b-261">Debug</span></span>             |
| <span data-ttu-id="3547b-262">4</span><span class="sxs-lookup"><span data-stu-id="3547b-262">4</span></span>      | <span data-ttu-id="3547b-263">Consola</span><span class="sxs-lookup"><span data-stu-id="3547b-263">Console</span></span>       | <span data-ttu-id="3547b-264">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="3547b-264">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="3547b-265">Error</span><span class="sxs-lookup"><span data-stu-id="3547b-265">Error</span></span>             |
| <span data-ttu-id="3547b-266">5</span><span class="sxs-lookup"><span data-stu-id="3547b-266">5</span></span>      | <span data-ttu-id="3547b-267">Consola</span><span class="sxs-lookup"><span data-stu-id="3547b-267">Console</span></span>       | <span data-ttu-id="3547b-268">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="3547b-268">All categories</span></span>                          | <span data-ttu-id="3547b-269">Información</span><span class="sxs-lookup"><span data-stu-id="3547b-269">Information</span></span>       |
| <span data-ttu-id="3547b-270">6</span><span class="sxs-lookup"><span data-stu-id="3547b-270">6</span></span>      | <span data-ttu-id="3547b-271">Todos los proveedores</span><span class="sxs-lookup"><span data-stu-id="3547b-271">All providers</span></span> | <span data-ttu-id="3547b-272">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="3547b-272">All categories</span></span>                          | <span data-ttu-id="3547b-273">Depuración</span><span class="sxs-lookup"><span data-stu-id="3547b-273">Debug</span></span>             |
| <span data-ttu-id="3547b-274">7</span><span class="sxs-lookup"><span data-stu-id="3547b-274">7</span></span>      | <span data-ttu-id="3547b-275">Todos los proveedores</span><span class="sxs-lookup"><span data-stu-id="3547b-275">All providers</span></span> | <span data-ttu-id="3547b-276">Sistema</span><span class="sxs-lookup"><span data-stu-id="3547b-276">System</span></span>                                  | <span data-ttu-id="3547b-277">Depuración</span><span class="sxs-lookup"><span data-stu-id="3547b-277">Debug</span></span>             |
| <span data-ttu-id="3547b-278">8</span><span class="sxs-lookup"><span data-stu-id="3547b-278">8</span></span>      | <span data-ttu-id="3547b-279">Depuración</span><span class="sxs-lookup"><span data-stu-id="3547b-279">Debug</span></span>         | <span data-ttu-id="3547b-280">Microsoft</span><span class="sxs-lookup"><span data-stu-id="3547b-280">Microsoft</span></span>                               | <span data-ttu-id="3547b-281">Seguimiento</span><span class="sxs-lookup"><span data-stu-id="3547b-281">Trace</span></span>             |

<span data-ttu-id="3547b-282">Cuando se crea un objeto `ILogger` con el que escribir los registros, el objeto `ILoggerFactory` selecciona una sola regla por proveedor para aplicar a ese registrador.</span><span class="sxs-lookup"><span data-stu-id="3547b-282">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="3547b-283">Todos los mensajes escritos por ese objeto `ILogger` se filtran según las reglas seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="3547b-283">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="3547b-284">De las reglas disponibles se selecciona la más específica posible para cada par de categoría y proveedor.</span><span class="sxs-lookup"><span data-stu-id="3547b-284">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="3547b-285">Cuando se crea un `ILogger` para una categoría determinada, se usa el algoritmo siguiente para cada proveedor:</span><span class="sxs-lookup"><span data-stu-id="3547b-285">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="3547b-286">Se seleccionan todas las reglas que coinciden con el proveedor o su alias.</span><span class="sxs-lookup"><span data-stu-id="3547b-286">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="3547b-287">Si no se encuentra ninguna, se seleccionan todas las reglas con un proveedor vacío.</span><span class="sxs-lookup"><span data-stu-id="3547b-287">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="3547b-288">Del resultado del paso anterior, se seleccionan las reglas con el prefijo de categoría coincidente más largo.</span><span class="sxs-lookup"><span data-stu-id="3547b-288">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="3547b-289">Si no se encuentra ninguna, se seleccionan todas las reglas que no especifican una categoría.</span><span class="sxs-lookup"><span data-stu-id="3547b-289">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="3547b-290">Si se seleccionan varias reglas, se toma la **última**.</span><span class="sxs-lookup"><span data-stu-id="3547b-290">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="3547b-291">Si no se selecciona ninguna regla, se usa `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="3547b-291">If no rules are selected, use `MinimumLevel`.</span></span>
 
<span data-ttu-id="3547b-292">Por ejemplo, supongamos que tiene la lista de reglas anterior y crea un objeto `ILogger` para la categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="3547b-292">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="3547b-293">Para el proveedor de depuración, se aplican las reglas 1, 6 y 8.</span><span class="sxs-lookup"><span data-stu-id="3547b-293">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="3547b-294">La regla 8 es la más específica, por lo que se selecciona.</span><span class="sxs-lookup"><span data-stu-id="3547b-294">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="3547b-295">Para el proveedor de la consola, se aplican las reglas 3, 4, 5 y 6.</span><span class="sxs-lookup"><span data-stu-id="3547b-295">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="3547b-296">La regla 3 es la más específica.</span><span class="sxs-lookup"><span data-stu-id="3547b-296">Rule 3 is most specific.</span></span>

<span data-ttu-id="3547b-297">Cuando crea registros con un `ILogger` para la categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", los registros de nivel `Trace` y superiores se dirigirán al proveedor de depuración y los registros de nivel `Debug` y superiores se dirigirán al proveedor de consola.</span><span class="sxs-lookup"><span data-stu-id="3547b-297">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="3547b-298">**Alias de proveedor**</span><span class="sxs-lookup"><span data-stu-id="3547b-298">**Provider aliases**</span></span>

<span data-ttu-id="3547b-299">Puede usar el nombre de tipo para especificar un proveedor en la configuración, pero cada proveedor define un *alias* más breve que es más fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="3547b-299">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that is easier to use.</span></span> <span data-ttu-id="3547b-300">Para los proveedores integrados, use los alias siguientes:</span><span class="sxs-lookup"><span data-stu-id="3547b-300">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="3547b-301">Consola</span><span class="sxs-lookup"><span data-stu-id="3547b-301">Console</span></span>
- <span data-ttu-id="3547b-302">Depuración</span><span class="sxs-lookup"><span data-stu-id="3547b-302">Debug</span></span>
- <span data-ttu-id="3547b-303">EventLog</span><span class="sxs-lookup"><span data-stu-id="3547b-303">EventLog</span></span>
- <span data-ttu-id="3547b-304">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="3547b-304">AzureAppServices</span></span>
- <span data-ttu-id="3547b-305">TraceSource</span><span class="sxs-lookup"><span data-stu-id="3547b-305">TraceSource</span></span>
- <span data-ttu-id="3547b-306">EventSource</span><span class="sxs-lookup"><span data-stu-id="3547b-306">EventSource</span></span>

<span data-ttu-id="3547b-307">**Nivel mínimo predeterminado**</span><span class="sxs-lookup"><span data-stu-id="3547b-307">**Default minimum level**</span></span>

<span data-ttu-id="3547b-308">Hay una configuración de nivel mínimo que solo tiene efecto si no se aplica ninguna regla de configuración o código para un proveedor y una categoría determinados.</span><span class="sxs-lookup"><span data-stu-id="3547b-308">There is a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="3547b-309">En el ejemplo siguiente se muestra cómo establecer el nivel mínimo:</span><span class="sxs-lookup"><span data-stu-id="3547b-309">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="3547b-310">Si no establece explícitamente el nivel mínimo, el valor predeterminado es `Information`, lo que significa que los registros `Trace` y `Debug` se omiten.</span><span class="sxs-lookup"><span data-stu-id="3547b-310">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="3547b-311">**Funciones de filtro**</span><span class="sxs-lookup"><span data-stu-id="3547b-311">**Filter functions**</span></span>

<span data-ttu-id="3547b-312">Puede escribir código en una función de filtro para aplicar las reglas de filtrado.</span><span class="sxs-lookup"><span data-stu-id="3547b-312">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="3547b-313">Se invoca una función de filtro para todos los proveedores y categorías que no tienen reglas asignadas mediante configuración o código.</span><span class="sxs-lookup"><span data-stu-id="3547b-313">A filter function is invoked for all providers and categories that do not have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="3547b-314">El código de la función tiene acceso al tipo de proveedor, categoría y nivel de registro para decidir si se debe registrar un mensaje.</span><span class="sxs-lookup"><span data-stu-id="3547b-314">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="3547b-315">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="3547b-315">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3547b-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3547b-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3547b-317">Algunos proveedores de registro permiten especificar cuándo deben escribirse los registros en un medio de almacenamiento o ignorarse en función de la categoría y el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="3547b-317">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="3547b-318">Los métodos de extensión `AddConsole` y `AddDebug` proporcionan sobrecargas que le permiten pasar criterios de filtrado.</span><span class="sxs-lookup"><span data-stu-id="3547b-318">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="3547b-319">El código de ejemplo siguiente hace que el proveedor de la consola ignore los registros por debajo del nivel `Warning`, mientras que el proveedor de depuración omite los registros creados por la plataforma.</span><span class="sxs-lookup"><span data-stu-id="3547b-319">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="3547b-320">El método `AddEventLog` tiene una sobrecarga que toma una instancia de `EventLogSettings`, que puede contener una función de filtrado en su propiedad `Filter`.</span><span class="sxs-lookup"><span data-stu-id="3547b-320">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="3547b-321">El proveedor de TraceSource no proporciona ninguna de estas sobrecargas, dado que su nivel de registro y otros parámetros se basan en el `SourceSwitch` y `TraceListener` que usa.</span><span class="sxs-lookup"><span data-stu-id="3547b-321">The TraceSource provider does not provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="3547b-322">Puede establecer reglas de filtrado para todos los proveedores que están registrados con un instancia de `ILoggerFactory` mediante el método de extensión `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="3547b-322">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="3547b-323">En el ejemplo siguiente se limitan los registros de la plataforma (la categoría comienza con "Microsoft" o "System") a las advertencias, mientras se permite el registro de la aplicación en el nivel de depuración.</span><span class="sxs-lookup"><span data-stu-id="3547b-323">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="3547b-324">Si quiere usar el filtrado para impedir que se escriban todos los registros para una categoría concreta, puede especificar `LogLevel.None` como el nivel de registro mínimo de esa categoría.</span><span class="sxs-lookup"><span data-stu-id="3547b-324">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="3547b-325">El valor entero de `LogLevel.None` es 6, que es superior a `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="3547b-325">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="3547b-326">El paquete NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) proporciona el método de extensión `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="3547b-326">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="3547b-327">El método devuelve una instancia nueva de `ILoggerFactory` que filtrará los mensajes de registro pasados a todos los proveedores de registrador registrados con ella.</span><span class="sxs-lookup"><span data-stu-id="3547b-327">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="3547b-328">No afecta a ninguna otra instancia de `ILoggerFactory`, incluida la instancia de `ILoggerFactory` original.</span><span class="sxs-lookup"><span data-stu-id="3547b-328">It does not affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="3547b-329">Ámbitos de registro</span><span class="sxs-lookup"><span data-stu-id="3547b-329">Log scopes</span></span>

<span data-ttu-id="3547b-330">Puede agrupar un conjunto de operaciones lógicas dentro de un *ámbito* para adjuntar los mismos datos para cada registro que se crea como parte de ese conjunto.</span><span class="sxs-lookup"><span data-stu-id="3547b-330">You can group a set of logical operations within a *scope* in order to attach the same data to each log that is created as part of that set.</span></span> <span data-ttu-id="3547b-331">Por ejemplo, es posible que quiera que todos los registros creados como parte del procesamiento de una transacción incluyan el identificador de la transacción.</span><span class="sxs-lookup"><span data-stu-id="3547b-331">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="3547b-332">Un ámbito es un tipo `IDisposable` devuelto por el método `ILogger.BeginScope<TState>` y se conserva hasta que se elimina.</span><span class="sxs-lookup"><span data-stu-id="3547b-332">A scope is an `IDisposable` type that is returned by the `ILogger.BeginScope<TState>` method and lasts until it is disposed.</span></span> <span data-ttu-id="3547b-333">Para usar un ámbito, las llamadas de registrador se encapsulan en un bloque `using`, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="3547b-333">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="3547b-334">El código siguiente permite ámbitos para el proveedor de la consola:</span><span class="sxs-lookup"><span data-stu-id="3547b-334">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3547b-335">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3547b-335">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3547b-336">En *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="3547b-336">In *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="3547b-337">Es necesario configurar la opción del registrador de consola `IncludeScopes` para habilitar el registro basado en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="3547b-337">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span> <span data-ttu-id="3547b-338">La configuración de `IncludeScopes` con archivos de configuración *appsettings* estará disponible con la versión de ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="3547b-338">Configuration of `IncludeScopes` using *appsettings* configuration files will be available with the release of ASP.NET Core 2.1.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3547b-339">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3547b-339">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3547b-340">En *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3547b-340">In *Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="3547b-341">Cada mensaje de registro incluye la información de ámbito:</span><span class="sxs-lookup"><span data-stu-id="3547b-341">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="3547b-342">Proveedores de registro integrados</span><span class="sxs-lookup"><span data-stu-id="3547b-342">Built-in logging providers</span></span>

<span data-ttu-id="3547b-343">ASP.NET Core incluye los proveedores siguientes:</span><span class="sxs-lookup"><span data-stu-id="3547b-343">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="3547b-344">Consola</span><span class="sxs-lookup"><span data-stu-id="3547b-344">Console</span></span>](#console)
* [<span data-ttu-id="3547b-345">Depurar</span><span class="sxs-lookup"><span data-stu-id="3547b-345">Debug</span></span>](#debug)
* [<span data-ttu-id="3547b-346">EventSource</span><span class="sxs-lookup"><span data-stu-id="3547b-346">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="3547b-347">EventLog</span><span class="sxs-lookup"><span data-stu-id="3547b-347">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="3547b-348">TraceSource</span><span class="sxs-lookup"><span data-stu-id="3547b-348">TraceSource</span></span>](#tracesource)
* [<span data-ttu-id="3547b-349">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3547b-349">Azure App Service</span></span>](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="3547b-350">El proveedor de la consola</span><span class="sxs-lookup"><span data-stu-id="3547b-350">The console provider</span></span>

<span data-ttu-id="3547b-351">El paquete de proveedor [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envía la salida del registro a la consola.</span><span class="sxs-lookup"><span data-stu-id="3547b-351">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3547b-352">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3547b-352">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3547b-353">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3547b-353">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="3547b-354">[Las sobrecargas de AddConsole](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) permiten pasar un nivel de registro mínimo, una función de filtro y un valor booleano que indica si se admiten los ámbitos.</span><span class="sxs-lookup"><span data-stu-id="3547b-354">[AddConsole overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="3547b-355">Otra opción consiste en pasar un objeto `IConfiguration`, que puede especificar niveles de registro y si se admiten los ámbitos.</span><span class="sxs-lookup"><span data-stu-id="3547b-355">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="3547b-356">Si está pensando en la posibilidad de usar la consola de proveedor en producción, tenga en cuenta que tiene un impacto significativo en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="3547b-356">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="3547b-357">Cuando se crea un proyecto en Visual Studio, el método `AddConsole` tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="3547b-357">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="3547b-358">Este código hace referencia a la sección `Logging` del archivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3547b-358">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="3547b-359">La configuración que se muestra limita los registros de la plataforma a las advertencias mientras que permite a la aplicación registrar en el nivel de depuración, como se explica en la sección [Filtrado del registro](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="3547b-359">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="3547b-360">Para obtener más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="3547b-360">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="3547b-361">El proveedor de depuración</span><span class="sxs-lookup"><span data-stu-id="3547b-361">The Debug provider</span></span>

<span data-ttu-id="3547b-362">El paquete de proveedor [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) escribe la salida del registro mediante la clase [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) (llamadas a métodos `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="3547b-362">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="3547b-363">En Linux, este proveedor escribe registros en */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="3547b-363">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3547b-364">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3547b-364">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3547b-365">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3547b-365">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="3547b-366">[Las sobrecargas de AddDebug](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) permiten pasar un nivel mínimo de registro o una función de filtro.</span><span class="sxs-lookup"><span data-stu-id="3547b-366">[AddDebug overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="3547b-367">El proveedor EventSource</span><span class="sxs-lookup"><span data-stu-id="3547b-367">The EventSource provider</span></span>

<span data-ttu-id="3547b-368">Para las aplicaciones que tienen como destino ASP.NET Core 1.1.0 o superior, el paquete de proveedor [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) puede implementar el seguimiento de eventos.</span><span class="sxs-lookup"><span data-stu-id="3547b-368">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="3547b-369">En Windows, usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="3547b-369">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="3547b-370">Es un proveedor multiplataforma, pero todavía no hay herramientas de recopilación y visualización de eventos para Linux o macOS.</span><span class="sxs-lookup"><span data-stu-id="3547b-370">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3547b-371">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3547b-371">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3547b-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3547b-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="3547b-373">Una buena manera de recopilar y ver los registros es usar la [utilidad PerfView](https://www.microsoft.com/download/details.aspx?id=28567).</span><span class="sxs-lookup"><span data-stu-id="3547b-373">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="3547b-374">Hay otras herramientas para ver los registros ETW, pero PerfView proporciona la mejor experiencia para trabajar con los eventos ETW emitidos por ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3547b-374">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="3547b-375">Para configurar PerfView para la recopilación de eventos registrados por este proveedor, agregue la cadena `*Microsoft-Extensions-Logging` a la lista **Proveedores adicionales**.</span><span class="sxs-lookup"><span data-stu-id="3547b-375">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="3547b-376">(No olvide el asterisco al principio de la cadena).</span><span class="sxs-lookup"><span data-stu-id="3547b-376">(Don't miss the asterisk at the start of the string.)</span></span>

![Proveedores adicionales de Perfview](index/_static/perfview-additional-providers.png)

<span data-ttu-id="3547b-378">La captura de eventos en Nano Server requiere configuración adicional:</span><span class="sxs-lookup"><span data-stu-id="3547b-378">Capturing events on Nano Server requires some additional setup:</span></span>

* <span data-ttu-id="3547b-379">Conectar la comunicación remota de PowerShell a Nano Server:</span><span class="sxs-lookup"><span data-stu-id="3547b-379">Connect PowerShell remoting to the Nano Server:</span></span>

  ```powershell
  Enter-PSSession [name]
  ```

* <span data-ttu-id="3547b-380">Crear una sesión de ETW:</span><span class="sxs-lookup"><span data-stu-id="3547b-380">Create an ETW session:</span></span>

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* <span data-ttu-id="3547b-381">Agregar proveedores ETW para [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core y otros según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="3547b-381">Add ETW providers for [CLR](https://docs.microsoft.com/dotnet/framework/performance/clr-etw-providers), ASP.NET Core, and others as needed.</span></span> <span data-ttu-id="3547b-382">El GUID del proveedor de ASP.NET Core es `3ac73b97-af73-50e9-0822-5da4367920d0`.</span><span class="sxs-lookup"><span data-stu-id="3547b-382">The ASP.NET Core provider GUID is `3ac73b97-af73-50e9-0822-5da4367920d0`.</span></span> 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* <span data-ttu-id="3547b-383">Ejecute el sitio y realice las acciones para las que quiera obtener información de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="3547b-383">Run the site and do whatever actions you want tracing information for.</span></span>

* <span data-ttu-id="3547b-384">Detenga la sesión de seguimiento cuando haya terminado:</span><span class="sxs-lookup"><span data-stu-id="3547b-384">Stop the tracing session when you're finished:</span></span>

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

<span data-ttu-id="3547b-385">El archivo *C:\trace.etl* resultante se puede analizar con PerfView como en otras ediciones de Windows.</span><span class="sxs-lookup"><span data-stu-id="3547b-385">The resulting *C:\trace.etl* file can be analyzed with PerfView as on other editions of Windows.</span></span>

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="3547b-386">El proveedor EventLog de Windows</span><span class="sxs-lookup"><span data-stu-id="3547b-386">The Windows EventLog provider</span></span>

<span data-ttu-id="3547b-387">El paquete de proveedor [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envía la salida del registro al Registro de eventos de Windows.</span><span class="sxs-lookup"><span data-stu-id="3547b-387">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3547b-388">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3547b-388">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3547b-389">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3547b-389">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="3547b-390">[Las sobrecargas de AddEventLog](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) permiten pasar `EventLogSettings` o un nivel de registro mínimo.</span><span class="sxs-lookup"><span data-stu-id="3547b-390">[AddEventLog overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="3547b-391">El proveedor TraceSource</span><span class="sxs-lookup"><span data-stu-id="3547b-391">The TraceSource provider</span></span>

<span data-ttu-id="3547b-392">El paquete de proveedor [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa las bibliotecas y proveedores de [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="3547b-392">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3547b-393">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3547b-393">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3547b-394">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3547b-394">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="3547b-395">[Las sobrecargas de AddTraceSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) permiten pasar un modificador de origen y un agente de escucha de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="3547b-395">[AddTraceSource overloads](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="3547b-396">Para usar este proveedor, una aplicación debe ejecutarse en .NET Framework (en lugar de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="3547b-396">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="3547b-397">El proveedor permite enrutar mensajes a una variedad de [agentes de escucha](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), como [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) que se usa en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="3547b-397">The provider lets you route messages to a variety of [listeners](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](https://docs.microsoft.com/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="3547b-398">En el ejemplo siguiente se configura un proveedor `TraceSource` que registra mensajes `Warning` y superiores en la ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="3547b-398">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="3547b-399">El proveedor Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3547b-399">The Azure App Service provider</span></span>

<span data-ttu-id="3547b-400">El paquete de proveedor [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) escribe los registros en archivos de texto en el sistema de archivos de una aplicación de Azure App Service y en [Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) en una cuenta de Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3547b-400">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="3547b-401">El proveedor solo está disponible para las aplicaciones que tienen como destino ASP.NET Core 1.1.0 o superior.</span><span class="sxs-lookup"><span data-stu-id="3547b-401">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3547b-402">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3547b-402">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3547b-403">Si el destino es .NET Core, no tiene que instalar el paquete de proveedor ni llamar explícitamente a `AddAzureWebAppDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="3547b-403">If targeting .NET Core, you don't have to install the provider package or explicitly call `AddAzureWebAppDiagnostics`.</span></span> <span data-ttu-id="3547b-404">El proveedor está disponible automáticamente para la aplicación al implementarla en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="3547b-404">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

<span data-ttu-id="3547b-405">Si el destino es .NET Framework, agregue el paquete de proveedor al proyecto e invoque a `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="3547b-405">If targeting .NET Framework, add the provider package to your project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3547b-406">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3547b-406">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="3547b-407">Una sobrecarga de `AddAzureWebAppDiagnostics` permite pasar [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) con lo que se puede invalidar la configuración predeterminada, como la plantilla de salida del registro, el nombre de blob y el límite de tamaño de archivo.</span><span class="sxs-lookup"><span data-stu-id="3547b-407">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="3547b-408">(La *plantilla de salida* es una plantilla de mensaje que se aplica a todos los registros además de la que se proporciona cuando se llama a un método `ILogger`).</span><span class="sxs-lookup"><span data-stu-id="3547b-408">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

---

<span data-ttu-id="3547b-409">Cuando se implementa en una aplicación de App Service, la aplicación respeta la configuración de la sección [Registros de diagnóstico](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) de la página **App Service** de Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="3547b-409">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="3547b-410">Cuando se cambia esa configuración, los cambios surten efecto inmediatamente sin necesidad de reiniciar la aplicación ni de volver a implementar código en ella.</span><span class="sxs-lookup"><span data-stu-id="3547b-410">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Configuración de los registros de Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="3547b-412">La ubicación predeterminada de los archivos de registro es la carpeta *D:\\home\\LogFiles\\Application* y el nombre de archivo predeterminado es *diagnostics-aaaammdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="3547b-412">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="3547b-413">El límite de tamaño de archivo predeterminado es 10 MB, y el número máximo predeterminado de archivos que se conservan es 2.</span><span class="sxs-lookup"><span data-stu-id="3547b-413">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="3547b-414">El nombre de blob predeterminado es *{nombre-de-la-aplicación}{marca de tiempo}/aaaa/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="3547b-414">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="3547b-415">Para más información sobre el comportamiento predeterminado, vea [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span><span class="sxs-lookup"><span data-stu-id="3547b-415">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="3547b-416">El proveedor solo funciona cuando el proyecto se ejecuta en el entorno de Azure.</span><span class="sxs-lookup"><span data-stu-id="3547b-416">The provider only works when your project runs in the Azure environment.</span></span> <span data-ttu-id="3547b-417">No tiene ningún efecto cuando se ejecuta de manera local; no escribe en archivos locales ni en almacenamiento de desarrollo local para los blobs.</span><span class="sxs-lookup"><span data-stu-id="3547b-417">It has no effect when you run locally &mdash; it does not write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="3547b-418">Proveedores de registro de terceros</span><span class="sxs-lookup"><span data-stu-id="3547b-418">Third-party logging providers</span></span>

<span data-ttu-id="3547b-419">Estos son algunas plataformas de registro de terceros que funcionan con ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3547b-419">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="3547b-420">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging): proveedor para el servicio Elmah.Io</span><span class="sxs-lookup"><span data-stu-id="3547b-420">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="3547b-421">[JSNLog](http://jsnlog.com): registra excepciones de JavaScript y otros eventos del lado cliente en el registro del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="3547b-421">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="3547b-422">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging): proveedor para el servicio Loggr</span><span class="sxs-lookup"><span data-stu-id="3547b-422">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="3547b-423">[NLog](https://github.com/NLog/NLog.Extensions.Logging): proveedor para la biblioteca de NLog</span><span class="sxs-lookup"><span data-stu-id="3547b-423">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="3547b-424">[Serilog](https://github.com/serilog/serilog-extensions-logging): proveedor para la biblioteca de Serilog</span><span class="sxs-lookup"><span data-stu-id="3547b-424">[Serilog](https://github.com/serilog/serilog-extensions-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="3547b-425">Algunas plataformas de terceros pueden realizar el [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="3547b-425">Some third-party frameworks can do [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="3547b-426">El uso de una plataforma de terceros es similar al de uno de los proveedores integrados: se agrega un paquete NuGet al proyecto y se llama a un método de extensión de `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="3547b-426">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="3547b-427">Para más información, vea la documentación de cada plataforma.</span><span class="sxs-lookup"><span data-stu-id="3547b-427">For more information, see each framework's documentation.</span></span>

<span data-ttu-id="3547b-428">También puede crear sus propios proveedores personalizados, para admitir otras plataformas de registro o sus propios requisitos de registro.</span><span class="sxs-lookup"><span data-stu-id="3547b-428">You can create your own custom providers as well, to support other logging frameworks or your own logging requirements.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="3547b-429">Secuencias de registro de Azure</span><span class="sxs-lookup"><span data-stu-id="3547b-429">Azure log streaming</span></span>

<span data-ttu-id="3547b-430">Las secuencias de registro de Azure permiten ver la actividad de registro en tiempo real desde:</span><span class="sxs-lookup"><span data-stu-id="3547b-430">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="3547b-431">El servidor de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="3547b-431">The application server</span></span> 
* <span data-ttu-id="3547b-432">El servidor web</span><span class="sxs-lookup"><span data-stu-id="3547b-432">The web server</span></span>
* <span data-ttu-id="3547b-433">Error del seguimiento de solicitudes</span><span class="sxs-lookup"><span data-stu-id="3547b-433">Failed request tracing</span></span> 

<span data-ttu-id="3547b-434">Para configurar las secuencias de registro de Azure:</span><span class="sxs-lookup"><span data-stu-id="3547b-434">To configure Azure log streaming:</span></span> 

* <span data-ttu-id="3547b-435">Navegue hasta la página **Registros de diagnóstico** desde la página de portal de la aplicación</span><span class="sxs-lookup"><span data-stu-id="3547b-435">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="3547b-436">Establezca **Registro de la aplicación (sistema de archivos)** en Activado.</span><span class="sxs-lookup"><span data-stu-id="3547b-436">Set **Application Logging (Filesystem)** to on.</span></span> 

![Página de registros de diagnóstico de Azure Portal](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="3547b-438">Navegue hasta la página **Secuencias de registro** para ver los mensajes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3547b-438">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="3547b-439">Se registran por la aplicación a través de la interfaz `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="3547b-439">They are logged by application through the `ILogger` interface.</span></span> 

![Secuencias de registro de aplicación de Azure Portal](index/_static/azure-log-streaming.png)


## <a name="see-also"></a><span data-ttu-id="3547b-441">Vea también</span><span class="sxs-lookup"><span data-stu-id="3547b-441">See also</span></span>

[<span data-ttu-id="3547b-442">Registro de alto rendimiento con LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="3547b-442">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
