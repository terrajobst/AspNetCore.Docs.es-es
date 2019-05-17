---
title: Hospedaje e implementación de Blazor del lado servidor
author: guardrex
description: Descubra cómo hospedar e implementar una aplicación Blazor del lado servidor con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/26/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8e44be09a4cceba2509f3e86abf3ce5fd2d077bd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887770"
---
# <a name="host-and-deploy-blazor-server-side"></a>Hospedaje e implementación de Blazor del lado servidor

Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Valores de configuración de host

Las aplicaciones del lado servidor que usan el [modelo de hospedaje del lado servidor](xref:blazor/hosting-models#server-side) pueden aceptar [valores de configuración del host genérico](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Implementación

Con el [modelo de hospedaje del lado servidor](xref:blazor/hosting-models#server-side), Blazor se ejecuta en el servidor desde una aplicación ASP.NET Core. Las actualizaciones de la interfaz de usuario, el control de eventos y las llamadas de JavaScript se controlan mediante una conexión de [SignalR](xref:signalr/introduction).

Se requiere un servidor web que pueda hospedar una aplicación ASP.NET Core. Visual Studio incluye la plantilla de proyecto **Blazor (servidor)** (`blazorserverside` cuando se usa el comando [dotnet new](/dotnet/core/tools/dotnet-new)).

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

## <a name="additional-resources"></a>Recursos adicionales

* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Implementar una versión preliminar de ASP.NET Core en Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
