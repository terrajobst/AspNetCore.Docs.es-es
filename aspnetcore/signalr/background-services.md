---
title: SignalR de ASP.NET Core host en los servicios en segundo plano
author: bradygaster
description: Obtenga información sobre cómo enviar mensajes a SignalR clientes desde clases BackgroundService de .NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/background-services
ms.openlocfilehash: 86319cc93febab18c29e2fb6366cef0d025943ba
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651587"
---
# <a name="host-aspnet-core-opno-locsignalr-in-background-services"></a><span data-ttu-id="6f9c3-103">SignalR de ASP.NET Core host en los servicios en segundo plano</span><span class="sxs-lookup"><span data-stu-id="6f9c3-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="6f9c3-104">Por el [Brady](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="6f9c3-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="6f9c3-105">En este artículo se proporcionan instrucciones para:</span><span class="sxs-lookup"><span data-stu-id="6f9c3-105">This article provides guidance for:</span></span>

* <span data-ttu-id="6f9c3-106">Hospedaje de SignalR hubs mediante un proceso de trabajo en segundo plano hospedado con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="6f9c3-107">Envío de mensajes a clientes conectados desde un [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)de .net Core.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="6f9c3-108">[Ver o descargar el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(cómo descargarlo)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="6f9c3-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="enable-opno-locsignalr-in-startup"></a><span data-ttu-id="6f9c3-109">Habilitar SignalR en el inicio</span><span class="sxs-lookup"><span data-stu-id="6f9c3-109">Enable SignalR in startup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6f9c3-110">Hospedar ASP.NET Core SignalR hubs en el contexto de un proceso de trabajo en segundo plano es idéntico a hospedar un centro en una aplicación Web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="6f9c3-111">En el método `Startup.ConfigureServices`, al llamar a `services.AddSignalR` se agregan los servicios necesarios a la capa ASP.NET Core de inserción de dependencias (DI) para admitir SignalR.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="6f9c3-112">En `Startup.Configure`, se llama al método de `MapHub` en la devolución de llamada de `UseEndpoints` para conectar los puntos de conexión del concentrador en la canalización de solicitudes de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-112">In `Startup.Configure`, the `MapHub` method is called in the `UseEndpoints` callback to connect the Hub endpoints in the ASP.NET Core request pipeline.</span></span>

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSignalR();
        services.AddHostedService<Worker>();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.UseRouting();
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHub<ClockHub>("/hubs/clock");
        });
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="6f9c3-113">Hospedar ASP.NET Core SignalR hubs en el contexto de un proceso de trabajo en segundo plano es idéntico a hospedar un centro en una aplicación Web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-113">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="6f9c3-114">En el método `Startup.ConfigureServices`, al llamar a `services.AddSignalR` se agregan los servicios necesarios a la capa ASP.NET Core de inserción de dependencias (DI) para admitir SignalR.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-114">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="6f9c3-115">En `Startup.Configure`, se llama al método `UseSignalR` para conectar los puntos de conexión del concentrador en la canalización de solicitudes de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-115">In `Startup.Configure`, the `UseSignalR` method is called to connect the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

<span data-ttu-id="6f9c3-116">En el ejemplo anterior, la clase `ClockHub` implementa la clase `Hub<T>` para crear un concentrador fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-116">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="6f9c3-117">El `ClockHub` se ha configurado en la clase `Startup` para responder a las solicitudes en el extremo `/hubs/clock`.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-117">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="6f9c3-118">Para obtener más información sobre los concentradores fuertemente tipados, consulte [uso de hubs en SignalR para ASP.net Core](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="6f9c3-118">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="6f9c3-119">Esta funcionalidad no se limita a la clase de [> Hub\<t](xref:Microsoft.AspNetCore.SignalR.Hub`1) .</span><span class="sxs-lookup"><span data-stu-id="6f9c3-119">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="6f9c3-120">Cualquier clase que herede de [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), como [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), también funcionará.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-120">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="6f9c3-121">La interfaz utilizada por el `ClockHub` fuertemente tipado es la interfaz de `IClock`.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-121">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-opno-locsignalr-hub-from-a-background-service"></a><span data-ttu-id="6f9c3-122">Llamada a un concentrador de SignalR desde un servicio en segundo plano</span><span class="sxs-lookup"><span data-stu-id="6f9c3-122">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="6f9c3-123">Durante el inicio, la clase `Worker`, una `BackgroundService`, se habilita mediante `AddHostedService`.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-123">During startup, the `Worker` class, a `BackgroundService`, is enabled using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="6f9c3-124">Como SignalR también está habilitado durante la fase de `Startup`, en la que cada concentrador se adjunta a un punto de conexión individual de la canalización de solicitudes HTTP de ASP.NET Core, cada concentrador se representa mediante una `IHubContext<T>` en el servidor.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-124">Since SignalR is also enabled up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="6f9c3-125">Mediante el uso de las características DI de ASP.NET Core, otras clases a las que se ha creado una instancia de la capa de hospedaje, como las clases `BackgroundService`, las clases de controlador de MVC o los modelos de página de Razor, pueden obtener referencias a los concentradores del lado servidor aceptando instancias de `IHubContext<ClockHub, IClock>` durante la construcción.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-125">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="6f9c3-126">A medida que se llama al método `ExecuteAsync` de forma iterativa en el servicio en segundo plano, la fecha y hora actuales del servidor se envían a los clientes conectados mediante el `ClockHub`.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-126">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-opno-locsignalr-events-with-background-services"></a><span data-ttu-id="6f9c3-127">Reaccionar ante eventos de SignalR con servicios en segundo plano</span><span class="sxs-lookup"><span data-stu-id="6f9c3-127">React to SignalR events with background services</span></span>

<span data-ttu-id="6f9c3-128">Al igual que una aplicación de una sola página que usa el cliente de JavaScript para SignalR o una aplicación de escritorio de .NET puede usar el <xref:signalr/dotnet-client>, también se puede usar una implementación de `BackgroundService` o `IHostedService` para conectarse a los concentradores de SignalR y responder a los eventos.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-128">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="6f9c3-129">La clase `ClockHubClient` implementa la interfaz `IClock` y la interfaz `IHostedService`.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-129">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="6f9c3-130">De este modo, se puede habilitar durante la ejecución de `Startup` para que se ejecute continuamente y responda a los eventos de concentrador del servidor.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-130">This way it can be enabled during `Startup` to run continuously and respond to Hub events from the server.</span></span>

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="6f9c3-131">Durante la inicialización, el `ClockHubClient` crea una instancia de un `HubConnection` y habilita el método `IClock.ShowTime` como el controlador para el evento de `ShowTime` del centro.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-131">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and enables the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="6f9c3-132">En la implementación de `IHostedService.StartAsync`, el `HubConnection` se inicia de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-132">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="6f9c3-133">Durante el método `IHostedService.StopAsync`, el `HubConnection` se elimina de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="6f9c3-133">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="6f9c3-134">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="6f9c3-134">Additional resources</span></span>

* [<span data-ttu-id="6f9c3-135">Introducción</span><span class="sxs-lookup"><span data-stu-id="6f9c3-135">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="6f9c3-136">Concentradores</span><span class="sxs-lookup"><span data-stu-id="6f9c3-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="6f9c3-137">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="6f9c3-137">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="6f9c3-138">Concentradores fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="6f9c3-138">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
