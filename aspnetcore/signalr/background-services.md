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
ms.openlocfilehash: 000732115153eeafed3948c2a07acf77ffc34218
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73964040"
---
# <a name="host-aspnet-core-opno-locsignalr-in-background-services"></a>SignalR de ASP.NET Core host en los servicios en segundo plano

Por el [Brady](https://twitter.com/bradygaster)

En este artículo se proporcionan instrucciones para:

* Hospedaje de SignalR hubs mediante un proceso de trabajo en segundo plano hospedado con ASP.NET Core.
* Envío de mensajes a clientes conectados desde un [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)de .net Core.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(cómo descargarlo)](xref:index#how-to-download-a-sample)

## <a name="wire-up-opno-locsignalr-during-startup"></a>Conectar SignalR durante el inicio

::: moniker range=">= aspnetcore-3.0"

Hospedar ASP.NET Core SignalR hubs en el contexto de un proceso de trabajo en segundo plano es idéntico a hospedar un centro en una aplicación Web de ASP.NET Core. En el método `Startup.ConfigureServices`, al llamar a `services.AddSignalR` se agregan los servicios necesarios a la capa ASP.NET Core de inserción de dependencias (DI) para admitir SignalR. En `Startup.Configure`, se llama al método de `MapHub` en la devolución de llamada de `UseEndpoints` para conectar los puntos de conexión del concentrador en la canalización de solicitudes de ASP.NET Core.

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

Hospedar ASP.NET Core SignalR hubs en el contexto de un proceso de trabajo en segundo plano es idéntico a hospedar un centro en una aplicación Web de ASP.NET Core. En el método `Startup.ConfigureServices`, al llamar a `services.AddSignalR` se agregan los servicios necesarios a la capa ASP.NET Core de inserción de dependencias (DI) para admitir SignalR. En `Startup.Configure`, se llama al método `UseSignalR` para conectar los puntos de conexión del concentrador en la canalización de solicitudes de ASP.NET Core.

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

En el ejemplo anterior, la clase `ClockHub` implementa la clase `Hub<T>` para crear un concentrador fuertemente tipado. El `ClockHub` se ha configurado en la clase `Startup` para responder a las solicitudes en el extremo `/hubs/clock`.

Para obtener más información sobre los concentradores fuertemente tipados, consulte [uso de hubs en SignalR para ASP.net Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Esta funcionalidad no se limita a la clase de [> Hub\<t](xref:Microsoft.AspNetCore.SignalR.Hub`1) . Cualquier clase que herede de [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), como [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), también funcionará.

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

La interfaz utilizada por el `ClockHub` fuertemente tipado es la interfaz de `IClock`.

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-opno-locsignalr-hub-from-a-background-service"></a>Llamada a un concentrador de SignalR desde un servicio en segundo plano

Durante el inicio, la clase `Worker`, una `BackgroundService`, se conecta mediante `AddHostedService`.

```csharp
services.AddHostedService<Worker>();
```

Como SignalR también se conecta durante la fase de `Startup`, en la que cada concentrador se adjunta a un punto de conexión individual de la canalización de solicitudes HTTP de ASP.NET Core, cada concentrador se representa mediante una `IHubContext<T>` en el servidor. Mediante el uso de las características DI de ASP.NET Core, otras clases a las que se ha creado una instancia de la capa de hospedaje, como las clases `BackgroundService`, las clases de controlador de MVC o los modelos de página de Razor, pueden obtener referencias a los concentradores del lado servidor aceptando instancias de `IHubContext<ClockHub, IClock>` durante la construcción.

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

A medida que se llama al método `ExecuteAsync` de forma iterativa en el servicio en segundo plano, la fecha y hora actuales del servidor se envían a los clientes conectados mediante el `ClockHub`.

## <a name="react-to-opno-locsignalr-events-with-background-services"></a>Reaccionar ante eventos de SignalR con servicios en segundo plano

Al igual que una aplicación de una sola página que usa el cliente de JavaScript para SignalR o una aplicación de escritorio de .NET puede usar el <xref:signalr/dotnet-client>, también se puede usar una implementación de `BackgroundService` o `IHostedService` para conectarse a los concentradores de SignalR y responder a los eventos.

La clase `ClockHubClient` implementa la interfaz `IClock` y la interfaz `IHostedService`. De este modo, se puede conectar durante `Startup` para ejecutarse continuamente y responder a los eventos del concentrador desde el servidor.

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Durante la inicialización, el `ClockHubClient` crea una instancia de un `HubConnection` y conecta el método `IClock.ShowTime` como el controlador del evento `ShowTime` del centro.

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

En la implementación de `IHostedService.StartAsync`, el `HubConnection` se inicia de forma asincrónica.

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

Durante el método `IHostedService.StopAsync`, el `HubConnection` se elimina de forma asincrónica.

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción](xref:tutorials/signalr)
* [Concentradores](xref:signalr/hubs)
* [Publicar en Azure](xref:signalr/publish-to-azure-web-app)
* [Concentradores fuertemente tipados](xref:signalr/hubs#strongly-typed-hubs)
