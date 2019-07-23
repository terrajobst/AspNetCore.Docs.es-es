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
# <a name="host-and-deploy-blazor-server-side"></a>Hospedaje e implementación de Blazor del lado servidor

Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Valores de configuración de host

Las aplicaciones del lado servidor que usan el [modelo de hospedaje del lado servidor](xref:blazor/hosting-models#server-side) pueden aceptar [valores de configuración del host genérico](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Implementación

Con el [modelo de hospedaje del lado servidor](xref:blazor/hosting-models#server-side), Blazor se ejecuta en el servidor desde una aplicación ASP.NET Core. Las actualizaciones de la interfaz de usuario, el control de eventos y las llamadas de JavaScript se controlan mediante una conexión de [SignalR](xref:signalr/introduction).

Se requiere un servidor web que pueda hospedar una aplicación ASP.NET Core. Visual Studio incluye la plantilla de proyecto **Blazor Server App** (plantilla `blazorserverside` cuando se usa el comando [dotnet new](/dotnet/core/tools/dotnet-new)).

## <a name="connection-scale-out"></a>Escalabilidad horizontal de la conexión

Las aplicaciones de servidor de Blazor requieren una conexión de SignalR activa para cada usuario. Una implementación de servidor de Blazor de producción requiere una solución para admitir tantas conexiones simultáneas como requiera la aplicación. [Azure SignalR Service](/azure/azure-signalr/) controla el escalado de conexiones y se recomienda como solución de escalado para las aplicaciones de servidor de Blazor. Para más información, consulte <xref:signalr/publish-to-azure-web-app>.

## <a name="signalr-configuration"></a>Configuración de SignalR

ASP.NET Core configura SignalR para los escenarios de servidor de Blazor más habituales. Para escenarios personalizados y avanzados, consulte los artículos de SignalR de la sección [Recursos adicionales](#additional-resources).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:signalr/introduction>
* [Documentación de Azure SignalR Service](/azure/azure-signalr/)
* [Inicio rápido: Creación de un salón de chat con SignalR Service](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Implementar una versión preliminar de ASP.NET Core en Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
