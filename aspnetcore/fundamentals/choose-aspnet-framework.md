---
title: Elección entre ASP.NET 4.x y ASP.NET Core
author: rick-anderson
description: Se explican las diferencias entre ASP.NET Core. y ASP.NET 4.x, y cómo elegir entre ellos.
ms.author: riande
ms.custom: seodec18
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: b75fbea330e48075c4a2789454e973d4c56ffa53
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121185"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a>Elección entre ASP.NET 4.x y ASP.NET Core

ASP.NET Core es un rediseño de ASP.NET 4.x. En este artículo se enumeran las diferencias entre ellos.

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core es un marco multiplataforma de código abierto que tiene como finalidad compilar modernas aplicaciones web basadas en la nube en Windows, macOS o Linux.

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a>ASP.NET 4.x

ASP.NET 4.x es un marco consolidado que proporciona los servicios necesarios para compilar aplicaciones web de nivel empresarial basadas en servidor en Windows.

## <a name="framework-selection"></a>Selección del marco

En la tabla siguiente se compara ASP.NET Core en ASP.NET 4.x.

| ASP.NET Core | ASP.NET 4.x |
|---|---|
|Compilación para Windows, macOS o Linux|Compilación para Windows|
|[Las páginas de Razor](xref:razor-pages/index) son el método recomendado para crear una interfaz de usuario web desde la aparición de ASP.NET Core 2.x. Vea también [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api) y [SignalR](xref:signalr/introduction).|Use [formularios Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) o [Web Pages](/aspnet/web-pages)|
|Varias versiones por equipo|Una versión por equipo|
|Desarrollo con Visual Studio, [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/) o [Visual Studio Code](https://code.visualstudio.com/) con C# o F#|Desarrollo con Visual Studio con C#, VB o F#|
|Mayor rendimiento que ASP.NET 4.x|Buen rendimiento|
|[Elegir .NET Framework o .NET Core](/dotnet/standard/choosing-core-framework-server)|Usar el tiempo de ejecución de .NET Framework|

Vea [ASP.NET Core con .NET Framework como destino](xref:index#target-framework) para obtener información sobre la compatibilidad de ASP.NET Core 2.x en .NET Framework.

## <a name="aspnet-core-scenarios"></a>Escenarios de ASP.NET Core

* [Las páginas de Razor](xref:razor-pages/index) son el método recomendado para crear una interfaz de usuario web desde la aparición de ASP.NET Core 2.x.
* [Sitios web](xref:tutorials/first-mvc-app/index)
* [API](xref:tutorials/first-web-api)
* [En tiempo real](xref:signalr/index)
* [Implementación de una aplicación ASP.NET Core en Azure](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a>Escenarios de ASP.NET 4.x

* [Sitios web](/aspnet/mvc)
* [API](/aspnet/web-api)
* [En tiempo real](/aspnet/signalr)
* [Creación de una aplicación web ASP.NET 4.x en Azure](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a ASP.NET](/aspnet/overview)
* [Introducción a ASP.NET Core](xref:index)
* <xref:host-and-deploy/azure-apps/index>

> [!NOTE]
> Vamos a probar la facilidad de uso de una nueva estructura propuesta para la tabla de contenido de ASP.NET Core.  Si tiene unos minutos para realizar un ejercicio de búsqueda de 7 temas diferentes en la tabla actual o propuesta de contenido, [haga clic aquí para participar en el estudio](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).
