---
title: ASP.NET Core SignalR plataformas admitidas
author: bradygaster
description: Obtenga información acerca de las plataformas admitidas para ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- SignalR
uid: signalr/supported-platforms
ms.openlocfilehash: 054965921c87c1a9be27e5ddaa8a87b0fa1f4113
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146503"
---
# <a name="aspnet-core-opno-locsignalr-supported-platforms"></a>ASP.NET Core SignalR plataformas admitidas

## <a name="server-system-requirements"></a>Requisitos del sistema de servidor de

SignalR para ASP.NET Core admite cualquier plataforma de servidor que ASP.NET Core admita.

## <a name="javascript-client"></a>Cliente de JavaScript

El [cliente de JavaScript](xref:signalr/javascript-client) se ejecuta en NodeJS 8 y versiones posteriores, y en los siguientes exploradores:

| Explorador                         | Version         |
| ------------------------------- | --------------- |
| Microsoft Edge                  | &dagger; actual |
| Mozilla Firefox                 | &dagger; actual |
| Google Chrome; incluye Android | &dagger; actual |
| Safari incluye iOS            | &dagger; actual |
| Microsoft Internet Explorer     | 11              |

&dagger;*actual* hace referencia a la versión más reciente del explorador.

## <a name="net-client"></a>Cliente .NET

El [cliente .net](xref:signalr/dotnet-client) se ejecuta en cualquier plataforma compatible con ASP.net Core. Por ejemplo, [los desarrolladores de Xamarin pueden usar SignalR](https://github.com/aspnet/Announcements/issues/305) para compilar aplicaciones de Android con Xamarin. Android 8.4.0.1 y versiones posteriores y aplicaciones de iOS con Xamarin. iOS 11.14.0.4 y versiones posteriores.

Si el servidor ejecuta IIS, el transporte de WebSockets requiere IIS 8,0 o posterior en Windows Server 2012 o posterior. Se admiten otros transportes en todas las plataformas.

## <a name="java-client"></a>Cliente de Java

El [cliente de Java](xref:signalr/java-client) es compatible con Java 8 y versiones posteriores.

## <a name="unsupported-clients"></a>Clientes no compatibles

Los siguientes clientes están disponibles pero son experimentales o no oficiales. No se admiten actualmente y nunca pueden ser.

* [C++Nº](https://github.com/aspnet/SignalR-Client-Cpp)

* [Cliente SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
