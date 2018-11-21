---
title: Tareas en segundo plano con servicios hospedados en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo implementar tareas en segundo plano con servicios hospedados en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: f8e13e13af22f1be4f14d5e59807c4dae3b78e84
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708496"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="51480-103">Tareas en segundo plano con servicios hospedados en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51480-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="51480-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="51480-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="51480-105">En ASP.NET Core, las tareas en segundo plano se pueden implementar como *servicios hospedados*.</span><span class="sxs-lookup"><span data-stu-id="51480-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="51480-106">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="51480-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="51480-107">En este tema se incluyen tres ejemplos de servicio hospedado:</span><span class="sxs-lookup"><span data-stu-id="51480-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="51480-108">Una tarea en segundo plano que se ejecuta según un temporizador.</span><span class="sxs-lookup"><span data-stu-id="51480-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="51480-109">Un servicio hospedado que activa un servicio con ámbito.</span><span class="sxs-lookup"><span data-stu-id="51480-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="51480-110">El servicio con ámbito puede usar la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="51480-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="51480-111">Tareas en segundo plano en cola que se ejecutan en secuencia.</span><span class="sxs-lookup"><span data-stu-id="51480-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="51480-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="51480-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="51480-113">La aplicación de ejemplo se ofrece en dos versiones:</span><span class="sxs-lookup"><span data-stu-id="51480-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="51480-114">Host de web: el host de web resulta útil para hospedar aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="51480-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="51480-115">El código de ejemplo que se muestra en este tema corresponde a la versión de host de web del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="51480-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="51480-116">Para más información, vea el sitio web [Host de web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="51480-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="51480-117">Host genérico: el host genérico es nuevo en ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="51480-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="51480-118">Para más información, vea el sitio web [Host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="51480-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="51480-119">Package</span><span class="sxs-lookup"><span data-stu-id="51480-119">Package</span></span>

<span data-ttu-id="51480-120">Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="51480-120">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="51480-121">Interfaz IHostedService</span><span class="sxs-lookup"><span data-stu-id="51480-121">IHostedService interface</span></span>

