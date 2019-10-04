---
title: Características de cliente de SignalR
author: bradygaster
description: Obtenga información sobre qué características son compatibles con los distintos clientes de Signalr ASP.NET Core.
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 6718722cdbcfae500026fcd429ca6b9de8f0f103
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925352"
---
# <a name="aspnet-core-signalr-client-features"></a>Características de cliente de Signalr ASP.NET Core

## <a name="feature-distribution"></a>Distribución de características

En la tabla siguiente se muestran las características y la compatibilidad de los clientes que ofrecen compatibilidad en tiempo real. Para cada característica, se muestra la versión *mínima* que admite esta característica. Si no aparece ninguna versión, no se admite la característica.

| Característica | .NET | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| Compatibilidad con Azure Signalr Service |1.0.0|1.0.0|1.0.0|
| [Streaming de servidor a cliente](xref:signalr/streaming)          |1.0.0|1.0.0|1.0.0|
| [Streaming de cliente a servidor](xref:signalr/streaming)          |3.0.0|3.0.0|3.0.0|
| Reconexión automática ([.net](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |3.0.0|3.0.0|❌|
| Transporte de WebSockets |1.0.0|1.0.0|1.0.0|
| Transporte de eventos enviados por servidor |1.0.0|1.0.0|❌|
| Transporte de sondeo prolongado |1.0.0|1.0.0|3.0.0|
| Protocolo de concentrador de JSON |1.0.0|1.0.0|1.0.0|
| Protocolo de concentrador MessagePack |1.0.0|1.0.0|❌|

Se realiza un seguimiento de la compatibilidad con la reconexión automática en el cliente de Java en nuestro seguimiento de [problemas](https://github.com/aspnet/AspNetCore/issues/8711).

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a Signalr para ASP.NET Core](xref:tutorials/signalr)
* [Plataformas compatibles](xref:signalr/supported-platforms)
* [Concentradores](xref:signalr/hubs)
* [Cliente de JavaScript](xref:signalr/javascript-client)
