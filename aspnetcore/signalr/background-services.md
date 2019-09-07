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
# <a name="host-aspnet-core-signalr-in-background-services"></a>Host ASP.NET Core Signalr en servicios en segundo plano

Por el [Brady](https://twitter.com/bradygaster)

En este artículo se proporcionan instrucciones para:

* Hospedaje de concentradores de Signalr mediante un proceso de trabajo en segundo plano hospedado con ASP.NET Core.
* Envío de mensajes a clientes conectados desde un [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)de .net Core.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(Cómo descargar)](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>Signalr de conexión durante el inicio

::: moniker range=">= aspnetcore-3.0"

El hospedaje de los concentradores de Signalr ASP.NET Core en el contexto de un proceso de trabajo en segundo plano es idéntico al hospedaje de un concentrador en una aplicación Web ASP.NET Core. En el `Startup.ConfigureServices` método, la `services.AddSignalR` llamada a agrega los servicios necesarios a la capa de ASP.net Core de la inserción de dependencias (di) para admitir signalr. En `Startup.Configure`, se `MapHub` llama al método en la `UseEndpoints` devolución de llamada para conectar los puntos de conexión del concentrador en el ASP.net Core canalización de solicitudes.

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

El hospedaje de los concentradores de Signalr ASP.NET Core en el contexto de un proceso de trabajo en segundo plano es idéntico al hospedaje de un concentrador en una aplicación Web ASP.NET Core. En el `Startup.ConfigureServices` método, la `services.AddSignalR` llamada a agrega los servicios necesarios a la capa de ASP.net Core de la inserción de dependencias (di) para admitir signalr. `Startup.Configure` En`UseSignalR` , se llama al método para conectar los puntos de conexión del concentrador en el ASP.net Core canalización de solicitudes.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

En el ejemplo anterior, la `ClockHub` clase implementa la `Hub<T>` clase para crear un concentrador fuertemente tipado. Se ha configurado en la `Startup` clase para responder a las solicitudes en el extremo `/hubs/clock`. `ClockHub`

Para obtener más información sobre los concentradores fuertemente tipados, consulte [uso de hubs en signalr para ASP.net Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Esta funcionalidad no se limita a la clase de [\<> Hub t](xref:Microsoft.AspNetCore.SignalR.Hub`1) . Cualquier clase que herede de [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), como [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), también funcionará.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

La interfaz utilizada por el fuertemente tipado `ClockHub` es la `IClock` interfaz.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>Llamada a un concentrador Signalr desde un servicio en segundo plano

Durante el inicio, `Worker` la clase, `BackgroundService`a, se conecta mediante `AddHostedService`.

```csharp
services.AddHostedService<Worker>();
```

Como signalr también se conecta durante la `Startup` fase, en la que cada concentrador se conecta a un punto de conexión individual en la canalización de solicitudes HTTP de ASP.net Core, cada concentrador se representa mediante un `IHubContext<T>` en el servidor. Mediante el uso de las características de di de ASP.net Core, otras clases creadas por la `BackgroundService` capa de hospedaje, como las clases, las clases de controlador de MVC o los modelos de página de Razor, pueden `IHubContext<ClockHub, IClock>` obtener referencias a los concentradores del lado servidor aceptando instancias de durante la construcción.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

A medida `ExecuteAsync` que se llama al método de forma iterativa en el servicio en segundo plano, la fecha y hora actuales del servidor se envían a `ClockHub`los clientes conectados mediante.

## <a name="react-to-signalr-events-with-background-services"></a>Reaccionar a eventos de Signalr con servicios en segundo plano

Al igual que una aplicación de una sola página que usa el cliente de JavaScript para signalr o una aplicación de escritorio <xref:signalr/dotnet-client>de .net `BackgroundService` puede `IHostedService` usar mediante el, también se puede usar una implementación de o para conectarse a los concentradores de signalr y responder a los eventos.

La `ClockHubClient` clase implementa la `IClock` interfaz y la `IHostedService` interfaz. De este modo, se puede conectar durante `Startup` para ejecutarse continuamente y responder a los eventos del concentrador desde el servidor.

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Durante la `ClockHubClient` inicialización, crea una instancia `HubConnection` de y conecta el `IClock.ShowTime` método como controlador para el evento del `ShowTime` centro.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

En la `IHostedService.StartAsync` implementación de `HubConnection` , se inicia de forma asincrónica.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Durante el `IHostedService.StopAsync` método `HubConnection` , se elimina de forma asincrónica.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción](xref:tutorials/signalr)
* [Concentradores](xref:signalr/hubs)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
* [Concentradores fuertemente tipados](xref:signalr/hubs#strongly-typed-hubs)
