---
title: Características de cliente de signalr
author: bradygaster
description: Obtenga información sobre qué características son compatibles con los distintos clientes de Signalr ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 55086673e0c9f9b73f07730ea25c3fa322f7fd98
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187393"
---
# <a name="aspnet-core-signalr-client-features"></a>Características de cliente de Signalr ASP.NET Core

## <a name="feature-distribution"></a>Distribución de características

En la tabla siguiente se muestran las características y la compatibilidad de los clientes que ofrecen compatibilidad en tiempo real.

| Característica | Núcleo de .NET | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| [Streaming de servidor a cliente](xref:signalr/streaming)          |✔|✔|✔|
| [Streaming de cliente a servidor](xref:signalr/streaming)          |✔|✔|✔|
| Reconexión automática ([.net](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |✔|✔| |

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a Signalr para ASP.NET Core](xref:tutorials/signalr)
* [Plataformas compatibles](xref:signalr/supported-platforms)
* [Concentradores](xref:signalr/hubs)
* [Cliente de JavaScript](xref:signalr/javascript-client)
