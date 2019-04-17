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
# <a name="host-and-deploy-blazor"></a><span data-ttu-id="e09b7-103">Hospedaje e implementación de Blazor</span><span class="sxs-lookup"><span data-stu-id="e09b7-103">Host and deploy Blazor</span></span>

<span data-ttu-id="e09b7-104">Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="e09b7-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="e09b7-105">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="e09b7-105">Publish the app</span></span>

<span data-ttu-id="e09b7-106">Las aplicaciones se publican para la implementación en la configuración de lanzamiento con el comando [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="e09b7-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="e09b7-107">Un entorno de desarrollo integrado (IDE) puede controlar la ejecución del comando `dotnet publish` automáticamente mediante sus características de publicación integradas, por lo que puede que no sea necesario ejecutar manualmente el comando desde un símbolo del sistema, en función de las herramientas de desarrollo que se usen.</span><span class="sxs-lookup"><span data-stu-id="e09b7-107">An integrated development environment (IDE) may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="e09b7-108">`dotnet publish` desencadena una [restauración](/dotnet/core/tools/dotnet-restore) de las dependencias del proyecto y [compila](/dotnet/core/tools/dotnet-build) el proyecto antes de crear los recursos para la implementación.</span><span class="sxs-lookup"><span data-stu-id="e09b7-108">`dotnet publish` triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="e09b7-109">Como parte del proceso de compilación, se quitan los ensamblados y métodos que no se usan para reducir los tiempos de carga y el tamaño de descarga de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e09b7-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="e09b7-110">La implementación se crea en la carpeta */bin/Release/{TARGET FRAMEWORK}/publish*.</span><span class="sxs-lookup"><span data-stu-id="e09b7-110">The deployment is created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="e09b7-111">Los recursos de la carpeta *publish* se implementan en el servidor web.</span><span class="sxs-lookup"><span data-stu-id="e09b7-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="e09b7-112">La implementación puede ser un proceso manual o automatizado, en función de las herramientas de desarrollo que se usen.</span><span class="sxs-lookup"><span data-stu-id="e09b7-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="e09b7-113">Implementación</span><span class="sxs-lookup"><span data-stu-id="e09b7-113">Deployment</span></span>

<span data-ttu-id="e09b7-114">Para una guía sobre la implementación, consulte los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="e09b7-114">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
