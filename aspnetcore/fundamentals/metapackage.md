---
title: Metapaquete Microsoft.AspNetCore.All para ASP.NET Core 2.0 y versiones posteriores
author: Rick-Anderson
description: El metapaquete Microsoft.AspNetCore.All incluye todos los paquetes de ASP.NET Core y Entity Framework Core, junto con sus dependencias.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: fbb76f41f3178ddc4e51faa14edece1869a30cd0
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729082"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>Metapaquete Microsoft.AspNetCore.All para ASP.NET Core 2.0

> [!NOTE]
> Se recomienda que las aplicaciones que tengan como destino ASP.NET Core 2.1 y versiones posteriores usen [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) en lugar de este paquete. Consulte [Migración desde Microsoft.AspNetCore.All a Microsoft.AspNetCore.App](#migrate) en este artículo.

Esta característica requiere ASP.NET Core 2.x con .NET Core 2.x como destino.

El metapaquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core incluye lo siguiente:

* Todos los paquetes admitidos por el equipo de ASP.NET Core.
* Todos los paquetes admitidos por Entity Framework Core. 
* Dependencias internas y de terceros usadas por ASP.NET Core y Entity Framework Core. 

Todas las características de ASP.NET Core 2.x y Entity Framework Core 2.x están incluidas en el paquete `Microsoft.AspNetCore.All`. Las plantillas de proyecto predeterminada destinadas a ASP.NET 2.0 usan este paquete.

El número de versión del metapaquete `Microsoft.AspNetCore.All` representa la versión de ASP.NET Core y la versión de Entity Framework Core.

Las aplicaciones que usan el metapaquete `Microsoft.AspNetCore.All` pueden aprovechar automáticamente el [almacén en tiempo de ejecución de .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). El almacén en tiempo de ejecución contiene todos los recursos en tiempo de ejecución necesarios para ejecutar aplicaciones de ASP.NET Core 2.x. Al usar el metapaquete `Microsoft.AspNetCore.All`, **no** se implementa ningún recurso de los paquetes NuGet de ASP.NET Core a los que se hace referencia con la aplicación, porque el almacén en tiempo de ejecución de .NET Core ya contiene esos recursos. Los recursos del almacén en tiempo de ejecución se precompilan para mejorar el tiempo de inicio de la aplicación.

Puede usar el proceso de recorte de paquetes para quitar los paquetes que no se usan. Los paquetes recortados se excluyen de la salida de la aplicación publicada.

El siguiente archivo *.csproj* hace referencia al metapaquete `Microsoft.AspNetCore.All` de ASP.NET Core:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>Migración desde Microsoft.AspNetCore.All a Microsoft.AspNetCore.App

En `Microsoft.AspNetCore.All` se incluyen los siguientes paquetes, pero no el paquete `Microsoft.AspNetCore.App`. 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

Para pasar de `Microsoft.AspNetCore.All` a `Microsoft.AspNetCore.App`, si su aplicación usa las API de los paquetes anteriores, o bien paquetes incluidos en ellos, agregue las referencias correspondientes a dichos paquetes en el proyecto.

No se incluye implícitamente ninguna dependencia de los paquetes anteriores que no sea, de otro modo, una dependencia de `Microsoft.AspNetCore.App`. Por ejemplo:

* `StackExchange.Redis` como dependencia de `Microsoft.Extensions.Caching.Redis`
* `Microsoft.ApplicationInsights` como dependencia de `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
