---
title: Hospedaje en ASP.NET Core
author: guardrex
description: Obtenga más información sobre el host web de ASP.NET Core y el genérico de .NET, los elementos responsables del inicio de las aplicaciones y la administración de la vigencia.
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/index
ms.openlocfilehash: 365c679e789c07818c6eb007f40f6aef43b82c44
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276622"
---
# <a name="host-in-aspnet-core"></a>Hospedaje en ASP.NET Core

Las aplicaciones .NET configuran e inician un *host*. El host es responsable de la administración del inicio y la duración de la aplicación. Hay dos API host disponibles para su uso:

* [Host web](xref:fundamentals/host/web-host): indicado para el hospedaje de aplicaciones web.
* [Host genérico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 y versiones posteriores): indicado para el hospedaje de aplicaciones que no sean web, por ejemplo, las que ejecutan tareas en segundo plano. En una versión posterior, el host genérico será adecuado para hospedar aplicaciones de cualquier tipo, incluidas las web. Llegado el momento, el host genérico reemplazará el host web.

Para hospedar *aplicaciones web* ASP.NET Core, los desarrolladores deben usar el host web basado en [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). Para hospedar *aplicaciones que no sean web*, los desarrolladores deben usar el host genérico basado en [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).