<span data-ttu-id="51480-122">Los servicios hospedados implementan la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="51480-122">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="51480-123">Esta interfaz define dos métodos para los objetos administrados por el host:</span><span class="sxs-lookup"><span data-stu-id="51480-123">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="51480-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*): `StartAsync` contiene la lógica para iniciar la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="51480-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="51480-125">Al utilizar el [host web](xref:fundamentals/host/web-host), se llama a `StartAsync` después de que el servidor se haya iniciado y se haya activado [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="51480-125">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="51480-126">Al utilizar el [host genérico](xref:fundamentals/host/generic-host), se llama a `StartAsync` antes de que se desencadene `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="51480-126">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="51480-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*): se activa cuando el host está realizando un cierre estable.</span><span class="sxs-lookup"><span data-stu-id="51480-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="51480-128">`StopAsync` contiene la lógica para finalizar la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="51480-128">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="51480-129">Implemente <xref:System.IDisposable> y los [finalizadores (destructores)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) para desechar los recursos no administrados.</span><span class="sxs-lookup"><span data-stu-id="51480-129">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span> 

  <span data-ttu-id="51480-130">El token de cancelación tiene un tiempo de espera predeterminado de cinco segundos para indicar que el proceso de cierre ya no debería ser estable.</span><span class="sxs-lookup"><span data-stu-id="51480-130">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="51480-131">Cuando se solicita la cancelación en el token:</span><span class="sxs-lookup"><span data-stu-id="51480-131">When cancellation is requested on the token:</span></span>
  
  * <span data-ttu-id="51480-132">Se deben anular las operaciones restantes en segundo plano que realiza la aplicación.</span><span class="sxs-lookup"><span data-stu-id="51480-132">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="51480-133">Los métodos llamados en `StopAsync` deberían devolver contenido al momento.</span><span class="sxs-lookup"><span data-stu-id="51480-133">Any methods called in `StopAsync` should return promptly.</span></span>
  
  <span data-ttu-id="51480-134">No obstante, las tareas no se abandonan después de solicitar la cancelación, sino que el autor de la llamada espera a que se completen todas las tareas.</span><span class="sxs-lookup"><span data-stu-id="51480-134">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="51480-135">Si la aplicación se cierra inesperadamente (por ejemplo, porque se produzca un error en el proceso de la aplicación), puede que no sea posible llamar a `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="51480-135">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="51480-136">Por lo tanto, los métodos llamados o las operaciones llevadas a cabo en `StopAsync` podrían no producirse.</span><span class="sxs-lookup"><span data-stu-id="51480-136">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

<span data-ttu-id="51480-137">El servicio hospedado se activa una vez al inicio de la aplicación y se cierra de manera estable cuando dicha aplicación se cierra.</span><span class="sxs-lookup"><span data-stu-id="51480-137">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="51480-138">Si se produce un error durante la ejecución de una tarea en segundo plano, hay que llamar a `Dispose`, aun cuando no se haya llamado a `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="51480-138">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="51480-139">Tareas en segundo plano temporizadas</span><span class="sxs-lookup"><span data-stu-id="51480-139">Timed background tasks</span></span>

<span data-ttu-id="51480-140">Una tarea en segundo plano temporizada hace uso de la clase [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="51480-140">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="51480-141">El temporizador activa el método `DoWork` de la tarea.</span><span class="sxs-lookup"><span data-stu-id="51480-141">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="51480-142">El temporizador está deshabilitado en `StopAsync` y se desecha cuando el contenedor de servicios se elimina en `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="51480-142">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="51480-143">El servicio se registra en `Startup.ConfigureServices` con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="51480-143">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="51480-144">Consumir un servicio con ámbito en una tarea en segundo plano</span><span class="sxs-lookup"><span data-stu-id="51480-144">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="51480-145">Para usar servicios con ámbito en un `IHostedService`, cree un ámbito.</span><span class="sxs-lookup"><span data-stu-id="51480-145">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="51480-146">No se crean ámbitos de forma predeterminada para los servicios hospedados.</span><span class="sxs-lookup"><span data-stu-id="51480-146">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="51480-147">El servicio de tareas en segundo plano con ámbito contiene la lógica de la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="51480-147">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="51480-148">En el siguiente ejemplo, <xref:Microsoft.Extensions.Logging.ILogger> se inserta en el servicio:</span><span class="sxs-lookup"><span data-stu-id="51480-148">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="51480-149">El servicio hospedado crea un ámbito con objeto de resolver el servicio de tareas en segundo plano con ámbito para llamar a su método `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="51480-149">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="51480-150">Los servicios se registran en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="51480-150">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="51480-151">La implementación `IHostedService` se registra con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="51480-151">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="51480-152">Tareas en segundo plano en cola</span><span class="sxs-lookup"><span data-stu-id="51480-152">Queued background tasks</span></span>

<span data-ttu-id="51480-153">Las colas de tareas en segundo plano se basan en <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> de .NET 4.x ([está previsto que se integre en ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="51480-153">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="51480-154">En `QueueHostedService`, las tareas en segundo plano en la cola se quitan de la cola y se ejecutan como un servicio <xref:Microsoft.Extensions.Hosting.BackgroundService>, que es una clase base para implementar `IHostedService` de ejecución prolongada:</span><span class="sxs-lookup"><span data-stu-id="51480-154">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="51480-155">Los servicios se registran en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="51480-155">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="51480-156">La implementación `IHostedService` se registra con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="51480-156">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="51480-157">En la clase de modelo de página de índice, `IBackgroundTaskQueue` se inserta en el constructor y se asigna a `Queue`:</span><span class="sxs-lookup"><span data-stu-id="51480-157">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="51480-158">Cuando se hace clic en el botón **Agregar tarea** en la página de índice, se ejecuta el método `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="51480-158">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="51480-159">Se llama a `QueueBackgroundWorkItem` para poner en cola el elemento de trabajo:</span><span class="sxs-lookup"><span data-stu-id="51480-159">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="51480-160">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="51480-160">Additional resources</span></span>

* [<span data-ttu-id="51480-161">Implementar tareas en segundo plano en microservicios con IHostedService y la clase BackgroundService</span><span class="sxs-lookup"><span data-stu-id="51480-161">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="51480-162">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="51480-162">System.Threading.Timer</span></span>](xref:System.Threading.Timer)
