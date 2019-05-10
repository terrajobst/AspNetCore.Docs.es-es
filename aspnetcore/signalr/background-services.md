---
title: Host ASP.NET Core SignalR en servicios en segundo plano
author: bradygaster
description: Obtenga información sobre cómo enviar mensajes a los clientes de SignalR desde las clases de .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: dcd62f0c7056a3f987291b6c8bb8b87f94160865
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087755"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="5fea1-103">Host ASP.NET Core SignalR en servicios en segundo plano</span><span class="sxs-lookup"><span data-stu-id="5fea1-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="5fea1-104">Por [Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="5fea1-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="5fea1-105">En este artículo se proporciona instrucciones para:</span><span class="sxs-lookup"><span data-stu-id="5fea1-105">This article provides guidance for:</span></span>

* <span data-ttu-id="5fea1-106">Hospedaje de concentradores de SignalR con un proceso de trabajo en segundo plano hospedado con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5fea1-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="5fea1-107">Envío de mensajes a los clientes desde dentro de un núcleo de .NET conectados [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span><span class="sxs-lookup"><span data-stu-id="5fea1-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="5fea1-108">[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(cómo descargar)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="5fea1-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="wire-up-signalr-during-startup"></a><span data-ttu-id="5fea1-109">Conectar SignalR durante el inicio</span><span class="sxs-lookup"><span data-stu-id="5fea1-109">Wire up SignalR during startup</span></span>

<span data-ttu-id="5fea1-110">Hospedaje de concentradores de SignalR de ASP.NET Core en el contexto de un proceso de trabajo en segundo plano es idéntico al hospedaje de Hub en una aplicación web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5fea1-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="5fea1-111">En el `Startup.ConfigureServices` , la llamada a método `services.AddSignalR` agrega los servicios necesarios a la capa de inserción de dependencias de ASP.NET Core (DI) para admitir SignalR.</span><span class="sxs-lookup"><span data-stu-id="5fea1-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="5fea1-112">En `Startup.Configure`, el `UseSignalR` se llama al método para conectar los puntos de conexión de concentrador en la canalización de solicitudes de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5fea1-112">In `Startup.Configure`, the `UseSignalR` method is called to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

<span data-ttu-id="5fea1-113">En el ejemplo anterior, el `ClockHub` la clase implementa la `Hub<T>` clase para crear un concentrador fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="5fea1-113">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="5fea1-114">El `ClockHub` se ha configurado en el `Startup` clase responder a las solicitudes en el punto de conexión `/hubs/clock`.</span><span class="sxs-lookup"><span data-stu-id="5fea1-114">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="5fea1-115">Para obtener más información sobre los concentradores fuertemente tipados, vea [usar concentradores de SignalR para ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="5fea1-115">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="5fea1-116">Esta funcionalidad no se limita a la [concentrador\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1) clase.</span><span class="sxs-lookup"><span data-stu-id="5fea1-116">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="5fea1-117">Cualquier clase que hereda de [concentrador](xref:Microsoft.AspNetCore.SignalR.Hub), tales como [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), también funcionará.</span><span class="sxs-lookup"><span data-stu-id="5fea1-117">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="5fea1-118">La interfaz utilizada por fuertemente tipado `ClockHub` es el `IClock` interfaz.</span><span class="sxs-lookup"><span data-stu-id="5fea1-118">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="5fea1-119">Llamar a un concentrador SignalR desde un servicio en segundo plano</span><span class="sxs-lookup"><span data-stu-id="5fea1-119">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="5fea1-120">Durante el inicio, el `Worker` (clase), un `BackgroundService`, está dispuesta utilizando `AddHostedService`.</span><span class="sxs-lookup"><span data-stu-id="5fea1-120">During startup, the `Worker` class, a `BackgroundService`, is wired up using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="5fea1-121">Puesto que también está dispuesta SignalR durante el `Startup` fase, en que cada concentrador se conecta a un punto de conexión individual en la canalización de solicitudes HTTP de ASP.NET Core, cada concentrador se representa mediante un `IHubContext<T>` en el servidor.</span><span class="sxs-lookup"><span data-stu-id="5fea1-121">Since SignalR is also wired up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="5fea1-122">Características de inserción de dependencias de mediante ASP.NET Core, otras clases que se crea una instancia de la capa de hospedaje, como `BackgroundService` clases, las clases de controlador de MVC o modelos de página de Razor, pueden obtener las referencias a los concentradores de servidor mediante la aceptación de las instancias de `IHubContext<ClockHub, IClock>` durante la construcción.</span><span class="sxs-lookup"><span data-stu-id="5fea1-122">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="5fea1-123">Como el `ExecuteAsync` método se llama de forma iterativa en el servicio en segundo plano, el servidor de la fecha y hora actuales se envían a los clientes conectados mediante el `ClockHub`.</span><span class="sxs-lookup"><span data-stu-id="5fea1-123">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="5fea1-124">Reacción a eventos de SignalR con servicios en segundo plano</span><span class="sxs-lookup"><span data-stu-id="5fea1-124">React to SignalR events with background services</span></span>

<span data-ttu-id="5fea1-125">Al igual que una aplicación de página única mediante el cliente de JavaScript para SignalR o una aplicación de escritorio de .NET puede hacer mediante el uso de la <xref:signalr/dotnet-client>, un `BackgroundService` o `IHostedService` implementación también se puede usar para conectarse a los concentradores de SignalR y responder a eventos.</span><span class="sxs-lookup"><span data-stu-id="5fea1-125">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="5fea1-126">El `ClockHubClient` clase implementa tanto la `IClock` interfaz y la `IHostedService` interfaz.</span><span class="sxs-lookup"><span data-stu-id="5fea1-126">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="5fea1-127">De este modo puede dispuesta durante `Startup` ejecutarse continuamente y responder a eventos de Hub desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="5fea1-127">This way it can be wired up during `Startup` to run continuously and respond to Hub events from the server.</span></span> 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="5fea1-128">Durante la inicialización, el `ClockHubClient` crea una instancia de un `HubConnection` y conecta la `IClock.ShowTime` método como controlador para el concentrador `ShowTime` eventos.</span><span class="sxs-lookup"><span data-stu-id="5fea1-128">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and wires up the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="5fea1-129">En el `IHostedService.StartAsync` implementación, el `HubConnection` se inicia de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="5fea1-129">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="5fea1-130">Durante la `IHostedService.StopAsync` método, el `HubConnection` se desecha de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="5fea1-130">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="5fea1-131">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5fea1-131">Additional resources</span></span>

* [<span data-ttu-id="5fea1-132">Introducción</span><span class="sxs-lookup"><span data-stu-id="5fea1-132">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="5fea1-133">Concentradores</span><span class="sxs-lookup"><span data-stu-id="5fea1-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="5fea1-134">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="5fea1-134">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="5fea1-135">Concentradores fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="5fea1-135">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
