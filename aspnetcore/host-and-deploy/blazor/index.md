---
title: Hospedaje e implementación de Blazor
author: guardrex
description: Descubra cómo hospedar e implementar aplicaciones de Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: a7739e2b240d7fd6c85e68105892c802228ebeb5
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614591"
---
# <a name="host-and-deploy-blazor"></a>Hospedaje e implementación de Blazor

Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>Publicar la aplicación

Las aplicaciones se publican para la implementación en la configuración de lanzamiento con el comando [dotnet publish](/dotnet/core/tools/dotnet-publish). Un entorno de desarrollo integrado (IDE) puede controlar la ejecución del comando `dotnet publish` automáticamente mediante sus características de publicación integradas, por lo que puede que no sea necesario ejecutar manualmente el comando desde un símbolo del sistema, en función de las herramientas de desarrollo que se usen.

```console
dotnet publish -c Release
```

`dotnet publish` desencadena una [restauración](/dotnet/core/tools/dotnet-restore) de las dependencias del proyecto y [compila](/dotnet/core/tools/dotnet-build) el proyecto antes de crear los recursos para la implementación. Como parte del proceso de compilación, se quitan los ensamblados y métodos que no se usan para reducir los tiempos de carga y el tamaño de descarga de la aplicación. La implementación se crea en la carpeta */bin/Release/{TARGET FRAMEWORK}/publish*.

Los recursos de la carpeta *publish* se implementan en el servidor web. La implementación puede ser un proceso manual o automatizado, en función de las herramientas de desarrollo que se usen.

## <a name="deployment"></a>Implementación

Para una guía sobre la implementación, consulte los temas siguientes:

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
