---
title: Plataformas compatibles con ASP.NET Core SignalR
author: tdykstra
description: Obtenga información acerca de las plataformas admitidas para ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/31/2018
uid: signalr/supported-platforms
ms.openlocfilehash: 773f6c020dbb2982911e177b55855473c750d52a
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758185"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>Plataformas compatibles con ASP.NET Core SignalR

## <a name="server-system-requirements"></a>Requisitos del sistema de servidor

SignalR para ASP.NET Core es compatible con cualquier plataforma de servidor ASP.NET Core admite.

## <a name="javascript-client"></a>Cliente de JavaScript

El [cliente JavaScript](https://www.npmjs.com/package/@aspnet/signalr) se ejecuta en NodeJS 8 y versiones posteriores y los siguientes exploradores:

| Explorador                         | Versión |
| ------------------------------- | ------- |
| Microsoft Edge                  | actuales |
| Mozilla Firefox                 | actuales |
| Google Chrome; incluye Android | actuales |
| Safari; incluye iOS            | actuales |
| Microsoft Internet Explorer     | 11      |
 
## <a name="net-client"></a>Cliente .NET

El [cliente.NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) se ejecuta en cualquier plataforma de servidor compatible con ASP.NET Core.

Si el servidor ejecuta IIS, el transporte de WebSockets requiere IIS 8.0 o posterior en Windows Server 2012 o posterior. Otros transportes se admiten en todas las plataformas.

## <a name="java-client"></a>Cliente de Java

El [cliente Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) admite Java 8 y versiones posteriores.

## <a name="unsupported-clients"></a>Clientes no compatibles

Los clientes siguientes están disponibles, pero son experimentales o no oficiales. Que no se admiten actualmente y nunca pueden ser.

* [Cliente de C++](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Cliente de SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
