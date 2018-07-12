---
title: Registro en ASP.NET Core
author: ardalis
description: Obtenga información sobre la plataforma de registro de ASP.NET Core. Descubra los proveedores de registro integrados y obtenga más información sobre proveedores de terceros conocidos.
ms.author: tdykstra
ms.date: 12/15/2017
uid: fundamentals/logging/index
ms.openlocfilehash: dde01129bb7ea29544c4c416dfe9b5522a738d01
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938490"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="98710-104">Registro en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="98710-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="98710-105">Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="98710-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="98710-106">ASP.NET Core es compatible con una API de registro que funciona con una variedad de proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="98710-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="98710-107">Los proveedores integrados permiten enviar registros a uno o varios destinos, y se puede conectar una plataforma de registro de terceros.</span><span class="sxs-lookup"><span data-stu-id="98710-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="98710-108">En este artículo se muestra cómo usar las API y los proveedores de registro integrados en el código.</span><span class="sxs-lookup"><span data-stu-id="98710-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="98710-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="98710-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="98710-110">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="98710-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker-end

<span data-ttu-id="98710-111">Para obtener información sobre el registro de stdout al hospedar con IIS, consulte <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="98710-111">For information on stdout logging when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span></span> <span data-ttu-id="98710-112">Para obtener información sobre el registro de stdout con Azure App Service, consulte <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="98710-112">For information on stdout logging with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

## <a name="how-to-create-logs"></a><span data-ttu-id="98710-113">Cómo crear registros</span><span class="sxs-lookup"><span data-stu-id="98710-113">How to create logs</span></span>

