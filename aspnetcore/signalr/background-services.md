---
title: Host ASP.NET Core SignalR en servicios en segundo plano
author: bradygaster
description: Obtenga información sobre cómo enviar mensajes a los clientes de SignalR desde las clases de .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: b359bd7f6b0667aeb8d9c8f5eb450637b1347b19
ms.sourcegitcommit: e418cb9cddeb3de06fa0cb4fdb5529da03ff6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55739675"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a>Host ASP.NET Core SignalR en servicios en segundo plano

Por [Brady Gaster](https://twitter.com/bradygaster)

En este artículo se proporciona instrucciones para:

* Hospedaje de concentradores de SignalR con un proceso de trabajo en segundo plano hospedado con ASP.NET Core.
* Envío de mensajes a los clientes desde dentro de un núcleo de .NET conectados [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(cómo descargar)](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>Conectar SignalR durante el inicio

Hospedaje de concentradores de SignalR de ASP.NET Core en el contexto de un proceso de trabajo en segundo plano es idéntico al hospedaje de Hub en una aplicación web ASP.NET Core. En el `Startup.ConfigureServices` , la llamada a método `services.AddSignalR` agrega los servicios necesarios a la capa de inserción de dependencias de ASP.NET Core (DI) para admitir SignalR. En `Startup.Configure`, el `UseSignalR` se llama al método para conectar los puntos de conexión de concentrador en la canalización de solicitudes de ASP.NET Core.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

En el ejemplo anterior, el `ClockHub` la clase implementa la `Hub<T>` clase para crear un concentrador fuertemente tipado. El `ClockHub` se ha configurado en el `Startup` clase responder a las solicitudes en el punto de conexión `/hubs/clock`.

Para obtener más información sobre los concentradores fuertemente tipados, vea [usar concentradores de SignalR para ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Esta funcionalidad no se limita a la [concentrador\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1) clase. Cualquier clase que hereda de [concentrador](xref:Microsoft.AspNetCore.SignalR.Hub), tales como [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), también funcionará.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

La interfaz utilizada por fuertemente tipado `ClockHub` es el `IClock` interfaz.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>Llamar a un concentrador SignalR desde un servicio en segundo plano

Durante el inicio, el `Worker` (clase), un `BackgroundService`, está dispuesta utilizando `AddHostedService`.

```csharp
services.AddHostedService<Worker>();
```

Puesto que también está dispuesta SignalR durante el `Startup` fase, en que cada concentrador se conecta a un punto de conexión individual en la canalización de solicitudes HTTP de ASP.NET Core, cada concentrador se representa mediante un `IHubContext<T>` en el servidor. Características de inserción de dependencias de mediante ASP.NET Core, otras clases que se crea una instancia de la capa de hospedaje, como `BackgroundService` clases, las clases de controlador de MVC o modelos de página de Razor, pueden obtener las referencias a los concentradores de servidor mediante la aceptación de las instancias de `IHubContext<ClockHub, IClock>` durante la construcción.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

Como el `ExecuteAsync` método se llama de forma iterativa en el servicio en segundo plano, el servidor de la fecha y hora actuales se envían a los clientes conectados mediante el `ClockHub`.

## <a name="react-to-signalr-events-with-background-services"></a>Reacción a eventos de SignalR con servicios en segundo plano

Al igual que una aplicación de página única mediante el cliente de JavaScript para SignalR o una aplicación de escritorio de .NET puede hacer mediante el uso de la <xref:signalr/dotnet-client>, un `BackgroundService` o `IHostedService` implementación también se puede usar para conectarse a los concentradores de SignalR y responder a eventos.

El `ClockHubClient` clase implementa tanto la `IClock` interfaz y la `IHostedService` interfaz. De este modo puede dispuesta durante `Startup` ejecutarse continuamente y responder a eventos de Hub desde el servidor. 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Durante la inicialización, el `ClockHubClient` crea una instancia de un `HubConnection` y conecta la `IClock.ShowTime` método como controlador para el concentrador `ShowTime` eventos.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

En el `IHostedService.StartAsync` implementación, el `HubConnection` se inicia de forma asincrónica.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Durante la `IHostedService.StopAsync` método, el `HubConnection` se desecha de forma asincrónica.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción](xref:tutorials/signalr)
* [Concentradores](xref:signalr/hubs)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
* [Concentradores fuertemente tipados](xref:signalr/hubs#strongly-typed-hubs)
