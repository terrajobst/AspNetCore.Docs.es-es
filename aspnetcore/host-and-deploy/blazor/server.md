---
title: Hospedaje e implementación de servidores Blazor con ASP.NET Core
author: guardrex
description: Descubra cómo hospedar e implementar una aplicación Blazor Server con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/07/2019
uid: host-and-deploy/blazor/server
ms.openlocfilehash: a393d620924d847e674a09972515a8130a15fc6a
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70964231"
---
# <a name="host-and-deploy-blazor-server"></a>Hospedaje e implementación de Blazor Server

Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Valores de configuración de host

Las [aplicaciones Blazor Server](xref:blazor/hosting-models#blazor-server) pueden aceptar [valores de configuración de host genéricos](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Implementación

Con el [modelo de hospedaje de Blazor Server](xref:blazor/hosting-models#blazor-server), Blazor se ejecuta en el servidor desde una aplicación ASP.NET Core. Las actualizaciones de la interfaz de usuario, el control de eventos y las llamadas de JavaScript se controlan mediante una conexión de [SignalR](xref:signalr/introduction).

Se requiere un servidor web que pueda hospedar una aplicación ASP.NET Core. Visual Studio incluye la plantilla de proyecto **Blazor Server App** (plantilla `blazorserverside` cuando se usa el comando [dotnet new](/dotnet/core/tools/dotnet-new)).

## <a name="scalability"></a>Escalabilidad

Planee una implementación para hacer el mejor uso de la infraestructura disponible para una aplicación de Blazor Server. Consulte los siguientes recursos para abordar la escalabilidad de las aplicaciones de Blazor Server:

* [Aspectos básicos de las aplicaciones de Blazor Server](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server>

### <a name="deployment-server"></a>Servidor de implementación

A la hora de considerar la escalabilidad de un solo servidor (escalado vertical), la memoria disponible para una aplicación es probablemente el primer recurso que la aplicación agotará a medida que la demanda de los usuarios aumente. La memoria disponible en el servidor afecta a lo siguiente:

* Número de circuitos activos que un servidor puede admitir.
* Latencia de la interfaz de usuario en el cliente.

Para obtener instrucciones sobre la creación de aplicaciones de Blazor Server seguras y escalables, consulte <xref:security/blazor/server>.

Cada circuito utiliza aproximadamente 250 KB de memoria para una aplicación mínima de estilo *Hola mundo*. El tamaño de un circuito depende del código de la aplicación y de los requisitos de mantenimiento del estado asociados a cada componente. Se recomienda que mida la demanda de recursos durante el desarrollo de la aplicación y la infraestructura, pero la línea de base siguiente puede ser un punto de partida para planear el destino de implementación: Si espera que la aplicación admita 5000 usuarios simultáneos, considere la posibilidad de presupuestar al menos 1,3 GB de memoria del servidor en la aplicación (o ~273 KB por usuario).

### <a name="signalr-configuration"></a>Configuración de SignalR

Las aplicaciones de Blazor Server usan ASP.NET Core SignalR para comunicarse con el explorador. [Las condiciones de hospedaje y escalado de SignalR](xref:signalr/publish-to-azure-web-app) se aplican a las aplicaciones de Blazor Server.

Blazor funciona mejor cuando se usa WebSockets como transporte de SignalR debido a su menor latencia, a su confiabilidad y a la [seguridad](xref:signalr/security). SignalR usa el sondeo largo cuando WebSockets no está disponible o cuando la aplicación está configurada explícitamente para usar el sondeo largo. Al implementar en Azure App Service, configure la aplicación para usar WebSockets en la configuración de Azure Portal del servicio. Para obtener más información sobre la configuración de la aplicación para Azure App Service, consulte las [directrices de publicación de SignalR](xref:signalr/publish-to-azure-web-app).

Se recomienda usar [Azure SignalR Service](/azure/azure-signalr) para aplicaciones de Blazor Server. El servicio permite el escalado vertical de una aplicación de Blazor Server a un gran número de conexiones simultáneas de SignalR. Además, los centros de datos de alto rendimiento y alcance global del servicio SignalR ayudan significativamente a reducir la latencia ocasionada por la geografía.

### <a name="measure-network-latency"></a>Medición de la latencia de red

[La interoperabilidad de JS](xref:blazor/javascript-interop) se puede usar para medir la latencia de red, como se muestra en el ejemplo siguiente:

```cshtml
@inject IJSRuntime JS

@if (latency is null)
{
    <span>Calculating...</span>
}
else
{
    <span>@(latency.Value.TotalMilliseconds)ms</span>
}

@code
{
    private DateTime startTime;
    private TimeSpan? latency;

    protected override async Task OnInitializedAsync()
    {
        startTime = DateTime.UtcNow;
        var _ = await JS.InvokeAsync<string>("toString");
        latency = DateTime.UtcNow - startTime;
    }
}
```

Para disfrutar de una experiencia de interfaz de usuario razonable, se recomienda una latencia de interfaz de usuario sostenida de 250 ms o menos.
