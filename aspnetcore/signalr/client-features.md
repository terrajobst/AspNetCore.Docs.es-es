---
title: Características de cliente de SignalR
author: bradygaster
description: Obtenga información sobre qué características son compatibles con los distintos clientes de Signalr ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 2d6759a5484c37aee6db3d22b3127414231605ae
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301186"
---
# <a name="aspnet-core-signalr-client-features"></a>Características de cliente de Signalr ASP.NET Core

## <a name="feature-distribution"></a>Distribución de características

En la tabla siguiente se muestran las características y la compatibilidad de los clientes que ofrecen compatibilidad en tiempo real.

| Característica | .NET | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| Compatibilidad con Azure Signalr Service |✔|✔|✔|
| [Streaming de servidor a cliente](xref:signalr/streaming)          |✔|✔|✔|
| [Streaming de cliente a servidor](xref:signalr/streaming)          |✔|✔|✔|
| Reconexión automática ([.net](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |✔|✔| |
| Transporte de WebSockets |✔|✔|✔|
| Transporte de eventos enviados por servidor |✔|✔| |
| Transporte de sondeo prolongado |✔|✔|✔|
| Protocolo de concentrador de JSON |✔|✔|✔|
| Protocolo de concentrador MessagePack |✔|✔| |

Se realiza un seguimiento de la compatibilidad con la reconexión automática en el cliente de Java en nuestro seguimiento de [problemas](https://github.com/aspnet/AspNetCore/issues/8711).

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a Signalr para ASP.NET Core](xref:tutorials/signalr)
* [Plataformas compatibles](xref:signalr/supported-platforms)
* [Concentradores](xref:signalr/hubs)
* [Cliente de JavaScript](xref:signalr/javascript-client)
