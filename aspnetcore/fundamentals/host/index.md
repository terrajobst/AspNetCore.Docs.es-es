---
title: Hospedaje en ASP.NET Core
author: guardrex
description: Obtenga más información sobre el host web de ASP.NET Core y el genérico de .NET, los elementos responsables del inicio de las aplicaciones y la administración de la vigencia.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a>Hospedaje en ASP.NET Core

Las aplicaciones .NET configuran e inician un *host*. El host es responsable de la administración del inicio y la duración de la aplicación. Hay dos API host disponibles para su uso:

* [Host web](xref:fundamentals/host/web-host): indicado para el hospedaje de aplicaciones web.
* [Host genérico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 y versiones posteriores): indicado para el hospedaje de aplicaciones que no sean web, por ejemplo, las que ejecutan tareas en segundo plano. En una versión posterior, el host genérico será adecuado para hospedar aplicaciones de cualquier tipo, incluidas las web. Llegado el momento, el host genérico reemplazará el host web.

En este momento, los desarrolladores deben usar el [host web](xref:fundamentals/host/web-host) basado en [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) para hospedar aplicaciones ASP.NET Core.
