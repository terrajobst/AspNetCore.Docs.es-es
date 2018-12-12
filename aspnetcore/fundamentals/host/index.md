---
title: Hosts web y hosts genéricos de ASP.NET Core
author: guardrex
description: Obtenga más información sobre el host web de ASP.NET Core y el genérico de .NET, los elementos responsables del inicio de las aplicaciones y la administración de la vigencia.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 43a68f67368ce37a1fb4032579c6c4e4c05d2719
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284583"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a>Hosts web y hosts genéricos de ASP.NET Core

Las aplicaciones .NET configuran e inician un *host*. El host es responsable de la administración del inicio y la duración de la aplicación. Hay dos API host disponibles para su uso:

* [Host web](xref:fundamentals/host/web-host): indicado para el hospedaje de aplicaciones web.
* [Host genérico](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 y versiones posteriores): indicado para el hospedaje de aplicaciones que no sean web, por ejemplo, las que ejecutan tareas en segundo plano. En una versión posterior, el host genérico será adecuado para hospedar aplicaciones de cualquier tipo, incluidas las web. Llegado el momento, el host genérico reemplazará el host web.

Para hospedar *aplicaciones web* ASP.NET Core, los desarrolladores deben usar el host web basado en <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>. Para hospedar *aplicaciones que no sean web*, los desarrolladores deben usar el host genérico basado en <xref:Microsoft.Extensions.Hosting.HostBuilder>.

<xref:fundamentals/host/hosted-services>  
Obtenga información sobre cómo implementar tareas en segundo plano con servicios hospedados en ASP.NET Core.

<xref:fundamentals/configuration/platform-specific-configuration>  
Descubra cómo mejorar una aplicación ASP.NET Core desde un ensamblado sin referencia o al que se hace referencia mediante una implementación de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup>.
