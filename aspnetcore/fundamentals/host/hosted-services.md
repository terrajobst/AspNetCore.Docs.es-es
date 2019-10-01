---
title: Tareas en segundo plano con servicios hospedados en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo implementar tareas en segundo plano con servicios hospedados en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/18/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 8df86b10d7ba853edb3265df0e02eabbf8a2c058
ms.sourcegitcommit: fa61d882be9d0c48bd681f2efcb97e05522051d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71205710"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="93400-103">Tareas en segundo plano con servicios hospedados en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93400-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="93400-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="93400-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="93400-105">En ASP.NET Core, las tareas en segundo plano se pueden implementar como *servicios hospedados*.</span><span class="sxs-lookup"><span data-stu-id="93400-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="93400-106">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="93400-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="93400-107">En este tema se incluyen tres ejemplos de servicio hospedado:</span><span class="sxs-lookup"><span data-stu-id="93400-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="93400-108">Una tarea en segundo plano que se ejecuta según un temporizador.</span><span class="sxs-lookup"><span data-stu-id="93400-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="93400-109">Un servicio hospedado que activa un [servicio con ámbito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="93400-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="93400-110">El servicio con ámbito puede usar la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="93400-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="93400-111">Tareas en segundo plano en cola que se ejecutan en secuencia.</span><span class="sxs-lookup"><span data-stu-id="93400-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="93400-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="93400-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="93400-113">La aplicación de ejemplo se ofrece en dos versiones:</span><span class="sxs-lookup"><span data-stu-id="93400-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="93400-114">Host web: el host web resulta útil para hospedar aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="93400-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="93400-115">El código de ejemplo que se muestra en este tema corresponde a la versión de host web del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="93400-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="93400-116">Para más información, vea el sitio web [Host de web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="93400-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="93400-117">Host genérico: el host genérico es nuevo en ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="93400-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="93400-118">Para más información, vea el sitio web [Host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="93400-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="93400-119">Plantilla Worker Service</span><span class="sxs-lookup"><span data-stu-id="93400-119">Worker Service template</span></span>

