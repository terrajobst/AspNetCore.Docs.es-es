---
title: Hospedaje e implementación de Blazor del lado servidor
author: guardrex
description: Descubra cómo hospedar e implementar una aplicación Blazor del lado servidor con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 940020ee44d72d50395aad64bc924413c1bbecfb
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614755"
---
# <a name="host-and-deploy-blazor-server-side"></a>Hospedaje e implementación de Blazor del lado servidor

Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Valores de configuración de host

Las aplicaciones del lado servidor que usan el [modelo de hospedaje del lado servidor](xref:blazor/hosting-models#server-side-hosting-model) pueden aceptar [valores de configuración del host genérico](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Implementación

Con el [modelo de hospedaje del lado servidor](xref:blazor/hosting-models#server-side-hosting-model), Blazor se ejecuta en el servidor desde una aplicación ASP.NET Core. Las actualizaciones de la interfaz de usuario, el control de eventos y las llamadas de JavaScript se controlan mediante una conexión de [SignalR](xref:signalr/introduction).

La aplicación se incluye con la aplicación ASP.NET Core en la salida publicada, y las dos aplicaciones se implementan juntas. Se requiere un servidor web que pueda hospedar una aplicación ASP.NET Core. En el caso de una implementación del lado servidor, Visual Studio incluye la plantilla de proyecto **Componentes de Razor** (la plantilla `razorcomponents` al usar el comando [dotnet new](/dotnet/core/tools/dotnet-new)).

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

Para obtener más información sobre la implementación y el hospedaje de aplicaciones de ASP.NET Core, consulte <xref:host-and-deploy/index>.

Para obtener información sobre cómo implementar en Azure App Service, vea <xref:tutorials/publish-to-azure-webapp-using-vs>.
