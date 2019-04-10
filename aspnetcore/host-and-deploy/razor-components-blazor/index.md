---
title: Hospedaje e implementación de Razor Components y Blazor
author: guardrex
description: Descubra cómo hospedar e implementar aplicaciones de Razor Components y Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/index
ms.openlocfilehash: eeaa27bef584523b77d8b47000b310fe0cac7341
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59070312"
---
# <a name="host-and-deploy-razor-components-and-blazor"></a><span data-ttu-id="866ed-103">Hospedaje e implementación de Razor Components y Blazor</span><span class="sxs-lookup"><span data-stu-id="866ed-103">Host and deploy Razor Components and Blazor</span></span>

<span data-ttu-id="866ed-104">Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="866ed-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="866ed-105">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="866ed-105">Publish the app</span></span>

<span data-ttu-id="866ed-106">Las aplicaciones se publican para la implementación en la configuración de lanzamiento con el comando [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="866ed-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="866ed-107">Un entorno de desarrollo integrado (IDE) puede controlar la ejecución del comando `dotnet publish` automáticamente mediante sus características de publicación integradas, por lo que puede que no sea necesario ejecutar manualmente el comando desde un símbolo del sistema, en función de las herramientas de desarrollo que se usen.</span><span class="sxs-lookup"><span data-stu-id="866ed-107">An integrated development environment (IDE) may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

`dotnet publish` <span data-ttu-id="866ed-108">desencadena una [restauración](/dotnet/core/tools/dotnet-restore) de las dependencias del proyecto y [compila](/dotnet/core/tools/dotnet-build) el proyecto antes de crear los recursos para la implementación.</span><span class="sxs-lookup"><span data-stu-id="866ed-108">triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="866ed-109">Como parte del proceso de compilación, se quitan los ensamblados y métodos que no se usan para reducir los tiempos de carga y el tamaño de descarga de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="866ed-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="866ed-110">La implementación se crea en la carpeta */bin/Release/{TARGET FRAMEWORK}/publish*.</span><span class="sxs-lookup"><span data-stu-id="866ed-110">The deployment is created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="866ed-111">Los recursos de la carpeta *publish* se implementan en el servidor web.</span><span class="sxs-lookup"><span data-stu-id="866ed-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="866ed-112">La implementación puede ser un proceso manual o automatizado, en función de las herramientas de desarrollo que se usen.</span><span class="sxs-lookup"><span data-stu-id="866ed-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="866ed-113">Implementación</span><span class="sxs-lookup"><span data-stu-id="866ed-113">Deployment</span></span>

<span data-ttu-id="866ed-114">Para una guía sobre la implementación, consulte los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="866ed-114">For deployment guidance, see the following topics:</span></span>

* [<span data-ttu-id="866ed-115">Blazor del lado cliente</span><span class="sxs-lookup"><span data-stu-id="866ed-115">Client-side Blazor</span></span>](xref:host-and-deploy/razor-components-blazor/blazor)
* [<span data-ttu-id="866ed-116">Razor Components de ASP.NET Core del lado servidor</span><span class="sxs-lookup"><span data-stu-id="866ed-116">Server-side ASP.NET Core Razor Components</span></span>](xref:host-and-deploy/razor-components-blazor/razor-components)
