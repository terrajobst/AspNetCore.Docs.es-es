---
title: Registro en ASP.NET Core
author: ardalis
description: Obtenga información sobre la plataforma de registro de ASP.NET Core. Descubra los proveedores de registro integrados y obtenga más información sobre proveedores de terceros conocidos.
manager: wpickett
ms.author: tdykstra
ms.date: 12/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/logging/index
ms.openlocfilehash: 7e2a4657211b0142ec87fd792d013f7ef397de2b
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2018
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="dfc03-104">Registro en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dfc03-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="dfc03-105">Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="dfc03-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="dfc03-106">ASP.NET Core es compatible con una API de registro que funciona con una variedad de proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="dfc03-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="dfc03-107">Los proveedores integrados permiten enviar registros a uno o varios destinos, y se puede conectar una plataforma de registro de terceros.</span><span class="sxs-lookup"><span data-stu-id="dfc03-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="dfc03-108">En este artículo se muestra cómo usar las API y los proveedores de registro integrados en el código.</span><span class="sxs-lookup"><span data-stu-id="dfc03-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dfc03-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="dfc03-110">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dfc03-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dfc03-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="dfc03-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dfc03-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="how-to-create-logs"></a><span data-ttu-id="dfc03-113">Cómo crear registros</span><span class="sxs-lookup"><span data-stu-id="dfc03-113">How to create logs</span></span>

