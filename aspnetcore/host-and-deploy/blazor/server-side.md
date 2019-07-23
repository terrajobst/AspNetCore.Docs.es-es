---
title: Hospedaje e implementación de ASP.NET Core Blazor del lado servidor
author: guardrex
description: Descubra cómo hospedar e implementar una aplicación Blazor del lado servidor con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/11/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 56a03ff583bf85497e2b3bacc70123845a046e3d
ms.sourcegitcommit: 040aedca220ed24ee1726e6886daf6906f95a028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "67892697"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="bc98a-103">Hospedaje e implementación de Blazor del lado servidor</span><span class="sxs-lookup"><span data-stu-id="bc98a-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="bc98a-104">Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="bc98a-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="bc98a-105">Valores de configuración de host</span><span class="sxs-lookup"><span data-stu-id="bc98a-105">Host configuration values</span></span>

<span data-ttu-id="bc98a-106">Las aplicaciones del lado servidor que usan el [modelo de hospedaje del lado servidor](xref:blazor/hosting-models#server-side) pueden aceptar [valores de configuración del host genérico](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="bc98a-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="bc98a-107">Implementación</span><span class="sxs-lookup"><span data-stu-id="bc98a-107">Deployment</span></span>

<span data-ttu-id="bc98a-108">Con el [modelo de hospedaje del lado servidor](xref:blazor/hosting-models#server-side), Blazor se ejecuta en el servidor desde una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc98a-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="bc98a-109">Las actualizaciones de la interfaz de usuario, el control de eventos y las llamadas de JavaScript se controlan mediante una conexión de [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="bc98a-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="bc98a-110">Se requiere un servidor web que pueda hospedar una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc98a-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="bc98a-111">Visual Studio incluye la plantilla de proyecto **Blazor Server App** (plantilla `blazorserverside` cuando se usa el comando [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="bc98a-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="connection-scale-out"></a><span data-ttu-id="bc98a-112">Escalabilidad horizontal de la conexión</span><span class="sxs-lookup"><span data-stu-id="bc98a-112">Connection scale out</span></span>

<span data-ttu-id="bc98a-113">Las aplicaciones de servidor de Blazor requieren una conexión de SignalR activa para cada usuario.</span><span class="sxs-lookup"><span data-stu-id="bc98a-113">Blazor server-side apps require one active SignalR connection for each user.</span></span> <span data-ttu-id="bc98a-114">Una implementación de servidor de Blazor de producción requiere una solución para admitir tantas conexiones simultáneas como requiera la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc98a-114">A production Blazor server-side deployment requires a solution for supporting as many concurrent connections as required by the app.</span></span> <span data-ttu-id="bc98a-115">[Azure SignalR Service](/azure/azure-signalr/) controla el escalado de conexiones y se recomienda como solución de escalado para las aplicaciones de servidor de Blazor.</span><span class="sxs-lookup"><span data-stu-id="bc98a-115">The [Azure SignalR Service](/azure/azure-signalr/) handles the scaling of connections and is recommended as a scaling solution for Blazor server-side apps.</span></span> <span data-ttu-id="bc98a-116">Para más información, consulte <xref:signalr/publish-to-azure-web-app>.</span><span class="sxs-lookup"><span data-stu-id="bc98a-116">For more information, see <xref:signalr/publish-to-azure-web-app>.</span></span>

## <a name="signalr-configuration"></a><span data-ttu-id="bc98a-117">Configuración de SignalR</span><span class="sxs-lookup"><span data-stu-id="bc98a-117">SignalR configuration</span></span>

<span data-ttu-id="bc98a-118">ASP.NET Core configura SignalR para los escenarios de servidor de Blazor más habituales.</span><span class="sxs-lookup"><span data-stu-id="bc98a-118">SignalR is configured by ASP.NET Core for the most common Blazor server-side scenarios.</span></span> <span data-ttu-id="bc98a-119">Para escenarios personalizados y avanzados, consulte los artículos de SignalR de la sección [Recursos adicionales](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="bc98a-119">For custom and advanced scenarios, consult the SignalR articles in the [Additional resources](#additional-resources) section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc98a-120">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="bc98a-120">Additional resources</span></span>

* <xref:signalr/introduction>
* [<span data-ttu-id="bc98a-121">Documentación de Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="bc98a-121">Azure SignalR Service Documentation</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="bc98a-122">Inicio rápido: Creación de un salón de chat con SignalR Service</span><span class="sxs-lookup"><span data-stu-id="bc98a-122">Quickstart: Create a chat room by using SignalR Service</span></span>](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="bc98a-123">Implementar una versión preliminar de ASP.NET Core en Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bc98a-123">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
