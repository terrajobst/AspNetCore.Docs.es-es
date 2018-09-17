---
title: Metapaquete Microsoft.AspNetCore.App para ASP.NET Core 2.1 y versiones posteriores
author: Rick-Anderson
description: El metapaquete Microsoft.AspNetCore.App incluye todos los paquetes de ASP.NET Core y Entity Framework Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: d27c3aa53d6edd235006dc136f09558395e15b6e
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538458"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>Metapaquete Microsoft.AspNetCore.App para ASP.NET Core 2.1

Esta característica requiere que ASP.NET Core 2.1 y versiones posteriores tengan como destino .NET Core 2.1 y versiones posteriores.

El [metapaquete](/dotnet/core/packages#metapackages) [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) para ASP.NET Core:

* No incluye dependencias de terceros excepto [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) e [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/). Estas dependencias de terceros se consideran necesarias para garantizar el funcionamiento de las características de los principales marcos.
* Incluye todos los paquetes admitidos por el equipo de ASP.NET Core, excepto aquellos que contienen dependencias de terceros (distintos de los mencionados anteriormente).
* Incluye todos los paquetes admitidos por el equipo de Entity Framework Core, excepto aquellos que contienen dependencias de terceros (distintos de los mencionados anteriormente).

Todas las características de ASP.NET Core 2.1 y versiones posteriores, así como de Entity Framework Core 2.1 y versiones posteriores, están incluidas en el paquete `Microsoft.AspNetCore.App`. Las plantillas de proyecto predeterminada destinadas a ASP.NET 2.1 y versiones posteriores usan este paquete. Se recomienda que las aplicaciones que tengan como destino ASP.NET Core 2.1 y versiones posteriores, así como Entity Framework Core 2.1 y versiones posteriores, usen el paquete `Microsoft.AspNetCore.App`.

El número de versión del metapaquete `Microsoft.AspNetCore.App` representa la versión de ASP.NET Core y la versión de Entity Framework Core.

Mediante el metapaquete `Microsoft.AspNetCore.App` se proporcionan restricciones de versión que protegen la aplicación:

* Si se incluye un paquete que tiene una dependencia transitiva (no directa) en un paquete en `Microsoft.AspNetCore.App` y los números de versión son distintos, NuGet generará un error.
* Los demás paquetes agregados a la aplicación no pueden cambiar la versión de los paquetes que se incluyen en `Microsoft.AspNetCore.App`.
* La coherencia de versiones garantiza una experiencia fiable. `Microsoft.AspNetCore.App` se ha diseñado para evitar las combinaciones de versiones no probadas de bits relacionados que se usan conjuntamente en la misma aplicación.

Las aplicaciones que usan el metapaquete `Microsoft.AspNetCore.App` pueden aprovechar automáticamente el marco de uso compartido de ASP.NET Core. Al usar el metapaquete `Microsoft.AspNetCore.App`, **ningún** recurso de los paquetes NuGet de ASP.NET Core a los que se hace referencia se implementa con la aplicación: el marco compartido de ASP.NET Core contiene esos recursos. Los recursos del marco de uso compartido se precompilan para mejorar el tiempo de inicio de la aplicación. Para más información, vea "Marco de uso compartido" en [Empaquetado de distribución de .NET Core](/dotnet/core/build/distribution-packaging).

El archivo de proyecto siguiente hace referencia al metapaquete `Microsoft.AspNetCore.App` de ASP.NET Core y representa una típica plantilla de ASP.NET Core 2.1:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" Version="2.1.4" />
  </ItemGroup>

</Project>
```

El número de versión en la referencia de `Microsoft.AspNetCore.App` **no** garantiza que se vaya a usar la versión del marco compartido. Por ejemplo, suponga que se ha especificado la versión `2.1.1`, pero se ha instalado la `2.1.3`. En ese caso, la aplicación usa `2.1.3`. Aunque no se recomienda, puede deshabilitar el comportamiento de puesta al día (revisión o secundaria). Para obtener más información sobre el comportamiento de puesta al día de la versión del paquete, vea [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md) (puesta al día del host de dotnet).

## <a name="update-aspnet-core"></a>Actualización de ASP.NET Core

El [metapaquete](/dotnet/core/packages#metapackages) `Microsoft.AspNetCore.App` no es un paquete habitual que se actualice desde NuGet. De forma similar a `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` representa un tiempo de ejecución compartido, con una semántica especial de control de versiones controlada de forma ajena a NuGet. Para obtener más información, vea [Paquetes, metapaquetes y marcos de trabajo](/dotnet/core/packages).

Para actualizar ASP.NET Core:

* En los equipos de desarrollo y los servidores de compilación: descargue e instale el [SDK de .NET Core](https://www.microsoft.com/net/download).
* En los servidores de implementación: descargue e instale el [.NET Core Runtime](https://www.microsoft.com/net/download).

 Las aplicaciones se pondrán al día con la última versión instalada al reiniciar la aplicación. No es necesario actualizar el número de versión de `Microsoft.AspNetCore.App` en el archivo de proyecto. Para obtener más información, vea [Puesta al día de las aplicaciones dependientes de la plataforma](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).

Si la aplicación ha usado `Microsoft.AspNetCore.All` anteriormente, consulte [Migración desde Microsoft.AspNetCore.All a Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).
