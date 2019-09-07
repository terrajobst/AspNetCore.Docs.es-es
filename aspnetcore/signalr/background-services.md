---
title: Host ASP.NET Core Signalr en servicios en segundo plano
author: bradygaster
description: Obtenga información sobre cómo enviar mensajes a los clientes de Signalr desde clases BackgroundService de .NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: 23a53f33a03ce3b76cf6846c3f214a6cad055209
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773940"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="54cdf-103">Host ASP.NET Core Signalr en servicios en segundo plano</span><span class="sxs-lookup"><span data-stu-id="54cdf-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="54cdf-104">Por el [Brady](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="54cdf-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="54cdf-105">En este artículo se proporcionan instrucciones para:</span><span class="sxs-lookup"><span data-stu-id="54cdf-105">This article provides guidance for:</span></span>

* <span data-ttu-id="54cdf-106">Hospedaje de concentradores de Signalr mediante un proceso de trabajo en segundo plano hospedado con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="54cdf-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="54cdf-107">Envío de mensajes a clientes conectados desde un [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)de .net Core.</span><span class="sxs-lookup"><span data-stu-id="54cdf-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="54cdf-108">[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(Cómo descargar)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="54cdf-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="wire-up-signalr-during-startup"></a><span data-ttu-id="54cdf-109">Signalr de conexión durante el inicio</span><span class="sxs-lookup"><span data-stu-id="54cdf-109">Wire up SignalR during startup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="54cdf-110">El hospedaje de los concentradores de Signalr ASP.NET Core en el contexto de un proceso de trabajo en segundo plano es idéntico al hospedaje de un concentrador en una aplicación Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="54cdf-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="54cdf-111">En el `Startup.ConfigureServices` método, la `services.AddSignalR` llamada a agrega los servicios necesarios a la capa de ASP.net Core de la inserción de dependencias (di) para admitir signalr.</span><span class="sxs-lookup"><span data-stu-id="54cdf-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="54cdf-112">En `Startup.Configure`, se `MapHub` llama al método en la `UseEndpoints` devolución de llamada para conectar los puntos de conexión del concentrador en el ASP.net Core canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="54cdf-112">In `Startup.Configure`, the `MapHub` method is called in the `UseEndpoints` callback to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

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

<span data-ttu-id="54cdf-113">El hospedaje de los concentradores de Signalr ASP.NET Core en el contexto de un proceso de trabajo en segundo plano es idéntico al hospedaje de un concentrador en una aplicación Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="54cdf-113">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="54cdf-114">En el `Startup.ConfigureServices` método, la `services.AddSignalR` llamada a agrega los servicios necesarios a la capa de ASP.net Core de la inserción de dependencias (di) para admitir signalr.</span><span class="sxs-lookup"><span data-stu-id="54cdf-114">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="54cdf-115">`Startup.Configure` En`UseSignalR` , se llama al método para conectar los puntos de conexión del concentrador en el ASP.net Core canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="54cdf-115">In `Startup.Configure`, the `UseSignalR` method is called to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

<span data-ttu-id="54cdf-116">En el ejemplo anterior, la `ClockHub` clase implementa la `Hub<T>` clase para crear un concentrador fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="54cdf-116">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="54cdf-117">Se ha configurado en la `Startup` clase para responder a las solicitudes en el extremo `/hubs/clock`. `ClockHub`</span><span class="sxs-lookup"><span data-stu-id="54cdf-117">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="54cdf-118">Para obtener más información sobre los concentradores fuertemente tipados, consulte [uso de hubs en signalr para ASP.net Core](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="54cdf-118">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="54cdf-119">Esta funcionalidad no se limita a la clase de [\<> Hub t](xref:Microsoft.AspNetCore.SignalR.Hub`1) .</span><span class="sxs-lookup"><span data-stu-id="54cdf-119">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="54cdf-120">Cualquier clase que herede de [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), como [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), también funcionará.</span><span class="sxs-lookup"><span data-stu-id="54cdf-120">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="54cdf-121">La interfaz utilizada por el fuertemente tipado `ClockHub` es la `IClock` interfaz.</span><span class="sxs-lookup"><span data-stu-id="54cdf-121">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="54cdf-122">Llamada a un concentrador Signalr desde un servicio en segundo plano</span><span class="sxs-lookup"><span data-stu-id="54cdf-122">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="54cdf-123">Durante el inicio, `Worker` la clase, `BackgroundService`a, se conecta mediante `AddHostedService`.</span><span class="sxs-lookup"><span data-stu-id="54cdf-123">During startup, the `Worker` class, a `BackgroundService`, is wired up using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="54cdf-124">Como signalr también se conecta durante la `Startup` fase, en la que cada concentrador se conecta a un punto de conexión individual en la canalización de solicitudes HTTP de ASP.net Core, cada concentrador se representa mediante un `IHubContext<T>` en el servidor.</span><span class="sxs-lookup"><span data-stu-id="54cdf-124">Since SignalR is also wired up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="54cdf-125">Mediante el uso de las características de di de ASP.net Core, otras clases creadas por la `BackgroundService` capa de hospedaje, como las clases, las clases de controlador de MVC o los modelos de página de Razor, pueden `IHubContext<ClockHub, IClock>` obtener referencias a los concentradores del lado servidor aceptando instancias de durante la construcción.</span><span class="sxs-lookup"><span data-stu-id="54cdf-125">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="54cdf-126">A medida `ExecuteAsync` que se llama al método de forma iterativa en el servicio en segundo plano, la fecha y hora actuales del servidor se envían a `ClockHub`los clientes conectados mediante.</span><span class="sxs-lookup"><span data-stu-id="54cdf-126">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="54cdf-127">Reaccionar a eventos de Signalr con servicios en segundo plano</span><span class="sxs-lookup"><span data-stu-id="54cdf-127">React to SignalR events with background services</span></span>

<span data-ttu-id="54cdf-128">Al igual que una aplicación de una sola página que usa el cliente de JavaScript para signalr o una aplicación de escritorio <xref:signalr/dotnet-client>de .net `BackgroundService` puede `IHostedService` usar mediante el, también se puede usar una implementación de o para conectarse a los concentradores de signalr y responder a los eventos.</span><span class="sxs-lookup"><span data-stu-id="54cdf-128">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="54cdf-129">La `ClockHubClient` clase implementa la `IClock` interfaz y la `IHostedService` interfaz.</span><span class="sxs-lookup"><span data-stu-id="54cdf-129">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="54cdf-130">De este modo, se puede conectar durante `Startup` para ejecutarse continuamente y responder a los eventos del concentrador desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="54cdf-130">This way it can be wired up during `Startup` to run continuously and respond to Hub events from the server.</span></span>

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="54cdf-131">Durante la `ClockHubClient` inicialización, crea una instancia `HubConnection` de y conecta el `IClock.ShowTime` método como controlador para el evento del `ShowTime` centro.</span><span class="sxs-lookup"><span data-stu-id="54cdf-131">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and wires up the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="54cdf-132">En la `IHostedService.StartAsync` implementación de `HubConnection` , se inicia de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="54cdf-132">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="54cdf-133">Durante el `IHostedService.StopAsync` método `HubConnection` , se elimina de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="54cdf-133">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="54cdf-134">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="54cdf-134">Additional resources</span></span>

* [<span data-ttu-id="54cdf-135">Introducción</span><span class="sxs-lookup"><span data-stu-id="54cdf-135">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="54cdf-136">Concentradores</span><span class="sxs-lookup"><span data-stu-id="54cdf-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="54cdf-137">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="54cdf-137">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="54cdf-138">Concentradores fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="54cdf-138">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