<span data-ttu-id="93400-120">La plantilla Worker Service de ASP.NET Core sirve de punto de partida para escribir aplicaciones de servicio de larga duración.</span><span class="sxs-lookup"><span data-stu-id="93400-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="93400-121">Para usar la plantilla como base de una aplicación de servicios hospedados:</span><span class="sxs-lookup"><span data-stu-id="93400-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93400-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93400-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="93400-123">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="93400-123">Create a new project.</span></span>
1. <span data-ttu-id="93400-124">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="93400-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="93400-125">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="93400-125">Select **Next**.</span></span>
1. <span data-ttu-id="93400-126">Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado.</span><span class="sxs-lookup"><span data-stu-id="93400-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="93400-127">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="93400-127">Select **Create**.</span></span>
1. <span data-ttu-id="93400-128">En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, confirme que las opciones **.NET Core** y **ASP.NET Core 3.0** estén seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="93400-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="93400-129">Seleccione la plantilla **Worker Service**.</span><span class="sxs-lookup"><span data-stu-id="93400-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="93400-130">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="93400-130">Select **Create**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="93400-131">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="93400-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="93400-132">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="93400-132">Create a new project.</span></span>
1. <span data-ttu-id="93400-133">Seleccione **Aplicación** en **.NET Core** en la barra lateral.</span><span class="sxs-lookup"><span data-stu-id="93400-133">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="93400-134">Seleccione **Trabajador** en **ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="93400-134">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="93400-135">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="93400-135">Select **Next**.</span></span>
1. <span data-ttu-id="93400-136">Seleccione **.NET Core 3.0** para **Marco de trabajo de destino**.</span><span class="sxs-lookup"><span data-stu-id="93400-136">Select **.NET Core 3.0** for the **Target Framework**.</span></span> <span data-ttu-id="93400-137">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="93400-137">Select **Next**.</span></span>
1. <span data-ttu-id="93400-138">Proporcione un nombre en el campo **Nombre del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="93400-138">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="93400-139">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="93400-139">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="93400-140">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="93400-140">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="93400-141">Desde un shell de comandos, use la plantilla Worker Service (`worker`) con el comando [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="93400-141">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="93400-142">En el ejemplo siguiente, se crea una aplicación Worker Service llamada `ContosoWorker`.</span><span class="sxs-lookup"><span data-stu-id="93400-142">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="93400-143">Al ejecutar el comando, se crea automáticamente una carpeta para la aplicación `ContosoWorker`.</span><span class="sxs-lookup"><span data-stu-id="93400-143">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```

---

## <a name="package"></a><span data-ttu-id="93400-144">Package</span><span class="sxs-lookup"><span data-stu-id="93400-144">Package</span></span>

<span data-ttu-id="93400-145">Se agrega implícitamente una referencia de paquete al paquete [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) para las aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93400-145">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="93400-146">Interfaz IHostedService</span><span class="sxs-lookup"><span data-stu-id="93400-146">IHostedService interface</span></span>

<span data-ttu-id="93400-147">La interfaz <xref:Microsoft.Extensions.Hosting.IHostedService> define dos métodos para los objetos administrados por el host:</span><span class="sxs-lookup"><span data-stu-id="93400-147">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="93400-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*): `StartAsync` contiene la lógica para iniciar la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="93400-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="93400-149">Se llama a `StartAsync` *antes de que*:</span><span class="sxs-lookup"><span data-stu-id="93400-149">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="93400-150">La canalización de procesamiento de solicitudes de la aplicación está configurada (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="93400-150">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="93400-151">El servidor se haya iniciado y [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) se haya activado.</span><span class="sxs-lookup"><span data-stu-id="93400-151">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="93400-152">El comportamiento predeterminado se puede cambiar para que el `StartAsync` del servicio hospedado se ejecute después de que se haya configurado la canalización de la aplicación y se haya llamado a `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="93400-152">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="93400-153">Para cambiar el comportamiento predeterminado, agregue el servicio hospedado (`VideosWatcher` en el ejemplo siguiente) después de llamar a `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="93400-153">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Hosting;
  using Microsoft.Extensions.DependencyInjection;
  using Microsoft.Extensions.Hosting;

  public class Program
  {
      public static void Main(string[] args)
      {
          CreateHostBuilder(args).Build().Run();
      }

      public static IHostBuilder CreateHostBuilder(string[] args) =>
          Host.CreateDefaultBuilder(args)
              .ConfigureWebHostDefaults(webBuilder =>
              {
                  webBuilder.UseStartup<Startup>();
              })
              .ConfigureServices(services =>
              {
                  services.AddHostedService<VideosWatcher>();
              });
  }
  ```

* <span data-ttu-id="93400-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*): se activa cuando el host está realizando un cierre estable.</span><span class="sxs-lookup"><span data-stu-id="93400-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="93400-155">`StopAsync` contiene la lógica para finalizar la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="93400-155">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="93400-156">Implemente <xref:System.IDisposable> y los [finalizadores (destructores)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) para desechar los recursos no administrados.</span><span class="sxs-lookup"><span data-stu-id="93400-156">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="93400-157">El token de cancelación tiene un tiempo de espera predeterminado de cinco segundos para indicar que el proceso de cierre ya no debería ser estable.</span><span class="sxs-lookup"><span data-stu-id="93400-157">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="93400-158">Cuando se solicita la cancelación en el token:</span><span class="sxs-lookup"><span data-stu-id="93400-158">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="93400-159">Se deben anular las operaciones restantes en segundo plano que realiza la aplicación.</span><span class="sxs-lookup"><span data-stu-id="93400-159">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="93400-160">Los métodos llamados en `StopAsync` deberían devolver contenido al momento.</span><span class="sxs-lookup"><span data-stu-id="93400-160">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="93400-161">No obstante, las tareas no se abandonan después de solicitar la cancelación, sino que el autor de la llamada espera a que se completen todas las tareas.</span><span class="sxs-lookup"><span data-stu-id="93400-161">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="93400-162">Si la aplicación se cierra inesperadamente (por ejemplo, porque se produzca un error en el proceso de la aplicación), puede que no sea posible llamar a `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="93400-162">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="93400-163">Por lo tanto, los métodos llamados o las operaciones llevadas a cabo en `StopAsync` podrían no producirse.</span><span class="sxs-lookup"><span data-stu-id="93400-163">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="93400-164">Para ampliar el tiempo de espera predeterminado de apagado de 5 segundos, establezca:</span><span class="sxs-lookup"><span data-stu-id="93400-164">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="93400-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> cuando se usa el host genérico.</span><span class="sxs-lookup"><span data-stu-id="93400-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="93400-166">Para más información, consulte <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="93400-166">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="93400-167">Configuración de los valores de host de tiempo de espera de apagado cuando se usa el host web.</span><span class="sxs-lookup"><span data-stu-id="93400-167">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="93400-168">Para más información, consulte <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="93400-168">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="93400-169">El servicio hospedado se activa una vez al inicio de la aplicación y se cierra de manera estable cuando dicha aplicación se cierra.</span><span class="sxs-lookup"><span data-stu-id="93400-169">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="93400-170">Si se produce un error durante la ejecución de una tarea en segundo plano, hay que llamar a `Dispose`, aun cuando no se haya llamado a `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="93400-170">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="93400-171">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="93400-171">BackgroundService</span></span>

<span data-ttu-id="93400-172">`BackgroundService` es una clase base para implementar un <xref:Microsoft.Extensions.Hosting.IHostedService> de larga duración.</span><span class="sxs-lookup"><span data-stu-id="93400-172">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="93400-173">`BackgroundService` define dos métodos para las operaciones en segundo plano:</span><span class="sxs-lookup"><span data-stu-id="93400-173">`BackgroundService` defines two methods for background operations:</span></span>

* <span data-ttu-id="93400-174">`ExecuteAsync(CancellationToken stoppingToken)` &ndash; Se llama a `ExecuteAsync` cuando <xref:Microsoft.Extensions.Hosting.IHostedService> se inicia.</span><span class="sxs-lookup"><span data-stu-id="93400-174">`ExecuteAsync(CancellationToken stoppingToken)` &ndash; `ExecuteAsync` Called when the <xref:Microsoft.Extensions.Hosting.IHostedService> starts.</span></span> <span data-ttu-id="93400-175">La implementación debe devolver un `Task` que representa la duración de las operaciones de larga duración realizadas.</span><span class="sxs-lookup"><span data-stu-id="93400-175">The implementation should return a `Task` that represents the lifetime of the long running operations performed.</span></span> <span data-ttu-id="93400-176">`stoppingToken` se desencadena cuando se llama a [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*).</span><span class="sxs-lookup"><span data-stu-id="93400-176">The `stoppingToken` Triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span>
* <span data-ttu-id="93400-177">`StopAsync(CancellationToken stoppingToken)` &ndash; `StopAsync` se activa cuando el host de la aplicación está realizando un cierre estable.</span><span class="sxs-lookup"><span data-stu-id="93400-177">`StopAsync(CancellationToken stoppingToken)` &ndash; `StopAsync` is triggered when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="93400-178">`stoppingToken` indica que el proceso de cierre ya no debe ser estable.</span><span class="sxs-lookup"><span data-stu-id="93400-178">The `stoppingToken` indicates that the shutdown process should no longer be graceful.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="93400-179">Tareas en segundo plano temporizadas</span><span class="sxs-lookup"><span data-stu-id="93400-179">Timed background tasks</span></span>

<span data-ttu-id="93400-180">Una tarea en segundo plano temporizada hace uso de la clase [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="93400-180">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="93400-181">El temporizador activa el método `DoWork` de la tarea.</span><span class="sxs-lookup"><span data-stu-id="93400-181">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="93400-182">El temporizador está deshabilitado en `StopAsync` y se desecha cuando el contenedor de servicios se elimina en `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="93400-182">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="93400-183">El servicio se registra en `IHostBuilder.ConfigureServices` (*Program.cs*) con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="93400-183">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="93400-184">Consumir un servicio con ámbito en una tarea en segundo plano</span><span class="sxs-lookup"><span data-stu-id="93400-184">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="93400-185">Para usar [servicios con ámbito](xref:fundamentals/dependency-injection#service-lifetimes) en un elemento `BackgroundService`, cree un ámbito.</span><span class="sxs-lookup"><span data-stu-id="93400-185">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="93400-186">No se crean ámbitos de forma predeterminada para los servicios hospedados.</span><span class="sxs-lookup"><span data-stu-id="93400-186">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="93400-187">El servicio de tareas en segundo plano con ámbito contiene la lógica de la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="93400-187">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="93400-188">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="93400-188">In the following example:</span></span>

* <span data-ttu-id="93400-189">El servicio es asincrónico.</span><span class="sxs-lookup"><span data-stu-id="93400-189">The service is asynchronous.</span></span> <span data-ttu-id="93400-190">El método `DoWork` devuelve un objeto `Task`.</span><span class="sxs-lookup"><span data-stu-id="93400-190">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="93400-191">Para fines de demostración, se espera un retraso de diez segundos en el método `DoWork`.</span><span class="sxs-lookup"><span data-stu-id="93400-191">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="93400-192"><xref:Microsoft.Extensions.Logging.ILogger> se inserta en el servicio.</span><span class="sxs-lookup"><span data-stu-id="93400-192">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="93400-193">El servicio hospedado crea un ámbito con el fin de resolver el servicio de tareas en segundo plano con ámbito para llamar a su método `DoWork`.</span><span class="sxs-lookup"><span data-stu-id="93400-193">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="93400-194">`DoWork` devuelve `Task`, que se espera en `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="93400-194">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="93400-195">Los servicios se registran en `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="93400-195">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="93400-196">El servicio hospedado se registra en con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="93400-196">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="93400-197">Tareas en segundo plano en cola</span><span class="sxs-lookup"><span data-stu-id="93400-197">Queued background tasks</span></span>

<span data-ttu-id="93400-198">La cola de tareas en segundo plano se basa en <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> de .NET 4.x ([está previsto que se integre en ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="93400-198">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="93400-199">En el ejemplo `QueueHostedService` siguiente:</span><span class="sxs-lookup"><span data-stu-id="93400-199">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="93400-200">El método `BackgroundProcessing` devuelve `Task`, que se espera en `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="93400-200">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="93400-201">Las tareas en segundo plano que están en cola se quitan de ella y se ejecutan en `BackgroundProcessing`.</span><span class="sxs-lookup"><span data-stu-id="93400-201">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="93400-202">Un servicio `MonitorLoop` controla las tareas de puesta en cola para el servicio hospedado cada vez que se selecciona la tecla `w` en un dispositivo de entrada:</span><span class="sxs-lookup"><span data-stu-id="93400-202">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="93400-203">`IBackgroundTaskQueue` se inserta en el servicio `MonitorLoop`.</span><span class="sxs-lookup"><span data-stu-id="93400-203">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="93400-204">Se llama a `IBackgroundTaskQueue.QueueBackgroundWorkItem` para poner en cola el elemento de trabajo.</span><span class="sxs-lookup"><span data-stu-id="93400-204">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="93400-205">Los servicios se registran en `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="93400-205">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="93400-206">El servicio hospedado se registra en con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="93400-206">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="93400-207">En ASP.NET Core, las tareas en segundo plano se pueden implementar como *servicios hospedados*.</span><span class="sxs-lookup"><span data-stu-id="93400-207">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="93400-208">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="93400-208">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="93400-209">En este tema se incluyen tres ejemplos de servicio hospedado:</span><span class="sxs-lookup"><span data-stu-id="93400-209">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="93400-210">Una tarea en segundo plano que se ejecuta según un temporizador.</span><span class="sxs-lookup"><span data-stu-id="93400-210">Background task that runs on a timer.</span></span>
* <span data-ttu-id="93400-211">Un servicio hospedado que activa un [servicio con ámbito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="93400-211">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="93400-212">El servicio con ámbito puede usar la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="93400-212">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="93400-213">Tareas en segundo plano en cola que se ejecutan en secuencia.</span><span class="sxs-lookup"><span data-stu-id="93400-213">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="93400-214">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="93400-214">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="93400-215">La aplicación de ejemplo se ofrece en dos versiones:</span><span class="sxs-lookup"><span data-stu-id="93400-215">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="93400-216">Host web: el host web resulta útil para hospedar aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="93400-216">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="93400-217">El código de ejemplo que se muestra en este tema corresponde a la versión de host web del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="93400-217">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="93400-218">Para más información, vea el sitio web [Host de web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="93400-218">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="93400-219">Host genérico: el host genérico es nuevo en ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="93400-219">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="93400-220">Para más información, vea el sitio web [Host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="93400-220">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="93400-221">Package</span><span class="sxs-lookup"><span data-stu-id="93400-221">Package</span></span>

<span data-ttu-id="93400-222">Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="93400-222">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="93400-223">Interfaz IHostedService</span><span class="sxs-lookup"><span data-stu-id="93400-223">IHostedService interface</span></span>

<span data-ttu-id="93400-224">Los servicios hospedados implementan la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="93400-224">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="93400-225">Esta interfaz define dos métodos para los objetos administrados por el host:</span><span class="sxs-lookup"><span data-stu-id="93400-225">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="93400-226">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*): `StartAsync` contiene la lógica para iniciar la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="93400-226">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="93400-227">Al utilizar el [host web](xref:fundamentals/host/web-host), se llama a `StartAsync` después de que el servidor se haya iniciado y se haya activado [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="93400-227">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="93400-228">Al utilizar el [host genérico](xref:fundamentals/host/generic-host), se llama a `StartAsync` antes de que se desencadene `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="93400-228">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="93400-229">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*): se activa cuando el host está realizando un cierre estable.</span><span class="sxs-lookup"><span data-stu-id="93400-229">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="93400-230">`StopAsync` contiene la lógica para finalizar la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="93400-230">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="93400-231">Implemente <xref:System.IDisposable> y los [finalizadores (destructores)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) para desechar los recursos no administrados.</span><span class="sxs-lookup"><span data-stu-id="93400-231">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="93400-232">El token de cancelación tiene un tiempo de espera predeterminado de cinco segundos para indicar que el proceso de cierre ya no debería ser estable.</span><span class="sxs-lookup"><span data-stu-id="93400-232">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="93400-233">Cuando se solicita la cancelación en el token:</span><span class="sxs-lookup"><span data-stu-id="93400-233">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="93400-234">Se deben anular las operaciones restantes en segundo plano que realiza la aplicación.</span><span class="sxs-lookup"><span data-stu-id="93400-234">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="93400-235">Los métodos llamados en `StopAsync` deberían devolver contenido al momento.</span><span class="sxs-lookup"><span data-stu-id="93400-235">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="93400-236">No obstante, las tareas no se abandonan después de solicitar la cancelación, sino que el autor de la llamada espera a que se completen todas las tareas.</span><span class="sxs-lookup"><span data-stu-id="93400-236">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="93400-237">Si la aplicación se cierra inesperadamente (por ejemplo, porque se produzca un error en el proceso de la aplicación), puede que no sea posible llamar a `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="93400-237">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="93400-238">Por lo tanto, los métodos llamados o las operaciones llevadas a cabo en `StopAsync` podrían no producirse.</span><span class="sxs-lookup"><span data-stu-id="93400-238">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="93400-239">Para ampliar el tiempo de espera predeterminado de apagado de 5 segundos, establezca:</span><span class="sxs-lookup"><span data-stu-id="93400-239">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="93400-240"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> cuando se usa el host genérico.</span><span class="sxs-lookup"><span data-stu-id="93400-240"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="93400-241">Para más información, consulte <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="93400-241">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="93400-242">Configuración de los valores de host de tiempo de espera de apagado cuando se usa el host web.</span><span class="sxs-lookup"><span data-stu-id="93400-242">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="93400-243">Para más información, consulte <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="93400-243">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="93400-244">El servicio hospedado se activa una vez al inicio de la aplicación y se cierra de manera estable cuando dicha aplicación se cierra.</span><span class="sxs-lookup"><span data-stu-id="93400-244">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="93400-245">Si se produce un error durante la ejecución de una tarea en segundo plano, hay que llamar a `Dispose`, aun cuando no se haya llamado a `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="93400-245">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="93400-246">Tareas en segundo plano temporizadas</span><span class="sxs-lookup"><span data-stu-id="93400-246">Timed background tasks</span></span>

<span data-ttu-id="93400-247">Una tarea en segundo plano temporizada hace uso de la clase [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="93400-247">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="93400-248">El temporizador activa el método `DoWork` de la tarea.</span><span class="sxs-lookup"><span data-stu-id="93400-248">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="93400-249">El temporizador está deshabilitado en `StopAsync` y se desecha cuando el contenedor de servicios se elimina en `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="93400-249">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="93400-250">El servicio se registra en `Startup.ConfigureServices` con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="93400-250">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="93400-251">Consumir un servicio con ámbito en una tarea en segundo plano</span><span class="sxs-lookup"><span data-stu-id="93400-251">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="93400-252">Para usar [servicios con ámbito](xref:fundamentals/dependency-injection#service-lifetimes) en un elemento `IHostedService`, cree un ámbito.</span><span class="sxs-lookup"><span data-stu-id="93400-252">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="93400-253">No se crean ámbitos de forma predeterminada para los servicios hospedados.</span><span class="sxs-lookup"><span data-stu-id="93400-253">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="93400-254">El servicio de tareas en segundo plano con ámbito contiene la lógica de la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="93400-254">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="93400-255">En el siguiente ejemplo, <xref:Microsoft.Extensions.Logging.ILogger> se inserta en el servicio:</span><span class="sxs-lookup"><span data-stu-id="93400-255">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="93400-256">El servicio hospedado crea un ámbito con objeto de resolver el servicio de tareas en segundo plano con ámbito para llamar a su método `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="93400-256">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="93400-257">Los servicios se registran en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="93400-257">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="93400-258">La implementación `IHostedService` se registra con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="93400-258">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="93400-259">Tareas en segundo plano en cola</span><span class="sxs-lookup"><span data-stu-id="93400-259">Queued background tasks</span></span>

<span data-ttu-id="93400-260">La cola de tareas en segundo plano se basa en <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> de .NET 4.x ([está previsto que se integre en ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="93400-260">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="93400-261">En `QueueHostedService`, las tareas en segundo plano en la cola se quitan de la cola y se ejecutan como un servicio <xref:Microsoft.Extensions.Hosting.BackgroundService>, que es una clase base para implementar `IHostedService` de ejecución prolongada:</span><span class="sxs-lookup"><span data-stu-id="93400-261">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="93400-262">Los servicios se registran en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="93400-262">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="93400-263">La implementación `IHostedService` se registra con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="93400-263">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="93400-264">En la clase de modelo de página de índice:</span><span class="sxs-lookup"><span data-stu-id="93400-264">In the Index page model class:</span></span>

* <span data-ttu-id="93400-265">Se inserta `IBackgroundTaskQueue` en el constructor y se asigna a `Queue`.</span><span class="sxs-lookup"><span data-stu-id="93400-265">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="93400-266">Se inserta <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> y se asigna a `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="93400-266">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="93400-267">Se usa el generador se para crear instancias de <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, que se usa para crear servicios dentro de un ámbito.</span><span class="sxs-lookup"><span data-stu-id="93400-267">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="93400-268">Se crea un ámbito para poder usar el elemento `AppDbContext` de la aplicación (un [servicio con ámbito](xref:fundamentals/dependency-injection#service-lifetimes)) para escribir registros de base de datos en `IBackgroundTaskQueue` (un servicio de singleton).</span><span class="sxs-lookup"><span data-stu-id="93400-268">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="93400-269">Cuando se hace clic en el botón **Agregar tarea** en la página de índice, se ejecuta el método `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="93400-269">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="93400-270">Se llama a `QueueBackgroundWorkItem` para poner en cola un elemento de trabajo:</span><span class="sxs-lookup"><span data-stu-id="93400-270">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="93400-271">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="93400-271">Additional resources</span></span>

* [<span data-ttu-id="93400-272">Implementar tareas en segundo plano en microservicios con IHostedService y la clase BackgroundService</span><span class="sxs-lookup"><span data-stu-id="93400-272">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
