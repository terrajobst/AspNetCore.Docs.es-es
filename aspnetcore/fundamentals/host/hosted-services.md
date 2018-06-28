---
title: Tareas en segundo plano con servicios hospedados en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo implementar tareas en segundo plano con servicios hospedados en ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 13ac7e266b657bc186188b2b6f40204cfd936fca
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341826"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Tareas en segundo plano con servicios hospedados en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

En ASP.NET Core, las tareas en segundo plano se pueden implementar como *servicios hospedados*. Un servicio hospedado es una clase con lógica de tarea en segundo plano que implementa la interfaz [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). En este tema se incluyen tres ejemplos de servicio hospedado:

* Una tarea en segundo plano que se ejecuta según un temporizador.
* Un servicio hospedado que activa un servicio con ámbito. El servicio con ámbito puede usar la inserción de dependencias.
* Tareas en segundo plano en cola que se ejecutan en secuencia.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

La aplicación de ejemplo se ofrece en dos versiones:

* Host de web: el host de web resulta útil para hospedar aplicaciones web. El código de ejemplo que se muestra en este tema corresponde a la versión de host de web del ejemplo. Para más información, vea el sitio web [Host de web](xref:fundamentals/host/web-host).
* Host genérico: el host genérico es nuevo en ASP.NET Core 2.1. Para más información, vea el sitio web [Host genérico](xref:fundamentals/host/generic-host).

::: moniker-end

## <a name="ihostedservice-interface"></a>Interfaz IHostedService

Los servicios hospedados implementan la interfaz [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice). Esta interfaz define dos métodos para los objetos administrados por el host:

* [StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync): se llama después de que el servidor se haya iniciado y [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) se haya activado. `StartAsync` contiene la lógica para iniciar la tarea en segundo plano.

* [StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync): se activa cuando el host está realizando un cierre estable. `StopAsync` contiene la lógica para finalizar la tarea en segundo plano y desechar los recursos no administrados. Si la aplicación se cierra inesperadamente (por ejemplo, porque se produzca un error en el proceso de la aplicación), puede que no sea posible llamar a `StopAsync`.

El servicio hospedado se activa una vez en el inicio de la aplicación y se cierra de manera estable cuando dicha aplicación se cierra. Si [IDisposable](/dotnet/api/system.idisposable) está implementada, se pueden desechar recursos cuando se deseche el contenedor de servicios. Si se produce un error durante la ejecución de una tarea en segundo plano, hay que llamar a `Dispose`, aun cuando no se haya llamado a `StopAsync`.

## <a name="timed-background-tasks"></a>Tareas en segundo plano temporizadas

Una tarea en segundo plano temporizada hace uso de la clase [System.Threading.Timer](/dotnet/api/system.threading.timer). El temporizador activa el método `DoWork` de la tarea. El temporizador está deshabilitado en `StopAsync` y se desecha cuando el contenedor de servicios se elimina en `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

::: moniker range=">= aspnetcore-2.1"

El servicio se registra en `Startup.ConfigureServices` con el método de extensión `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

El servicio se registra en `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Consumir un servicio con ámbito en una tarea en segundo plano

Para usar servicios con ámbito en un `IHostedService`, cree un ámbito. No se crean ámbitos de forma predeterminada para los servicios hospedados.

El servicio de tareas en segundo plano con ámbito contiene la lógica de la tarea en segundo plano. En el siguiente ejemplo, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) se inserta en el servicio:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

El servicio hospedado crea un ámbito con objeto de resolver el servicio de tareas en segundo plano con ámbito para llamar a su método `DoWork`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

::: moniker range=">= aspnetcore-2.1"

Los servicios se registran en `Startup.ConfigureServices`. La implementación `IHostedService` se registra con el método de extensión `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Los servicios se registran en `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

::: moniker-end

## <a name="queued-background-tasks"></a>Tareas en segundo plano en cola

Las colas de tareas en segundo plano se basan en [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) de .NET 4.x ([está previsto que se integre en ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

En `QueueHostedService`, las tareas en segundo plano (`workItem`) que están en cola se quitan de ella y se ejecutan:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

::: moniker range=">= aspnetcore-2.1"

Los servicios se registran en `Startup.ConfigureServices`. La implementación `IHostedService` se registra con el método de extensión `AddHostedService`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Los servicios se registran en `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

::: moniker-end

En la clase de modelo de página de índice, `IBackgroundTaskQueue` se inserta en el constructor y se asigna a `Queue`:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

Cuando se hace clic en el botón **Agregar tarea** en la página de índice, se ejecuta el método `OnPostAddTask`. Se llama a `QueueBackgroundWorkItem` para poner en cola el elemento de trabajo:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Recursos adicionales

* [Implementar tareas en segundo plano en microservicios con IHostedService y la clase BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [System.Threading.Timer](/dotnet/api/system.threading.timer)
