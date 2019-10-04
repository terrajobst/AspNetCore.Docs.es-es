---
title: Tareas en segundo plano con servicios hospedados en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo implementar tareas en segundo plano con servicios hospedados en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 0eaa3a62370c1e413840bb65f597dc664adafc38
ms.sourcegitcommit: fe88748b762525cb490f7e39089a4760f6a73a24
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71688095"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="7e412-103">Tareas en segundo plano con servicios hospedados en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7e412-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="7e412-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7e412-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7e412-105">En ASP.NET Core, las tareas en segundo plano se pueden implementar como *servicios hospedados*.</span><span class="sxs-lookup"><span data-stu-id="7e412-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="7e412-106">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="7e412-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="7e412-107">En este tema se incluyen tres ejemplos de servicio hospedado:</span><span class="sxs-lookup"><span data-stu-id="7e412-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="7e412-108">Una tarea en segundo plano que se ejecuta según un temporizador.</span><span class="sxs-lookup"><span data-stu-id="7e412-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="7e412-109">Un servicio hospedado que activa un [servicio con ámbito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="7e412-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="7e412-110">El servicio con ámbito puede usar la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7e412-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="7e412-111">Tareas en segundo plano en cola que se ejecutan en secuencia.</span><span class="sxs-lookup"><span data-stu-id="7e412-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="7e412-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7e412-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7e412-113">La aplicación de ejemplo se ofrece en dos versiones:</span><span class="sxs-lookup"><span data-stu-id="7e412-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="7e412-114">Host web: el host web resulta útil para hospedar aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="7e412-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="7e412-115">El código de ejemplo que se muestra en este tema corresponde a la versión de host web del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="7e412-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="7e412-116">Para más información, vea el sitio web [Host de web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="7e412-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="7e412-117">Host genérico: el host genérico es nuevo en ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="7e412-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="7e412-118">Para más información, vea el sitio web [Host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="7e412-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="7e412-119">Plantilla Worker Service</span><span class="sxs-lookup"><span data-stu-id="7e412-119">Worker Service template</span></span>

<span data-ttu-id="7e412-120">La plantilla Worker Service de ASP.NET Core sirve de punto de partida para escribir aplicaciones de servicio de larga duración.</span><span class="sxs-lookup"><span data-stu-id="7e412-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="7e412-121">Para usar la plantilla como base de una aplicación de servicios hospedados:</span><span class="sxs-lookup"><span data-stu-id="7e412-121">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

## <a name="package"></a><span data-ttu-id="7e412-122">Package</span><span class="sxs-lookup"><span data-stu-id="7e412-122">Package</span></span>

<span data-ttu-id="7e412-123">Se agrega implícitamente una referencia de paquete al paquete [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) para las aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7e412-123">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="7e412-124">Interfaz IHostedService</span><span class="sxs-lookup"><span data-stu-id="7e412-124">IHostedService interface</span></span>

<span data-ttu-id="7e412-125">La interfaz <xref:Microsoft.Extensions.Hosting.IHostedService> define dos métodos para los objetos administrados por el host:</span><span class="sxs-lookup"><span data-stu-id="7e412-125">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="7e412-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*): `StartAsync` contiene la lógica para iniciar la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="7e412-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="7e412-127">Se llama a `StartAsync` *antes de que*:</span><span class="sxs-lookup"><span data-stu-id="7e412-127">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="7e412-128">La canalización de procesamiento de solicitudes de la aplicación está configurada (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="7e412-128">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="7e412-129">El servidor se haya iniciado y [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) se haya activado.</span><span class="sxs-lookup"><span data-stu-id="7e412-129">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="7e412-130">El comportamiento predeterminado se puede cambiar para que el `StartAsync` del servicio hospedado se ejecute después de que se haya configurado la canalización de la aplicación y se haya llamado a `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="7e412-130">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="7e412-131">Para cambiar el comportamiento predeterminado, agregue el servicio hospedado (`VideosWatcher` en el ejemplo siguiente) después de llamar a `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="7e412-131">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="7e412-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*): se activa cuando el host está realizando un cierre estable.</span><span class="sxs-lookup"><span data-stu-id="7e412-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="7e412-133">`StopAsync` contiene la lógica para finalizar la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="7e412-133">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="7e412-134">Implemente <xref:System.IDisposable> y los [finalizadores (destructores)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) para desechar los recursos no administrados.</span><span class="sxs-lookup"><span data-stu-id="7e412-134">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="7e412-135">El token de cancelación tiene un tiempo de espera predeterminado de cinco segundos para indicar que el proceso de cierre ya no debería ser estable.</span><span class="sxs-lookup"><span data-stu-id="7e412-135">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="7e412-136">Cuando se solicita la cancelación en el token:</span><span class="sxs-lookup"><span data-stu-id="7e412-136">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="7e412-137">Se deben anular las operaciones restantes en segundo plano que realiza la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7e412-137">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="7e412-138">Los métodos llamados en `StopAsync` deberían devolver contenido al momento.</span><span class="sxs-lookup"><span data-stu-id="7e412-138">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="7e412-139">No obstante, las tareas no se abandonan después de solicitar la cancelación, sino que el autor de la llamada espera a que se completen todas las tareas.</span><span class="sxs-lookup"><span data-stu-id="7e412-139">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="7e412-140">Si la aplicación se cierra inesperadamente (por ejemplo, porque se produzca un error en el proceso de la aplicación), puede que no sea posible llamar a `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="7e412-140">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="7e412-141">Por lo tanto, los métodos llamados o las operaciones llevadas a cabo en `StopAsync` podrían no producirse.</span><span class="sxs-lookup"><span data-stu-id="7e412-141">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="7e412-142">Para ampliar el tiempo de espera predeterminado de apagado de 5 segundos, establezca:</span><span class="sxs-lookup"><span data-stu-id="7e412-142">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="7e412-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> cuando se usa el host genérico.</span><span class="sxs-lookup"><span data-stu-id="7e412-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="7e412-144">Para más información, consulte <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="7e412-144">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="7e412-145">Configuración de los valores de host de tiempo de espera de apagado cuando se usa el host web.</span><span class="sxs-lookup"><span data-stu-id="7e412-145">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="7e412-146">Para más información, consulte <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="7e412-146">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="7e412-147">El servicio hospedado se activa una vez al inicio de la aplicación y se cierra de manera estable cuando dicha aplicación se cierra.</span><span class="sxs-lookup"><span data-stu-id="7e412-147">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="7e412-148">Si se produce un error durante la ejecución de una tarea en segundo plano, hay que llamar a `Dispose`, aun cuando no se haya llamado a `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="7e412-148">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="7e412-149">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="7e412-149">BackgroundService</span></span>

<span data-ttu-id="7e412-150">`BackgroundService` es una clase base para implementar un <xref:Microsoft.Extensions.Hosting.IHostedService> de larga duración.</span><span class="sxs-lookup"><span data-stu-id="7e412-150">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="7e412-151">`BackgroundService` proporciona el método abstracto `ExecuteAsync(CancellationToken stoppingToken)` para contener la lógica del servicio.</span><span class="sxs-lookup"><span data-stu-id="7e412-151">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="7e412-152">`stoppingToken` se desencadena cuando se realiza una llamada a [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*).</span><span class="sxs-lookup"><span data-stu-id="7e412-152">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="7e412-153">La implementación de este método devuelve `Task`, que representa toda la duración del servicio en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="7e412-153">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="7e412-154">Además, también *puede* invalidar los métodos definidos en `IHostedService` para ejecutar el código de inicio y de apagado para el servicio:</span><span class="sxs-lookup"><span data-stu-id="7e412-154">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="7e412-155">Se realiza una llamada a `StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` cuando el host de la aplicación está realizando un apagado estable.</span><span class="sxs-lookup"><span data-stu-id="7e412-155">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="7e412-156">Se envía una señal a `cancellationToken` cuando el host decide finalizar forzosamente el servicio.</span><span class="sxs-lookup"><span data-stu-id="7e412-156">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="7e412-157">Si se invalida este método, **debe** realizar una llamada (y `await`) al método de la clase base para asegurarse de que el servicio se apaga correctamente.</span><span class="sxs-lookup"><span data-stu-id="7e412-157">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="7e412-158">Se realiza una llamada a `StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` para iniciar el servicio en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="7e412-158">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="7e412-159">Se envía una señal a `cancellationToken` si se interrumpe el proceso de inicio.</span><span class="sxs-lookup"><span data-stu-id="7e412-159">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="7e412-160">La implementación devuelve `Task`, que representa el proceso de inicio del servicio.</span><span class="sxs-lookup"><span data-stu-id="7e412-160">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="7e412-161">No se inicia ningún otro servicio hasta que se complete `Task`.</span><span class="sxs-lookup"><span data-stu-id="7e412-161">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="7e412-162">Si se invalida este método, **debe** realizar una llamada (y `await`) al método de la clase base para asegurarse de que el servicio se inicia correctamente.</span><span class="sxs-lookup"><span data-stu-id="7e412-162">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="7e412-163">Tareas en segundo plano temporizadas</span><span class="sxs-lookup"><span data-stu-id="7e412-163">Timed background tasks</span></span>

<span data-ttu-id="7e412-164">Una tarea en segundo plano temporizada hace uso de la clase [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="7e412-164">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="7e412-165">El temporizador activa el método `DoWork` de la tarea.</span><span class="sxs-lookup"><span data-stu-id="7e412-165">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="7e412-166">El temporizador está deshabilitado en `StopAsync` y se desecha cuando el contenedor de servicios se elimina en `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="7e412-166">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="7e412-167">El servicio se registra en `IHostBuilder.ConfigureServices` (*Program.cs*) con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="7e412-167">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="7e412-168">Consumir un servicio con ámbito en una tarea en segundo plano</span><span class="sxs-lookup"><span data-stu-id="7e412-168">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="7e412-169">Para usar [servicios con ámbito](xref:fundamentals/dependency-injection#service-lifetimes) en un elemento `BackgroundService`, cree un ámbito.</span><span class="sxs-lookup"><span data-stu-id="7e412-169">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="7e412-170">No se crean ámbitos de forma predeterminada para los servicios hospedados.</span><span class="sxs-lookup"><span data-stu-id="7e412-170">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="7e412-171">El servicio de tareas en segundo plano con ámbito contiene la lógica de la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="7e412-171">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="7e412-172">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7e412-172">In the following example:</span></span>

* <span data-ttu-id="7e412-173">El servicio es asincrónico.</span><span class="sxs-lookup"><span data-stu-id="7e412-173">The service is asynchronous.</span></span> <span data-ttu-id="7e412-174">El método `DoWork` devuelve un objeto `Task`.</span><span class="sxs-lookup"><span data-stu-id="7e412-174">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="7e412-175">Para fines de demostración, se espera un retraso de diez segundos en el método `DoWork`.</span><span class="sxs-lookup"><span data-stu-id="7e412-175">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="7e412-176"><xref:Microsoft.Extensions.Logging.ILogger> se inserta en el servicio.</span><span class="sxs-lookup"><span data-stu-id="7e412-176">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="7e412-177">El servicio hospedado crea un ámbito con el fin de resolver el servicio de tareas en segundo plano con ámbito para llamar a su método `DoWork`.</span><span class="sxs-lookup"><span data-stu-id="7e412-177">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="7e412-178">`DoWork` devuelve `Task`, que se espera en `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="7e412-178">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="7e412-179">Los servicios se registran en `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="7e412-179">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="7e412-180">El servicio hospedado se registra en con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="7e412-180">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="7e412-181">Tareas en segundo plano en cola</span><span class="sxs-lookup"><span data-stu-id="7e412-181">Queued background tasks</span></span>

<span data-ttu-id="7e412-182">La cola de tareas en segundo plano se basa en <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> de .NET 4.x ([está previsto que se integre en ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="7e412-182">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="7e412-183">En el ejemplo `QueueHostedService` siguiente:</span><span class="sxs-lookup"><span data-stu-id="7e412-183">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="7e412-184">El método `BackgroundProcessing` devuelve `Task`, que se espera en `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="7e412-184">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="7e412-185">Las tareas en segundo plano que están en cola se quitan de ella y se ejecutan en `BackgroundProcessing`.</span><span class="sxs-lookup"><span data-stu-id="7e412-185">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="7e412-186">Un servicio `MonitorLoop` controla las tareas de puesta en cola para el servicio hospedado cada vez que se selecciona la tecla `w` en un dispositivo de entrada:</span><span class="sxs-lookup"><span data-stu-id="7e412-186">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="7e412-187">`IBackgroundTaskQueue` se inserta en el servicio `MonitorLoop`.</span><span class="sxs-lookup"><span data-stu-id="7e412-187">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="7e412-188">Se llama a `IBackgroundTaskQueue.QueueBackgroundWorkItem` para poner en cola el elemento de trabajo.</span><span class="sxs-lookup"><span data-stu-id="7e412-188">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="7e412-189">Los servicios se registran en `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="7e412-189">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="7e412-190">El servicio hospedado se registra en con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="7e412-190">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7e412-191">En ASP.NET Core, las tareas en segundo plano se pueden implementar como *servicios hospedados*.</span><span class="sxs-lookup"><span data-stu-id="7e412-191">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="7e412-192">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="7e412-192">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="7e412-193">En este tema se incluyen tres ejemplos de servicio hospedado:</span><span class="sxs-lookup"><span data-stu-id="7e412-193">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="7e412-194">Una tarea en segundo plano que se ejecuta según un temporizador.</span><span class="sxs-lookup"><span data-stu-id="7e412-194">Background task that runs on a timer.</span></span>
* <span data-ttu-id="7e412-195">Un servicio hospedado que activa un [servicio con ámbito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="7e412-195">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="7e412-196">El servicio con ámbito puede usar la [inserción de dependencias (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7e412-196">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="7e412-197">Tareas en segundo plano en cola que se ejecutan en secuencia.</span><span class="sxs-lookup"><span data-stu-id="7e412-197">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="7e412-198">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7e412-198">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7e412-199">La aplicación de ejemplo se ofrece en dos versiones:</span><span class="sxs-lookup"><span data-stu-id="7e412-199">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="7e412-200">Host web: el host web resulta útil para hospedar aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="7e412-200">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="7e412-201">El código de ejemplo que se muestra en este tema corresponde a la versión de host web del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="7e412-201">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="7e412-202">Para más información, vea el sitio web [Host de web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="7e412-202">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="7e412-203">Host genérico: el host genérico es nuevo en ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="7e412-203">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="7e412-204">Para más información, vea el sitio web [Host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="7e412-204">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="7e412-205">Package</span><span class="sxs-lookup"><span data-stu-id="7e412-205">Package</span></span>

<span data-ttu-id="7e412-206">Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="7e412-206">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="7e412-207">Interfaz IHostedService</span><span class="sxs-lookup"><span data-stu-id="7e412-207">IHostedService interface</span></span>

<span data-ttu-id="7e412-208">Los servicios hospedados implementan la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="7e412-208">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="7e412-209">Esta interfaz define dos métodos para los objetos administrados por el host:</span><span class="sxs-lookup"><span data-stu-id="7e412-209">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="7e412-210">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*): `StartAsync` contiene la lógica para iniciar la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="7e412-210">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="7e412-211">Al utilizar el [host web](xref:fundamentals/host/web-host), se llama a `StartAsync` después de que el servidor se haya iniciado y se haya activado [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="7e412-211">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="7e412-212">Al utilizar el [host genérico](xref:fundamentals/host/generic-host), se llama a `StartAsync` antes de que se desencadene `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="7e412-212">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="7e412-213">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*): se activa cuando el host está realizando un cierre estable.</span><span class="sxs-lookup"><span data-stu-id="7e412-213">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="7e412-214">`StopAsync` contiene la lógica para finalizar la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="7e412-214">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="7e412-215">Implemente <xref:System.IDisposable> y los [finalizadores (destructores)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) para desechar los recursos no administrados.</span><span class="sxs-lookup"><span data-stu-id="7e412-215">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="7e412-216">El token de cancelación tiene un tiempo de espera predeterminado de cinco segundos para indicar que el proceso de cierre ya no debería ser estable.</span><span class="sxs-lookup"><span data-stu-id="7e412-216">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="7e412-217">Cuando se solicita la cancelación en el token:</span><span class="sxs-lookup"><span data-stu-id="7e412-217">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="7e412-218">Se deben anular las operaciones restantes en segundo plano que realiza la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7e412-218">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="7e412-219">Los métodos llamados en `StopAsync` deberían devolver contenido al momento.</span><span class="sxs-lookup"><span data-stu-id="7e412-219">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="7e412-220">No obstante, las tareas no se abandonan después de solicitar la cancelación, sino que el autor de la llamada espera a que se completen todas las tareas.</span><span class="sxs-lookup"><span data-stu-id="7e412-220">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="7e412-221">Si la aplicación se cierra inesperadamente (por ejemplo, porque se produzca un error en el proceso de la aplicación), puede que no sea posible llamar a `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="7e412-221">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="7e412-222">Por lo tanto, los métodos llamados o las operaciones llevadas a cabo en `StopAsync` podrían no producirse.</span><span class="sxs-lookup"><span data-stu-id="7e412-222">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="7e412-223">Para ampliar el tiempo de espera predeterminado de apagado de 5 segundos, establezca:</span><span class="sxs-lookup"><span data-stu-id="7e412-223">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="7e412-224"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> cuando se usa el host genérico.</span><span class="sxs-lookup"><span data-stu-id="7e412-224"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="7e412-225">Para más información, consulte <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="7e412-225">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="7e412-226">Configuración de los valores de host de tiempo de espera de apagado cuando se usa el host web.</span><span class="sxs-lookup"><span data-stu-id="7e412-226">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="7e412-227">Para más información, consulte <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="7e412-227">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="7e412-228">El servicio hospedado se activa una vez al inicio de la aplicación y se cierra de manera estable cuando dicha aplicación se cierra.</span><span class="sxs-lookup"><span data-stu-id="7e412-228">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="7e412-229">Si se produce un error durante la ejecución de una tarea en segundo plano, hay que llamar a `Dispose`, aun cuando no se haya llamado a `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="7e412-229">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="7e412-230">Tareas en segundo plano temporizadas</span><span class="sxs-lookup"><span data-stu-id="7e412-230">Timed background tasks</span></span>

<span data-ttu-id="7e412-231">Una tarea en segundo plano temporizada hace uso de la clase [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="7e412-231">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="7e412-232">El temporizador activa el método `DoWork` de la tarea.</span><span class="sxs-lookup"><span data-stu-id="7e412-232">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="7e412-233">El temporizador está deshabilitado en `StopAsync` y se desecha cuando el contenedor de servicios se elimina en `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="7e412-233">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="7e412-234">El servicio se registra en `Startup.ConfigureServices` con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="7e412-234">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="7e412-235">Consumir un servicio con ámbito en una tarea en segundo plano</span><span class="sxs-lookup"><span data-stu-id="7e412-235">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="7e412-236">Para usar [servicios con ámbito](xref:fundamentals/dependency-injection#service-lifetimes) en un elemento `IHostedService`, cree un ámbito.</span><span class="sxs-lookup"><span data-stu-id="7e412-236">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="7e412-237">No se crean ámbitos de forma predeterminada para los servicios hospedados.</span><span class="sxs-lookup"><span data-stu-id="7e412-237">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="7e412-238">El servicio de tareas en segundo plano con ámbito contiene la lógica de la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="7e412-238">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="7e412-239">En el siguiente ejemplo, <xref:Microsoft.Extensions.Logging.ILogger> se inserta en el servicio:</span><span class="sxs-lookup"><span data-stu-id="7e412-239">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="7e412-240">El servicio hospedado crea un ámbito con objeto de resolver el servicio de tareas en segundo plano con ámbito para llamar a su método `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="7e412-240">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="7e412-241">Los servicios se registran en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7e412-241">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7e412-242">La implementación `IHostedService` se registra con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="7e412-242">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="7e412-243">Tareas en segundo plano en cola</span><span class="sxs-lookup"><span data-stu-id="7e412-243">Queued background tasks</span></span>

<span data-ttu-id="7e412-244">La cola de tareas en segundo plano se basa en <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> de .NET 4.x ([está previsto que se integre en ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="7e412-244">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="7e412-245">En `QueueHostedService`, las tareas en segundo plano en la cola se quitan de la cola y se ejecutan como un servicio <xref:Microsoft.Extensions.Hosting.BackgroundService>, que es una clase base para implementar `IHostedService` de ejecución prolongada:</span><span class="sxs-lookup"><span data-stu-id="7e412-245">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="7e412-246">Los servicios se registran en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7e412-246">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7e412-247">La implementación `IHostedService` se registra con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="7e412-247">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="7e412-248">En la clase de modelo de página de índice:</span><span class="sxs-lookup"><span data-stu-id="7e412-248">In the Index page model class:</span></span>

* <span data-ttu-id="7e412-249">Se inserta `IBackgroundTaskQueue` en el constructor y se asigna a `Queue`.</span><span class="sxs-lookup"><span data-stu-id="7e412-249">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="7e412-250">Se inserta <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> y se asigna a `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="7e412-250">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="7e412-251">Se usa el generador se para crear instancias de <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, que se usa para crear servicios dentro de un ámbito.</span><span class="sxs-lookup"><span data-stu-id="7e412-251">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="7e412-252">Se crea un ámbito para poder usar el elemento `AppDbContext` de la aplicación (un [servicio con ámbito](xref:fundamentals/dependency-injection#service-lifetimes)) para escribir registros de base de datos en `IBackgroundTaskQueue` (un servicio de singleton).</span><span class="sxs-lookup"><span data-stu-id="7e412-252">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="7e412-253">Cuando se hace clic en el botón **Agregar tarea** en la página de índice, se ejecuta el método `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="7e412-253">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="7e412-254">Se llama a `QueueBackgroundWorkItem` para poner en cola un elemento de trabajo:</span><span class="sxs-lookup"><span data-stu-id="7e412-254">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="7e412-255">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7e412-255">Additional resources</span></span>

* [<span data-ttu-id="7e412-256">Implementar tareas en segundo plano en microservicios con IHostedService y la clase BackgroundService</span><span class="sxs-lookup"><span data-stu-id="7e412-256">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