<span data-ttu-id="98710-114">Para crear registros, implemente un objeto [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) desde el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="98710-114">To create logs, implement an [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="98710-115">Después, llame a los métodos de registro de ese objeto de registrador:</span><span class="sxs-lookup"><span data-stu-id="98710-115">Then call logging methods on that logger object:</span></span>

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="98710-116">En este ejemplo se crean registros con la clase `TodoController` como *categoría*.</span><span class="sxs-lookup"><span data-stu-id="98710-116">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="98710-117">Las categorías se explican [más adelante en este artículo](#log-category).</span><span class="sxs-lookup"><span data-stu-id="98710-117">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="98710-118">ASP.NET Core no proporciona métodos de registrador asincrónicos porque el registro debe ser tan rápido que el costo de usarlos no vale la pena.</span><span class="sxs-lookup"><span data-stu-id="98710-118">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="98710-119">Si se encuentra en una situación en la que no sea así, considere la posibilidad de cambiar el modo de registro.</span><span class="sxs-lookup"><span data-stu-id="98710-119">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="98710-120">Si el almacén de datos es lento, escriba primero los mensajes de registro en un almacén rápido y, después, muévalos a un almacén de baja velocidad.</span><span class="sxs-lookup"><span data-stu-id="98710-120">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="98710-121">Por ejemplo, realice el registro en una cola de mensajes que otro proceso lea y conserve en almacenamiento lento.</span><span class="sxs-lookup"><span data-stu-id="98710-121">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="98710-122">Cómo agregar proveedores</span><span class="sxs-lookup"><span data-stu-id="98710-122">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="98710-123">Un proveedor de registro toma los mensajes que se crean con un objeto `ILogger` y los muestra o almacena.</span><span class="sxs-lookup"><span data-stu-id="98710-123">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="98710-124">Por ejemplo, el proveedor de la consola muestra mensajes en la consola y el proveedor de Azure App Service puede almacenarlos en Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="98710-124">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="98710-125">Para usar un proveedor, llame al método de extensión `Add<ProviderName>` del proveedor en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="98710-125">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="98710-126">La plantilla de proyecto predeterminada permite el registro con el método [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___):</span><span class="sxs-lookup"><span data-stu-id="98710-126">The default project template enables logging with the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0#Microsoft_AspNetCore_WebHost_CreateDefaultBuilder_System_String___) method:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="98710-127">Un proveedor de registro toma los mensajes que se crean con un objeto `ILogger` y los muestra o almacena.</span><span class="sxs-lookup"><span data-stu-id="98710-127">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="98710-128">Por ejemplo, el proveedor de la consola muestra mensajes en la consola y el proveedor de Azure App Service puede almacenarlos en Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="98710-128">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="98710-129">Para usar un proveedor, instale su paquete NuGet y llame al método de extensión del proveedor en una instancia de [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="98710-129">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="98710-130">La [inserción de dependencias](xref:fundamentals/dependency-injection) (DI) de ASP.NET Core proporciona la instancia de `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="98710-130">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="98710-131">Los métodos de extensión `AddConsole` y `AddDebug` se definen en los paquetes [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) y [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="98710-131">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="98710-132">Cada método de extensión llama al método `ILoggerFactory.AddProvider`, pasando una instancia del proveedor.</span><span class="sxs-lookup"><span data-stu-id="98710-132">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span> 

> [!NOTE]
> <span data-ttu-id="98710-133">La [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) agrega proveedores de registro en el método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="98710-133">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="98710-134">Si quiere obtener la salida de registro de código que se ejecuta antes, agregue los proveedores de registro en el constructor de la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="98710-134">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="98710-135">Puede encontrar información sobre cada [proveedor de registro integrado](#built-in-logging-providers) y vínculos a [proveedores de registro de terceros](#third-party-logging-providers) más adelante en el artículo.</span><span class="sxs-lookup"><span data-stu-id="98710-135">You'll find information about each [built-in logging provider](#built-in-logging-providers) and links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="settings-file-configuration"></a><span data-ttu-id="98710-136">Configuración del archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="98710-136">Settings file configuration</span></span>

<span data-ttu-id="98710-137">Cada uno de los ejemplos anteriores de la sección [Cómo agregar proveedores](#how-to-add-providers) carga la configuración de proveedor de registro de la sección `Logging` de los archivos de configuración de aplicación.</span><span class="sxs-lookup"><span data-stu-id="98710-137">Each of the preceding examples in the [How to add providers](#how-to-add-providers) section loads logging provider configuration from the `Logging` section of app settings files.</span></span> <span data-ttu-id="98710-138">En el ejemplo siguiente se muestra el contenido de un archivo *appsettings.Development.json* típico:</span><span class="sxs-lookup"><span data-stu-id="98710-138">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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
      "IncludeScopes": "true"
    }
  }
}
```

<span data-ttu-id="98710-139">Las claves `LogLevel` representan los nombres de registro.</span><span class="sxs-lookup"><span data-stu-id="98710-139">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="98710-140">La clave `Default` se aplica a los registros que no se enumeran de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="98710-140">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="98710-141">El valor representa el [nivel de registro](#log-level) aplicado al registro determinado.</span><span class="sxs-lookup"><span data-stu-id="98710-141">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="98710-142">Las claves de registro que establecen `IncludeScopes` (`Console` en el ejemplo), especifican si los [ámbitos de registro](#log-scopes) están habilitados para el registro indicado.</span><span class="sxs-lookup"><span data-stu-id="98710-142">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

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

<span data-ttu-id="98710-143">Las claves `LogLevel` representan los nombres de registro.</span><span class="sxs-lookup"><span data-stu-id="98710-143">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="98710-144">La clave `Default` se aplica a los registros que no se enumeran de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="98710-144">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="98710-145">El valor representa el [nivel de registro](#log-level) aplicado al registro determinado.</span><span class="sxs-lookup"><span data-stu-id="98710-145">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

## <a name="sample-logging-output"></a><span data-ttu-id="98710-146">Salida de registro de ejemplo</span><span class="sxs-lookup"><span data-stu-id="98710-146">Sample logging output</span></span>

<span data-ttu-id="98710-147">Con el código de ejemplo que se muestra en la sección anterior, verá los registros en la consola cuando se ejecute desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="98710-147">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="98710-148">Este es un ejemplo de salida de la consola:</span><span class="sxs-lookup"><span data-stu-id="98710-148">Here's an example of console output:</span></span>

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

<span data-ttu-id="98710-149">Estos registros se crearon yendo a `http://localhost:5000/api/todo/0`, lo que desencadena la ejecución de las dos llamadas a `ILogger` que se muestran en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="98710-149">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="98710-150">Este es un ejemplo de los mismos registros tal y como aparecen en la ventana de depuración cuando se ejecuta la aplicación de ejemplo en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="98710-150">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

<span data-ttu-id="98710-151">Los registros creados por las llamadas a `ILogger` que se muestran en la sección anterior comienzan por "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="98710-151">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="98710-152">Los registros que comienzan por categorías de "Microsoft" son de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="98710-152">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="98710-153">El propio ASP.NET Core y el código de la aplicación usan la misma API y los mismos proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="98710-153">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="98710-154">En el resto de este artículo se explican algunos detalles y opciones para el registro.</span><span class="sxs-lookup"><span data-stu-id="98710-154">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="98710-155">Paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="98710-155">NuGet packages</span></span>

<span data-ttu-id="98710-156">Las interfaces `ILogger` e `ILoggerFactory` se encuentran en [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), y sus implementaciones predeterminadas en [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="98710-156">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="98710-157">Categoría de registro</span><span class="sxs-lookup"><span data-stu-id="98710-157">Log category</span></span>

<span data-ttu-id="98710-158">Con cada registro que cree se incluye una *categoría*.</span><span class="sxs-lookup"><span data-stu-id="98710-158">A *category* is included with each log that you create.</span></span> <span data-ttu-id="98710-159">La categoría se especifica cuando se crea un objeto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="98710-159">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="98710-160">La categoría puede ser cualquier cadena, pero una convención consiste en usar el nombre completo de la clase desde la que se escriben los registros.</span><span class="sxs-lookup"><span data-stu-id="98710-160">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="98710-161">Por ejemplo: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="98710-161">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="98710-162">Puede especificar la categoría como una cadena o usar un método de extensión que derive la categoría del tipo.</span><span class="sxs-lookup"><span data-stu-id="98710-162">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="98710-163">Para especificar la categoría como una cadena, llame a `CreateLogger` en una instancia de `ILoggerFactory`, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="98710-163">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

<span data-ttu-id="98710-164">En la mayoría de los casos, será más fácil usar `ILogger<T>`, como en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="98710-164">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

<span data-ttu-id="98710-165">Esto equivale a llamar a `CreateLogger` con el nombre de tipo completo de `T`.</span><span class="sxs-lookup"><span data-stu-id="98710-165">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="98710-166">Nivel de registro</span><span class="sxs-lookup"><span data-stu-id="98710-166">Log level</span></span>

<span data-ttu-id="98710-167">Cada vez que se escribe un registro, se especifica su [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="98710-167">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="98710-168">El nivel de registro indica el grado de gravedad o importancia.</span><span class="sxs-lookup"><span data-stu-id="98710-168">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="98710-169">Por ejemplo, es posible que escriba un registro `Information` cuando un método finaliza normalmente, un registro `Warning` cuando un método devuelve un código de retorno 404 y un registro `Error` cuando capture una excepción inesperada.</span><span class="sxs-lookup"><span data-stu-id="98710-169">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="98710-170">En el ejemplo de código siguiente, los nombres de los métodos (por ejemplo, `LogWarning`) especifican el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="98710-170">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="98710-171">El primer parámetro es el [Id. del evento del Registro](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="98710-171">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="98710-172">El segundo parámetro es una [plantilla de mensaje](#log-message-template) con marcadores de posición para los valores de argumento proporcionados por el resto de parámetros de método.</span><span class="sxs-lookup"><span data-stu-id="98710-172">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="98710-173">Los parámetros de método se explican detalladamente más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="98710-173">The method parameters are explained in more detail later in this article.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="98710-174">Los métodos de registro que incluyen el nivel en el nombre del método son [métodos de extensión para ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="98710-174">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="98710-175">En segundo plano, estos métodos llaman a un método `Log` que toma un parámetro `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="98710-175">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="98710-176">Puede llamar directamente al método `Log` en lugar de a uno de estos métodos de extensión, pero la sintaxis es relativamente complicada.</span><span class="sxs-lookup"><span data-stu-id="98710-176">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="98710-177">Para más información, vea la [interfaz ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) y el [código fuente de las extensiones de registrador](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="98710-177">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="98710-178">ASP.NET Core define los [niveles de registro](/dotnet/api/microsoft.extensions.logging.loglevel) siguientes, que aquí se ordenan de menor a mayor gravedad.</span><span class="sxs-lookup"><span data-stu-id="98710-178">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="98710-179">Seguimiento = 0</span><span class="sxs-lookup"><span data-stu-id="98710-179">Trace = 0</span></span>

  <span data-ttu-id="98710-180">Para la información que solo es útil para un desarrollador que depura un problema.</span><span class="sxs-lookup"><span data-stu-id="98710-180">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="98710-181">Estos mensajes pueden contener datos confidenciales de la aplicación, por lo que no deben habilitarse en un entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="98710-181">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="98710-182">*Deshabilitado de forma predeterminada.*</span><span class="sxs-lookup"><span data-stu-id="98710-182">*Disabled by default.*</span></span> <span data-ttu-id="98710-183">Ejemplo: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="98710-183">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="98710-184">Depurar = 1</span><span class="sxs-lookup"><span data-stu-id="98710-184">Debug = 1</span></span>

  <span data-ttu-id="98710-185">Para la información que tiene utilidad a corto plazo durante el desarrollo y la depuración.</span><span class="sxs-lookup"><span data-stu-id="98710-185">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="98710-186">Ejemplo: `Entering method Configure with flag set to true.` normalmente los registros de nivel `Debug` no se habilitarían en producción, a menos que se esté solucionando un problema, debido al elevado volumen de registros.</span><span class="sxs-lookup"><span data-stu-id="98710-186">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="98710-187">Información = 2</span><span class="sxs-lookup"><span data-stu-id="98710-187">Information = 2</span></span>

  <span data-ttu-id="98710-188">Para realizar el seguimiento del flujo general de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="98710-188">For tracking the general flow of the application.</span></span> <span data-ttu-id="98710-189">Estos registros suelen tener algún valor a largo plazo.</span><span class="sxs-lookup"><span data-stu-id="98710-189">These logs typically have some long-term value.</span></span> <span data-ttu-id="98710-190">Ejemplo: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="98710-190">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="98710-191">Advertencia = 3</span><span class="sxs-lookup"><span data-stu-id="98710-191">Warning = 3</span></span>

  <span data-ttu-id="98710-192">Para los eventos anómalos o inesperados en el flujo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="98710-192">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="98710-193">Estos pueden incluir errores u otras condiciones que no hacen que la aplicación se detenga, pero que puede que sea necesario investigar.</span><span class="sxs-lookup"><span data-stu-id="98710-193">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="98710-194">Las excepciones controladas son un lugar común para usar el nivel de registro `Warning`.</span><span class="sxs-lookup"><span data-stu-id="98710-194">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="98710-195">Ejemplo: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="98710-195">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="98710-196">Error = 4</span><span class="sxs-lookup"><span data-stu-id="98710-196">Error = 4</span></span>

  <span data-ttu-id="98710-197">Para los errores y excepciones que no se pueden controlar.</span><span class="sxs-lookup"><span data-stu-id="98710-197">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="98710-198">Estos mensajes indican un error en la actividad u operación actual (por ejemplo, la solicitud HTTP actual), no un error de toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="98710-198">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="98710-199">Mensaje de registro de ejemplo: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="98710-199">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="98710-200">Crítico = 5</span><span class="sxs-lookup"><span data-stu-id="98710-200">Critical = 5</span></span>

  <span data-ttu-id="98710-201">Para los errores que requieren atención inmediata.</span><span class="sxs-lookup"><span data-stu-id="98710-201">For failures that require immediate attention.</span></span> <span data-ttu-id="98710-202">Ejemplos: escenarios de pérdida de datos, espacio en disco insuficiente.</span><span class="sxs-lookup"><span data-stu-id="98710-202">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="98710-203">Puede usar el nivel de registro para controlar la cantidad de salida del registro que se escribe en un medio de almacenamiento determinado o ventana de presentación.</span><span class="sxs-lookup"><span data-stu-id="98710-203">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="98710-204">Por ejemplo, en producción es posible que quiera que todos los registros de nivel `Information` e inferiores vayan a un almacén de datos de volumen y que todos los registros de nivel `Warning` y superiores vayan un almacén de datos de valor.</span><span class="sxs-lookup"><span data-stu-id="98710-204">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="98710-205">Durante el desarrollo, es posible que normalmente envíe los registros de gravedad `Warning` o superior a la consola.</span><span class="sxs-lookup"><span data-stu-id="98710-205">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="98710-206">Después, si tiene que solucionar problemas, puede agregar el nivel `Debug`.</span><span class="sxs-lookup"><span data-stu-id="98710-206">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="98710-207">En la sección [Filtrado del registro](#log-filtering) de este artículo se explica cómo controlar los niveles de registro que controla un proveedor.</span><span class="sxs-lookup"><span data-stu-id="98710-207">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="98710-208">La plataforma ASP.NET Core escribe registros de nivel `Debug` para los eventos de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="98710-208">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="98710-209">En los ejemplos de registro anteriores de este artículo se excluyeron los registros por debajo del nivel `Information`, por lo que no se mostraron los registros de nivel `Debug`.</span><span class="sxs-lookup"><span data-stu-id="98710-209">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="98710-210">Este es un ejemplo de registros de consola si ejecuta la aplicación de ejemplo configurada para mostrar `Debug` y registros superiores para el proveedor de la consola.</span><span class="sxs-lookup"><span data-stu-id="98710-210">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="98710-211">Id. de evento del registro</span><span class="sxs-lookup"><span data-stu-id="98710-211">Log event ID</span></span>

<span data-ttu-id="98710-212">Cada vez que se escribe un registro, puede especificar un *Id. de evento*.</span><span class="sxs-lookup"><span data-stu-id="98710-212">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="98710-213">La aplicación de ejemplo lo hace mediante una clase `LoggingEvents` definida de forma local:</span><span class="sxs-lookup"><span data-stu-id="98710-213">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

<span data-ttu-id="98710-214">Un identificador de evento es un valor entero que se puede usar para asociar un conjunto de eventos registrados entre sí.</span><span class="sxs-lookup"><span data-stu-id="98710-214">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="98710-215">Por ejemplo, un registro para agregar un elemento a un carro de la compra podría tener el identificador de evento 1000 y un registro para completar una compra podría tener el identificador de evento 1001.</span><span class="sxs-lookup"><span data-stu-id="98710-215">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="98710-216">En la salida del registro, el identificador de evento podría estar almacenado en un campo o incluido en el mensaje de texto, en función del proveedor.</span><span class="sxs-lookup"><span data-stu-id="98710-216">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="98710-217">El proveedor de depuración no muestra los identificadores de evento, pero el proveedor de la consola los muestra entre paréntesis después de la categoría:</span><span class="sxs-lookup"><span data-stu-id="98710-217">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="98710-218">Plantilla de mensaje de registro</span><span class="sxs-lookup"><span data-stu-id="98710-218">Log message template</span></span>

<span data-ttu-id="98710-219">Cada vez que se escribe un mensaje de registro, se proporciona una plantilla de mensaje.</span><span class="sxs-lookup"><span data-stu-id="98710-219">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="98710-220">La plantilla de mensaje puede ser una cadena o puede contener marcadores de posición con nombre en los que se colocan los valores de argumento.</span><span class="sxs-lookup"><span data-stu-id="98710-220">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="98710-221">La plantilla no es una cadena de formato y los marcadores de posición deben tener un nombre, no un número.</span><span class="sxs-lookup"><span data-stu-id="98710-221">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

<span data-ttu-id="98710-222">El orden de los marcadores de posición, no sus nombres, determina qué parámetros se usan para proporcionar sus valores.</span><span class="sxs-lookup"><span data-stu-id="98710-222">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="98710-223">Si tiene el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="98710-223">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="98710-224">El mensaje del registro resultante tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="98710-224">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="98710-225">La plataforma de registro aplica este formato de mensajes para que los proveedores de registro puedan implementar el [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="98710-225">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="98710-226">Dado que los propios argumentos se pasan al sistema de registro, no solo la plantilla de mensaje con formato, los proveedores de registro pueden almacenar los valores de parámetros como campos además de la plantilla de mensaje.</span><span class="sxs-lookup"><span data-stu-id="98710-226">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="98710-227">Si va a dirigir la salida del registro a Azure Table Storage y la llamada al método de registrador tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="98710-227">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="98710-228">Cada entidad de la Tabla de Azure puede tener propiedades `ID` y `RequestTime`, lo que simplifica las consultas en los datos de registro.</span><span class="sxs-lookup"><span data-stu-id="98710-228">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="98710-229">Puede buscar todos los registros dentro de un intervalo `RequestTime` determinado sin necesidad de analizar el tiempo de espera del mensaje de texto.</span><span class="sxs-lookup"><span data-stu-id="98710-229">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="98710-230">Excepciones de registro</span><span class="sxs-lookup"><span data-stu-id="98710-230">Logging exceptions</span></span>

<span data-ttu-id="98710-231">Los métodos de registrador tienen sobrecargas que le permiten pasar una excepción, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="98710-231">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

<span data-ttu-id="98710-232">Cada proveedor controla la información de la excepción de maneras diferentes.</span><span class="sxs-lookup"><span data-stu-id="98710-232">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="98710-233">Este es un ejemplo de salida del proveedor de depuración del código mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="98710-233">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="98710-234">Filtrado del registro</span><span class="sxs-lookup"><span data-stu-id="98710-234">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="98710-235">Puede especificar un nivel de registro mínimo para un proveedor y una categoría específicos, o para todos los proveedores o todas las categorías.</span><span class="sxs-lookup"><span data-stu-id="98710-235">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="98710-236">Los registros por debajo del nivel mínimo no se pasan a ese proveedor, de modo que no se muestran o almacenan.</span><span class="sxs-lookup"><span data-stu-id="98710-236">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span> 

<span data-ttu-id="98710-237">Si quiere suprimir todos los registros, puede especificar `LogLevel.None` como el nivel de registro mínimo.</span><span class="sxs-lookup"><span data-stu-id="98710-237">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="98710-238">El valor entero de `LogLevel.None` es 6, que es superior a `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="98710-238">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="98710-239">**Crear reglas de filtro en la configuración**</span><span class="sxs-lookup"><span data-stu-id="98710-239">**Create filter rules in configuration**</span></span>

<span data-ttu-id="98710-240">Las plantillas de proyecto crean código que llama a `CreateDefaultBuilder` para configurar el registro para los proveedores de consola y de depuración.</span><span class="sxs-lookup"><span data-stu-id="98710-240">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="98710-241">El método `CreateDefaultBuilder` también establece el registro para buscar la configuración en una sección `Logging`, mediante código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="98710-241">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="98710-242">Los datos de configuración especifican niveles de registro mínimo por proveedor y categoría, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="98710-242">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/sample2/appsettings.json)]

<span data-ttu-id="98710-243">Este archivo JSON crea seis reglas de filtro, una para el proveedor de depuración, cuatro para el proveedor de la consola y una que se aplica a todos los proveedores.</span><span class="sxs-lookup"><span data-stu-id="98710-243">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="98710-244">Más adelante verá que solo una de estas reglas se elige para cada proveedor cuando se crea un objeto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="98710-244">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

<span data-ttu-id="98710-245">**Reglas de filtro en el código**</span><span class="sxs-lookup"><span data-stu-id="98710-245">**Filter rules in code**</span></span>

<span data-ttu-id="98710-246">Puede registrar las reglas de filtro en el código, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="98710-246">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="98710-247">El segundo `AddFilter` especifica el proveedor de depuración mediante su nombre de tipo.</span><span class="sxs-lookup"><span data-stu-id="98710-247">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="98710-248">El primer `AddFilter` se aplica a todos los proveedores, dado que no especifica un tipo de proveedor.</span><span class="sxs-lookup"><span data-stu-id="98710-248">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

<span data-ttu-id="98710-249">**Cómo se aplican las reglas de filtro**</span><span class="sxs-lookup"><span data-stu-id="98710-249">**How filtering rules are applied**</span></span>

<span data-ttu-id="98710-250">Los datos de configuración y el código de `AddFilter` que se muestran en los ejemplos anteriores crean las reglas que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="98710-250">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="98710-251">Las seis primeras proceden del ejemplo de configuración y las dos últimas del ejemplo de código.</span><span class="sxs-lookup"><span data-stu-id="98710-251">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="98710-252">número</span><span class="sxs-lookup"><span data-stu-id="98710-252">Number</span></span> | <span data-ttu-id="98710-253">Proveedor</span><span class="sxs-lookup"><span data-stu-id="98710-253">Provider</span></span>      | <span data-ttu-id="98710-254">Categorías que comienzan por...</span><span class="sxs-lookup"><span data-stu-id="98710-254">Categories that begin with ...</span></span>          | <span data-ttu-id="98710-255">Nivel de registro mínimo</span><span class="sxs-lookup"><span data-stu-id="98710-255">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="98710-256">1</span><span class="sxs-lookup"><span data-stu-id="98710-256">1</span></span>      | <span data-ttu-id="98710-257">Depuración</span><span class="sxs-lookup"><span data-stu-id="98710-257">Debug</span></span>         | <span data-ttu-id="98710-258">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="98710-258">All categories</span></span>                          | <span data-ttu-id="98710-259">Información</span><span class="sxs-lookup"><span data-stu-id="98710-259">Information</span></span>       |
| <span data-ttu-id="98710-260">2</span><span class="sxs-lookup"><span data-stu-id="98710-260">2</span></span>      | <span data-ttu-id="98710-261">Consola</span><span class="sxs-lookup"><span data-stu-id="98710-261">Console</span></span>       | <span data-ttu-id="98710-262">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="98710-262">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="98710-263">Advertencia</span><span class="sxs-lookup"><span data-stu-id="98710-263">Warning</span></span>           |
| <span data-ttu-id="98710-264">3</span><span class="sxs-lookup"><span data-stu-id="98710-264">3</span></span>      | <span data-ttu-id="98710-265">Consola</span><span class="sxs-lookup"><span data-stu-id="98710-265">Console</span></span>       | <span data-ttu-id="98710-266">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="98710-266">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="98710-267">Depuración</span><span class="sxs-lookup"><span data-stu-id="98710-267">Debug</span></span>             |
| <span data-ttu-id="98710-268">4</span><span class="sxs-lookup"><span data-stu-id="98710-268">4</span></span>      | <span data-ttu-id="98710-269">Consola</span><span class="sxs-lookup"><span data-stu-id="98710-269">Console</span></span>       | <span data-ttu-id="98710-270">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="98710-270">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="98710-271">Error</span><span class="sxs-lookup"><span data-stu-id="98710-271">Error</span></span>             |
| <span data-ttu-id="98710-272">5</span><span class="sxs-lookup"><span data-stu-id="98710-272">5</span></span>      | <span data-ttu-id="98710-273">Consola</span><span class="sxs-lookup"><span data-stu-id="98710-273">Console</span></span>       | <span data-ttu-id="98710-274">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="98710-274">All categories</span></span>                          | <span data-ttu-id="98710-275">Información</span><span class="sxs-lookup"><span data-stu-id="98710-275">Information</span></span>       |
| <span data-ttu-id="98710-276">6</span><span class="sxs-lookup"><span data-stu-id="98710-276">6</span></span>      | <span data-ttu-id="98710-277">Todos los proveedores</span><span class="sxs-lookup"><span data-stu-id="98710-277">All providers</span></span> | <span data-ttu-id="98710-278">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="98710-278">All categories</span></span>                          | <span data-ttu-id="98710-279">Depuración</span><span class="sxs-lookup"><span data-stu-id="98710-279">Debug</span></span>             |
| <span data-ttu-id="98710-280">7</span><span class="sxs-lookup"><span data-stu-id="98710-280">7</span></span>      | <span data-ttu-id="98710-281">Todos los proveedores</span><span class="sxs-lookup"><span data-stu-id="98710-281">All providers</span></span> | <span data-ttu-id="98710-282">Sistema</span><span class="sxs-lookup"><span data-stu-id="98710-282">System</span></span>                                  | <span data-ttu-id="98710-283">Depuración</span><span class="sxs-lookup"><span data-stu-id="98710-283">Debug</span></span>             |
| <span data-ttu-id="98710-284">8</span><span class="sxs-lookup"><span data-stu-id="98710-284">8</span></span>      | <span data-ttu-id="98710-285">Depuración</span><span class="sxs-lookup"><span data-stu-id="98710-285">Debug</span></span>         | <span data-ttu-id="98710-286">Microsoft</span><span class="sxs-lookup"><span data-stu-id="98710-286">Microsoft</span></span>                               | <span data-ttu-id="98710-287">Seguimiento</span><span class="sxs-lookup"><span data-stu-id="98710-287">Trace</span></span>             |

<span data-ttu-id="98710-288">Cuando se crea un objeto `ILogger` con el que escribir los registros, el objeto `ILoggerFactory` selecciona una sola regla por proveedor para aplicar a ese registrador.</span><span class="sxs-lookup"><span data-stu-id="98710-288">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="98710-289">Todos los mensajes escritos por ese objeto `ILogger` se filtran según las reglas seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="98710-289">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="98710-290">De las reglas disponibles se selecciona la más específica posible para cada par de categoría y proveedor.</span><span class="sxs-lookup"><span data-stu-id="98710-290">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="98710-291">Cuando se crea un `ILogger` para una categoría determinada, se usa el algoritmo siguiente para cada proveedor:</span><span class="sxs-lookup"><span data-stu-id="98710-291">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="98710-292">Se seleccionan todas las reglas que coinciden con el proveedor o su alias.</span><span class="sxs-lookup"><span data-stu-id="98710-292">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="98710-293">Si no se encuentra ninguna, se seleccionan todas las reglas con un proveedor vacío.</span><span class="sxs-lookup"><span data-stu-id="98710-293">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="98710-294">Del resultado del paso anterior, se seleccionan las reglas con el prefijo de categoría coincidente más largo.</span><span class="sxs-lookup"><span data-stu-id="98710-294">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="98710-295">Si no se encuentra ninguna, se seleccionan todas las reglas que no especifican una categoría.</span><span class="sxs-lookup"><span data-stu-id="98710-295">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="98710-296">Si se seleccionan varias reglas, se toma la **última**.</span><span class="sxs-lookup"><span data-stu-id="98710-296">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="98710-297">Si no se selecciona ninguna regla, se usa `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="98710-297">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="98710-298">Por ejemplo, supongamos que tiene la lista de reglas anterior y crea un objeto `ILogger` para la categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="98710-298">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="98710-299">Para el proveedor de depuración, se aplican las reglas 1, 6 y 8.</span><span class="sxs-lookup"><span data-stu-id="98710-299">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="98710-300">La regla 8 es la más específica, por lo que se selecciona.</span><span class="sxs-lookup"><span data-stu-id="98710-300">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="98710-301">Para el proveedor de la consola, se aplican las reglas 3, 4, 5 y 6.</span><span class="sxs-lookup"><span data-stu-id="98710-301">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="98710-302">La regla 3 es la más específica.</span><span class="sxs-lookup"><span data-stu-id="98710-302">Rule 3 is most specific.</span></span>

<span data-ttu-id="98710-303">Cuando crea registros con un `ILogger` para la categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", los registros de nivel `Trace` y superiores se dirigirán al proveedor de depuración y los registros de nivel `Debug` y superiores se dirigirán al proveedor de consola.</span><span class="sxs-lookup"><span data-stu-id="98710-303">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

<span data-ttu-id="98710-304">**Alias de proveedor**</span><span class="sxs-lookup"><span data-stu-id="98710-304">**Provider aliases**</span></span>

<span data-ttu-id="98710-305">Puede usar el nombre de tipo para especificar un proveedor en la configuración, pero cada proveedor define un *alias* más breve que es más fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="98710-305">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="98710-306">Para los proveedores integrados, use los alias siguientes:</span><span class="sxs-lookup"><span data-stu-id="98710-306">For the built-in providers, use the following aliases:</span></span>

- <span data-ttu-id="98710-307">Consola</span><span class="sxs-lookup"><span data-stu-id="98710-307">Console</span></span>
- <span data-ttu-id="98710-308">Depuración</span><span class="sxs-lookup"><span data-stu-id="98710-308">Debug</span></span>
- <span data-ttu-id="98710-309">EventLog</span><span class="sxs-lookup"><span data-stu-id="98710-309">EventLog</span></span>
- <span data-ttu-id="98710-310">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="98710-310">AzureAppServices</span></span>
- <span data-ttu-id="98710-311">TraceSource</span><span class="sxs-lookup"><span data-stu-id="98710-311">TraceSource</span></span>
- <span data-ttu-id="98710-312">EventSource</span><span class="sxs-lookup"><span data-stu-id="98710-312">EventSource</span></span>

<span data-ttu-id="98710-313">**Nivel mínimo predeterminado**</span><span class="sxs-lookup"><span data-stu-id="98710-313">**Default minimum level**</span></span>

<span data-ttu-id="98710-314">Hay una configuración de nivel mínimo que solo tiene efecto si no se aplica ninguna regla de configuración o código para un proveedor y una categoría determinados.</span><span class="sxs-lookup"><span data-stu-id="98710-314">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="98710-315">En el ejemplo siguiente se muestra cómo establecer el nivel mínimo:</span><span class="sxs-lookup"><span data-stu-id="98710-315">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="98710-316">Si no establece explícitamente el nivel mínimo, el valor predeterminado es `Information`, lo que significa que los registros `Trace` y `Debug` se omiten.</span><span class="sxs-lookup"><span data-stu-id="98710-316">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

<span data-ttu-id="98710-317">**Funciones de filtro**</span><span class="sxs-lookup"><span data-stu-id="98710-317">**Filter functions**</span></span>

<span data-ttu-id="98710-318">Puede escribir código en una función de filtro para aplicar las reglas de filtrado.</span><span class="sxs-lookup"><span data-stu-id="98710-318">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="98710-319">Se invoca una función de filtro para todos los proveedores y categorías que no tienen reglas asignadas mediante configuración o código.</span><span class="sxs-lookup"><span data-stu-id="98710-319">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="98710-320">El código de la función tiene acceso al tipo de proveedor, categoría y nivel de registro para decidir si se debe registrar un mensaje.</span><span class="sxs-lookup"><span data-stu-id="98710-320">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="98710-321">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="98710-321">For example:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="98710-322">Algunos proveedores de registro permiten especificar cuándo deben escribirse los registros en un medio de almacenamiento o ignorarse en función de la categoría y el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="98710-322">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="98710-323">Los métodos de extensión `AddConsole` y `AddDebug` proporcionan sobrecargas que le permiten pasar criterios de filtrado.</span><span class="sxs-lookup"><span data-stu-id="98710-323">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="98710-324">El código de ejemplo siguiente hace que el proveedor de la consola ignore los registros por debajo del nivel `Warning`, mientras que el proveedor de depuración omite los registros creados por la plataforma.</span><span class="sxs-lookup"><span data-stu-id="98710-324">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="98710-325">El método `AddEventLog` tiene una sobrecarga que toma una instancia de `EventLogSettings`, que puede contener una función de filtrado en su propiedad `Filter`.</span><span class="sxs-lookup"><span data-stu-id="98710-325">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="98710-326">El proveedor de TraceSource no proporciona ninguna de estas sobrecargas, dado que su nivel de registro y otros parámetros se basan en el `SourceSwitch` y `TraceListener` que usa.</span><span class="sxs-lookup"><span data-stu-id="98710-326">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="98710-327">Puede establecer reglas de filtrado para todos los proveedores que están registrados con un instancia de `ILoggerFactory` mediante el método de extensión `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="98710-327">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="98710-328">En el ejemplo siguiente se limitan los registros de la plataforma (la categoría comienza con "Microsoft" o "System") a las advertencias, mientras se permite el registro de la aplicación en el nivel de depuración.</span><span class="sxs-lookup"><span data-stu-id="98710-328">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="98710-329">Si quiere usar el filtrado para impedir que se escriban todos los registros para una categoría concreta, puede especificar `LogLevel.None` como el nivel de registro mínimo de esa categoría.</span><span class="sxs-lookup"><span data-stu-id="98710-329">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="98710-330">El valor entero de `LogLevel.None` es 6, que es superior a `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="98710-330">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="98710-331">El paquete NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) proporciona el método de extensión `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="98710-331">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="98710-332">El método devuelve una instancia nueva de `ILoggerFactory` que filtrará los mensajes de registro pasados a todos los proveedores de registrador registrados con ella.</span><span class="sxs-lookup"><span data-stu-id="98710-332">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="98710-333">No afecta a ninguna otra instancia de `ILoggerFactory`, incluida la instancia de `ILoggerFactory` original.</span><span class="sxs-lookup"><span data-stu-id="98710-333">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="98710-334">Ámbitos de registro</span><span class="sxs-lookup"><span data-stu-id="98710-334">Log scopes</span></span>

<span data-ttu-id="98710-335">Puede agrupar un conjunto de operaciones lógicas dentro de un *ámbito* para adjuntar los mismos datos para cada registro que se crea como parte de ese conjunto.</span><span class="sxs-lookup"><span data-stu-id="98710-335">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="98710-336">Por ejemplo, es posible que quiera que todos los registros creados como parte del procesamiento de una transacción incluyan el identificador de la transacción.</span><span class="sxs-lookup"><span data-stu-id="98710-336">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="98710-337">Un ámbito es un tipo `IDisposable` devuelto por el método [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) que se conserva hasta que se elimina.</span><span class="sxs-lookup"><span data-stu-id="98710-337">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="98710-338">Para usar un ámbito, las llamadas de registrador se encapsulan en un bloque `using`, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="98710-338">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="98710-339">El código siguiente permite ámbitos para el proveedor de la consola:</span><span class="sxs-lookup"><span data-stu-id="98710-339">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="98710-340">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="98710-340">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="98710-341">Es necesario configurar la opción del registrador de consola `IncludeScopes` para habilitar el registro basado en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="98710-341">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="98710-342">`IncludeScopes` se puede configurar a través de archivos de configuración *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="98710-342">`IncludeScopes` can be configured via *appsettings* configuration files.</span></span> <span data-ttu-id="98710-343">Para obtener más información, vea la sección [Configuración del archivo de configuración](#settings-file-configuration).</span><span class="sxs-lookup"><span data-stu-id="98710-343">For more information, see the [Settings file configuration](#settings-file-configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="98710-344">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="98710-344">*Program.cs*:</span></span>

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="98710-345">Es necesario configurar la opción del registrador de consola `IncludeScopes` para habilitar el registro basado en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="98710-345">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="98710-346">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="98710-346">*Startup.cs*:</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="98710-347">Cada mensaje de registro incluye la información de ámbito:</span><span class="sxs-lookup"><span data-stu-id="98710-347">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="98710-348">Proveedores de registro integrados</span><span class="sxs-lookup"><span data-stu-id="98710-348">Built-in logging providers</span></span>

<span data-ttu-id="98710-349">ASP.NET Core incluye los proveedores siguientes:</span><span class="sxs-lookup"><span data-stu-id="98710-349">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="98710-350">Consola</span><span class="sxs-lookup"><span data-stu-id="98710-350">Console</span></span>](#console-provider)
* [<span data-ttu-id="98710-351">Depurar</span><span class="sxs-lookup"><span data-stu-id="98710-351">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="98710-352">EventSource</span><span class="sxs-lookup"><span data-stu-id="98710-352">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="98710-353">EventLog</span><span class="sxs-lookup"><span data-stu-id="98710-353">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="98710-354">TraceSource</span><span class="sxs-lookup"><span data-stu-id="98710-354">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="98710-355">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="98710-355">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="98710-356">Proveedor de la consola</span><span class="sxs-lookup"><span data-stu-id="98710-356">Console provider</span></span>

<span data-ttu-id="98710-357">El paquete de proveedor [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envía la salida del registro a la consola.</span><span class="sxs-lookup"><span data-stu-id="98710-357">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span> 

::: moniker range=">= aspnetcore-2.0"


```csharp
logging.AddConsole()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole()
```

<span data-ttu-id="98710-358">[Las sobrecargas de AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) permiten pasar un nivel de registro mínimo, una función de filtro y un valor booleano que indica si se admiten los ámbitos.</span><span class="sxs-lookup"><span data-stu-id="98710-358">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="98710-359">Otra opción consiste en pasar un objeto `IConfiguration`, que puede especificar niveles de registro y si se admiten los ámbitos.</span><span class="sxs-lookup"><span data-stu-id="98710-359">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span> 

<span data-ttu-id="98710-360">Si está pensando en la posibilidad de usar la consola de proveedor en producción, tenga en cuenta que tiene un impacto significativo en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="98710-360">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="98710-361">Cuando se crea un proyecto en Visual Studio, el método `AddConsole` tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="98710-361">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="98710-362">Este código hace referencia a la sección `Logging` del archivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="98710-362">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/sample//appsettings.json)]

<span data-ttu-id="98710-363">La configuración que se muestra limita los registros de la plataforma a las advertencias mientras que permite a la aplicación registrar en el nivel de depuración, como se explica en la sección [Filtrado del registro](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="98710-363">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="98710-364">Para obtener más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="98710-364">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="98710-365">Proveedor de depuración</span><span class="sxs-lookup"><span data-stu-id="98710-365">Debug provider</span></span>

<span data-ttu-id="98710-366">El paquete de proveedor [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) escribe la salida del registro mediante la clase [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (llamadas a métodos `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="98710-366">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="98710-367">En Linux, este proveedor escribe registros en */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="98710-367">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug()
```

<span data-ttu-id="98710-368">[Las sobrecargas de AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) permiten pasar un nivel mínimo de registro o una función de filtro.</span><span class="sxs-lookup"><span data-stu-id="98710-368">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="98710-369">Proveedor EventSource</span><span class="sxs-lookup"><span data-stu-id="98710-369">EventSource provider</span></span>

<span data-ttu-id="98710-370">Para las aplicaciones que tienen como destino ASP.NET Core 1.1.0 o superior, el paquete de proveedor [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) puede implementar el seguimiento de eventos.</span><span class="sxs-lookup"><span data-stu-id="98710-370">For apps that target ASP.NET Core 1.1.0 or higher, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="98710-371">En Windows, usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="98710-371">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="98710-372">Es un proveedor multiplataforma, pero todavía no hay herramientas de recopilación y visualización de eventos para Linux o macOS.</span><span class="sxs-lookup"><span data-stu-id="98710-372">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span> 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger()
```

::: moniker-end

<span data-ttu-id="98710-373">Una buena manera de recopilar y ver los registros es usar la [utilidad PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="98710-373">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="98710-374">Hay otras herramientas para ver los registros ETW, pero PerfView proporciona la mejor experiencia para trabajar con los eventos ETW emitidos por ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="98710-374">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span> 

<span data-ttu-id="98710-375">Para configurar PerfView para la recopilación de eventos registrados por este proveedor, agregue la cadena `*Microsoft-Extensions-Logging` a la lista **Proveedores adicionales**.</span><span class="sxs-lookup"><span data-stu-id="98710-375">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="98710-376">(No olvide el asterisco al principio de la cadena).</span><span class="sxs-lookup"><span data-stu-id="98710-376">(Don't miss the asterisk at the start of the string.)</span></span>

![Proveedores adicionales de Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="98710-378">Proveedor EventLog de Windows</span><span class="sxs-lookup"><span data-stu-id="98710-378">Windows EventLog provider</span></span>

<span data-ttu-id="98710-379">El paquete de proveedor [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envía la salida del registro al Registro de eventos de Windows.</span><span class="sxs-lookup"><span data-stu-id="98710-379">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog()
```

<span data-ttu-id="98710-380">[Las sobrecargas de AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) permiten pasar `EventLogSettings` o un nivel de registro mínimo.</span><span class="sxs-lookup"><span data-stu-id="98710-380">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="98710-381">Proveedor TraceSource</span><span class="sxs-lookup"><span data-stu-id="98710-381">TraceSource provider</span></span>

<span data-ttu-id="98710-382">El paquete de proveedor [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa las bibliotecas y proveedores de [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="98710-382">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

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

<span data-ttu-id="98710-383">[Las sobrecargas de AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) permiten pasar un modificador de origen y un agente de escucha de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="98710-383">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="98710-384">Para usar este proveedor, una aplicación debe ejecutarse en .NET Framework (en lugar de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="98710-384">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="98710-385">El proveedor permite enrutar mensajes a una variedad de [agentes de escucha](/dotnet/framework/debug-trace-profile/trace-listeners), como [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) que se usa en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="98710-385">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="98710-386">En el ejemplo siguiente se configura un proveedor `TraceSource` que registra mensajes `Warning` y superiores en la ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="98710-386">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a><span data-ttu-id="98710-387">Proveedor Azure App Service</span><span class="sxs-lookup"><span data-stu-id="98710-387">Azure App Service provider</span></span>

<span data-ttu-id="98710-388">El paquete de proveedor [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) escribe los registros en archivos de texto en el sistema de archivos de una aplicación de Azure App Service y en [Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) en una cuenta de Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="98710-388">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="98710-389">El proveedor solo está disponible para las aplicaciones que tienen como destino ASP.NET Core 1.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="98710-389">The provider is available only for apps that target ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="98710-390">Si el destino es .NET Core, no instale el paquete de proveedor ni llame explícitamente a [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="98710-390">If targeting .NET Core, don't install the provider package or explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="98710-391">El proveedor está disponible automáticamente para la aplicación cuando esta se implementa en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="98710-391">The provider is automatically available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="98710-392">Si el destino es .NET Framework, agregue el paquete de proveedor al proyecto e invoque a `AddAzureWebAppDiagnostics`:</span><span class="sxs-lookup"><span data-stu-id="98710-392">If targeting .NET Framework, add the provider package to the project and invoke `AddAzureWebAppDiagnostics`:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

<span data-ttu-id="98710-393">Una sobrecarga de [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) le permite pasar [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings), con lo que se puede invalidar la configuración predeterminada, como la plantilla de salida del registro, el nombre de blob y el límite de tamaño de archivo.</span><span class="sxs-lookup"><span data-stu-id="98710-393">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="98710-394">(La *plantilla de salida* es una plantilla de mensaje que se aplica a todos los registros además de la que se proporciona cuando se llama a un método `ILogger`).</span><span class="sxs-lookup"><span data-stu-id="98710-394">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

::: moniker-end

<span data-ttu-id="98710-395">Cuando se realiza una implementación en una aplicación de App Service, la aplicación respeta la configuración de la sección [Registros de diagnóstico](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) situada en la página **App Service** de Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="98710-395">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="98710-396">Cuando se actualiza esta configuración, los cambios se aplican inmediatamente sin necesidad de reiniciar ni de volver a implementar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="98710-396">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Configuración de los registros de Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="98710-398">La ubicación predeterminada de los archivos de registro es la carpeta *D:\\home\\LogFiles\\Application* y el nombre de archivo predeterminado es *diagnostics-aaaammdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="98710-398">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="98710-399">El límite de tamaño de archivo predeterminado es 10 MB, y el número máximo predeterminado de archivos que se conservan es 2.</span><span class="sxs-lookup"><span data-stu-id="98710-399">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="98710-400">El nombre de blob predeterminado es *{nombre-de-la-aplicación}{marca de tiempo}/aaaa/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="98710-400">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="98710-401">Para más información sobre el comportamiento predeterminado, vea [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="98710-401">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="98710-402">El proveedor solo funciona cuando el proyecto se ejecuta en el entorno de Azure.</span><span class="sxs-lookup"><span data-stu-id="98710-402">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="98710-403">No tiene ningún efecto cuando el proyecto se ejecuta de manera local (no escribe en los archivos locales ni en el almacenamiento de desarrollo local de blobs).</span><span class="sxs-lookup"><span data-stu-id="98710-403">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="98710-404">Proveedores de registro de terceros</span><span class="sxs-lookup"><span data-stu-id="98710-404">Third-party logging providers</span></span>

<span data-ttu-id="98710-405">Plataformas de registro de terceros que funcionan con ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="98710-405">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="98710-406">[elmah.io](https://elmah.io/) ([repositorio de GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="98710-406">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="98710-407">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repositorio de GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="98710-407">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="98710-408">[JSNLog](http://jsnlog.com/) ([repositorio de GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="98710-408">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="98710-409">[Loggr](http://loggr.net/) ([repositorio de GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="98710-409">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="98710-410">[NLog](http://nlog-project.org/) ([repositorio de GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="98710-410">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="98710-411">[Serilog](https://serilog.net/) ([repositorio de GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="98710-411">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="98710-412">Algunas plataformas de terceros pueden realizar [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="98710-412">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="98710-413">El uso de una plataforma de terceros es similar al uso de uno de los proveedores integrados:</span><span class="sxs-lookup"><span data-stu-id="98710-413">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="98710-414">Agregue un paquete NuGet al proyecto.</span><span class="sxs-lookup"><span data-stu-id="98710-414">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="98710-415">Llame a un método de extensión en `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="98710-415">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="98710-416">Para más información, vea la documentación de cada plataforma.</span><span class="sxs-lookup"><span data-stu-id="98710-416">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="98710-417">Secuencias de registro de Azure</span><span class="sxs-lookup"><span data-stu-id="98710-417">Azure log streaming</span></span>

<span data-ttu-id="98710-418">Las secuencias de registro de Azure permiten ver la actividad de registro en tiempo real desde:</span><span class="sxs-lookup"><span data-stu-id="98710-418">Azure log streaming enables you to view log activity in real time from:</span></span> 

* <span data-ttu-id="98710-419">El servidor de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="98710-419">The application server</span></span>
* <span data-ttu-id="98710-420">El servidor web</span><span class="sxs-lookup"><span data-stu-id="98710-420">The web server</span></span>
* <span data-ttu-id="98710-421">Error del seguimiento de solicitudes</span><span class="sxs-lookup"><span data-stu-id="98710-421">Failed request tracing</span></span>

<span data-ttu-id="98710-422">Para configurar las secuencias de registro de Azure:</span><span class="sxs-lookup"><span data-stu-id="98710-422">To configure Azure log streaming:</span></span>

* <span data-ttu-id="98710-423">Navegue hasta la página **Registros de diagnóstico** desde la página de portal de la aplicación</span><span class="sxs-lookup"><span data-stu-id="98710-423">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="98710-424">Establezca **Registro de la aplicación (sistema de archivos)** en Activado.</span><span class="sxs-lookup"><span data-stu-id="98710-424">Set **Application Logging (Filesystem)** to on.</span></span>

![Página de registros de diagnóstico de Azure Portal](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="98710-426">Navegue hasta la página **Secuencias de registro** para ver los mensajes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="98710-426">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="98710-427">Se registran por la aplicación a través de la interfaz `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="98710-427">They're logged by application through the `ILogger` interface.</span></span>

![Secuencias de registro de aplicación de Azure Portal](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="98710-429">Registro de seguimiento de Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="98710-429">Azure Application Insights trace logging</span></span>

<span data-ttu-id="98710-430">El SDK de [Application Insights](https://azure.microsoft.com/services/application-insights/) es capaz de recopilar información de telemetría de seguimiento de registros generados mediante la infraestructura de registro de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="98710-430">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="98710-431">Para obtener más información, vea la [wiki de Microsoft/ApplicationInsights-aspnetcore sobre el registro](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span><span class="sxs-lookup"><span data-stu-id="98710-431">For more information, see [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="98710-432">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="98710-432">Additional resources</span></span>

[<span data-ttu-id="98710-433">Registro de alto rendimiento con LoggerMessage</span><span class="sxs-lookup"><span data-stu-id="98710-433">High-performance logging with LoggerMessage</span></span>](xref:fundamentals/logging/loggermessage)
