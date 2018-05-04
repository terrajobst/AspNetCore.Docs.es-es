---
title: Metapaquete Microsoft.AspNetCore.All para ASP.NET Core 2.x y versiones posteriores
author: Rick-Anderson
description: El metapaquete Microsoft.AspNetCore.All incluye todos los paquetes de ASP.NET Core y Entity Framework Core, junto con sus dependencias.
manager: wpickett
monikerRange: = aspnetcore-2.0
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 4c11f15e659565325bfe8b8d91188b62177b251d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2018
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>Metapaquete Microsoft.AspNetCore.All para ASP.NET Core 2.x

Esta característica requiere ASP.NET Core 2.x con .NET Core 2.x como destino.

El metapaquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core incluye lo siguiente:

* Todos los paquetes admitidos por el equipo de ASP.NET Core.
* Todos los paquetes admitidos por Entity Framework Core. 
* Dependencias internas y de terceros usadas por ASP.NET Core y Entity Framework Core. 

Todas las características de ASP.NET Core 2.x y Entity Framework Core 2.x están incluidas en el paquete `Microsoft.AspNetCore.All`. Las plantillas de proyecto predeterminada destinadas a ASP.NET 2.0 usan este paquete.

El número de versión del metapaquete `Microsoft.AspNetCore.All` representa la versión de ASP.NET Core y la versión de Entity Framework Core (alineado con la versión de .NET Core).

Las aplicaciones que usan el metapaquete `Microsoft.AspNetCore.All` pueden aprovechar automáticamente el [almacén en tiempo de ejecución de .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). El almacén en tiempo de ejecución contiene todos los recursos en tiempo de ejecución necesarios para ejecutar aplicaciones de ASP.NET Core 2.x. Al usar el metapaquete `Microsoft.AspNetCore.All`, **no** se implementa ningún recurso de los paquetes NuGet de ASP.NET Core a los que se hace referencia con la aplicación, porque el almacén en tiempo de ejecución de .NET Core ya contiene esos recursos. Los recursos del almacén en tiempo de ejecución se precompilan para mejorar el tiempo de inicio de la aplicación.

Puede usar el proceso de recorte de paquetes para quitar los paquetes que no se usan. Los paquetes recortados se excluyen de la salida de la aplicación publicada.

El siguiente archivo *.csproj* hace referencia al metapaquete `Microsoft.AspNetCore.All` de ASP.NET Core:

[!code-xml[](../mvc/views/view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=9)]
