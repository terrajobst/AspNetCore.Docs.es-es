---
title: Registro en ASP.NET Core
author: ardalis
description: Obtenga información sobre la plataforma de registro de ASP.NET Core. Descubra los proveedores de registro integrados y obtenga más información sobre proveedores de terceros conocidos.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/27/2018
uid: fundamentals/logging/index
ms.openlocfilehash: c6e9aae06df6ebec373b1296f86e37380bf08b15
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055766"
---
# <a name="logging-in-aspnet-core"></a><span data-ttu-id="ddb06-104">Registro en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ddb06-104">Logging in ASP.NET Core</span></span>

<span data-ttu-id="ddb06-105">Por [Steve Smith](https://ardalis.com/) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ddb06-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ddb06-106">ASP.NET Core es compatible con una API de registro que funciona con una variedad de proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="ddb06-106">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="ddb06-107">Los proveedores integrados permiten enviar registros a uno o varios destinos, y se puede conectar una plataforma de registro de terceros.</span><span class="sxs-lookup"><span data-stu-id="ddb06-107">Built-in providers let you send logs to one or more destinations, and you can plug in a third-party logging framework.</span></span> <span data-ttu-id="ddb06-108">En este artículo se muestra cómo usar las API y los proveedores de registro integrados en el código.</span><span class="sxs-lookup"><span data-stu-id="ddb06-108">This article shows how to use the built-in logging API and providers in your code.</span></span>

<span data-ttu-id="ddb06-109">Para obtener información sobre el registro de stdout al hospedar con IIS, consulte <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="ddb06-109">For information on stdout logging when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>.</span></span> <span data-ttu-id="ddb06-110">Para obtener información sobre el registro de stdout con Azure App Service, consulte <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span><span class="sxs-lookup"><span data-stu-id="ddb06-110">For information on stdout logging with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.</span></span>

<span data-ttu-id="ddb06-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ddb06-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="how-to-create-logs"></a><span data-ttu-id="ddb06-112">Cómo crear registros</span><span class="sxs-lookup"><span data-stu-id="ddb06-112">How to create logs</span></span>

<span data-ttu-id="ddb06-113">Para crear registros, implemente un objeto [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) desde el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="ddb06-113">To create logs, implement an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) object from the [dependency injection](xref:fundamentals/dependency-injection) container:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="ddb06-114">Después, llame a los métodos de registro de ese objeto de registrador:</span><span class="sxs-lookup"><span data-stu-id="ddb06-114">Then call logging methods on that logger object:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="ddb06-115">En este ejemplo se crean registros con la clase `TodoController` como *categoría*.</span><span class="sxs-lookup"><span data-stu-id="ddb06-115">This example creates logs with the `TodoController` class as the *category*.</span></span> <span data-ttu-id="ddb06-116">Las categorías se explican [más adelante en este artículo](#log-category).</span><span class="sxs-lookup"><span data-stu-id="ddb06-116">Categories are explained [later in this article](#log-category).</span></span>

<span data-ttu-id="ddb06-117">ASP.NET Core no proporciona métodos de registrador asincrónicos porque el registro debe ser tan rápido que el costo de usarlos no vale la pena.</span><span class="sxs-lookup"><span data-stu-id="ddb06-117">ASP.NET Core doesn't provide async logger methods because logging should be so fast that it isn't worth the cost of using async.</span></span> <span data-ttu-id="ddb06-118">Si se encuentra en una situación en la que no sea así, considere la posibilidad de cambiar el modo de registro.</span><span class="sxs-lookup"><span data-stu-id="ddb06-118">If you're in a situation where that's not true, consider changing the way you log.</span></span> <span data-ttu-id="ddb06-119">Si el almacén de datos es lento, escriba primero los mensajes de registro en un almacén rápido y, después, muévalos a un almacén de baja velocidad.</span><span class="sxs-lookup"><span data-stu-id="ddb06-119">If your data store is slow, write the log messages to a fast store first, then move them to a slow store later.</span></span> <span data-ttu-id="ddb06-120">Por ejemplo, realice el registro en una cola de mensajes que otro proceso lea y conserve en almacenamiento lento.</span><span class="sxs-lookup"><span data-stu-id="ddb06-120">For example, log to a message queue that's read and persisted to slow storage by another process.</span></span>

## <a name="how-to-add-providers"></a><span data-ttu-id="ddb06-121">Cómo agregar proveedores</span><span class="sxs-lookup"><span data-stu-id="ddb06-121">How to add providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ddb06-122">Un proveedor de registro toma los mensajes que se crean con un objeto `ILogger` y los muestra o almacena.</span><span class="sxs-lookup"><span data-stu-id="ddb06-122">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="ddb06-123">Por ejemplo, el proveedor de la consola muestra mensajes en la consola y el proveedor de Azure App Service puede almacenarlos en Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="ddb06-123">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="ddb06-124">Para usar un proveedor, llame al método de extensión `Add<ProviderName>` del proveedor en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ddb06-124">To use a provider, call the provider's `Add<ProviderName>` extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

<span data-ttu-id="ddb06-125">La plantilla de proyecto predeterminada habilita los proveedores de registro de la consola y de depuración con una llamada al método de extensión [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ddb06-125">The default project template enables the Console and Debug logging providers with a call to the [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) extension method in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ddb06-126">Un proveedor de registro toma los mensajes que se crean con un objeto `ILogger` y los muestra o almacena.</span><span class="sxs-lookup"><span data-stu-id="ddb06-126">A logging provider takes the messages that you create with an `ILogger` object and displays or stores them.</span></span> <span data-ttu-id="ddb06-127">Por ejemplo, el proveedor de la consola muestra mensajes en la consola y el proveedor de Azure App Service puede almacenarlos en Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="ddb06-127">For example, the Console provider displays messages on the console, and the Azure App Service provider can store them in Azure blob storage.</span></span>

<span data-ttu-id="ddb06-128">Para usar un proveedor, instale su paquete NuGet y llame al método de extensión del proveedor en una instancia de [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ddb06-128">To use a provider, install its NuGet package and call the provider's extension method on an instance of [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory), as shown in the following example:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

<span data-ttu-id="ddb06-129">La [inserción de dependencias](xref:fundamentals/dependency-injection) (DI) de ASP.NET Core proporciona la instancia de `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-129">ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection) (DI) provides the `ILoggerFactory` instance.</span></span> <span data-ttu-id="ddb06-130">Los métodos de extensión `AddConsole` y `AddDebug` se definen en los paquetes [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) y [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/).</span><span class="sxs-lookup"><span data-stu-id="ddb06-130">The `AddConsole` and `AddDebug` extension methods are defined in the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) and [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) packages.</span></span> <span data-ttu-id="ddb06-131">Cada método de extensión llama al método `ILoggerFactory.AddProvider`, pasando una instancia del proveedor.</span><span class="sxs-lookup"><span data-stu-id="ddb06-131">Each extension method calls the `ILoggerFactory.AddProvider` method, passing in an instance of the provider.</span></span>

> [!NOTE]
> <span data-ttu-id="ddb06-132">La [aplicación de ejemplo 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) agrega proveedores de registro en el método `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-132">The [1.x sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) adds logging providers in the `Startup.Configure` method.</span></span> <span data-ttu-id="ddb06-133">Si quiere obtener la salida de registro de código que se ejecuta antes, agregue los proveedores de registro en el constructor de la clase `Startup`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-133">If you want to obtain log output from code that executes earlier, add logging providers in the `Startup` class constructor.</span></span>

::: moniker-end

<span data-ttu-id="ddb06-134">Obtenga más información sobre los [proveedores de registro integrados](#built-in-logging-providers) y encuentre [proveedores de registro de terceros](#third-party-logging-providers) más adelante en el artículo.</span><span class="sxs-lookup"><span data-stu-id="ddb06-134">Learn more about the [built-in logging providers](#built-in-logging-providers) and find links to [third-party logging providers](#third-party-logging-providers) later in the article.</span></span>

## <a name="configuration"></a><span data-ttu-id="ddb06-135">Configuración</span><span class="sxs-lookup"><span data-stu-id="ddb06-135">Configuration</span></span>

<span data-ttu-id="ddb06-136">Uno o varios proveedores de configuración proporcionan la configuración del proveedor de registro:</span><span class="sxs-lookup"><span data-stu-id="ddb06-136">Logging provider configuration is provided by one or more configuration providers:</span></span>

* <span data-ttu-id="ddb06-137">Formatos de archivo (INI, JSON y XML).</span><span class="sxs-lookup"><span data-stu-id="ddb06-137">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="ddb06-138">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="ddb06-138">Command-line arguments.</span></span>
* <span data-ttu-id="ddb06-139">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="ddb06-139">Environment variables.</span></span>
* <span data-ttu-id="ddb06-140">Objetos de .NET en memoria.</span><span class="sxs-lookup"><span data-stu-id="ddb06-140">In-memory .NET objects.</span></span>
* <span data-ttu-id="ddb06-141">El almacenamiento de [administrador secreto](xref:security/app-secrets) sin cifrar.</span><span class="sxs-lookup"><span data-stu-id="ddb06-141">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="ddb06-142">Un almacén de usuario cifrado, como [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="ddb06-142">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="ddb06-143">Proveedores personalizados (instalados o creados).</span><span class="sxs-lookup"><span data-stu-id="ddb06-143">Custom providers (installed or created).</span></span>

<span data-ttu-id="ddb06-144">Por ejemplo, la sección `Logging` de archivos de configuración de aplicación suele proporcionar la configuración de registro.</span><span class="sxs-lookup"><span data-stu-id="ddb06-144">For example, logging configuration is commonly provided by the `Logging` section of app settings files.</span></span> <span data-ttu-id="ddb06-145">En el ejemplo siguiente se muestra el contenido de un archivo *appsettings.Development.json* típico:</span><span class="sxs-lookup"><span data-stu-id="ddb06-145">The following example shows the contents of a typical *appsettings.Development.json* file:</span></span>

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

<span data-ttu-id="ddb06-146">Las claves `LogLevel` representan los nombres de registro.</span><span class="sxs-lookup"><span data-stu-id="ddb06-146">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="ddb06-147">La clave `Default` se aplica a los registros que no se enumeran de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="ddb06-147">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="ddb06-148">El valor representa el [nivel de registro](#log-level) aplicado al registro determinado.</span><span class="sxs-lookup"><span data-stu-id="ddb06-148">The value represents the [log level](#log-level) applied to the given log.</span></span> <span data-ttu-id="ddb06-149">Las claves de registro que establecen `IncludeScopes` (`Console` en el ejemplo), especifican si los [ámbitos de registro](#log-scopes) están habilitados para el registro indicado.</span><span class="sxs-lookup"><span data-stu-id="ddb06-149">Log keys that set `IncludeScopes` (`Console` in the example), specify if [log scopes](#log-scopes) are enabled for the indicated log.</span></span>

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

<span data-ttu-id="ddb06-150">Las claves `LogLevel` representan los nombres de registro.</span><span class="sxs-lookup"><span data-stu-id="ddb06-150">`LogLevel` keys represent log names.</span></span> <span data-ttu-id="ddb06-151">La clave `Default` se aplica a los registros que no se enumeran de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="ddb06-151">The `Default` key applies to logs not explicitly listed.</span></span> <span data-ttu-id="ddb06-152">El valor representa el [nivel de registro](#log-level) aplicado al registro determinado.</span><span class="sxs-lookup"><span data-stu-id="ddb06-152">The value represents the [log level](#log-level) applied to the given log.</span></span>

::: moniker-end

<span data-ttu-id="ddb06-153">Para obtener información sobre cómo implementar proveedores de configuración, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ddb06-153">For information on implementing configuration providers, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="sample-logging-output"></a><span data-ttu-id="ddb06-154">Salida de registro de ejemplo</span><span class="sxs-lookup"><span data-stu-id="ddb06-154">Sample logging output</span></span>

<span data-ttu-id="ddb06-155">Con el código de ejemplo que se muestra en la sección anterior, verá los registros en la consola cuando se ejecute desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="ddb06-155">With the sample code shown in the preceding section, you'll see logs in the console when you run from the command line.</span></span> <span data-ttu-id="ddb06-156">Este es un ejemplo de salida de la consola:</span><span class="sxs-lookup"><span data-stu-id="ddb06-156">Here's an example of console output:</span></span>

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

<span data-ttu-id="ddb06-157">Estos registros se crearon yendo a `http://localhost:5000/api/todo/0`, lo que desencadena la ejecución de las dos llamadas a `ILogger` que se muestran en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="ddb06-157">These logs were created by going to `http://localhost:5000/api/todo/0`, which triggers execution of both `ILogger` calls shown in the preceding section.</span></span>

<span data-ttu-id="ddb06-158">Este es un ejemplo de los mismos registros tal y como aparecen en la ventana de depuración cuando se ejecuta la aplicación de ejemplo en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ddb06-158">Here's an example of the same logs as they appear in the Debug window when you run the sample application in Visual Studio:</span></span>

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

<span data-ttu-id="ddb06-159">Los registros creados por las llamadas a `ILogger` que se muestran en la sección anterior comienzan por "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="ddb06-159">The logs that were created by the `ILogger` calls shown in the preceding section begin with "TodoApi.Controllers.TodoController".</span></span> <span data-ttu-id="ddb06-160">Los registros que comienzan por categorías de "Microsoft" son de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ddb06-160">The logs that begin with "Microsoft" categories are from ASP.NET Core.</span></span> <span data-ttu-id="ddb06-161">El propio ASP.NET Core y el código de la aplicación usan la misma API y los mismos proveedores de registro.</span><span class="sxs-lookup"><span data-stu-id="ddb06-161">ASP.NET Core itself and your application code are using the same logging API and the same logging providers.</span></span>

<span data-ttu-id="ddb06-162">En el resto de este artículo se explican algunos detalles y opciones para el registro.</span><span class="sxs-lookup"><span data-stu-id="ddb06-162">The remainder of this article explains some details and options for logging.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="ddb06-163">Paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="ddb06-163">NuGet packages</span></span>

<span data-ttu-id="ddb06-164">Las interfaces `ILogger` e `ILoggerFactory` se encuentran en [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), y sus implementaciones predeterminadas en [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span><span class="sxs-lookup"><span data-stu-id="ddb06-164">The `ILogger` and `ILoggerFactory` interfaces are in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), and default implementations for them are in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).</span></span>

## <a name="log-category"></a><span data-ttu-id="ddb06-165">Categoría de registro</span><span class="sxs-lookup"><span data-stu-id="ddb06-165">Log category</span></span>

<span data-ttu-id="ddb06-166">Con cada registro que cree se incluye una *categoría*.</span><span class="sxs-lookup"><span data-stu-id="ddb06-166">A *category* is included with each log that you create.</span></span> <span data-ttu-id="ddb06-167">La categoría se especifica cuando se crea un objeto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-167">You specify the category when you create an `ILogger` object.</span></span> <span data-ttu-id="ddb06-168">La categoría puede ser cualquier cadena, pero una convención consiste en usar el nombre completo de la clase desde la que se escriben los registros.</span><span class="sxs-lookup"><span data-stu-id="ddb06-168">The category may be any string, but a convention is to use the fully qualified name of the class from which the logs are written.</span></span> <span data-ttu-id="ddb06-169">Por ejemplo: "TodoApi.Controllers.TodoController".</span><span class="sxs-lookup"><span data-stu-id="ddb06-169">For example: "TodoApi.Controllers.TodoController".</span></span>

<span data-ttu-id="ddb06-170">Puede especificar la categoría como una cadena o usar un método de extensión que derive la categoría del tipo.</span><span class="sxs-lookup"><span data-stu-id="ddb06-170">You can specify the category as a string or use an extension method that derives the category from the type.</span></span> <span data-ttu-id="ddb06-171">Para especificar la categoría como una cadena, llame a `CreateLogger` en una instancia de `ILoggerFactory`, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="ddb06-171">To specify the category as a string, call `CreateLogger` on an `ILoggerFactory` instance, as shown below.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

<span data-ttu-id="ddb06-172">En la mayoría de los casos, será más fácil usar `ILogger<T>`, como en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="ddb06-172">Most of the time, it will be easier to use `ILogger<T>`, as in the following example.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

<span data-ttu-id="ddb06-173">Esto equivale a llamar a `CreateLogger` con el nombre de tipo completo de `T`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-173">This is equivalent to calling `CreateLogger` with the fully qualified type name of `T`.</span></span>

## <a name="log-level"></a><span data-ttu-id="ddb06-174">Nivel de registro</span><span class="sxs-lookup"><span data-stu-id="ddb06-174">Log level</span></span>

<span data-ttu-id="ddb06-175">Cada vez que se escribe un registro, se especifica su [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span><span class="sxs-lookup"><span data-stu-id="ddb06-175">Each time you write a log, you specify its [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel).</span></span> <span data-ttu-id="ddb06-176">El nivel de registro indica el grado de gravedad o importancia.</span><span class="sxs-lookup"><span data-stu-id="ddb06-176">The log level indicates the degree of severity or importance.</span></span> <span data-ttu-id="ddb06-177">Por ejemplo, es posible que escriba un registro `Information` cuando un método finaliza normalmente, un registro `Warning` cuando un método devuelve un código de retorno 404 y un registro `Error` cuando capture una excepción inesperada.</span><span class="sxs-lookup"><span data-stu-id="ddb06-177">For example, you might write an `Information` log when a method ends normally, a `Warning` log when a method returns a 404 return code, and an `Error` log when you catch an unexpected exception.</span></span>

<span data-ttu-id="ddb06-178">En el ejemplo de código siguiente, los nombres de los métodos (por ejemplo, `LogWarning`) especifican el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="ddb06-178">In the following code example, the names of the methods (for example, `LogWarning`) specify the log level.</span></span> <span data-ttu-id="ddb06-179">El primer parámetro es el [Id. del evento del Registro](#log-event-id).</span><span class="sxs-lookup"><span data-stu-id="ddb06-179">The first parameter is the [Log event ID](#log-event-id).</span></span> <span data-ttu-id="ddb06-180">El segundo parámetro es una [plantilla de mensaje](#log-message-template) con marcadores de posición para los valores de argumento proporcionados por el resto de parámetros de método.</span><span class="sxs-lookup"><span data-stu-id="ddb06-180">The second parameter is a [message template](#log-message-template) with placeholders for argument values provided by the remaining method parameters.</span></span> <span data-ttu-id="ddb06-181">Los parámetros de método se explican detalladamente más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="ddb06-181">The method parameters are explained in more detail later in this article.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="ddb06-182">Los métodos de registro que incluyen el nivel en el nombre del método son [métodos de extensión para ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span><span class="sxs-lookup"><span data-stu-id="ddb06-182">Log methods that include the level in the method name are [extension methods for ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions).</span></span> <span data-ttu-id="ddb06-183">En segundo plano, estos métodos llaman a un método `Log` que toma un parámetro `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-183">Behind the scenes, these methods call a `Log` method that takes a `LogLevel` parameter.</span></span> <span data-ttu-id="ddb06-184">Puede llamar directamente al método `Log` en lugar de a uno de estos métodos de extensión, pero la sintaxis es relativamente complicada.</span><span class="sxs-lookup"><span data-stu-id="ddb06-184">You can call the `Log` method directly rather than one of these extension methods, but the syntax is relatively complicated.</span></span> <span data-ttu-id="ddb06-185">Para más información, vea la [interfaz ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) y el [código fuente de las extensiones de registrador](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span><span class="sxs-lookup"><span data-stu-id="ddb06-185">For more information, see the [ILogger interface](/dotnet/api/microsoft.extensions.logging.ilogger) and the [logger extensions source code](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).</span></span>

<span data-ttu-id="ddb06-186">ASP.NET Core define los [niveles de registro](/dotnet/api/microsoft.extensions.logging.loglevel) siguientes, que aquí se ordenan de menor a mayor gravedad.</span><span class="sxs-lookup"><span data-stu-id="ddb06-186">ASP.NET Core defines the following [log levels](/dotnet/api/microsoft.extensions.logging.loglevel), ordered here from least to highest severity.</span></span>

* <span data-ttu-id="ddb06-187">Seguimiento = 0</span><span class="sxs-lookup"><span data-stu-id="ddb06-187">Trace = 0</span></span>

  <span data-ttu-id="ddb06-188">Para la información que solo es útil para un desarrollador que depura un problema.</span><span class="sxs-lookup"><span data-stu-id="ddb06-188">For information that's valuable only to a developer debugging an issue.</span></span> <span data-ttu-id="ddb06-189">Estos mensajes pueden contener datos confidenciales de la aplicación, por lo que no deben habilitarse en un entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="ddb06-189">These messages may contain sensitive application data and so shouldn't be enabled in a production environment.</span></span> <span data-ttu-id="ddb06-190">*Deshabilitado de forma predeterminada.*</span><span class="sxs-lookup"><span data-stu-id="ddb06-190">*Disabled by default.*</span></span> <span data-ttu-id="ddb06-191">Ejemplo: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span><span class="sxs-lookup"><span data-stu-id="ddb06-191">Example: `Credentials: {"User":"someuser", "Password":"P@ssword"}`</span></span>

* <span data-ttu-id="ddb06-192">Depurar = 1</span><span class="sxs-lookup"><span data-stu-id="ddb06-192">Debug = 1</span></span>

  <span data-ttu-id="ddb06-193">Para la información que tiene utilidad a corto plazo durante el desarrollo y la depuración.</span><span class="sxs-lookup"><span data-stu-id="ddb06-193">For information that has short-term usefulness during development and debugging.</span></span> <span data-ttu-id="ddb06-194">Ejemplo: `Entering method Configure with flag set to true.` normalmente los registros de nivel `Debug` no se habilitarían en producción, a menos que se esté solucionando un problema, debido al elevado volumen de registros.</span><span class="sxs-lookup"><span data-stu-id="ddb06-194">Example: `Entering method Configure with flag set to true.` You typically wouldn't enable `Debug` level logs in production unless you are troubleshooting, due to the high volume of logs.</span></span>

* <span data-ttu-id="ddb06-195">Información = 2</span><span class="sxs-lookup"><span data-stu-id="ddb06-195">Information = 2</span></span>

  <span data-ttu-id="ddb06-196">Para realizar el seguimiento del flujo general de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ddb06-196">For tracking the general flow of the application.</span></span> <span data-ttu-id="ddb06-197">Estos registros suelen tener algún valor a largo plazo.</span><span class="sxs-lookup"><span data-stu-id="ddb06-197">These logs typically have some long-term value.</span></span> <span data-ttu-id="ddb06-198">Ejemplo: `Request received for path /api/todo`</span><span class="sxs-lookup"><span data-stu-id="ddb06-198">Example: `Request received for path /api/todo`</span></span>

* <span data-ttu-id="ddb06-199">Advertencia = 3</span><span class="sxs-lookup"><span data-stu-id="ddb06-199">Warning = 3</span></span>

  <span data-ttu-id="ddb06-200">Para los eventos anómalos o inesperados en el flujo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ddb06-200">For abnormal or unexpected events in the application flow.</span></span> <span data-ttu-id="ddb06-201">Estos pueden incluir errores u otras condiciones que no hacen que la aplicación se detenga, pero que puede que sea necesario investigar.</span><span class="sxs-lookup"><span data-stu-id="ddb06-201">These may include errors or other conditions that don't cause the application to stop, but which may need to be investigated.</span></span> <span data-ttu-id="ddb06-202">Las excepciones controladas son un lugar común para usar el nivel de registro `Warning`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-202">Handled exceptions are a common place to use the `Warning` log level.</span></span> <span data-ttu-id="ddb06-203">Ejemplo: `FileNotFoundException for file quotes.txt.`</span><span class="sxs-lookup"><span data-stu-id="ddb06-203">Example: `FileNotFoundException for file quotes.txt.`</span></span>

* <span data-ttu-id="ddb06-204">Error = 4</span><span class="sxs-lookup"><span data-stu-id="ddb06-204">Error = 4</span></span>

  <span data-ttu-id="ddb06-205">Para los errores y excepciones que no se pueden controlar.</span><span class="sxs-lookup"><span data-stu-id="ddb06-205">For errors and exceptions that cannot be handled.</span></span> <span data-ttu-id="ddb06-206">Estos mensajes indican un error en la actividad u operación actual (por ejemplo, la solicitud HTTP actual), no un error de toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ddb06-206">These messages indicate a failure in the current activity or operation (such as the current HTTP request), not an application-wide failure.</span></span> <span data-ttu-id="ddb06-207">Mensaje de registro de ejemplo: `Cannot insert record due to duplicate key violation.`</span><span class="sxs-lookup"><span data-stu-id="ddb06-207">Example log message: `Cannot insert record due to duplicate key violation.`</span></span>

* <span data-ttu-id="ddb06-208">Crítico = 5</span><span class="sxs-lookup"><span data-stu-id="ddb06-208">Critical = 5</span></span>

  <span data-ttu-id="ddb06-209">Para los errores que requieren atención inmediata.</span><span class="sxs-lookup"><span data-stu-id="ddb06-209">For failures that require immediate attention.</span></span> <span data-ttu-id="ddb06-210">Ejemplos: escenarios de pérdida de datos, espacio en disco insuficiente.</span><span class="sxs-lookup"><span data-stu-id="ddb06-210">Examples: data loss scenarios, out of disk space.</span></span>

<span data-ttu-id="ddb06-211">Puede usar el nivel de registro para controlar la cantidad de salida del registro que se escribe en un medio de almacenamiento determinado o ventana de presentación.</span><span class="sxs-lookup"><span data-stu-id="ddb06-211">You can use the log level to control how much log output is written to a particular storage medium or display window.</span></span> <span data-ttu-id="ddb06-212">Por ejemplo, en producción es posible que quiera que todos los registros de nivel `Information` e inferiores vayan a un almacén de datos de volumen y que todos los registros de nivel `Warning` y superiores vayan un almacén de datos de valor.</span><span class="sxs-lookup"><span data-stu-id="ddb06-212">For example, in production you might want all logs of `Information` level and lower to go to a volume data store, and all logs of `Warning` level and higher to go to a value data store.</span></span> <span data-ttu-id="ddb06-213">Durante el desarrollo, es posible que normalmente envíe los registros de gravedad `Warning` o superior a la consola.</span><span class="sxs-lookup"><span data-stu-id="ddb06-213">During development, you might normally send logs of `Warning` or higher severity to the console.</span></span> <span data-ttu-id="ddb06-214">Después, si tiene que solucionar problemas, puede agregar el nivel `Debug`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-214">Then when you need to troubleshoot, you can add `Debug` level.</span></span> <span data-ttu-id="ddb06-215">En la sección [Filtrado del registro](#log-filtering) de este artículo se explica cómo controlar los niveles de registro que controla un proveedor.</span><span class="sxs-lookup"><span data-stu-id="ddb06-215">The [Log filtering](#log-filtering) section later in this article explains how to control which log levels a provider handles.</span></span>

<span data-ttu-id="ddb06-216">La plataforma ASP.NET Core escribe registros de nivel `Debug` para los eventos de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="ddb06-216">The ASP.NET Core framework writes `Debug` level logs for framework events.</span></span> <span data-ttu-id="ddb06-217">En los ejemplos de registro anteriores de este artículo se excluyeron los registros por debajo del nivel `Information`, por lo que no se mostraron los registros de nivel `Debug`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-217">The log examples earlier in this article excluded logs below `Information` level, so no `Debug` level logs were shown.</span></span> <span data-ttu-id="ddb06-218">Este es un ejemplo de registros de consola si ejecuta la aplicación de ejemplo configurada para mostrar `Debug` y registros superiores para el proveedor de la consola.</span><span class="sxs-lookup"><span data-stu-id="ddb06-218">Here's an example of console logs if you run the sample application configured to show `Debug` and higher logs for the console provider.</span></span>

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

## <a name="log-event-id"></a><span data-ttu-id="ddb06-219">Id. de evento del registro</span><span class="sxs-lookup"><span data-stu-id="ddb06-219">Log event ID</span></span>

<span data-ttu-id="ddb06-220">Cada vez que se escribe un registro, puede especificar un *Id. de evento*.</span><span class="sxs-lookup"><span data-stu-id="ddb06-220">Each time you write a log, you can specify an *event ID*.</span></span> <span data-ttu-id="ddb06-221">La aplicación de ejemplo lo hace mediante una clase `LoggingEvents` definida de forma local:</span><span class="sxs-lookup"><span data-stu-id="ddb06-221">The sample app does this by using a locally-defined `LoggingEvents` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

<span data-ttu-id="ddb06-222">Un identificador de evento es un valor entero que se puede usar para asociar un conjunto de eventos registrados entre sí.</span><span class="sxs-lookup"><span data-stu-id="ddb06-222">An event ID is an integer value that you can use to associate a set of logged events with one another.</span></span> <span data-ttu-id="ddb06-223">Por ejemplo, un registro para agregar un elemento a un carro de la compra podría tener el identificador de evento 1000 y un registro para completar una compra podría tener el identificador de evento 1001.</span><span class="sxs-lookup"><span data-stu-id="ddb06-223">For instance, a log for adding an item to a shopping cart could be event ID 1000 and a log for completing a purchase could be event ID 1001.</span></span>

<span data-ttu-id="ddb06-224">En la salida del registro, el identificador de evento podría estar almacenado en un campo o incluido en el mensaje de texto, en función del proveedor.</span><span class="sxs-lookup"><span data-stu-id="ddb06-224">In logging output, the event ID may be stored in a field or included in the text message, depending on the provider.</span></span> <span data-ttu-id="ddb06-225">El proveedor de depuración no muestra los identificadores de evento, pero el proveedor de la consola los muestra entre paréntesis después de la categoría:</span><span class="sxs-lookup"><span data-stu-id="ddb06-225">The Debug provider doesn't show event IDs, but the console provider shows them in brackets after the category:</span></span>

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a><span data-ttu-id="ddb06-226">Plantilla de mensaje de registro</span><span class="sxs-lookup"><span data-stu-id="ddb06-226">Log message template</span></span>

<span data-ttu-id="ddb06-227">Cada vez que se escribe un mensaje de registro, se proporciona una plantilla de mensaje.</span><span class="sxs-lookup"><span data-stu-id="ddb06-227">Each time you write a log message, you provide a message template.</span></span> <span data-ttu-id="ddb06-228">La plantilla de mensaje puede ser una cadena o puede contener marcadores de posición con nombre en los que se colocan los valores de argumento.</span><span class="sxs-lookup"><span data-stu-id="ddb06-228">The message template can be a string or it can contain named placeholders into which argument values are placed.</span></span> <span data-ttu-id="ddb06-229">La plantilla no es una cadena de formato y los marcadores de posición deben tener un nombre, no un número.</span><span class="sxs-lookup"><span data-stu-id="ddb06-229">The template isn't a format string, and placeholders should be named, not numbered.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

<span data-ttu-id="ddb06-230">El orden de los marcadores de posición, no sus nombres, determina qué parámetros se usan para proporcionar sus valores.</span><span class="sxs-lookup"><span data-stu-id="ddb06-230">The order of placeholders, not their names, determines which parameters are used to provide their values.</span></span> <span data-ttu-id="ddb06-231">Si tiene el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="ddb06-231">If you have the following code:</span></span>

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

<span data-ttu-id="ddb06-232">El mensaje del registro resultante tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="ddb06-232">The resulting log message looks like this:</span></span>

```
Parameter values: parm1, parm2
```

<span data-ttu-id="ddb06-233">La plataforma de registro aplica este formato de mensajes para que los proveedores de registro puedan implementar el [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="ddb06-233">The logging framework does message formatting in this way to make it possible for logging providers to implement [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span> <span data-ttu-id="ddb06-234">Dado que los propios argumentos se pasan al sistema de registro, no solo la plantilla de mensaje con formato, los proveedores de registro pueden almacenar los valores de parámetros como campos además de la plantilla de mensaje.</span><span class="sxs-lookup"><span data-stu-id="ddb06-234">Because the arguments themselves are passed to the logging system, not just the formatted message template, logging providers can store the parameter values as fields in addition to the message template.</span></span> <span data-ttu-id="ddb06-235">Si va a dirigir la salida del registro a Azure Table Storage y la llamada al método de registrador tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="ddb06-235">If you're directing your log output to Azure Table Storage and your logger method call looks like this:</span></span>

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

<span data-ttu-id="ddb06-236">Cada entidad de la Tabla de Azure puede tener propiedades `ID` y `RequestTime`, lo que simplifica las consultas en los datos de registro.</span><span class="sxs-lookup"><span data-stu-id="ddb06-236">Each Azure Table entity can have `ID` and `RequestTime` properties, which simplifies queries on log data.</span></span> <span data-ttu-id="ddb06-237">Puede buscar todos los registros dentro de un intervalo `RequestTime` determinado sin necesidad de analizar el tiempo de espera del mensaje de texto.</span><span class="sxs-lookup"><span data-stu-id="ddb06-237">You can find all logs within a particular `RequestTime` range without the need to parse the time out of the text message.</span></span>

## <a name="logging-exceptions"></a><span data-ttu-id="ddb06-238">Excepciones de registro</span><span class="sxs-lookup"><span data-stu-id="ddb06-238">Logging exceptions</span></span>

<span data-ttu-id="ddb06-239">Los métodos de registrador tienen sobrecargas que le permiten pasar una excepción, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ddb06-239">The logger methods have overloads that let you pass in an exception, as in the following example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

<span data-ttu-id="ddb06-240">Cada proveedor controla la información de la excepción de maneras diferentes.</span><span class="sxs-lookup"><span data-stu-id="ddb06-240">Different providers handle the exception information in different ways.</span></span> <span data-ttu-id="ddb06-241">Este es un ejemplo de salida del proveedor de depuración del código mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="ddb06-241">Here's an example of Debug provider output from the code shown above.</span></span>

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a><span data-ttu-id="ddb06-242">Filtrado del registro</span><span class="sxs-lookup"><span data-stu-id="ddb06-242">Log filtering</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ddb06-243">Puede especificar un nivel de registro mínimo para un proveedor y una categoría específicos, o para todos los proveedores o todas las categorías.</span><span class="sxs-lookup"><span data-stu-id="ddb06-243">You can specify a minimum log level for a specific provider and category or for all providers or all categories.</span></span> <span data-ttu-id="ddb06-244">Los registros por debajo del nivel mínimo no se pasan a ese proveedor, de modo que no se muestran o almacenan.</span><span class="sxs-lookup"><span data-stu-id="ddb06-244">Any logs below the minimum level aren't passed to that provider, so they don't get displayed or stored.</span></span>

<span data-ttu-id="ddb06-245">Si quiere suprimir todos los registros, puede especificar `LogLevel.None` como el nivel de registro mínimo.</span><span class="sxs-lookup"><span data-stu-id="ddb06-245">If you want to suppress all logs, you can specify `LogLevel.None` as the minimum log level.</span></span> <span data-ttu-id="ddb06-246">El valor entero de `LogLevel.None` es 6, que es superior a `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="ddb06-246">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

### <a name="create-filter-rules-in-configuration"></a><span data-ttu-id="ddb06-247">Creación de reglas de filtro en la configuración</span><span class="sxs-lookup"><span data-stu-id="ddb06-247">Create filter rules in configuration</span></span>

<span data-ttu-id="ddb06-248">Las plantillas de proyecto crean código que llama a `CreateDefaultBuilder` para configurar el registro para los proveedores de consola y de depuración.</span><span class="sxs-lookup"><span data-stu-id="ddb06-248">The project templates create code that calls `CreateDefaultBuilder` to set up logging for the Console and Debug providers.</span></span> <span data-ttu-id="ddb06-249">El método `CreateDefaultBuilder` también establece el registro para buscar la configuración en una sección `Logging`, mediante código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="ddb06-249">The `CreateDefaultBuilder` method also sets up logging to look for configuration in a `Logging` section, using code like the following:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=15)]

<span data-ttu-id="ddb06-250">Los datos de configuración especifican niveles de registro mínimo por proveedor y categoría, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ddb06-250">The configuration data specifies minimum log levels by provider and category, as in the following example:</span></span>

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

<span data-ttu-id="ddb06-251">Este archivo JSON crea seis reglas de filtro, una para el proveedor de depuración, cuatro para el proveedor de la consola y una que se aplica a todos los proveedores.</span><span class="sxs-lookup"><span data-stu-id="ddb06-251">This JSON creates six filter rules, one for the Debug provider, four for the Console provider, and one that applies to all providers.</span></span> <span data-ttu-id="ddb06-252">Más adelante verá que solo una de estas reglas se elige para cada proveedor cuando se crea un objeto `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-252">You'll see later how just one of these rules is chosen for each provider when an `ILogger` object is created.</span></span>

### <a name="filter-rules-in-code"></a><span data-ttu-id="ddb06-253">Reglas de filtro en el código</span><span class="sxs-lookup"><span data-stu-id="ddb06-253">Filter rules in code</span></span>

<span data-ttu-id="ddb06-254">Puede registrar las reglas de filtro en el código, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ddb06-254">You can register filter rules in code, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

<span data-ttu-id="ddb06-255">El segundo `AddFilter` especifica el proveedor de depuración mediante su nombre de tipo.</span><span class="sxs-lookup"><span data-stu-id="ddb06-255">The second `AddFilter` specifies the Debug provider by using its type name.</span></span> <span data-ttu-id="ddb06-256">El primer `AddFilter` se aplica a todos los proveedores, dado que no especifica un tipo de proveedor.</span><span class="sxs-lookup"><span data-stu-id="ddb06-256">The first `AddFilter` applies to all providers because it doesn't specify a provider type.</span></span>

### <a name="how-filtering-rules-are-applied"></a><span data-ttu-id="ddb06-257">Cómo se aplican las reglas de filtro</span><span class="sxs-lookup"><span data-stu-id="ddb06-257">How filtering rules are applied</span></span>

<span data-ttu-id="ddb06-258">Los datos de configuración y el código de `AddFilter` que se muestran en los ejemplos anteriores crean las reglas que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="ddb06-258">The configuration data and the `AddFilter` code shown in the preceding examples create the rules shown in the following table.</span></span> <span data-ttu-id="ddb06-259">Las seis primeras proceden del ejemplo de configuración y las dos últimas del ejemplo de código.</span><span class="sxs-lookup"><span data-stu-id="ddb06-259">The first six come from the configuration example and the last two come from the code example.</span></span>

| <span data-ttu-id="ddb06-260">número</span><span class="sxs-lookup"><span data-stu-id="ddb06-260">Number</span></span> | <span data-ttu-id="ddb06-261">Proveedor</span><span class="sxs-lookup"><span data-stu-id="ddb06-261">Provider</span></span>      | <span data-ttu-id="ddb06-262">Categorías que comienzan por...</span><span class="sxs-lookup"><span data-stu-id="ddb06-262">Categories that begin with ...</span></span>          | <span data-ttu-id="ddb06-263">Nivel de registro mínimo</span><span class="sxs-lookup"><span data-stu-id="ddb06-263">Minimum log level</span></span> |
| :----: | ------------- | --------------------------------------- | ----------------- |
| <span data-ttu-id="ddb06-264">1</span><span class="sxs-lookup"><span data-stu-id="ddb06-264">1</span></span>      | <span data-ttu-id="ddb06-265">Depuración</span><span class="sxs-lookup"><span data-stu-id="ddb06-265">Debug</span></span>         | <span data-ttu-id="ddb06-266">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="ddb06-266">All categories</span></span>                          | <span data-ttu-id="ddb06-267">Información</span><span class="sxs-lookup"><span data-stu-id="ddb06-267">Information</span></span>       |
| <span data-ttu-id="ddb06-268">2</span><span class="sxs-lookup"><span data-stu-id="ddb06-268">2</span></span>      | <span data-ttu-id="ddb06-269">Consola</span><span class="sxs-lookup"><span data-stu-id="ddb06-269">Console</span></span>       | <span data-ttu-id="ddb06-270">Microsoft.AspNetCore.Mvc.Razor.Internal</span><span class="sxs-lookup"><span data-stu-id="ddb06-270">Microsoft.AspNetCore.Mvc.Razor.Internal</span></span> | <span data-ttu-id="ddb06-271">Advertencia</span><span class="sxs-lookup"><span data-stu-id="ddb06-271">Warning</span></span>           |
| <span data-ttu-id="ddb06-272">3</span><span class="sxs-lookup"><span data-stu-id="ddb06-272">3</span></span>      | <span data-ttu-id="ddb06-273">Consola</span><span class="sxs-lookup"><span data-stu-id="ddb06-273">Console</span></span>       | <span data-ttu-id="ddb06-274">Microsoft.AspNetCore.Mvc.Razor.Razor</span><span class="sxs-lookup"><span data-stu-id="ddb06-274">Microsoft.AspNetCore.Mvc.Razor.Razor</span></span>    | <span data-ttu-id="ddb06-275">Depuración</span><span class="sxs-lookup"><span data-stu-id="ddb06-275">Debug</span></span>             |
| <span data-ttu-id="ddb06-276">4</span><span class="sxs-lookup"><span data-stu-id="ddb06-276">4</span></span>      | <span data-ttu-id="ddb06-277">Consola</span><span class="sxs-lookup"><span data-stu-id="ddb06-277">Console</span></span>       | <span data-ttu-id="ddb06-278">Microsoft.AspNetCore.Mvc.Razor</span><span class="sxs-lookup"><span data-stu-id="ddb06-278">Microsoft.AspNetCore.Mvc.Razor</span></span>          | <span data-ttu-id="ddb06-279">Error</span><span class="sxs-lookup"><span data-stu-id="ddb06-279">Error</span></span>             |
| <span data-ttu-id="ddb06-280">5</span><span class="sxs-lookup"><span data-stu-id="ddb06-280">5</span></span>      | <span data-ttu-id="ddb06-281">Consola</span><span class="sxs-lookup"><span data-stu-id="ddb06-281">Console</span></span>       | <span data-ttu-id="ddb06-282">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="ddb06-282">All categories</span></span>                          | <span data-ttu-id="ddb06-283">Información</span><span class="sxs-lookup"><span data-stu-id="ddb06-283">Information</span></span>       |
| <span data-ttu-id="ddb06-284">6</span><span class="sxs-lookup"><span data-stu-id="ddb06-284">6</span></span>      | <span data-ttu-id="ddb06-285">Todos los proveedores</span><span class="sxs-lookup"><span data-stu-id="ddb06-285">All providers</span></span> | <span data-ttu-id="ddb06-286">Todas las categorías</span><span class="sxs-lookup"><span data-stu-id="ddb06-286">All categories</span></span>                          | <span data-ttu-id="ddb06-287">Depuración</span><span class="sxs-lookup"><span data-stu-id="ddb06-287">Debug</span></span>             |
| <span data-ttu-id="ddb06-288">7</span><span class="sxs-lookup"><span data-stu-id="ddb06-288">7</span></span>      | <span data-ttu-id="ddb06-289">Todos los proveedores</span><span class="sxs-lookup"><span data-stu-id="ddb06-289">All providers</span></span> | <span data-ttu-id="ddb06-290">Sistema</span><span class="sxs-lookup"><span data-stu-id="ddb06-290">System</span></span>                                  | <span data-ttu-id="ddb06-291">Depuración</span><span class="sxs-lookup"><span data-stu-id="ddb06-291">Debug</span></span>             |
| <span data-ttu-id="ddb06-292">8</span><span class="sxs-lookup"><span data-stu-id="ddb06-292">8</span></span>      | <span data-ttu-id="ddb06-293">Depuración</span><span class="sxs-lookup"><span data-stu-id="ddb06-293">Debug</span></span>         | <span data-ttu-id="ddb06-294">Microsoft</span><span class="sxs-lookup"><span data-stu-id="ddb06-294">Microsoft</span></span>                               | <span data-ttu-id="ddb06-295">Seguimiento</span><span class="sxs-lookup"><span data-stu-id="ddb06-295">Trace</span></span>             |

<span data-ttu-id="ddb06-296">Cuando se crea un objeto `ILogger` con el que escribir los registros, el objeto `ILoggerFactory` selecciona una sola regla por proveedor para aplicar a ese registrador.</span><span class="sxs-lookup"><span data-stu-id="ddb06-296">When you create an `ILogger` object to write logs with, the `ILoggerFactory` object selects a single rule per provider to apply to that logger.</span></span> <span data-ttu-id="ddb06-297">Todos los mensajes escritos por ese objeto `ILogger` se filtran según las reglas seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="ddb06-297">All messages written by that `ILogger` object are filtered based on the selected rules.</span></span> <span data-ttu-id="ddb06-298">De las reglas disponibles se selecciona la más específica posible para cada par de categoría y proveedor.</span><span class="sxs-lookup"><span data-stu-id="ddb06-298">The most specific rule possible for each provider and category pair is selected from the available rules.</span></span>

<span data-ttu-id="ddb06-299">Cuando se crea un `ILogger` para una categoría determinada, se usa el algoritmo siguiente para cada proveedor:</span><span class="sxs-lookup"><span data-stu-id="ddb06-299">The following algorithm is used for each provider when an `ILogger` is created for a given category:</span></span>

* <span data-ttu-id="ddb06-300">Se seleccionan todas las reglas que coinciden con el proveedor o su alias.</span><span class="sxs-lookup"><span data-stu-id="ddb06-300">Select all rules that match the provider or its alias.</span></span> <span data-ttu-id="ddb06-301">Si no se encuentra ninguna, se seleccionan todas las reglas con un proveedor vacío.</span><span class="sxs-lookup"><span data-stu-id="ddb06-301">If none are found, select all rules with an empty provider.</span></span>
* <span data-ttu-id="ddb06-302">Del resultado del paso anterior, se seleccionan las reglas con el prefijo de categoría coincidente más largo.</span><span class="sxs-lookup"><span data-stu-id="ddb06-302">From the result of the preceding step, select rules with longest matching category prefix.</span></span> <span data-ttu-id="ddb06-303">Si no se encuentra ninguna, se seleccionan todas las reglas que no especifican una categoría.</span><span class="sxs-lookup"><span data-stu-id="ddb06-303">If none are found, select all rules that don't specify a category.</span></span>
* <span data-ttu-id="ddb06-304">Si se seleccionan varias reglas, se toma la **última**.</span><span class="sxs-lookup"><span data-stu-id="ddb06-304">If multiple rules are selected take the **last** one.</span></span>
* <span data-ttu-id="ddb06-305">Si no se selecciona ninguna regla, se usa `MinimumLevel`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-305">If no rules are selected, use `MinimumLevel`.</span></span>

<span data-ttu-id="ddb06-306">Por ejemplo, supongamos que tiene la lista de reglas anterior y crea un objeto `ILogger` para la categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span><span class="sxs-lookup"><span data-stu-id="ddb06-306">For example, suppose you have the preceding list of rules and you create an `ILogger` object for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":</span></span>

* <span data-ttu-id="ddb06-307">Para el proveedor de depuración, se aplican las reglas 1, 6 y 8.</span><span class="sxs-lookup"><span data-stu-id="ddb06-307">For the Debug provider, rules 1, 6, and 8 apply.</span></span> <span data-ttu-id="ddb06-308">La regla 8 es la más específica, por lo que se selecciona.</span><span class="sxs-lookup"><span data-stu-id="ddb06-308">Rule 8 is most specific, so that's the one selected.</span></span>
* <span data-ttu-id="ddb06-309">Para el proveedor de la consola, se aplican las reglas 3, 4, 5 y 6.</span><span class="sxs-lookup"><span data-stu-id="ddb06-309">For the Console provider, rules 3, 4, 5, and 6 apply.</span></span> <span data-ttu-id="ddb06-310">La regla 3 es la más específica.</span><span class="sxs-lookup"><span data-stu-id="ddb06-310">Rule 3 is most specific.</span></span>

<span data-ttu-id="ddb06-311">Cuando crea registros con un `ILogger` para la categoría "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", los registros de nivel `Trace` y superiores se dirigirán al proveedor de depuración y los registros de nivel `Debug` y superiores se dirigirán al proveedor de consola.</span><span class="sxs-lookup"><span data-stu-id="ddb06-311">When you create logs with an `ILogger` for category "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine", logs of `Trace` level and above will go to the Debug provider, and logs of `Debug` level and above will go to the Console provider.</span></span>

### <a name="provider-aliases"></a><span data-ttu-id="ddb06-312">Alias de proveedor</span><span class="sxs-lookup"><span data-stu-id="ddb06-312">Provider aliases</span></span>

<span data-ttu-id="ddb06-313">Puede usar el nombre de tipo para especificar un proveedor en la configuración, pero cada proveedor define un *alias* más breve que es más fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="ddb06-313">You can use the type name to specify a provider in configuration, but each provider defines a shorter *alias* that's easier to use.</span></span> <span data-ttu-id="ddb06-314">Para los proveedores integrados, use los alias siguientes:</span><span class="sxs-lookup"><span data-stu-id="ddb06-314">For the built-in providers, use the following aliases:</span></span>

* <span data-ttu-id="ddb06-315">Consola</span><span class="sxs-lookup"><span data-stu-id="ddb06-315">Console</span></span>
* <span data-ttu-id="ddb06-316">Depuración</span><span class="sxs-lookup"><span data-stu-id="ddb06-316">Debug</span></span>
* <span data-ttu-id="ddb06-317">EventLog</span><span class="sxs-lookup"><span data-stu-id="ddb06-317">EventLog</span></span>
* <span data-ttu-id="ddb06-318">AzureAppServices</span><span class="sxs-lookup"><span data-stu-id="ddb06-318">AzureAppServices</span></span>
* <span data-ttu-id="ddb06-319">TraceSource</span><span class="sxs-lookup"><span data-stu-id="ddb06-319">TraceSource</span></span>
* <span data-ttu-id="ddb06-320">EventSource</span><span class="sxs-lookup"><span data-stu-id="ddb06-320">EventSource</span></span>

### <a name="default-minimum-level"></a><span data-ttu-id="ddb06-321">Nivel mínimo predeterminado</span><span class="sxs-lookup"><span data-stu-id="ddb06-321">Default minimum level</span></span>

<span data-ttu-id="ddb06-322">Hay una configuración de nivel mínimo que solo tiene efecto si no se aplica ninguna regla de configuración o código para un proveedor y una categoría determinados.</span><span class="sxs-lookup"><span data-stu-id="ddb06-322">There's a minimum level setting that takes effect only if no rules from configuration or code apply for a given provider and category.</span></span> <span data-ttu-id="ddb06-323">En el ejemplo siguiente se muestra cómo establecer el nivel mínimo:</span><span class="sxs-lookup"><span data-stu-id="ddb06-323">The following example shows how to set the minimum level:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

<span data-ttu-id="ddb06-324">Si no establece explícitamente el nivel mínimo, el valor predeterminado es `Information`, lo que significa que los registros `Trace` y `Debug` se omiten.</span><span class="sxs-lookup"><span data-stu-id="ddb06-324">If you don't explicitly set the minimum level, the default value is `Information`, which means that `Trace` and `Debug` logs are ignored.</span></span>

### <a name="filter-functions"></a><span data-ttu-id="ddb06-325">Funciones de filtro</span><span class="sxs-lookup"><span data-stu-id="ddb06-325">Filter functions</span></span>

<span data-ttu-id="ddb06-326">Puede escribir código en una función de filtro para aplicar las reglas de filtrado.</span><span class="sxs-lookup"><span data-stu-id="ddb06-326">You can write code in a filter function to apply filtering rules.</span></span> <span data-ttu-id="ddb06-327">Se invoca una función de filtro para todos los proveedores y categorías que no tienen reglas asignadas mediante configuración o código.</span><span class="sxs-lookup"><span data-stu-id="ddb06-327">A filter function is invoked for all providers and categories that don't have rules assigned to them by configuration or code.</span></span> <span data-ttu-id="ddb06-328">El código de la función tiene acceso al tipo de proveedor, categoría y nivel de registro para decidir si se debe registrar un mensaje.</span><span class="sxs-lookup"><span data-stu-id="ddb06-328">Code in the function has access to the provider type, category, and log level to decide whether or not a message should be logged.</span></span> <span data-ttu-id="ddb06-329">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ddb06-329">For example:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ddb06-330">Algunos proveedores de registro permiten especificar cuándo deben escribirse los registros en un medio de almacenamiento o ignorarse en función de la categoría y el nivel de registro.</span><span class="sxs-lookup"><span data-stu-id="ddb06-330">Some logging providers let you specify when logs should be written to a storage medium or ignored based on log level and category.</span></span>

<span data-ttu-id="ddb06-331">Los métodos de extensión `AddConsole` y `AddDebug` proporcionan sobrecargas que le permiten pasar criterios de filtrado.</span><span class="sxs-lookup"><span data-stu-id="ddb06-331">The `AddConsole` and `AddDebug` extension methods provide overloads that let you pass in filtering criteria.</span></span> <span data-ttu-id="ddb06-332">El código de ejemplo siguiente hace que el proveedor de la consola ignore los registros por debajo del nivel `Warning`, mientras que el proveedor de depuración omite los registros creados por la plataforma.</span><span class="sxs-lookup"><span data-stu-id="ddb06-332">The following sample code causes the console provider to ignore logs below `Warning` level, while the Debug provider ignores logs that the framework creates.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

<span data-ttu-id="ddb06-333">El método `AddEventLog` tiene una sobrecarga que toma una instancia de `EventLogSettings`, que puede contener una función de filtrado en su propiedad `Filter`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-333">The `AddEventLog` method has an overload that takes an `EventLogSettings` instance, which may contain a filtering function in its `Filter` property.</span></span> <span data-ttu-id="ddb06-334">El proveedor de TraceSource no proporciona ninguna de estas sobrecargas, dado que su nivel de registro y otros parámetros se basan en el `SourceSwitch` y `TraceListener` que usa.</span><span class="sxs-lookup"><span data-stu-id="ddb06-334">The TraceSource provider doesn't provide any of those overloads, since its logging level and other parameters are based on the `SourceSwitch` and `TraceListener` it uses.</span></span>

<span data-ttu-id="ddb06-335">Puede establecer reglas de filtrado para todos los proveedores que están registrados con un instancia de `ILoggerFactory` mediante el método de extensión `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-335">You can set filtering rules for all providers that are registered with an `ILoggerFactory` instance by using the `WithFilter` extension method.</span></span> <span data-ttu-id="ddb06-336">En el ejemplo siguiente se limitan los registros de la plataforma (la categoría comienza con "Microsoft" o "System") a las advertencias, mientras se permite el registro de la aplicación en el nivel de depuración.</span><span class="sxs-lookup"><span data-stu-id="ddb06-336">The example below limits framework logs (category begins with "Microsoft" or "System") to warnings while letting the app log at debug level.</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

<span data-ttu-id="ddb06-337">Si quiere usar el filtrado para impedir que se escriban todos los registros para una categoría concreta, puede especificar `LogLevel.None` como el nivel de registro mínimo de esa categoría.</span><span class="sxs-lookup"><span data-stu-id="ddb06-337">If you want to use filtering to prevent all logs from being written for a particular category, you can specify `LogLevel.None` as the minimum log level for that category.</span></span> <span data-ttu-id="ddb06-338">El valor entero de `LogLevel.None` es 6, que es superior a `LogLevel.Critical` (5).</span><span class="sxs-lookup"><span data-stu-id="ddb06-338">The integer value of `LogLevel.None` is 6, which is higher than `LogLevel.Critical` (5).</span></span>

<span data-ttu-id="ddb06-339">El paquete NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) proporciona el método de extensión `WithFilter`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-339">The `WithFilter` extension method is provided by the [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet package.</span></span> <span data-ttu-id="ddb06-340">El método devuelve una instancia nueva de `ILoggerFactory` que filtrará los mensajes de registro pasados a todos los proveedores de registrador registrados con ella.</span><span class="sxs-lookup"><span data-stu-id="ddb06-340">The method returns a new `ILoggerFactory` instance that will filter the log messages passed to all logger providers registered with it.</span></span> <span data-ttu-id="ddb06-341">No afecta a ninguna otra instancia de `ILoggerFactory`, incluida la instancia de `ILoggerFactory` original.</span><span class="sxs-lookup"><span data-stu-id="ddb06-341">It doesn't affect any other `ILoggerFactory` instances, including the original `ILoggerFactory` instance.</span></span>

::: moniker-end

## <a name="log-scopes"></a><span data-ttu-id="ddb06-342">Ámbitos de registro</span><span class="sxs-lookup"><span data-stu-id="ddb06-342">Log scopes</span></span>

<span data-ttu-id="ddb06-343">Puede agrupar un conjunto de operaciones lógicas dentro de un *ámbito* para adjuntar los mismos datos para cada registro que se crea como parte de ese conjunto.</span><span class="sxs-lookup"><span data-stu-id="ddb06-343">You can group a set of logical operations within a *scope* in order to attach the same data to each log that's created as part of that set.</span></span> <span data-ttu-id="ddb06-344">Por ejemplo, es posible que quiera que todos los registros creados como parte del procesamiento de una transacción incluyan el identificador de la transacción.</span><span class="sxs-lookup"><span data-stu-id="ddb06-344">For example, you might want every log created as part of processing a transaction to include the transaction ID.</span></span>

<span data-ttu-id="ddb06-345">Un ámbito es un tipo `IDisposable` devuelto por el método [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) que se conserva hasta que se elimina.</span><span class="sxs-lookup"><span data-stu-id="ddb06-345">A scope is an `IDisposable` type that's returned by the [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) method and lasts until it's disposed.</span></span> <span data-ttu-id="ddb06-346">Para usar un ámbito, las llamadas de registrador se encapsulan en un bloque `using`, como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="ddb06-346">You use a scope by wrapping your logger calls in a `using` block, as shown here:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

<span data-ttu-id="ddb06-347">El código siguiente permite ámbitos para el proveedor de la consola:</span><span class="sxs-lookup"><span data-stu-id="ddb06-347">The following code enables scopes for the console provider:</span></span>

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="ddb06-348">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ddb06-348">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="ddb06-349">Es necesario configurar la opción del registrador de consola `IncludeScopes` para habilitar el registro basado en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="ddb06-349">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>
>
> <span data-ttu-id="ddb06-350">Para obtener información sobre la configuración, consulte la sección [Configuración](#configuration).</span><span class="sxs-lookup"><span data-stu-id="ddb06-350">For information on configuration, see the [Configuration](#configuration) section.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ddb06-351">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ddb06-351">*Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> <span data-ttu-id="ddb06-352">Es necesario configurar la opción del registrador de consola `IncludeScopes` para habilitar el registro basado en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="ddb06-352">Configuring the `IncludeScopes` console logger option is required to enable scope-based logging.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ddb06-353">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ddb06-353">*Startup.cs*:</span></span>

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

<span data-ttu-id="ddb06-354">Cada mensaje de registro incluye la información de ámbito:</span><span class="sxs-lookup"><span data-stu-id="ddb06-354">Each log message includes the scoped information:</span></span>

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a><span data-ttu-id="ddb06-355">Proveedores de registro integrados</span><span class="sxs-lookup"><span data-stu-id="ddb06-355">Built-in logging providers</span></span>

<span data-ttu-id="ddb06-356">ASP.NET Core incluye los proveedores siguientes:</span><span class="sxs-lookup"><span data-stu-id="ddb06-356">ASP.NET Core ships the following providers:</span></span>

* [<span data-ttu-id="ddb06-357">Consola</span><span class="sxs-lookup"><span data-stu-id="ddb06-357">Console</span></span>](#console-provider)
* [<span data-ttu-id="ddb06-358">Depurar</span><span class="sxs-lookup"><span data-stu-id="ddb06-358">Debug</span></span>](#debug-provider)
* [<span data-ttu-id="ddb06-359">EventSource</span><span class="sxs-lookup"><span data-stu-id="ddb06-359">EventSource</span></span>](#eventsource-provider)
* [<span data-ttu-id="ddb06-360">EventLog</span><span class="sxs-lookup"><span data-stu-id="ddb06-360">EventLog</span></span>](#windows-eventlog-provider)
* [<span data-ttu-id="ddb06-361">TraceSource</span><span class="sxs-lookup"><span data-stu-id="ddb06-361">TraceSource</span></span>](#tracesource-provider)
* [<span data-ttu-id="ddb06-362">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ddb06-362">Azure App Service</span></span>](#azure-app-service-provider)

### <a name="console-provider"></a><span data-ttu-id="ddb06-363">Proveedor de la consola</span><span class="sxs-lookup"><span data-stu-id="ddb06-363">Console provider</span></span>

<span data-ttu-id="ddb06-364">El paquete de proveedor [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envía la salida del registro a la consola.</span><span class="sxs-lookup"><span data-stu-id="ddb06-364">The [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) provider package sends log output to the console.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

<span data-ttu-id="ddb06-365">[Las sobrecargas de AddConsole](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) permiten pasar un nivel de registro mínimo, una función de filtro y un valor booleano que indica si se admiten los ámbitos.</span><span class="sxs-lookup"><span data-stu-id="ddb06-365">[AddConsole overloads](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) let you pass in an a minimum log level, a filter function, and a boolean that indicates whether scopes are supported.</span></span> <span data-ttu-id="ddb06-366">Otra opción consiste en pasar un objeto `IConfiguration`, que puede especificar niveles de registro y si se admiten los ámbitos.</span><span class="sxs-lookup"><span data-stu-id="ddb06-366">Another option is to pass in an `IConfiguration` object, which can specify scopes support and logging levels.</span></span>

<span data-ttu-id="ddb06-367">Si está pensando en la posibilidad de usar la consola de proveedor en producción, tenga en cuenta que tiene un impacto significativo en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ddb06-367">If you are considering the console provider for use in production, be aware that it has a significant impact on performance.</span></span>

<span data-ttu-id="ddb06-368">Cuando se crea un proyecto en Visual Studio, el método `AddConsole` tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="ddb06-368">When you create a new project in Visual Studio, the `AddConsole` method looks like this:</span></span>

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

<span data-ttu-id="ddb06-369">Este código hace referencia a la sección `Logging` del archivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="ddb06-369">This code refers to the `Logging` section of the *appSettings.json* file:</span></span>

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

<span data-ttu-id="ddb06-370">La configuración que se muestra limita los registros de la plataforma a las advertencias mientras que permite a la aplicación registrar en el nivel de depuración, como se explica en la sección [Filtrado del registro](#log-filtering).</span><span class="sxs-lookup"><span data-stu-id="ddb06-370">The settings shown limit framework logs to warnings while allowing the app to log at debug level, as explained in the [Log filtering](#log-filtering) section.</span></span> <span data-ttu-id="ddb06-371">Para obtener más información, vea [Configuración](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="ddb06-371">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

::: moniker-end

### <a name="debug-provider"></a><span data-ttu-id="ddb06-372">Proveedor de depuración</span><span class="sxs-lookup"><span data-stu-id="ddb06-372">Debug provider</span></span>

<span data-ttu-id="ddb06-373">El paquete de proveedor [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) escribe la salida del registro mediante la clase [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (llamadas a métodos `Debug.WriteLine`).</span><span class="sxs-lookup"><span data-stu-id="ddb06-373">The [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) provider package writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class (`Debug.WriteLine` method calls).</span></span>

<span data-ttu-id="ddb06-374">En Linux, este proveedor escribe registros en */var/log/message*.</span><span class="sxs-lookup"><span data-stu-id="ddb06-374">On Linux, this provider writes logs to */var/log/message*.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

<span data-ttu-id="ddb06-375">[Las sobrecargas de AddDebug](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) permiten pasar un nivel mínimo de registro o una función de filtro.</span><span class="sxs-lookup"><span data-stu-id="ddb06-375">[AddDebug overloads](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) let you pass in a minimum log level or a filter function.</span></span>

::: moniker-end

### <a name="eventsource-provider"></a><span data-ttu-id="ddb06-376">Proveedor EventSource</span><span class="sxs-lookup"><span data-stu-id="ddb06-376">EventSource provider</span></span>

<span data-ttu-id="ddb06-377">Para las aplicaciones que tengan como destino ASP.NET Core 1.1.0 o una versión posterior, el paquete de proveedor [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) puede implementar el seguimiento de eventos.</span><span class="sxs-lookup"><span data-stu-id="ddb06-377">For apps that target ASP.NET Core 1.1.0 or later, the [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) provider package can implement event tracing.</span></span> <span data-ttu-id="ddb06-378">En Windows, usa [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span><span class="sxs-lookup"><span data-stu-id="ddb06-378">On Windows, it uses [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803).</span></span> <span data-ttu-id="ddb06-379">Es un proveedor multiplataforma, pero todavía no hay herramientas de recopilación y visualización de eventos para Linux o macOS.</span><span class="sxs-lookup"><span data-stu-id="ddb06-379">The provider is cross-platform, but there are no event collection and display tools yet for Linux or macOS.</span></span>

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

<span data-ttu-id="ddb06-380">Una buena manera de recopilar y ver los registros es usar la [utilidad PerfView](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="ddb06-380">A good way to collect and view logs is to use the [PerfView utility](https://github.com/Microsoft/perfview).</span></span> <span data-ttu-id="ddb06-381">Hay otras herramientas para ver los registros ETW, pero PerfView proporciona la mejor experiencia para trabajar con los eventos ETW emitidos por ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ddb06-381">There are other tools for viewing ETW logs, but PerfView provides the best experience for working with the ETW events emitted by ASP.NET.</span></span>

<span data-ttu-id="ddb06-382">Para configurar PerfView para la recopilación de eventos registrados por este proveedor, agregue la cadena `*Microsoft-Extensions-Logging` a la lista **Proveedores adicionales**.</span><span class="sxs-lookup"><span data-stu-id="ddb06-382">To configure PerfView for collecting events logged by this provider, add the string `*Microsoft-Extensions-Logging` to the **Additional Providers** list.</span></span> <span data-ttu-id="ddb06-383">(No olvide el asterisco al principio de la cadena).</span><span class="sxs-lookup"><span data-stu-id="ddb06-383">(Don't miss the asterisk at the start of the string.)</span></span>

![Proveedores adicionales de Perfview](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a><span data-ttu-id="ddb06-385">Proveedor EventLog de Windows</span><span class="sxs-lookup"><span data-stu-id="ddb06-385">Windows EventLog provider</span></span>

<span data-ttu-id="ddb06-386">El paquete de proveedor [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envía la salida del registro al Registro de eventos de Windows.</span><span class="sxs-lookup"><span data-stu-id="ddb06-386">The [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) provider package sends log output to the Windows Event Log.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

<span data-ttu-id="ddb06-387">[Las sobrecargas de AddEventLog](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) permiten pasar `EventLogSettings` o un nivel de registro mínimo.</span><span class="sxs-lookup"><span data-stu-id="ddb06-387">[AddEventLog overloads](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) let you pass in `EventLogSettings` or a minimum log level.</span></span>

::: moniker-end

### <a name="tracesource-provider"></a><span data-ttu-id="ddb06-388">Proveedor TraceSource</span><span class="sxs-lookup"><span data-stu-id="ddb06-388">TraceSource provider</span></span>

<span data-ttu-id="ddb06-389">El paquete de proveedor [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) usa las bibliotecas y proveedores de [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource).</span><span class="sxs-lookup"><span data-stu-id="ddb06-389">The [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) provider package uses the [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) libraries and providers.</span></span>

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

<span data-ttu-id="ddb06-390">[Las sobrecargas de AddTraceSource](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) permiten pasar un modificador de origen y un agente de escucha de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="ddb06-390">[AddTraceSource overloads](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) let you pass in a source switch and a trace listener.</span></span>

<span data-ttu-id="ddb06-391">Para usar este proveedor, una aplicación debe ejecutarse en .NET Framework (en lugar de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="ddb06-391">To use this provider, an application has to run on the .NET Framework (rather than .NET Core).</span></span> <span data-ttu-id="ddb06-392">El proveedor permite enrutar mensajes a una variedad de [agentes de escucha](/dotnet/framework/debug-trace-profile/trace-listeners), como [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) que se usa en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="ddb06-392">The provider lets you route messages to a variety of [listeners](/dotnet/framework/debug-trace-profile/trace-listeners), such as the [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr) used in the sample application.</span></span>

<span data-ttu-id="ddb06-393">En el ejemplo siguiente se configura un proveedor `TraceSource` que registra mensajes `Warning` y superiores en la ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="ddb06-393">The following example configures a `TraceSource` provider that logs `Warning` and higher messages to the console window.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

### <a name="azure-app-service-provider"></a><span data-ttu-id="ddb06-394">Proveedor Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ddb06-394">Azure App Service provider</span></span>

<span data-ttu-id="ddb06-395">El paquete de proveedor [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) escribe los registros en archivos de texto en el sistema de archivos de una aplicación de Azure App Service y en [Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) en una cuenta de Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="ddb06-395">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in an Azure Storage account.</span></span> <span data-ttu-id="ddb06-396">El paquete de proveedor está disponible para aplicaciones cuyo destino sea .NET Core 1.1 o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="ddb06-396">The provider package is available for apps targeting .NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ddb06-397">Si el destino es .NET Core, tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ddb06-397">If targeting .NET Core, note the following points:</span></span>

* <span data-ttu-id="ddb06-398">El paquete de proveedor está incluido en el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) de ASP.NET Core, pero no en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ddb06-398">The provider package is included in the ASP.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) but not in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="ddb06-399">No realice llamadas explícitas a [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span><span class="sxs-lookup"><span data-stu-id="ddb06-399">Don't explicitly call [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics).</span></span> <span data-ttu-id="ddb06-400">El proveedor está disponible automáticamente para la aplicación cuando esta se implementa en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ddb06-400">The provider is automatically made available to the app when the app is deployed to Azure App Service.</span></span>

<span data-ttu-id="ddb06-401">Si el destino es .NET Framework o hace referencia al metapaquete `Microsoft.AspNetCore.App`, agregue el paquete de proveedor al proyecto.</span><span class="sxs-lookup"><span data-stu-id="ddb06-401">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, add the provider package to the project.</span></span> <span data-ttu-id="ddb06-402">Invoque `AddAzureWebAppDiagnostics` en una instancia [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory):</span><span class="sxs-lookup"><span data-stu-id="ddb06-402">Invoke `AddAzureWebAppDiagnostics` on an [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) instance:</span></span>

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

<span data-ttu-id="ddb06-403">Una sobrecarga de [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) permite pasar [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings), con lo que se puede invalidar la configuración predeterminada, como la plantilla de salida del registro, el nombre de blob y el límite de tamaño de archivo.</span><span class="sxs-lookup"><span data-stu-id="ddb06-403">An [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) overload lets you pass in [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) with which you can override default settings, such as the logging output template, blob name, and file size limit.</span></span> <span data-ttu-id="ddb06-404">(La *plantilla de salida* es una plantilla de mensaje que se aplica a todos los registros además de la que se proporciona cuando se llama a un método `ILogger`).</span><span class="sxs-lookup"><span data-stu-id="ddb06-404">(*Output template* is a message template that's applied to all logs on top of the one that you provide when you call an `ILogger` method.)</span></span>

<span data-ttu-id="ddb06-405">Cuando se realiza una implementación en una aplicación de App Service, la aplicación respeta la configuración de la sección [Registros de diagnóstico](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) situada en la página **App Service** de Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ddb06-405">When you deploy to an App Service app, the app honors the settings in the [Diagnostic Logs](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="ddb06-406">Cuando se actualiza esta configuración, los cambios se aplican inmediatamente sin necesidad de reiniciar ni de volver a implementar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ddb06-406">When these settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

![Configuración de los registros de Azure](index/_static/azure-logging-settings.png)

<span data-ttu-id="ddb06-408">La ubicación predeterminada de los archivos de registro es la carpeta *D:\\home\\LogFiles\\Application* y el nombre de archivo predeterminado es *diagnostics-aaaammdd.txt*.</span><span class="sxs-lookup"><span data-stu-id="ddb06-408">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="ddb06-409">El límite de tamaño de archivo predeterminado es 10 MB, y el número máximo predeterminado de archivos que se conservan es 2.</span><span class="sxs-lookup"><span data-stu-id="ddb06-409">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="ddb06-410">El nombre de blob predeterminado es *{nombre-de-la-aplicación}{marca de tiempo}/aaaa/mm/dd/hh/{guid}-applicationLog.txt*.</span><span class="sxs-lookup"><span data-stu-id="ddb06-410">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span> <span data-ttu-id="ddb06-411">Para más información sobre el comportamiento predeterminado, vea [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span><span class="sxs-lookup"><span data-stu-id="ddb06-411">For more information about default behavior, see [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).</span></span>

<span data-ttu-id="ddb06-412">El proveedor solo funciona cuando el proyecto se ejecuta en el entorno de Azure.</span><span class="sxs-lookup"><span data-stu-id="ddb06-412">The provider only works when the project runs in the Azure environment.</span></span> <span data-ttu-id="ddb06-413">No tiene ningún efecto cuando el proyecto se ejecuta de manera local (no escribe en los archivos locales ni en el almacenamiento de desarrollo local de blobs).</span><span class="sxs-lookup"><span data-stu-id="ddb06-413">It has no effect when the project is run locally&mdash;it doesn't write to local files or local development storage for blobs.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="ddb06-414">Proveedores de registro de terceros</span><span class="sxs-lookup"><span data-stu-id="ddb06-414">Third-party logging providers</span></span>

<span data-ttu-id="ddb06-415">Plataformas de registro de terceros que funcionan con ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="ddb06-415">Third-party logging frameworks that work with ASP.NET Core:</span></span>

* <span data-ttu-id="ddb06-416">[elmah.io](https://elmah.io/) ([repositorio de GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="ddb06-416">[elmah.io](https://elmah.io/) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
* <span data-ttu-id="ddb06-417">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([repositorio de GitHub](https://github.com/mattwcole/gelf-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="ddb06-417">[Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
* <span data-ttu-id="ddb06-418">[JSNLog](http://jsnlog.com/) ([repositorio de GitHub](https://github.com/mperdeck/jsnlog))</span><span class="sxs-lookup"><span data-stu-id="ddb06-418">[JSNLog](http://jsnlog.com/) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
* <span data-ttu-id="ddb06-419">[KissLog.net](https://kisslog.net/) ([repositorio de GitHub](https://github.com/catalingavan/KissLog-net))</span><span class="sxs-lookup"><span data-stu-id="ddb06-419">[KissLog.net](https://kisslog.net/) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
* <span data-ttu-id="ddb06-420">[Loggr](http://loggr.net/) ([repositorio de GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="ddb06-420">[Loggr](http://loggr.net/) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
* <span data-ttu-id="ddb06-421">[NLog](http://nlog-project.org/) ([repositorio de GitHub](https://github.com/NLog/NLog.Extensions.Logging))</span><span class="sxs-lookup"><span data-stu-id="ddb06-421">[NLog](http://nlog-project.org/) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
* <span data-ttu-id="ddb06-422">[Serilog](https://serilog.net/) ([repositorio de GitHub](https://github.com/serilog/serilog-extensions-logging))</span><span class="sxs-lookup"><span data-stu-id="ddb06-422">[Serilog](https://serilog.net/) ([GitHub repo](https://github.com/serilog/serilog-extensions-logging))</span></span>

<span data-ttu-id="ddb06-423">Algunas plataformas de terceros pueden realizar [registro semántico, también conocido como registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span><span class="sxs-lookup"><span data-stu-id="ddb06-423">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="ddb06-424">El uso de una plataforma de terceros es similar al uso de uno de los proveedores integrados:</span><span class="sxs-lookup"><span data-stu-id="ddb06-424">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="ddb06-425">Agregue un paquete NuGet al proyecto.</span><span class="sxs-lookup"><span data-stu-id="ddb06-425">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="ddb06-426">Llame a un método de extensión en `ILoggerFactory`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-426">Call an extension method on `ILoggerFactory`.</span></span>

<span data-ttu-id="ddb06-427">Para más información, vea la documentación de cada plataforma.</span><span class="sxs-lookup"><span data-stu-id="ddb06-427">For more information, see each framework's documentation.</span></span>

## <a name="azure-log-streaming"></a><span data-ttu-id="ddb06-428">Secuencias de registro de Azure</span><span class="sxs-lookup"><span data-stu-id="ddb06-428">Azure log streaming</span></span>

<span data-ttu-id="ddb06-429">Las secuencias de registro de Azure permiten ver la actividad de registro en tiempo real desde:</span><span class="sxs-lookup"><span data-stu-id="ddb06-429">Azure log streaming enables you to view log activity in real time from:</span></span>

* <span data-ttu-id="ddb06-430">El servidor de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="ddb06-430">The application server</span></span>
* <span data-ttu-id="ddb06-431">El servidor web</span><span class="sxs-lookup"><span data-stu-id="ddb06-431">The web server</span></span>
* <span data-ttu-id="ddb06-432">Error del seguimiento de solicitudes</span><span class="sxs-lookup"><span data-stu-id="ddb06-432">Failed request tracing</span></span>

<span data-ttu-id="ddb06-433">Para configurar las secuencias de registro de Azure:</span><span class="sxs-lookup"><span data-stu-id="ddb06-433">To configure Azure log streaming:</span></span>

* <span data-ttu-id="ddb06-434">Navegue hasta la página **Registros de diagnóstico** desde la página de portal de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ddb06-434">Navigate to the **Diagnostics Logs** page from your application's portal page</span></span>
* <span data-ttu-id="ddb06-435">Establezca **Registro de la aplicación (sistema de archivos)** en Activado.</span><span class="sxs-lookup"><span data-stu-id="ddb06-435">Set **Application Logging (Filesystem)** to on.</span></span>

![Página de registros de diagnóstico de Azure Portal](index/_static/azure-diagnostic-logs.png)

<span data-ttu-id="ddb06-437">Navegue hasta la página **Secuencias de registro** para ver los mensajes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ddb06-437">Navigate to the **Log Streaming** page to view application messages.</span></span> <span data-ttu-id="ddb06-438">Se registran por la aplicación a través de la interfaz `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="ddb06-438">They're logged by application through the `ILogger` interface.</span></span>

![Secuencias de registro de aplicación de Azure Portal](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a><span data-ttu-id="ddb06-440">Registro de seguimiento de Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="ddb06-440">Azure Application Insights trace logging</span></span>

<span data-ttu-id="ddb06-441">El SDK de [Application Insights](https://azure.microsoft.com/services/application-insights/) es capaz de recopilar información de telemetría de seguimiento de registros generados mediante la infraestructura de registro de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ddb06-441">The [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK is capable of collecting trace telemetry from logs generated via the ASP.NET Core logging infrastructure.</span></span> <span data-ttu-id="ddb06-442">Para obtener más información, vea [Application Insights para ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core) y la [wiki de Microsoft/ApplicationInsights-aspnetcore sobre el registro](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span><span class="sxs-lookup"><span data-stu-id="ddb06-442">For more information, see [Application Insights for ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core) and [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ddb06-443">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ddb06-443">Additional resources</span></span>

* <xref:fundamentals/logging/loggermessage>
