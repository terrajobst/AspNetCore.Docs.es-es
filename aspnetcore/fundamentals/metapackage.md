---
title: Microsoft.AspNetCore.All metapackage para ASP.NET Core 2.x y versiones posteriores
author: Rick-Anderson
description: Microsoft.AspNetCore.All metapackage incluye todos los paquetes.
keywords: "Núcleo de ASP.NET, NuGet, empaquetar, Microsoft.AspNetCore.All, metapackage"
ms.author: riande
manager: wpickett
ms.date: 07/16/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 255438a4ce36ce4978f8c8ee298388a25ac00d17
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>Microsoft.AspNetCore.All metapackage para ASP.NET Core 2.x

Esta característica requiere ASP.NET Core 2.x.

El [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage para ASP.NET Core incluye:

* Paquetes todos los admiten por el equipo de ASP.NET Core.
* Todos los paquetes mediante el núcleo de Entity Framework admiten. 
* Dependencias internas y 3rd terceros utilizadas por ASP.NET Core y Entity Framework Core. 

Todas las características de ASP.NET Core 2.x y Entity Framework Core 2.x se incluyen en el `Microsoft.AspNetCore.All` paquete. Las plantillas de proyecto predeterminadas usan este paquete.

El número de versión de la `Microsoft.AspNetCore.All` metapackage representa la versión de ASP.NET Core y la versión de Entity Framework Core (alineado con la versión de .NET Core).

Las aplicaciones que utilizan el `Microsoft.AspNetCore.All` metapackage automáticamente aprovechar el almacén de tiempo de ejecución de .NET Core. El almacén de tiempo de ejecución contiene todos los activos en tiempo de ejecución necesarios para ejecutar ASP.NET Core aplicaciones 2.x. Cuando se usa el `Microsoft.AspNetCore.All` metapackage, **no** activos de los paquetes de NuGet de núcleo de ASP.NET que se hace referencia se implementan con la aplicación &mdash; el almacén de tiempo de ejecución de .NET Core contiene estos activos. <!-- todo add link to Runtime store -->Los activos en el almacén de tiempo de ejecución se precompilan para mejorar el tiempo de inicio de la aplicación.

Puede usar el proceso de recorte de paquete para quitar los paquetes que no se usan. Se excluyen recortados paquetes de salida de la aplicación publicada.

El siguiente *.csproj* referencias de archivo la `Microsoft.AspNetCore.All` metapackage principales de ASP.NET:

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