<span data-ttu-id="dfc03-114">Para crear registros, obtenga un objeto `ILogger` desde el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="dfc03-114">To create logs, get an `ILogger` object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="dfc03-115">Después, llame a los métodos de registro de ese objeto de registrador:</span><span class="sxs-lookup"><span data-stu-id="dfc03-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="dfc03-116">En este ejemplo se crean registros con la clase `TodoController` como *categoría*.</span><span class="sxs-lookup"><span data-stu-id="dfc03-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="dfc03-117">Las categorías se explican [más adelante en este artículo](#log-category).</span><span class="sxs-lookup"><span data-stu-id="dfc03-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="dfc03-118">ASP.NET Core no proporciona métodos de registrador asincrónicos porque el registro debe ser tan rápido que el costo de usarlos no vale la pena.</span><span class="sxs-lookup"><span data-stu-id="dfc03-118">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="dfc03-119">Si se encuentra en una situación en la que no sea así, considere la posibilidad de cambiar el modo de registro.</span><span class="sxs-lookup"><span data-stu-id="dfc03-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="dfc03-120">Si el almacén de datos es lento, escriba primero los mensajes de registro en un almacén rápido y, después, muévalos a un almacén de baja velocidad.</span><span class="sxs-lookup"><span data-stu-id="dfc03-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="dfc03-121">Por ejemplo, realice el registro en una cola de mensajes que otro proceso lea y conserve en almacenamiento lento.</span><span class="sxs-lookup"><span data-stu-id="dfc03-121">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="dfc03-122">Cómo agregar proveedores</span><span class="sxs-lookup"><span data-stu-id="dfc03-122">How to add providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dfc03-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="dfc03-124">Un proveedor de registro toma los mensajes que se crean con un objeto `ILogger` y los muestra o almacena.</span><span class="sxs-lookup"><span data-stu-id="dfc03-124">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="dfc03-125">Por ejemplo, el proveedor de la consola muestra mensajes en la consola y el proveedor de Azure App Service puede almacenarlos en Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="dfc03-125">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="dfc03-126">Para usar un proveedor, llame al método de extensión `Add<ProviderName>` del proveedor en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="dfc03-126">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="dfc03-127">La plantilla de proyecto predeterminada permite el registro con el método [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___):</span><span class="sxs-lookup"><span data-stu-id="dfc03-127">The default project template enables logging with the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dfc03-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="dfc03-129">Un proveedor de registro toma los mensajes que se crean con un objeto `ILogger` y los muestra o almacena.</span><span class="sxs-lookup"><span data-stu-id="dfc03-129">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="dfc03-130">Por ejemplo, el proveedor de la consola muestra mensajes en la consola y el proveedor de Azure App Service puede almacenarlos en Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="dfc03-130">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="dfc03-131">Para usar un proveedor, instale su paquete NuGet y llame al método de extensión del proveedor en una instancia de `ILoggerFactory`, como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="dfc03-131">To use a provider, install its NuGet package and call the provider's extension method on an instance of `ILoggerFactory`, as shown in the following example.</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="dfc03-132">La [inserción de dependencias](xref:fundamentals/dependency-injection) (DI) de ASP.NET Core proporciona la instancia de `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-132">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="dfc03-133">Los métodos de extensión `AddConsole` y `AddDebug` se definen en los paquetes [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) y [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="dfc03-133">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="dfc03-134">Cada método de extensión llama al método `ILoggerFactory.AddProvider`, pasando una instancia del proveedor.</span><span class="sxs-lookup"><span data-stu-id="dfc03-134">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="dfc03-135">En la aplicación de ejemplo de este artículo se agregan proveedores de registro al método `Configure` de la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-135">The sample application for this article adds logging providers in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="dfc03-136">Si quiere obtener la salida de registro de código que se ejecuta antes, agregue los proveedores de registro en el constructor de la clase `Startup` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="dfc03-136">If you want to get log output from code that executes earlier, add logging providers in the `Startup` class constructor instead.</span></span> 

---

<span data-ttu-id="dfc03-137">Puede encontrar información sobre cada [proveedor de registro integrado](#built-in-logging-providers) y vínculos a [proveedores de registro de terceros](#third-party-logging-providers) más adelante en el artículo.</span><span class="sxs-lookup"><span data-stu-id="dfc03-137">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="dfc03-138">Salida de registro de ejemplo</span><span class="sxs-lookup"><span data-stu-id="dfc03-138">Sample logging output</span></span>

<span data-ttu-id="dfc03-139">Con el código de ejemplo que se muestra en la sección anterior, verá los registros en la consola cuando se ejecute desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="dfc03-139">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="dfc03-140">Este es un ejemplo de salida de la consola:</span><span class="sxs-lookup"><span data-stu-id="dfc03-140">Here's an example of console output:</span></span>

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

<span data-ttu-id="dfc03-141">Estos registros se crearon yendo a `http://localhost:5000/api/todo/0`, lo que desencadena la ejecución de las dos llamadas a `ILogger` que se muestran en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="dfc03-141">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="dfc03-142">Este es un ejemplo de los mismos registros tal y como aparecen en la ventana de depuración cuando se ejecuta la aplicación de ejemplo en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="dfc03-142">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="dfc03-143">Los registros creados por las llamadas a `ILogger` que se muestran en la sección anterior comienzan por "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="dfc03-143">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="dfc03-144">Los registros que comienzan por categorías de "Microsoft" son de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dfc03-144">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="dfc03-145">El propio ASP.NET Core y el código de la aplicación usan la misma API y los mismos proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="dfc03-145">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="dfc03-146">En el resto de este artículo se explican algunos detalles y opciones para el registro.</span><span class="sxs-lookup"><span data-stu-id="dfc03-146">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="dfc03-147">Paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="dfc03-147">NuGet packages</span></span>

<span data-ttu-id="dfc03-148">Las interfaces `ILogger` e `ILoggerFactory` se encuentran en [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), y sus implementaciones predeterminadas en [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="dfc03-148">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="dfc03-149">Categoría de registro</span><span class="sxs-lookup"><span data-stu-id="dfc03-149">Log category</span></span>

<span data-ttu-id="dfc03-150">Con cada registro que cree se incluye una *categoría*.</span><span class="sxs-lookup"><span data-stu-id="dfc03-150">A *category* is included with each log that you create.</span></span> <span data-ttu-id="dfc03-151">La categoría se especifica cuando se crea un objeto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-151">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="dfc03-152">La categoría puede ser cualquier cadena, pero una convención consiste en usar el nombre completo de la clase desde la que se escriben los registros.</span><span class="sxs-lookup"><span data-stu-id="dfc03-152">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="dfc03-153">Por ejemplo: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="dfc03-153">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="dfc03-154">Puede especificar la categoría como una cadena o usar un método de extensión que derive la categoría del tipo.</span><span class="sxs-lookup"><span data-stu-id="dfc03-154">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="dfc03-155">Para especificar la categoría como una cadena, llame a `CreateLogger` en una instancia de `ILoggerFactory`, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="dfc03-155">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="dfc03-156">En la mayoría de los casos, será más fácil usar `ILogger<T>`, como en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="dfc03-156">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="dfc03-157">Esto equivale a llamar a `CreateLogger` con el nombre de tipo completo de `T`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-157">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="dfc03-158">Nivel de registro</span><span class="sxs-lookup"><span data-stu-id="dfc03-158">Log level</span></span>

<span data-ttu-id="dfc03-159">Cada vez que se escribe un registro, se especifica su [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="dfc03-159">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="dfc03-160">El nivel de registro indica el grado de gravedad o importancia.</span><span class="sxs-lookup"><span data-stu-id="dfc03-160">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="dfc03-161">Por ejemplo, es posible que escriba un registro `Information` cuando un método finaliza normalmente, un registro `Warning` cuando un método devuelve un código de retorno 404 y un registro `Error` cuando capture una excepción inesperada.</span><span class="sxs-lookup"><span data-stu-id="dfc03-161">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="dfc03-162">En el ejemplo de código siguiente, los nombres de los métodos (por ejemplo, `LogWarning`) especifican el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="dfc03-162">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="dfc03-163">El primer parámetro es el [Id. del evento del Registro](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="dfc03-163">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="dfc03-164">El segundo parámetro es una [plantilla de mensaje](#log-message-template) con marcadores de posición para los valores de argumento proporcionados por el resto de parámetros de método.</span><span class="sxs-lookup"><span data-stu-id="dfc03-164">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="dfc03-165">Los parámetros de método se explican detalladamente más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="dfc03-165">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="dfc03-166">Los métodos de registro que incluyen el nivel en el nombre del método son [métodos de extensión para ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="dfc03-166">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="dfc03-167">En segundo plano, estos métodos llaman a un método `Log` que toma un parámetro `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-167">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="dfc03-168">Puede llamar directamente al método `Log` en lugar de a uno de estos métodos de extensión, pero la sintaxis es relativamente complicada.</span><span class="sxs-lookup"><span data-stu-id="dfc03-168">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="dfc03-169">Para más información, vea la [interfaz ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) y el [código fuente de las extensiones de registrador](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="dfc03-169">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="dfc03-170">ASP.NET Core define los [niveles de registro](/dotnet/api/microsoft.extensions.logging.loglevel) siguientes, que aquí se ordenan de menor a mayor gravedad.</span><span class="sxs-lookup"><span data-stu-id="dfc03-170">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="dfc03-171">Seguimiento = 0</span><span class="sxs-lookup"><span data-stu-id="dfc03-171">Trace = 0</span></span>

  <span data-ttu-id="dfc03-172">Para la información que solo es útil para un desarrollador que depura un problema.</span><span class="sxs-lookup"><span data-stu-id="dfc03-172">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="dfc03-173">Estos mensajes pueden contener datos confidenciales de la aplicación, por lo que no deben habilitarse en un entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="dfc03-173">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="dfc03-174">*Deshabilitado de forma predeterminada.*</span><span class="sxs-lookup"><span data-stu-id="dfc03-174">*Disabled by default.*</span></span> <span data-ttu-id="dfc03-175">Ejemplo: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="dfc03-175">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="dfc03-176">Depurar = 1</span><span class="sxs-lookup"><span data-stu-id="dfc03-176">Debug = 1</span></span>

  <span data-ttu-id="dfc03-177">Para la información que tiene utilidad a corto plazo durante el desarrollo y la depuración.</span><span class="sxs-lookup"><span data-stu-id="dfc03-177">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="dfc03-178">Ejemplo: `Entering method Configure with flag set to true.` normalmente los registros de nivel `Debug` no se habilitarían en producción, a menos que se esté solucionando un problema, debido al elevado volumen de registros.</span><span class="sxs-lookup"><span data-stu-id="dfc03-178">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="dfc03-179">Información = 2</span><span class="sxs-lookup"><span data-stu-id="dfc03-179">Information = 2</span></span>

  <span data-ttu-id="dfc03-180">Para realizar el seguimiento del flujo general de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dfc03-180">For tracking the general flow of the application.</span></span> <span data-ttu-id="dfc03-181">Estos registros suelen tener algún valor a largo plazo.</span><span class="sxs-lookup"><span data-stu-id="dfc03-181">These logs typically have some long-term value.</span></span> <span data-ttu-id="dfc03-182">Ejemplo: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="dfc03-182">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="dfc03-183">Advertencia = 3</span><span class="sxs-lookup"><span data-stu-id="dfc03-183">Warning = 3</span></span>

  <span data-ttu-id="dfc03-184">Para los eventos anómalos o inesperados en el flujo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dfc03-184">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="dfc03-185">Estos pueden incluir errores u otras condiciones que no hacen que la aplicación se detenga, pero que puede que sea necesario investigar.</span><span class="sxs-lookup"><span data-stu-id="dfc03-185">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="dfc03-186">Las excepciones controladas son un lugar común para usar el nivel de registro `Warning`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-186">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="dfc03-187">Ejemplo: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="dfc03-187">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="dfc03-188">Error = 4</span><span class="sxs-lookup"><span data-stu-id="dfc03-188">Error = 4</span></span>

  <span data-ttu-id="dfc03-189">Para los errores y excepciones que no se pueden controlar.</span><span class="sxs-lookup"><span data-stu-id="dfc03-189">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="dfc03-190">Estos mensajes indican un error en la actividad u operación actual (por ejemplo, la solicitud HTTP actual), no un error de toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dfc03-190">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="dfc03-191">Mensaje de registro de ejemplo: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="dfc03-191">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="dfc03-192">Crítico = 5</span><span class="sxs-lookup"><span data-stu-id="dfc03-192">Critical = 5</span></span>

  <span data-ttu-id="dfc03-193">Para los errores que requieren atención inmediata.</span><span class="sxs-lookup"><span data-stu-id="dfc03-193">For failures that require immediate attention.</span></span> <span data-ttu-id="dfc03-194">Ejemplos: escenarios de pérdida de datos, espacio en disco insuficiente.</span><span class="sxs-lookup"><span data-stu-id="dfc03-194">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="dfc03-195">Puede usar el nivel de registro para controlar la cantidad de salida del registro que se escribe en un medio de almacenamiento determinado o ventana de presentación.</span><span class="sxs-lookup"><span data-stu-id="dfc03-195">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="dfc03-196">Por ejemplo, en producción es posible que quiera que todos los registros de nivel `Information` e inferiores vayan a un almacén de datos de volumen y que todos los registros de nivel `Warning` y superiores vayan un almacén de datos de valor.</span><span class="sxs-lookup"><span data-stu-id="dfc03-196">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="dfc03-197">Durante el desarrollo, es posible que normalmente envíe los registros de gravedad `Warning` o superior a la consola.</span><span class="sxs-lookup"><span data-stu-id="dfc03-197">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="dfc03-198">Después, si tiene que solucionar problemas, puede agregar el nivel `Debug`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-198">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="dfc03-199">En la sección [Filtrado del registro](#log-filtering) de este artículo se explica cómo controlar los niveles de registro que controla un proveedor.</span><span class="sxs-lookup"><span data-stu-id="dfc03-199">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="dfc03-200">La plataforma ASP.NET Core escribe registros de nivel `Debug` para los eventos de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="dfc03-200">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="dfc03-201">En los ejemplos de registro anteriores de este artículo se excluyeron los registros por debajo del nivel `Information`, por lo que no se mostraron los registros de nivel `Debug`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-201">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="dfc03-202">Este es un ejemplo de registros de consola si ejecuta la aplicación de ejemplo configurada para mostrar `Debug` y registros superiores para el proveedor de la consola.</span><span class="sxs-lookup"><span data-stu-id="dfc03-202">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="dfc03-203">Id. de evento del registro</span><span class="sxs-lookup"><span data-stu-id="dfc03-203">Log event ID</span></span>

<span data-ttu-id="dfc03-204">Cada vez que se escribe un registro, puede especificar un *Id. de evento*.</span><span class="sxs-lookup"><span data-stu-id="dfc03-204">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="dfc03-205">La aplicación de ejemplo lo hace mediante una clase `LoggingEvents` definida de forma local:</span><span class="sxs-lookup"><span data-stu-id="dfc03-205">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="dfc03-206">Un identificador de evento es un valor entero que se puede usar para asociar un conjunto de eventos registrados entre sí.</span><span class="sxs-lookup"><span data-stu-id="dfc03-206">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="dfc03-207">Por ejemplo, un registro para agregar un elemento a un carro de la compra podría tener el identificador de evento 1000 y un registro para completar una compra podría tener el identificador de evento 1001.</span><span class="sxs-lookup"><span data-stu-id="dfc03-207">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="dfc03-208">En la salida del registro, el identificador de evento podría estar almacenado en un campo o incluido en el mensaje de texto, en función del proveedor.</span><span class="sxs-lookup"><span data-stu-id="dfc03-208">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="dfc03-209">El proveedor de depuración no muestra los identificadores de evento, pero el proveedor de la consola los muestra entre paréntesis después de la categoría:</span><span class="sxs-lookup"><span data-stu-id="dfc03-209">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="dfc03-210">Plantilla de mensaje de registro</span><span class="sxs-lookup"><span data-stu-id="dfc03-210">Log message template</span></span>

<span data-ttu-id="dfc03-211">Cada vez que se escribe un mensaje de registro, se proporciona una plantilla de mensaje.</span><span class="sxs-lookup"><span data-stu-id="dfc03-211">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="dfc03-212">La plantilla de mensaje puede ser una cadena o puede contener marcadores de posición con nombre en los que se colocan los valores de argumento.</span><span class="sxs-lookup"><span data-stu-id="dfc03-212">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="dfc03-213">La plantilla no es una cadena de formato y los marcadores de posición deben tener un nombre, no un número.</span><span class="sxs-lookup"><span data-stu-id="dfc03-213">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="dfc03-214">El orden de los marcadores de posición, no sus nombres, determina qué parámetros se usan para proporcionar sus valores.</span><span class="sxs-lookup"><span data-stu-id="dfc03-214">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="dfc03-215">Si tiene el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="dfc03-215">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="dfc03-216">El mensaje del registro resultante tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="dfc03-216">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="dfc03-217">La plataforma de registro aplica este formato de mensajes para que los proveedores de registro puedan implementar el [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="dfc03-217">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="dfc03-218">Dado que los propios argumentos se pasan al sistema de registro, no solo la plantilla de mensaje con formato, los proveedores de registro pueden almacenar los valores de parámetros como campos además de la plantilla de mensaje.</span><span class="sxs-lookup"><span data-stu-id="dfc03-218">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="dfc03-219">Si va a dirigir la salida del registro a Azure Table Storage y la llamada al método de registrador tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="dfc03-219">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="dfc03-220">Cada entidad de la Tabla de Azure puede tener propiedades `ID` y `RequestTime`, lo que simplifica las consultas en los datos de registro.</span><span class="sxs-lookup"><span data-stu-id="dfc03-220">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="dfc03-221">Puede buscar todos los registros dentro de un intervalo `RequestTime` determinado sin necesidad de analizar el tiempo de espera del mensaje de texto.</span><span class="sxs-lookup"><span data-stu-id="dfc03-221">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="dfc03-222">Excepciones de registro</span><span class="sxs-lookup"><span data-stu-id="dfc03-222">Logging exceptions</span></span>

<span data-ttu-id="dfc03-223">Los métodos de registrador tienen sobrecargas que le permiten pasar una excepción, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="dfc03-223">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="dfc03-224">Cada proveedor controla la información de la excepción de maneras diferentes.</span><span class="sxs-lookup"><span data-stu-id="dfc03-224">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="dfc03-225">Este es un ejemplo de salida del proveedor de depuración del código mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="dfc03-225">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="dfc03-226">Filtrado del registro</span><span class="sxs-lookup"><span data-stu-id="dfc03-226">Log filtering</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dfc03-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="dfc03-228">Puede especificar un nivel de registro mínimo para un proveedor y una categoría específicos, o para todos los proveedores o todas las categorías.</span><span class="sxs-lookup"><span data-stu-id="dfc03-228">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="dfc03-229">Los registros por debajo del nivel mínimo no se pasan a ese proveedor, de modo que no se muestran o almacenan.</span><span class="sxs-lookup"><span data-stu-id="dfc03-229">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="dfc03-230">Si quiere suprimir todos los registros, puede especificar `LogLevel.None` como el nivel de registro mínimo.</span><span class="sxs-lookup"><span data-stu-id="dfc03-230">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="dfc03-231">El valor entero de `LogLevel.None` es 6, que es superior a `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="dfc03-231">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="dfc03-232">**Crear reglas de filtro en la configuración**</span><span class="sxs-lookup"><span data-stu-id="dfc03-232">**Create filter rules in configuration**</span></span>

<span data-ttu-id="dfc03-233">Las plantillas de proyecto crean código que llama a `CreateDefaultBuilder` para configurar el registro para los proveedores de consola y de depuración.</span><span class="sxs-lookup"><span data-stu-id="dfc03-233">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="dfc03-234">El método `CreateDefaultBuilder` también establece el registro para buscar la configuración en una sección `Logging`, mediante código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="dfc03-234">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="dfc03-235">Los datos de configuración especifican niveles de registro mínimo por proveedor y categoría, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="dfc03-235">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="dfc03-236">Este archivo JSON crea seis reglas de filtro, una para el proveedor de depuración, cuatro para el proveedor de la consola y una que se aplica a todos los proveedores.</span><span class="sxs-lookup"><span data-stu-id="dfc03-236">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="dfc03-237">Más adelante verá que solo una de estas reglas se elige para cada proveedor cuando se crea un objeto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-237">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="dfc03-238">**Reglas de filtro en el código**</span><span class="sxs-lookup"><span data-stu-id="dfc03-238">**Filter rules in code**</span></span>

<span data-ttu-id="dfc03-239">Puede registrar las reglas de filtro en el código, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="dfc03-239">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="dfc03-240">El segundo `AddFilter` especifica el proveedor de depuración mediante su nombre de tipo.</span><span class="sxs-lookup"><span data-stu-id="dfc03-240">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="dfc03-241">El primer `AddFilter` se aplica a todos los proveedores, dado que no especifica un tipo de proveedor.</span><span class="sxs-lookup"><span data-stu-id="dfc03-241">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="dfc03-242">**Cómo se aplican las reglas de filtro**</span><span class="sxs-lookup"><span data-stu-id="dfc03-242">**How filtering rules are applied**</span></span>

<span data-ttu-id="dfc03-243">Los datos de configuración y el código de `AddFilter` que se muestran en los ejemplos anteriores crean las reglas que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="dfc03-243">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="dfc03-244">Las seis primeras proceden del ejemplo de configuración y las dos últimas del ejemplo de código.</span><span class="sxs-lookup"><span data-stu-id="dfc03-244">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="dfc03-245">número</span><span class="sxs-lookup"><span data-stu-id="dfc03-245">Number</span></span> | <span data-ttu-id="dfc03-246">Proveedor</span><span class="sxs-lookup"><span data-stu-id="dfc03-246">Provider</span></span>      | <span data-ttu-id="dfc03-247">Categorías que comienzan por...</span><span class="sxs-lookup"><span data-stu-id="dfc03-247">Categories that begin with ...</span></span>          | <span data-ttu-id="dfc03-248">Nivel de registro mínimo</span><span class="sxs-lookup"><span data-stu-id="dfc03-248">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="dfc03-249">1</span><span class="sxs-lookup"><span data-stu-id="dfc03-249">1</span></span>      | <span data-ttu-id="dfc03-250">Depuración</span><span class="sxs-lookup"><span data-stu-id="dfc03-250">Debug</span></span>         | <span data-ttu-id="dfc03-251">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="dfc03-251">All categories</span></span>                          | <span data-ttu-id="dfc03-252">Información</span><span class="sxs-lookup"><span data-stu-id="dfc03-252">Information</span></span>       |
| <span data-ttu-id="dfc03-253">2</span><span class="sxs-lookup"><span data-stu-id="dfc03-253">2</span></span>      | <span data-ttu-id="dfc03-254">Consola</span><span class="sxs-lookup"><span data-stu-id="dfc03-254">Console</span></span>       | <span data-ttu-id="dfc03-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="dfc03-255">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="dfc03-256">Advertencia</span><span class="sxs-lookup"><span data-stu-id="dfc03-256">Warning</span></span>           |
| <span data-ttu-id="dfc03-257">3</span><span class="sxs-lookup"><span data-stu-id="dfc03-257">3</span></span>      | <span data-ttu-id="dfc03-258">Consola</span><span class="sxs-lookup"><span data-stu-id="dfc03-258">Console</span></span>       | <span data-ttu-id="dfc03-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="dfc03-259">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="dfc03-260">Depuración</span><span class="sxs-lookup"><span data-stu-id="dfc03-260">Debug</span></span>             |
| <span data-ttu-id="dfc03-261">4</span><span class="sxs-lookup"><span data-stu-id="dfc03-261">4</span></span>      | <span data-ttu-id="dfc03-262">Consola</span><span class="sxs-lookup"><span data-stu-id="dfc03-262">Console</span></span>       | <span data-ttu-id="dfc03-263">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="dfc03-263">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="dfc03-264">Error</span><span class="sxs-lookup"><span data-stu-id="dfc03-264">Error</span></span>             |
| <span data-ttu-id="dfc03-265">5</span><span class="sxs-lookup"><span data-stu-id="dfc03-265">5</span></span>      | <span data-ttu-id="dfc03-266">Consola</span><span class="sxs-lookup"><span data-stu-id="dfc03-266">Console</span></span>       | <span data-ttu-id="dfc03-267">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="dfc03-267">All categories</span></span>                          | <span data-ttu-id="dfc03-268">Información</span><span class="sxs-lookup"><span data-stu-id="dfc03-268">Information</span></span>       |
| <span data-ttu-id="dfc03-269">6</span><span class="sxs-lookup"><span data-stu-id="dfc03-269">6</span></span>      | <span data-ttu-id="dfc03-270">Todos los proveedores</span><span class="sxs-lookup"><span data-stu-id="dfc03-270">All providers</span></span> | <span data-ttu-id="dfc03-271">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="dfc03-271">All categories</span></span>                          | <span data-ttu-id="dfc03-272">Depuración</span><span class="sxs-lookup"><span data-stu-id="dfc03-272">Debug</span></span>             |
| <span data-ttu-id="dfc03-273">7</span><span class="sxs-lookup"><span data-stu-id="dfc03-273">7</span></span>      | <span data-ttu-id="dfc03-274">Todos los proveedores</span><span class="sxs-lookup"><span data-stu-id="dfc03-274">All providers</span></span> | <span data-ttu-id="dfc03-275">Sistema</span><span class="sxs-lookup"><span data-stu-id="dfc03-275">System</span></span>                                  | <span data-ttu-id="dfc03-276">Depuración</span><span class="sxs-lookup"><span data-stu-id="dfc03-276">Debug</span></span>             |
| <span data-ttu-id="dfc03-277">8</span><span class="sxs-lookup"><span data-stu-id="dfc03-277">8</span></span>      | <span data-ttu-id="dfc03-278">Depuración</span><span class="sxs-lookup"><span data-stu-id="dfc03-278">Debug</span></span>         | <span data-ttu-id="dfc03-279">Microsoft</span><span class="sxs-lookup"><span data-stu-id="dfc03-279">Microsoft</span></span>                               | <span data-ttu-id="dfc03-280">Seguimiento</span><span class="sxs-lookup"><span data-stu-id="dfc03-280">Trace</span></span>             |

<span data-ttu-id="dfc03-281">Cuando se crea un objeto `ILogger` con el que escribir los registros, el objeto `ILoggerFactory` selecciona una sola regla por proveedor para aplicar a ese registrador.</span><span class="sxs-lookup"><span data-stu-id="dfc03-281">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="dfc03-282">Todos los mensajes escritos por ese objeto `ILogger` se filtran según las reglas seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="dfc03-282">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="dfc03-283">De las reglas disponibles se selecciona la más específica posible para cada par de categoría y proveedor.</span><span class="sxs-lookup"><span data-stu-id="dfc03-283">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="dfc03-284">Cuando se crea un `ILogger` para una categoría determinada, se usa el algoritmo siguiente para cada proveedor:</span><span class="sxs-lookup"><span data-stu-id="dfc03-284">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="dfc03-285">Se seleccionan todas las reglas que coinciden con el proveedor o su alias.</span><span class="sxs-lookup"><span data-stu-id="dfc03-285">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="dfc03-286">Si no se encuentra ninguna, se seleccionan todas las reglas con un proveedor vacío.</span><span class="sxs-lookup"><span data-stu-id="dfc03-286">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="dfc03-287">Del resultado del paso anterior, se seleccionan las reglas con el prefijo de categoría coincidente más largo.</span><span class="sxs-lookup"><span data-stu-id="dfc03-287">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="dfc03-288">Si no se encuentra ninguna, se seleccionan todas las reglas que no especifican una categoría.</span><span class="sxs-lookup"><span data-stu-id="dfc03-288">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="dfc03-289">Si se seleccionan varias reglas, se toma la **última**.</span><span class="sxs-lookup"><span data-stu-id="dfc03-289">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="dfc03-290">Si no se selecciona ninguna regla, se usa `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-290">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="dfc03-291">Por ejemplo, supongamos que tiene la lista de reglas anterior y crea un objeto `ILogger` para la categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="dfc03-291">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="dfc03-292">Para el proveedor de depuración, se aplican las reglas 1, 6 y 8.</span><span class="sxs-lookup"><span data-stu-id="dfc03-292">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="dfc03-293">La regla 8 es la más específica, por lo que se selecciona.</span><span class="sxs-lookup"><span data-stu-id="dfc03-293">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="dfc03-294">Para el proveedor de la consola, se aplican las reglas 3, 4, 5 y 6.</span><span class="sxs-lookup"><span data-stu-id="dfc03-294">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="dfc03-295">La regla 3 es la más específica.</span><span class="sxs-lookup"><span data-stu-id="dfc03-295">Rule 3 is most specific.</span></span>

<span data-ttu-id="dfc03-296">Cuando crea registros con un `ILogger` para la categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", los registros de nivel `Trace` y superiores se dirigirán al proveedor de depuración y los registros de nivel `Debug` y superiores se dirigirán al proveedor de consola.</span><span class="sxs-lookup"><span data-stu-id="dfc03-296">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="dfc03-297">**Alias de proveedor**</span><span class="sxs-lookup"><span data-stu-id="dfc03-297">**Provider aliases**</span></span>

<span data-ttu-id="dfc03-298">Puede usar el nombre de tipo para especificar un proveedor en la configuración, pero cada proveedor define un *alias* más breve que es más fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="dfc03-298">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="dfc03-299">Para los proveedores integrados, use los alias siguientes:</span><span class="sxs-lookup"><span data-stu-id="dfc03-299">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="dfc03-300">Consola</span><span class="sxs-lookup"><span data-stu-id="dfc03-300">Console</span></span>
- <span data-ttu-id="dfc03-301">Depuración</span><span class="sxs-lookup"><span data-stu-id="dfc03-301">Debug</span></span>
- <span data-ttu-id="dfc03-302">EventLog</span><span class="sxs-lookup"><span data-stu-id="dfc03-302">EventLog</span></span>
- <span data-ttu-id="dfc03-303">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="dfc03-303">AzureAppServices</span></span>
- <span data-ttu-id="dfc03-304">TraceSource</span><span class="sxs-lookup"><span data-stu-id="dfc03-304">TraceSource</span></span>
- <span data-ttu-id="dfc03-305">EventSource</span><span class="sxs-lookup"><span data-stu-id="dfc03-305">EventSource</span></span>

<span data-ttu-id="dfc03-306">**Nivel mínimo predeterminado**</span><span class="sxs-lookup"><span data-stu-id="dfc03-306">**Default minimum level**</span></span>

<span data-ttu-id="dfc03-307">Hay una configuración de nivel mínimo que solo tiene efecto si no se aplica ninguna regla de configuración o código para un proveedor y una categoría determinados.</span><span class="sxs-lookup"><span data-stu-id="dfc03-307">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="dfc03-308">En el ejemplo siguiente se muestra cómo establecer el nivel mínimo:</span><span class="sxs-lookup"><span data-stu-id="dfc03-308">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="dfc03-309">Si no establece explícitamente el nivel mínimo, el valor predeterminado es `Information`, lo que significa que los registros `Trace` y `Debug` se omiten.</span><span class="sxs-lookup"><span data-stu-id="dfc03-309">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="dfc03-310">**Funciones de filtro**</span><span class="sxs-lookup"><span data-stu-id="dfc03-310">**Filter functions**</span></span>

<span data-ttu-id="dfc03-311">Puede escribir código en una función de filtro para aplicar las reglas de filtrado.</span><span class="sxs-lookup"><span data-stu-id="dfc03-311">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="dfc03-312">Se invoca una función de filtro para todos los proveedores y categorías que no tienen reglas asignadas mediante configuración o código.</span><span class="sxs-lookup"><span data-stu-id="dfc03-312">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="dfc03-313">El código de la función tiene acceso al tipo de proveedor, categoría y nivel de registro para decidir si se debe registrar un mensaje.</span><span class="sxs-lookup"><span data-stu-id="dfc03-313">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="dfc03-314">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="dfc03-314">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dfc03-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="dfc03-316">Algunos proveedores de registro permiten especificar cuándo deben escribirse los registros en un medio de almacenamiento o ignorarse en función de la categoría y el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="dfc03-316">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="dfc03-317">Los métodos de extensión `AddConsole` y `AddDebug` proporcionan sobrecargas que le permiten pasar criterios de filtrado.</span><span class="sxs-lookup"><span data-stu-id="dfc03-317">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="dfc03-318">El código de ejemplo siguiente hace que el proveedor de la consola ignore los registros por debajo del nivel `Warning`, mientras que el proveedor de depuración omite los registros creados por la plataforma.</span><span class="sxs-lookup"><span data-stu-id="dfc03-318">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="dfc03-319">El método `AddEventLog` tiene una sobrecarga que toma una instancia de `EventLogSettings`, que puede contener una función de filtrado en su propiedad `Filter`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-319">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="dfc03-320">El proveedor de TraceSource no proporciona ninguna de estas sobrecargas, dado que su nivel de registro y otros parámetros se basan en el `SourceSwitch` y `TraceListener` que usa.</span><span class="sxs-lookup"><span data-stu-id="dfc03-320">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="dfc03-321">Puede establecer reglas de filtrado para todos los proveedores que están registrados con un instancia de `ILoggerFactory` mediante el método de extensión `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-321">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="dfc03-322">En el ejemplo siguiente se limitan los registros de la plataforma (la categoría comienza con "Microsoft" o "System") a las advertencias, mientras se permite el registro de la aplicación en el nivel de depuración.</span><span class="sxs-lookup"><span data-stu-id="dfc03-322">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="dfc03-323">Si quiere usar el filtrado para impedir que se escriban todos los registros para una categoría concreta, puede especificar `LogLevel.None` como el nivel de registro mínimo de esa categoría.</span><span class="sxs-lookup"><span data-stu-id="dfc03-323">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="dfc03-324">El valor entero de `LogLevel.None` es 6, que es superior a `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="dfc03-324">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="dfc03-325">El paquete NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) proporciona el método de extensión `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-325">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="dfc03-326">El método devuelve una instancia nueva de `ILoggerFactory` que filtrará los mensajes de registro pasados a todos los proveedores de registrador registrados con ella.</span><span class="sxs-lookup"><span data-stu-id="dfc03-326">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="dfc03-327">No afecta a ninguna otra instancia de `ILoggerFactory`, incluida la instancia de `ILoggerFactory` original.</span><span class="sxs-lookup"><span data-stu-id="dfc03-327">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

---

## <a name="log-scopes"></a><span data-ttu-id="dfc03-328">Ámbitos de registro</span><span class="sxs-lookup"><span data-stu-id="dfc03-328">Log scopes</span></span>

<span data-ttu-id="dfc03-329">Puede agrupar un conjunto de operaciones lógicas dentro de un *ámbito* para adjuntar los mismos datos para cada registro que se crea como parte de ese conjunto.</span><span class="sxs-lookup"><span data-stu-id="dfc03-329">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="dfc03-330">Por ejemplo, es posible que quiera que todos los registros creados como parte del procesamiento de una transacción incluyan el identificador de la transacción.</span><span class="sxs-lookup"><span data-stu-id="dfc03-330">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="dfc03-331">Un ámbito es un tipo `IDisposable` devuelto por el método `ILogger.BeginScope<TState>` y se conserva hasta que se elimina.</span><span class="sxs-lookup"><span data-stu-id="dfc03-331">A scope is an `IDisposable` type that's returned by the `ILogger.BeginScope<TState>` method and lasts until it's disposed.</span></span> <span data-ttu-id="dfc03-332">Para usar un ámbito, las llamadas de registrador se encapsulan en un bloque `using`, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="dfc03-332">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="dfc03-333">El código siguiente permite ámbitos para el proveedor de la consola:</span><span class="sxs-lookup"><span data-stu-id="dfc03-333">The following code enables scopes for the console provider:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dfc03-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="dfc03-335">En *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="dfc03-335">In *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="dfc03-336">Es necesario configurar la opción del registrador de consola `IncludeScopes` para habilitar el registro basado en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="dfc03-336">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span> <span data-ttu-id="dfc03-337">La configuración de `IncludeScopes` con archivos de configuración *appsettings* estará disponible con la versión de ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="dfc03-337">Configuration of `IncludeScopes` using *appsettings* configuration files will be available with the release of ASP.NET Core 2.1.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dfc03-338">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-338">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="dfc03-339">En *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="dfc03-339">In *Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

<span data-ttu-id="dfc03-340">Cada mensaje de registro incluye la información de ámbito:</span><span class="sxs-lookup"><span data-stu-id="dfc03-340">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="dfc03-341">Proveedores de registro integrados</span><span class="sxs-lookup"><span data-stu-id="dfc03-341">Built-in logging providers</span></span>

<span data-ttu-id="dfc03-342">ASP.NET Core incluye los proveedores siguientes:</span><span class="sxs-lookup"><span data-stu-id="dfc03-342">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="dfc03-343">Consola</span><span class="sxs-lookup"><span data-stu-id="dfc03-343">Console</span></span>](#console)
* [<span data-ttu-id="dfc03-344">Depurar</span><span class="sxs-lookup"><span data-stu-id="dfc03-344">Debug</span></span>](#debug)
* [<span data-ttu-id="dfc03-345">EventSource</span><span class="sxs-lookup"><span data-stu-id="dfc03-345">EventSource</span></span>](#eventsource)
* [<span data-ttu-id="dfc03-346">EventLog</span><span class="sxs-lookup"><span data-stu-id="dfc03-346">EventLog</span></span>](#eventlog)
* [<span data-ttu-id="dfc03-347">TraceSource</span><span class="sxs-lookup"><span data-stu-id="dfc03-347">TraceSource</span></span>](#tracesource)
* [<span data-ttu-id="dfc03-348">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="dfc03-348">Azure App Service</span></span>](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a><span data-ttu-id="dfc03-349">El proveedor de la consola</span><span class="sxs-lookup"><span data-stu-id="dfc03-349">The console provider</span></span>

<span data-ttu-id="dfc03-350">El paquete de proveedor [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envía la salida del registro a la consola.</span><span class="sxs-lookup"><span data-stu-id="dfc03-350">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dfc03-351">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-351">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dfc03-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="dfc03-353">[Las sobrecargas de AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) permiten pasar un nivel de registro mínimo, una función de filtro y un valor booleano que indica si se admiten los ámbitos.</span><span class="sxs-lookup"><span data-stu-id="dfc03-353">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="dfc03-354">Otra opción consiste en pasar un objeto `IConfiguration`, que puede especificar niveles de registro y si se admiten los ámbitos.</span><span class="sxs-lookup"><span data-stu-id="dfc03-354">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="dfc03-355">Si está pensando en la posibilidad de usar la consola de proveedor en producción, tenga en cuenta que tiene un impacto significativo en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="dfc03-355">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="dfc03-356">Cuando se crea un proyecto en Visual Studio, el método `AddConsole` tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="dfc03-356">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="dfc03-357">Este código hace referencia a la sección `Logging` del archivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="dfc03-357">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="dfc03-358">La configuración que se muestra limita los registros de la plataforma a las advertencias mientras que permite a la aplicación registrar en el nivel de depuración, como se explica en la sección [Filtrado del registro](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="dfc03-358">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="dfc03-359">Para obtener más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="dfc03-359">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

---

<a id="debug"></a>
### <a name="the-debug-provider"></a><span data-ttu-id="dfc03-360">El proveedor de depuración</span><span class="sxs-lookup"><span data-stu-id="dfc03-360">The Debug provider</span></span>

<span data-ttu-id="dfc03-361">El paquete de proveedor [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) escribe la salida del registro mediante la clase [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (llamadas a métodos `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="dfc03-361">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="dfc03-362">En Linux, este proveedor escribe registros en */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="dfc03-362">On Linux, this provider writes logs to */var/log/message*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dfc03-363">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-363">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dfc03-364">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-364">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="dfc03-365">[Las sobrecargas de AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) permiten pasar un nivel mínimo de registro o una función de filtro.</span><span class="sxs-lookup"><span data-stu-id="dfc03-365">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a><span data-ttu-id="dfc03-366">El proveedor EventSource</span><span class="sxs-lookup"><span data-stu-id="dfc03-366">The EventSource provider</span></span>

<span data-ttu-id="dfc03-367">Para las aplicaciones que tienen como destino ASP.NET Core 1.1.0 o superior, el paquete de proveedor [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) puede implementar el seguimiento de eventos.</span><span class="sxs-lookup"><span data-stu-id="dfc03-367">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="dfc03-368">En Windows, usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="dfc03-368">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="dfc03-369">Es un proveedor multiplataforma, pero todavía no hay herramientas de recopilación y visualización de eventos para Linux o macOS.</span><span class="sxs-lookup"><span data-stu-id="dfc03-369">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dfc03-370">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dfc03-371">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

<span data-ttu-id="dfc03-372">Una buena manera de recopilar y ver los registros es usar la [utilidad PerfView](https://www.microsoft.com/download/details.aspx?id=28567).</span><span class="sxs-lookup"><span data-stu-id="dfc03-372">A good way to collect and view logs is to use the [PerfView utility](https://www.microsoft.com/download/details.aspx?id=28567).</span></span> <span data-ttu-id="dfc03-373">Hay otras herramientas para ver los registros ETW, pero PerfView proporciona la mejor experiencia para trabajar con los eventos ETW emitidos por ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="dfc03-373">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="dfc03-374">Para configurar PerfView para la recopilación de eventos registrados por este proveedor, agregue la cadena `*Microsoft-Extensions-Logging` a la lista **Proveedores adicionales**.</span><span class="sxs-lookup"><span data-stu-id="dfc03-374">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="dfc03-375">(No olvide el asterisco al principio de la cadena).</span><span class="sxs-lookup"><span data-stu-id="dfc03-375">(Don't miss the asterisk at the start of the string.)</span></span>

![Proveedores adicionales de Perfview](index/_static/perfview-additional-providers.png)

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a><span data-ttu-id="dfc03-377">El proveedor EventLog de Windows</span><span class="sxs-lookup"><span data-stu-id="dfc03-377">The Windows EventLog provider</span></span>

<span data-ttu-id="dfc03-378">El paquete de proveedor [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envía la salida del registro al Registro de eventos de Windows.</span><span class="sxs-lookup"><span data-stu-id="dfc03-378">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dfc03-379">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-379">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dfc03-380">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-380">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="dfc03-381">[Las sobrecargas de AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) permiten pasar `EventLogSettings` o un nivel de registro mínimo.</span><span class="sxs-lookup"><span data-stu-id="dfc03-381">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a><span data-ttu-id="dfc03-382">El proveedor TraceSource</span><span class="sxs-lookup"><span data-stu-id="dfc03-382">The TraceSource provider</span></span>

<span data-ttu-id="dfc03-383">El paquete de proveedor [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa las bibliotecas y proveedores de [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="dfc03-383">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dfc03-384">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-384">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dfc03-385">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-385">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

<span data-ttu-id="dfc03-386">[Las sobrecargas de AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) permiten pasar un modificador de origen y un agente de escucha de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="dfc03-386">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="dfc03-387">Para usar este proveedor, una aplicación debe ejecutarse en .NET Framework (en lugar de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="dfc03-387">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="dfc03-388">El proveedor permite enrutar mensajes a una variedad de [agentes de escucha](/dotnet/framework/debug-trace-profile/trace-listeners), como [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) que se usa en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="dfc03-388">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="dfc03-389">En el ejemplo siguiente se configura un proveedor `TraceSource` que registra mensajes `Warning` y superiores en la ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="dfc03-389">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a><span data-ttu-id="dfc03-390">El proveedor Azure App Service</span><span class="sxs-lookup"><span data-stu-id="dfc03-390">The Azure App Service provider</span></span>

<span data-ttu-id="dfc03-391">El paquete de proveedor [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) escribe los registros en archivos de texto en el sistema de archivos de una aplicación de Azure App Service y en [Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) en una cuenta de Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="dfc03-391">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="dfc03-392">El proveedor solo está disponible para las aplicaciones que tienen como destino ASP.NET Core 1.1.0 o superior.</span><span class="sxs-lookup"><span data-stu-id="dfc03-392">The provider is available only for apps that target ASP.NET Core 1.1.0 or higher.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="dfc03-393">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-393">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="dfc03-394">Si el destino es .NET Core, no tiene que instalar el paquete de proveedor ni llamar explícitamente a `AddAzureWebAppDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-394">If targeting .NET Core, you don't have to install the provider package or explicitly call `AddAzureWebAppDiagnostics`.</span></span> <span data-ttu-id="dfc03-395">El proveedor está disponible automáticamente para la aplicación al implementarla en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="dfc03-395">The provider is automatically available to your app when you deploy the app to Azure App Service.</span></span>

<span data-ttu-id="dfc03-396">Si el destino es .NET Framework, agregue el paquete de proveedor al proyecto e invoque a `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="dfc03-396">If targeting .NET Framework, add the provider package to your project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="dfc03-397">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="dfc03-397">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="dfc03-398">Una sobrecarga de `AddAzureWebAppDiagnostics` permite pasar [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) con lo que se puede invalidar la configuración predeterminada, como la plantilla de salida del registro, el nombre de blob y el límite de tamaño de archivo.</span><span class="sxs-lookup"><span data-stu-id="dfc03-398">An `AddAzureWebAppDiagnostics` overload lets you pass in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="dfc03-399">(La *plantilla de salida* es una plantilla de mensaje que se aplica a todos los registros además de la que se proporciona cuando se llama a un método `ILogger`).</span><span class="sxs-lookup"><span data-stu-id="dfc03-399">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

---

<span data-ttu-id="dfc03-400">Cuando se implementa en una aplicación de App Service, la aplicación respeta la configuración de la sección [Registros de diagnóstico](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) de la página **App Service** de Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="dfc03-400">When you deploy to an App Service app, your application honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="dfc03-401">Cuando se cambia esa configuración, los cambios surten efecto inmediatamente sin necesidad de reiniciar la aplicación ni de volver a implementar código en ella.</span><span class="sxs-lookup"><span data-stu-id="dfc03-401">When you change those settings, the changes take effect immediately without requiring that you restart the app or redeploy code to it.</span></span> 

![Configuración de los registros de Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="dfc03-403">La ubicación predeterminada de los archivos de registro es la carpeta *D:\\home\\LogFiles\\Application* y el nombre de archivo predeterminado es *diagnostics-aaaammdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="dfc03-403">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="dfc03-404">El límite de tamaño de archivo predeterminado es 10 MB, y el número máximo predeterminado de archivos que se conservan es 2.</span><span class="sxs-lookup"><span data-stu-id="dfc03-404">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="dfc03-405">El nombre de blob predeterminado es *{nombre-de-la-aplicación}{marca de tiempo}/aaaa/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="dfc03-405">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="dfc03-406">Para más información sobre el comportamiento predeterminado, vea [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span><span class="sxs-lookup"><span data-stu-id="dfc03-406">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).</span></span>

<span data-ttu-id="dfc03-407">El proveedor solo funciona cuando el proyecto se ejecuta en el entorno de Azure.</span><span class="sxs-lookup"><span data-stu-id="dfc03-407">The provider only works when your project runs in the Azure environment.</span></span> <span data-ttu-id="dfc03-408">No tiene ningún efecto cuando se ejecuta de manera local &mdash; no escribe en archivos locales ni en almacenamiento de desarrollo local para los blobs.</span><span class="sxs-lookup"><span data-stu-id="dfc03-408">It has no effect when you run locally &mdash; it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="dfc03-409">Proveedores de registro de terceros</span><span class="sxs-lookup"><span data-stu-id="dfc03-409">Third-party logging providers</span></span>

<span data-ttu-id="dfc03-410">Estos son algunas plataformas de registro de terceros que funcionan con ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="dfc03-410">Here are some third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="dfc03-411">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging): proveedor para el servicio Elmah.Io</span><span class="sxs-lookup"><span data-stu-id="dfc03-411">[elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging) - provider for the Elmah.Io service</span></span>

* <span data-ttu-id="dfc03-412">[JSNLog](http://jsnlog.com): registra excepciones de JavaScript y otros eventos del lado cliente en el registro del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="dfc03-412">[JSNLog](http://jsnlog.com) - logs JavaScript exceptions and other client-side events in your server-side log.</span></span>

* <span data-ttu-id="dfc03-413">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging): proveedor para el servicio Loggr</span><span class="sxs-lookup"><span data-stu-id="dfc03-413">[Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) - provider for the Loggr service</span></span>

* <span data-ttu-id="dfc03-414">[NLog](https://github.com/NLog/NLog.Extensions.Logging): proveedor para la biblioteca de NLog</span><span class="sxs-lookup"><span data-stu-id="dfc03-414">[NLog](https://github.com/NLog/NLog.Extensions.Logging) - provider for the NLog library</span></span>

* <span data-ttu-id="dfc03-415">[Serilog](https://github.com/serilog/serilog-extensions-logging): proveedor para la biblioteca de Serilog</span><span class="sxs-lookup"><span data-stu-id="dfc03-415">[Serilog](https://github.com/serilog/serilog-extensions-logging) - provider for the Serilog library</span></span>

<span data-ttu-id="dfc03-416">Algunas plataformas de terceros pueden realizar el [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="dfc03-416">Some third-party frameworks can do [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="dfc03-417">El uso de una plataforma de terceros es similar al de uno de los proveedores integrados: se agrega un paquete NuGet al proyecto y se llama a un método de extensión de `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-417">Using a third-party framework is similar to using one of the built-in providers: add a NuGet package to your project and call an extension method on `ILoggerFactory`.</span></span> <span data-ttu-id="dfc03-418">Para más información, vea la documentación de cada plataforma.</span><span class="sxs-lookup"><span data-stu-id="dfc03-418">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="dfc03-419">Secuencias de registro de Azure</span><span class="sxs-lookup"><span data-stu-id="dfc03-419">Azure log streaming</span></span>

<span data-ttu-id="dfc03-420">Las secuencias de registro de Azure permiten ver la actividad de registro en tiempo real desde:</span><span class="sxs-lookup"><span data-stu-id="dfc03-420">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="dfc03-421">El servidor de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="dfc03-421">The application server</span></span> 
* <span data-ttu-id="dfc03-422">El servidor web</span><span class="sxs-lookup"><span data-stu-id="dfc03-422">The web server</span></span>
* <span data-ttu-id="dfc03-423">Error del seguimiento de solicitudes</span><span class="sxs-lookup"><span data-stu-id="dfc03-423">Failed request tracing</span></span> 

<span data-ttu-id="dfc03-424">Para configurar las secuencias de registro de Azure:</span><span class="sxs-lookup"><span data-stu-id="dfc03-424">To configure Azure log streaming:</span></span> 

* <span data-ttu-id="dfc03-425">Navegue hasta la página **Registros de diagnóstico** desde la página de portal de la aplicación</span><span class="sxs-lookup"><span data-stu-id="dfc03-425">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="dfc03-426">Establezca **Registro de la aplicación (sistema de archivos)** en Activado.</span><span class="sxs-lookup"><span data-stu-id="dfc03-426">Set **Application Logging (Filesystem)** to on.</span></span> 

![Página de registros de diagnóstico de Azure Portal](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="dfc03-428">Navegue hasta la página **Secuencias de registro** para ver los mensajes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dfc03-428">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="dfc03-429">Se registran por la aplicación a través de la interfaz `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="dfc03-429">They're logged by application through the `ILogger` interface.</span></span> 

![Secuencias de registro de aplicación de Azure Portal](index/_static/azure-log-streaming.png)


## <a name="see-also"></a><span data-ttu-id="dfc03-431">Vea también</span><span class="sxs-lookup"><span data-stu-id="dfc03-431">See also</span></span>

[<span data-ttu-id="dfc03-432">Registro de alto rendimiento con LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="dfc03-432">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
