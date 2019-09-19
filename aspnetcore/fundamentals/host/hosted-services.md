---
title: Tareas en segundo plano con servicios hospedados en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo implementar tareas en segundo plano con servicios hospedados en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/03/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 1db3ee1a9bcc0d41edf24df55bcd8d54fb0e9724
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081778"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="94b6c-103">Tareas en segundo plano con servicios hospedados en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="94b6c-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="94b6c-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="94b6c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="94b6c-105">En ASP.NET Core, las tareas en segundo plano se pueden implementar como *servicios hospedados*.</span><span class="sxs-lookup"><span data-stu-id="94b6c-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="94b6c-106">Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="94b6c-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="94b6c-107">En este tema se incluyen tres ejemplos de servicio hospedado:</span><span class="sxs-lookup"><span data-stu-id="94b6c-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="94b6c-108">Una tarea en segundo plano que se ejecuta según un temporizador.</span><span class="sxs-lookup"><span data-stu-id="94b6c-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="94b6c-109">Un servicio hospedado que activa un [servicio con ámbito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="94b6c-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="94b6c-110">El servicio con ámbito puede usar la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="94b6c-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="94b6c-111">Tareas en segundo plano en cola que se ejecutan en secuencia.</span><span class="sxs-lookup"><span data-stu-id="94b6c-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="94b6c-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="94b6c-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="94b6c-113">La aplicación de ejemplo se ofrece en dos versiones:</span><span class="sxs-lookup"><span data-stu-id="94b6c-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="94b6c-114">Host web: el host web resulta útil para hospedar aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="94b6c-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="94b6c-115">El código de ejemplo que se muestra en este tema corresponde a la versión de host web del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="94b6c-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="94b6c-116">Para más información, vea el sitio web [Host de web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="94b6c-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="94b6c-117">Host genérico: el host genérico es nuevo en ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="94b6c-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="94b6c-118">Para más información, vea el sitio web [Host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="94b6c-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="94b6c-119">Plantilla Worker Service</span><span class="sxs-lookup"><span data-stu-id="94b6c-119">Worker Service template</span></span>

<span data-ttu-id="94b6c-120">La plantilla Worker Service de ASP.NET Core sirve de punto de partida para escribir aplicaciones de servicio de larga duración.</span><span class="sxs-lookup"><span data-stu-id="94b6c-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="94b6c-121">Para usar la plantilla como base de una aplicación de servicios hospedados:</span><span class="sxs-lookup"><span data-stu-id="94b6c-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="94b6c-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="94b6c-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="94b6c-123">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="94b6c-123">Create a new project.</span></span>
1. <span data-ttu-id="94b6c-124">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="94b6c-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="94b6c-125">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="94b6c-125">Select **Next**.</span></span>
1. <span data-ttu-id="94b6c-126">Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado.</span><span class="sxs-lookup"><span data-stu-id="94b6c-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="94b6c-127">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="94b6c-127">Select **Create**.</span></span>
1. <span data-ttu-id="94b6c-128">En el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, confirme que las opciones **.NET Core** y **ASP.NET Core 3.0** estén seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="94b6c-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="94b6c-129">Seleccione la plantilla **Worker Service**.</span><span class="sxs-lookup"><span data-stu-id="94b6c-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="94b6c-130">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="94b6c-130">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="94b6c-131">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="94b6c-131">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="94b6c-132">Desde un shell de comandos, use la plantilla Worker Service (`worker`) con el comando [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="94b6c-132">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="94b6c-133">En el ejemplo siguiente, se crea una aplicación Worker Service llamada `ContosoWorkerService`.</span><span class="sxs-lookup"><span data-stu-id="94b6c-133">In the following example, a Worker Service app is created named `ContosoWorkerService`.</span></span> <span data-ttu-id="94b6c-134">Al ejecutar el comando, se crea automáticamente una carpeta para la aplicación `ContosoWorkerService`.</span><span class="sxs-lookup"><span data-stu-id="94b6c-134">A folder for the `ContosoWorkerService` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="package"></a><span data-ttu-id="94b6c-135">Package</span><span class="sxs-lookup"><span data-stu-id="94b6c-135">Package</span></span>

<span data-ttu-id="94b6c-136">Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="94b6c-136">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="94b6c-137">Interfaz IHostedService</span><span class="sxs-lookup"><span data-stu-id="94b6c-137">IHostedService interface</span></span>

<span data-ttu-id="94b6c-138">Los servicios hospedados implementan la interfaz <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="94b6c-138">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="94b6c-139">Esta interfaz define dos métodos para los objetos administrados por el host:</span><span class="sxs-lookup"><span data-stu-id="94b6c-139">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="94b6c-140">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*): `StartAsync` contiene la lógica para iniciar la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="94b6c-140">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="94b6c-141">Al utilizar el [host web](xref:fundamentals/host/web-host), se llama a `StartAsync` después de que el servidor se haya iniciado y se haya activado [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="94b6c-141">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="94b6c-142">Al utilizar el [host genérico](xref:fundamentals/host/generic-host), se llama a `StartAsync` antes de que se desencadene `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="94b6c-142">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="94b6c-143">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*): se activa cuando el host está realizando un cierre estable.</span><span class="sxs-lookup"><span data-stu-id="94b6c-143">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="94b6c-144">`StopAsync` contiene la lógica para finalizar la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="94b6c-144">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="94b6c-145">Implemente <xref:System.IDisposable> y los [finalizadores (destructores)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) para desechar los recursos no administrados.</span><span class="sxs-lookup"><span data-stu-id="94b6c-145">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="94b6c-146">El token de cancelación tiene un tiempo de espera predeterminado de cinco segundos para indicar que el proceso de cierre ya no debería ser estable.</span><span class="sxs-lookup"><span data-stu-id="94b6c-146">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="94b6c-147">Cuando se solicita la cancelación en el token:</span><span class="sxs-lookup"><span data-stu-id="94b6c-147">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="94b6c-148">Se deben anular las operaciones restantes en segundo plano que realiza la aplicación.</span><span class="sxs-lookup"><span data-stu-id="94b6c-148">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="94b6c-149">Los métodos llamados en `StopAsync` deberían devolver contenido al momento.</span><span class="sxs-lookup"><span data-stu-id="94b6c-149">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="94b6c-150">No obstante, las tareas no se abandonan después de solicitar la cancelación, sino que el autor de la llamada espera a que se completen todas las tareas.</span><span class="sxs-lookup"><span data-stu-id="94b6c-150">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="94b6c-151">Si la aplicación se cierra inesperadamente (por ejemplo, porque se produzca un error en el proceso de la aplicación), puede que no sea posible llamar a `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="94b6c-151">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="94b6c-152">Por lo tanto, los métodos llamados o las operaciones llevadas a cabo en `StopAsync` podrían no producirse.</span><span class="sxs-lookup"><span data-stu-id="94b6c-152">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="94b6c-153">Para ampliar el tiempo de espera predeterminado de apagado de 5 segundos, establezca:</span><span class="sxs-lookup"><span data-stu-id="94b6c-153">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="94b6c-154"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> cuando se usa el host genérico.</span><span class="sxs-lookup"><span data-stu-id="94b6c-154"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="94b6c-155">Para más información, consulte <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="94b6c-155">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="94b6c-156">Configuración de los valores de host de tiempo de espera de apagado cuando se usa el host web.</span><span class="sxs-lookup"><span data-stu-id="94b6c-156">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="94b6c-157">Para más información, consulte <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="94b6c-157">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="94b6c-158">El servicio hospedado se activa una vez al inicio de la aplicación y se cierra de manera estable cuando dicha aplicación se cierra.</span><span class="sxs-lookup"><span data-stu-id="94b6c-158">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="94b6c-159">Si se produce un error durante la ejecución de una tarea en segundo plano, hay que llamar a `Dispose`, aun cuando no se haya llamado a `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="94b6c-159">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="94b6c-160">Tareas en segundo plano temporizadas</span><span class="sxs-lookup"><span data-stu-id="94b6c-160">Timed background tasks</span></span>

<span data-ttu-id="94b6c-161">Una tarea en segundo plano temporizada hace uso de la clase [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="94b6c-161">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="94b6c-162">El temporizador activa el método `DoWork` de la tarea.</span><span class="sxs-lookup"><span data-stu-id="94b6c-162">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="94b6c-163">El temporizador está deshabilitado en `StopAsync` y se desecha cuando el contenedor de servicios se elimina en `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="94b6c-163">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="94b6c-164">El servicio se registra en `Startup.ConfigureServices` con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="94b6c-164">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="94b6c-165">Consumir un servicio con ámbito en una tarea en segundo plano</span><span class="sxs-lookup"><span data-stu-id="94b6c-165">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="94b6c-166">Para usar [servicios con ámbito](xref:fundamentals/dependency-injection#service-lifetimes) en un elemento `IHostedService`, cree un ámbito.</span><span class="sxs-lookup"><span data-stu-id="94b6c-166">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="94b6c-167">No se crean ámbitos de forma predeterminada para los servicios hospedados.</span><span class="sxs-lookup"><span data-stu-id="94b6c-167">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="94b6c-168">El servicio de tareas en segundo plano con ámbito contiene la lógica de la tarea en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="94b6c-168">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="94b6c-169">En el siguiente ejemplo, <xref:Microsoft.Extensions.Logging.ILogger> se inserta en el servicio:</span><span class="sxs-lookup"><span data-stu-id="94b6c-169">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="94b6c-170">El servicio hospedado crea un ámbito con objeto de resolver el servicio de tareas en segundo plano con ámbito para llamar a su método `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="94b6c-170">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="94b6c-171">Los servicios se registran en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="94b6c-171">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="94b6c-172">La implementación `IHostedService` se registra con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="94b6c-172">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="94b6c-173">Tareas en segundo plano en cola</span><span class="sxs-lookup"><span data-stu-id="94b6c-173">Queued background tasks</span></span>

<span data-ttu-id="94b6c-174">Las colas de tareas en segundo plano se basan en <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> de .NET 4.x ([está previsto que se integre en ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="94b6c-174">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="94b6c-175">En `QueueHostedService`, las tareas en segundo plano en la cola se quitan de la cola y se ejecutan como un servicio <xref:Microsoft.Extensions.Hosting.BackgroundService>, que es una clase base para implementar `IHostedService` de ejecución prolongada:</span><span class="sxs-lookup"><span data-stu-id="94b6c-175">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="94b6c-176">Los servicios se registran en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="94b6c-176">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="94b6c-177">La implementación `IHostedService` se registra con el método de extensión `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="94b6c-177">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="94b6c-178">En la clase de modelo de página de índice:</span><span class="sxs-lookup"><span data-stu-id="94b6c-178">In the Index page model class:</span></span>

* <span data-ttu-id="94b6c-179">Se inserta `IBackgroundTaskQueue` en el constructor y se asigna a `Queue`.</span><span class="sxs-lookup"><span data-stu-id="94b6c-179">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="94b6c-180">Se inserta <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> y se asigna a `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="94b6c-180">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="94b6c-181">Se usa el generador se para crear instancias de <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, que se usa para crear servicios dentro de un ámbito.</span><span class="sxs-lookup"><span data-stu-id="94b6c-181">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="94b6c-182">Se crea un ámbito para poder usar el elemento `AppDbContext` de la aplicación (un [servicio con ámbito](xref:fundamentals/dependency-injection#service-lifetimes)) para escribir registros de base de datos en `IBackgroundTaskQueue` (un servicio de singleton).</span><span class="sxs-lookup"><span data-stu-id="94b6c-182">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="94b6c-183">Cuando se hace clic en el botón **Agregar tarea** en la página de índice, se ejecuta el método `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="94b6c-183">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="94b6c-184">Se llama a `QueueBackgroundWorkItem` para poner en cola el elemento de trabajo:</span><span class="sxs-lookup"><span data-stu-id="94b6c-184">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="94b6c-185">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="94b6c-185">Additional resources</span></span>

* [<span data-ttu-id="94b6c-186">Implementar tareas en segundo plano en microservicios con IHostedService y la clase BackgroundService</span><span class="sxs-lookup"><span data-stu-id="94b6c-186">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
