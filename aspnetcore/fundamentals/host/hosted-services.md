---
title: Tareas en segundo plano con servicios hospedados en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo implementar tareas en segundo plano con servicios hospedados en ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/hosted-services
ms.openlocfilehash: cc39d125b639719599eca68d627fda014fb107e0
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555305"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="5e575-103">Tareas en segundo plano con servicios hospedados en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e575-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="5e575-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5e575-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5e575-105">En ASP.NET Core, las tareas en segundo plano se pueden implementar como *servicios hospedados*.</span><span class="sxs-lookup"><span data-stu-id="5e575-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="5e575-106">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="5e575-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="5e575-107">En este tema se incluyen tres ejemplos de servicio hospedado:</span><span class="sxs-lookup"><span data-stu-id="5e575-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="5e575-108">Una tarea en segundo plano que se ejecuta según un temporizador.</span><span class="sxs-lookup"><span data-stu-id="5e575-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="5e575-109">Un servicio hospedado que activa un servicio con ámbito.</span><span class="sxs-lookup"><span data-stu-id="5e575-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="5e575-110">El servicio con ámbito puede usar la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="5e575-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="5e575-111">Tareas en segundo plano en cola que se ejecutan en secuencia.</span><span class="sxs-lookup"><span data-stu-id="5e575-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="5e575-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5e575-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5e575-113">La aplicación de ejemplo se ofrece en dos versiones:</span><span class="sxs-lookup"><span data-stu-id="5e575-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="5e575-114">Host de web: el host de web resulta útil para hospedar aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="5e575-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="5e575-115">El código de ejemplo que se muestra en este tema corresponde a la versión de host de web del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="5e575-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="5e575-116">Para más información, vea el sitio web [Host de web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="5e575-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="5e575-117">Host genérico: el host genérico es nuevo en ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="5e575-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="5e575-118">Para más información, vea el sitio web [Host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="5e575-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker-end

## <a name="ihostedservice-interface"></a><span data-ttu-id="5e575-119">Interfaz IHostedService</span><span class="sxs-lookup"><span data-stu-id="5e575-119">IHostedService interface</span></span>

<span data-ttu-id="5e575-120">Los servicios hospedados implementan la interfaz [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="5e575-120">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="5e575-121">Esta interfaz define dos métodos para los objetos administrados por el host:</span><span class="sxs-lookup"><span data-stu-id="5e575-121">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="5e575-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync): se llama después de que el servidor se haya iniciado y [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) se haya activado.</span><span class="sxs-lookup"><span data-stu-id="5e575-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="5e575-123">`StartAsync` contiene la lógica para iniciar la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="5e575-123">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="5e575-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync): se activa cuando el host está realizando un cierre estable.</span><span class="sxs-lookup"><span data-stu-id="5e575-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="5e575-125">`StopAsync` contiene la lógica para finalizar la tarea en segundo plano y desechar los recursos no administrados.</span><span class="sxs-lookup"><span data-stu-id="5e575-125">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="5e575-126">Si la aplicación se cierra inesperadamente (por ejemplo, porque se produzca un error en el proceso de la aplicación), puede que no sea posible llamar a `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="5e575-126">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="5e575-127">El servicio hospedado es un singleton que se activa una vez en el inicio de la aplicación y se cierra de manera estable cuando dicha aplicación se cierra.</span><span class="sxs-lookup"><span data-stu-id="5e575-127">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="5e575-128">Si [IDisposable](/dotnet/api/system.idisposable) está implementada, se pueden desechar recursos cuando se deseche el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="5e575-128">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="5e575-129">Si se produce un error durante la ejecución de una tarea en segundo plano, hay que llamar a `Dispose`, aun cuando no se haya llamado a `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="5e575-129">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="5e575-130">Tareas en segundo plano temporizadas</span><span class="sxs-lookup"><span data-stu-id="5e575-130">Timed background tasks</span></span>

<span data-ttu-id="5e575-131">Una tarea en segundo plano temporizada hace uso de la clase [System.Threading.Timer](/dotnet/api/system.threading.timer).</span><span class="sxs-lookup"><span data-stu-id="5e575-131">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="5e575-132">El temporizador activa el método `DoWork` de la tarea.</span><span class="sxs-lookup"><span data-stu-id="5e575-132">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="5e575-133">El temporizador está deshabilitado en `StopAsync` y se desecha cuando el contenedor de servicios se elimina en `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="5e575-133">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="5e575-134">El servicio se registra en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5e575-134">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="5e575-135">Consumir un servicio con ámbito en una tarea en segundo plano</span><span class="sxs-lookup"><span data-stu-id="5e575-135">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="5e575-136">Para usar servicios con ámbito en un `IHostedService`, cree un ámbito.</span><span class="sxs-lookup"><span data-stu-id="5e575-136">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="5e575-137">No se crean ámbitos de forma predeterminada para los servicios hospedados.</span><span class="sxs-lookup"><span data-stu-id="5e575-137">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="5e575-138">El servicio de tareas en segundo plano con ámbito contiene la lógica de la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="5e575-138">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="5e575-139">En el siguiente ejemplo, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) se inserta en el servicio:</span><span class="sxs-lookup"><span data-stu-id="5e575-139">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="5e575-140">El servicio hospedado crea un ámbito con objeto de resolver el servicio de tareas en segundo plano con ámbito para llamar a su método `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="5e575-140">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="5e575-141">Los servicios se registran en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5e575-141">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="5e575-142">Tareas en segundo plano en cola</span><span class="sxs-lookup"><span data-stu-id="5e575-142">Queued background tasks</span></span>

<span data-ttu-id="5e575-143">Las colas de tareas en segundo plano se basan en [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) de .NET 4.x ([está previsto que se integre en ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="5e575-143">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="5e575-144">En `QueueHostedService`, las tareas en segundo plano (`workItem`) que están en cola se quitan de ella y se ejecutan:</span><span class="sxs-lookup"><span data-stu-id="5e575-144">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="5e575-145">Los servicios se registran en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5e575-145">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="5e575-146">En la clase de modelo de página de índice, `IBackgroundTaskQueue` se inserta en el constructor y se asigna a `Queue`:</span><span class="sxs-lookup"><span data-stu-id="5e575-146">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="5e575-147">Cuando se hace clic en el botón **Agregar tarea** en la página de índice, se ejecuta el método `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="5e575-147">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="5e575-148">Se llama a `QueueBackgroundWorkItem` para poner en cola el elemento de trabajo:</span><span class="sxs-lookup"><span data-stu-id="5e575-148">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="5e575-149">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5e575-149">Additional resources</span></span>

* [<span data-ttu-id="5e575-150">Implementar tareas en segundo plano en microservicios con IHostedService y la clase BackgroundService</span><span class="sxs-lookup"><span data-stu-id="5e575-150">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="5e575-151">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="5e575-151">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
