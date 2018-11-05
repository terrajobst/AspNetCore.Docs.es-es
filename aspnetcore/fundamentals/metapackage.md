---
title: Metapaquete Microsoft.AspNetCore.All para ASP.NET Core 2.0
author: Rick-Anderson
description: El metapaquete Microsoft.AspNetCore.All no se recomienda para ASP.NET Core 2.1 y versiones posteriores.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: d95bafd412969bb8db38499bd2ff01af510d872c
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148855"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>Metapaquete Microsoft.AspNetCore.All para ASP.NET Core 2.0

> [!NOTE]
> Se recomienda que las aplicaciones que tengan como destino ASP.NET Core 2.1 y versiones posteriores usen el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) en lugar de este paquete. Consulte [Migración desde Microsoft.AspNetCore.All a Microsoft.AspNetCore.App](#migrate) en este artículo.

Esta característica requiere ASP.NET Core 2.x con .NET Core 2.x como destino.

El metapaquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core incluye lo siguiente:

* Todos los paquetes admitidos por el equipo de ASP.NET Core.
* Todos los paquetes admitidos por Entity Framework Core.
* Dependencias internas y de terceros usadas por ASP.NET Core y Entity Framework Core.

Todas las características de ASP.NET Core 2.x y Entity Framework Core 2.x están incluidas en el paquete `Microsoft.AspNetCore.All`. Las plantillas de proyecto predeterminada destinadas a ASP.NET 2.0 usan este paquete.

El número de versión del metapaquete `Microsoft.AspNetCore.All` representa la versión de ASP.NET Core y la versión de Entity Framework Core.

Las aplicaciones que usan el metapaquete `Microsoft.AspNetCore.All` pueden aprovechar automáticamente el [almacén en tiempo de ejecución de .NET Core](/dotnet/core/deploying/runtime-store). El almacén en tiempo de ejecución contiene todos los recursos en tiempo de ejecución necesarios para ejecutar aplicaciones de ASP.NET Core 2.x. Al usar el metapaquete `Microsoft.AspNetCore.All`, **no** se implementa ningún recurso de los paquetes NuGet de ASP.NET Core a los que se hace referencia con la aplicación, porque el almacén en tiempo de ejecución de .NET Core ya contiene esos recursos. Los recursos del almacén en tiempo de ejecución se precompilan para mejorar el tiempo de inicio de la aplicación.

Puede usar el proceso de recorte de paquetes para quitar los paquetes que no se usan. Los paquetes recortados se excluyen de la salida de la aplicación publicada.

El siguiente archivo *.csproj* hace referencia al metapaquete `Microsoft.AspNetCore.All` de ASP.NET Core:

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a>Control de versiones implícitas

En ASP.NET Core 2.1 o una versión posterior, puede especificar la referencia de paquete `Microsoft.AspNetCore.All` sin una versión. Si no se especifica la versión, el SDK define una versión implícita (`Microsoft.NET.Sdk.Web`). Es recomendable confiar en la versión implícita especificada por el SDK y no establecer de forma explícita el número de versión en la referencia del paquete. Si tiene alguna pregunta sobre este enfoque, deje un comentario de GitHub en [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430) (Debate sobre la versión implícita de Microsoft.AspNetCore.App).

La versión implícita se establece en `major.minor.0` para las aplicaciones portátiles. El mecanismo de puesta al día del marco de uso compartido ejecuta la aplicación en la versión más reciente compatible entre los marcos de uso compartidos instalados. Para garantizar que se use la misma versión en el desarrollo, las pruebas y la producción, asegúrese de que en todos los entornos esté instalada la misma versión del marco de uso compartido. Para las aplicaciones autocontenidas, el número de versión implícita se establece en el valor `major.minor.patch` del marco de uso compartido incluido en el SDK instalado.

Especificar un número de versión en la referencia de paquete `Microsoft.AspNetCore.All` **no** garantiza que se seleccione la versión del marco de uso compartido. Por ejemplo, suponga que se especifica la versión "2.1.1", pero está instalada la "2.1.3". En ese caso, la aplicación usará el valor "2.1.3". Aunque no se recomienda, puede deshabilitar la puesta al día (revisión o secundaria). Para obtener más información sobre la puesta al día del host de dotnet y cómo configurar su comportamiento, vea [Dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md) (Puesta al día del host de dotnet).

El SDK del proyecto se debe establecer en `Microsoft.NET.Sdk.Web` en el archivo de proyecto para usar la versión implícita de `Microsoft.AspNetCore.All`. Cuando se especifica el SDK `Microsoft.NET.Sdk` (`<Project Sdk="Microsoft.NET.Sdk">` en la parte superior del archivo del proyecto), se genera la advertencia siguiente:

*Advertencia NU1604: La dependencia del proyecto Microsoft.AspNetCore.All no contiene un límite inferior inclusivo. Incluya un límite inferior en la versión de dependencia para garantizar que los resultados de restauración son coherentes.*

Este es un problema conocido con el SDK de .NET Core 2.1 y se corregirá en el SDK de .NET Core 2.2.

::: moniker-end

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

## <a name="update-aspnet-core-21"></a>Actualización de ASP.NET Core 2.1

Se recomienda migrar al metapaquete `Microsoft.AspNetCore.App` con la versión 2.1 y versiones posteriores. Para seguir usando el metapaquete `Microsoft.AspNetCore.All` y asegurarse de que se ha implementado la última versión de la revisión:

* En los equipos de desarrollo y los servidores de compilación: instale el último [SDK de .NET Core](https://www.microsoft.com/net/download).
* En los servidores de implementación: instale el último [.NET Core Runtime](https://www.microsoft.com/net/download).
 Su aplicación se pondrá al día con la última versión instalada al reiniciar la aplicación.
