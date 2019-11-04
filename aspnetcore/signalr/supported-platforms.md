---
title: Plataformas compatibles con Signalr ASP.NET Core
author: bradygaster
description: Obtenga información sobre las plataformas admitidas para ASP.NET Core Signalr.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/01/2019
uid: signalr/supported-platforms
ms.openlocfilehash: 1be7a307710e6e522c0088fd1ca01da11a13eda1
ms.sourcegitcommit: 77c8be22d5e88dd710f42c739748869f198865dd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426973"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>Plataformas compatibles con Signalr ASP.NET Core

## <a name="server-system-requirements"></a>Requisitos del sistema de servidor

Signalr para ASP.NET Core admite cualquier plataforma de servidor que ASP.NET Core admita.

## <a name="javascript-client"></a>Cliente de JavaScript

El [cliente de JavaScript](https://www.npmjs.com/package/@aspnet/signalr) se ejecuta en NodeJS 8 y versiones posteriores, y en los siguientes exploradores:

| Explorador                         | Versión         |
| ------------------------------- | --------------- |
| Microsoft Edge                  | &dagger; actual |
| Mozilla Firefox                 | &dagger; actual |
| Google Chrome; incluye Android | &dagger; actual |
| Safari incluye iOS            | &dagger; actual |
| Microsoft Internet Explorer     | 11              |

&dagger;*actual* hace referencia a la versión más reciente del explorador.

## <a name="net-client"></a>Cliente .NET

El [cliente .net](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) se ejecuta en cualquier plataforma compatible con ASP.net Core. Por ejemplo, [los desarrolladores de Xamarin pueden usar signalr](https://github.com/aspnet/Announcements/issues/305) para compilar aplicaciones de Android con Xamarin. Android 8.4.0.1 y las aplicaciones iOS y posteriores con Xamarin. iOS 11.14.0.4 y versiones posteriores.

Si el servidor ejecuta IIS, el transporte de WebSockets requiere IIS 8,0 o posterior en Windows Server 2012 o posterior. Se admiten otros transportes en todas las plataformas.

## <a name="java-client"></a>Cliente de Java

El [cliente de Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) es compatible con Java 8 y versiones posteriores.

## <a name="unsupported-clients"></a>Clientes no compatibles

Los siguientes clientes están disponibles pero son experimentales o no oficiales. No se admiten actualmente y nunca pueden ser.

* [C++Nº](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Cliente SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
