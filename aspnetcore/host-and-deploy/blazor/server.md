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
# <a name="host-and-deploy-blazor-server"></a><span data-ttu-id="c7e0d-103">Hospedaje e implementación de Blazor Server</span><span class="sxs-lookup"><span data-stu-id="c7e0d-103">Host and deploy Blazor Server</span></span>

<span data-ttu-id="c7e0d-104">Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="c7e0d-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="c7e0d-105">Valores de configuración de host</span><span class="sxs-lookup"><span data-stu-id="c7e0d-105">Host configuration values</span></span>

<span data-ttu-id="c7e0d-106">Las [aplicaciones Blazor Server](xref:blazor/hosting-models#blazor-server) pueden aceptar [valores de configuración de host genéricos](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="c7e0d-106">[Blazor Server apps](xref:blazor/hosting-models#blazor-server) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="c7e0d-107">Implementación</span><span class="sxs-lookup"><span data-stu-id="c7e0d-107">Deployment</span></span>

<span data-ttu-id="c7e0d-108">Con el [modelo de hospedaje de Blazor Server](xref:blazor/hosting-models#blazor-server), Blazor se ejecuta en el servidor desde una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-108">Using the [Blazor Server hosting model](xref:blazor/hosting-models#blazor-server), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="c7e0d-109">Las actualizaciones de la interfaz de usuario, el control de eventos y las llamadas de JavaScript se controlan mediante una conexión de [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="c7e0d-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="c7e0d-110">Se requiere un servidor web que pueda hospedar una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="c7e0d-111">Visual Studio incluye la plantilla de proyecto **Blazor Server App** (plantilla `blazorserverside` cuando se usa el comando [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="c7e0d-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="scalability"></a><span data-ttu-id="c7e0d-112">Escalabilidad</span><span class="sxs-lookup"><span data-stu-id="c7e0d-112">Scalability</span></span>

<span data-ttu-id="c7e0d-113">Planee una implementación para hacer el mejor uso de la infraestructura disponible para una aplicación de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-113">Plan a deployment to make the best use of the available infrastructure for a Blazor Server app.</span></span> <span data-ttu-id="c7e0d-114">Consulte los siguientes recursos para abordar la escalabilidad de las aplicaciones de Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="c7e0d-114">See the following resources to address Blazor Server app scalability:</span></span>

* [<span data-ttu-id="c7e0d-115">Aspectos básicos de las aplicaciones de Blazor Server</span><span class="sxs-lookup"><span data-stu-id="c7e0d-115">Fundamentals of Blazor Server apps</span></span>](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server>

### <a name="deployment-server"></a><span data-ttu-id="c7e0d-116">Servidor de implementación</span><span class="sxs-lookup"><span data-stu-id="c7e0d-116">Deployment server</span></span>

<span data-ttu-id="c7e0d-117">A la hora de considerar la escalabilidad de un solo servidor (escalado vertical), la memoria disponible para una aplicación es probablemente el primer recurso que la aplicación agotará a medida que la demanda de los usuarios aumente.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-117">When considering the scalability of a single server (scale up), the memory available to an app is likely the first resource that the app will exhaust as user demands increase.</span></span> <span data-ttu-id="c7e0d-118">La memoria disponible en el servidor afecta a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c7e0d-118">The available memory on the server affects the:</span></span>

* <span data-ttu-id="c7e0d-119">Número de circuitos activos que un servidor puede admitir.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-119">Number of active circuits that a server can support.</span></span>
* <span data-ttu-id="c7e0d-120">Latencia de la interfaz de usuario en el cliente.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-120">UI latency on the client.</span></span>

<span data-ttu-id="c7e0d-121">Para obtener instrucciones sobre la creación de aplicaciones de Blazor Server seguras y escalables, consulte <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-121">For guidance on building secure and scalable Blazor server apps, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="c7e0d-122">Cada circuito utiliza aproximadamente 250 KB de memoria para una aplicación mínima de estilo *Hola mundo*.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-122">Each circuit uses approximately 250 KB of memory for a minimal *Hello World*-style app.</span></span> <span data-ttu-id="c7e0d-123">El tamaño de un circuito depende del código de la aplicación y de los requisitos de mantenimiento del estado asociados a cada componente.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-123">The size of a circuit depends on the app's code and the state maintenance requirements associated with each component.</span></span> <span data-ttu-id="c7e0d-124">Se recomienda que mida la demanda de recursos durante el desarrollo de la aplicación y la infraestructura, pero la línea de base siguiente puede ser un punto de partida para planear el destino de implementación: Si espera que la aplicación admita 5000 usuarios simultáneos, considere la posibilidad de presupuestar al menos 1,3 GB de memoria del servidor en la aplicación (o ~273 KB por usuario).</span><span class="sxs-lookup"><span data-stu-id="c7e0d-124">We recommend that you measure resource demands during development for your app and infrastructure, but the following baseline can be a starting point in planning your deployment target: If you expect your app to support 5,000 concurrent users, consider budgeting at least 1.3 GB of server memory to the app (or ~273 KB per user).</span></span>

### <a name="signalr-configuration"></a><span data-ttu-id="c7e0d-125">Configuración de SignalR</span><span class="sxs-lookup"><span data-stu-id="c7e0d-125">SignalR configuration</span></span>

<span data-ttu-id="c7e0d-126">Las aplicaciones de Blazor Server usan ASP.NET Core SignalR para comunicarse con el explorador.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-126">Blazor Server apps use ASP.NET Core SignalR to communicate with the browser.</span></span> <span data-ttu-id="c7e0d-127">[Las condiciones de hospedaje y escalado de SignalR](xref:signalr/publish-to-azure-web-app) se aplican a las aplicaciones de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-127">[SignalR's hosting and scaling conditions](xref:signalr/publish-to-azure-web-app) apply to Blazor Server apps.</span></span>

<span data-ttu-id="c7e0d-128">Blazor funciona mejor cuando se usa WebSockets como transporte de SignalR debido a su menor latencia, a su confiabilidad y a la [seguridad](xref:signalr/security).</span><span class="sxs-lookup"><span data-stu-id="c7e0d-128">Blazor works best when using WebSockets as the SignalR transport due to lower latency, reliability, and [security](xref:signalr/security).</span></span> <span data-ttu-id="c7e0d-129">SignalR usa el sondeo largo cuando WebSockets no está disponible o cuando la aplicación está configurada explícitamente para usar el sondeo largo.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-129">Long Polling is used by SignalR when WebSockets isn't available or when the app is explicitly configured to use Long Polling.</span></span> <span data-ttu-id="c7e0d-130">Al implementar en Azure App Service, configure la aplicación para usar WebSockets en la configuración de Azure Portal del servicio.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-130">When deploying to Azure App Service, configure the app to use WebSockets in the Azure portal settings for the service.</span></span> <span data-ttu-id="c7e0d-131">Para obtener más información sobre la configuración de la aplicación para Azure App Service, consulte las [directrices de publicación de SignalR](xref:signalr/publish-to-azure-web-app).</span><span class="sxs-lookup"><span data-stu-id="c7e0d-131">For details on configuring the app for Azure App Service, see the [SignalR publishing guidelines](xref:signalr/publish-to-azure-web-app).</span></span>

<span data-ttu-id="c7e0d-132">Se recomienda usar [Azure SignalR Service](/azure/azure-signalr) para aplicaciones de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-132">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="c7e0d-133">El servicio permite el escalado vertical de una aplicación de Blazor Server a un gran número de conexiones simultáneas de SignalR.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-133">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="c7e0d-134">Además, los centros de datos de alto rendimiento y alcance global del servicio SignalR ayudan significativamente a reducir la latencia ocasionada por la geografía.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-134">In addition, the SignalR service's global reach and high-performance data centers significantly aid in reducing latency due to geography.</span></span>

### <a name="measure-network-latency"></a><span data-ttu-id="c7e0d-135">Medición de la latencia de red</span><span class="sxs-lookup"><span data-stu-id="c7e0d-135">Measure network latency</span></span>

<span data-ttu-id="c7e0d-136">[La interoperabilidad de JS](xref:blazor/javascript-interop) se puede usar para medir la latencia de red, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c7e0d-136">[JS interop](xref:blazor/javascript-interop) can be used to measure network latency, as the following example demonstrates:</span></span>

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

<span data-ttu-id="c7e0d-137">Para disfrutar de una experiencia de interfaz de usuario razonable, se recomienda una latencia de interfaz de usuario sostenida de 250 ms o menos.</span><span class="sxs-lookup"><span data-stu-id="c7e0d-137">For a reasonable UI experience, we recommend a sustained UI latency of 250ms or less.</span></span>
