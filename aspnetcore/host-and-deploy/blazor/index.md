---
title: Hospedaje e implementación de ASP.NET Core Blazor
author: guardrex
description: Descubra cómo hospedar e implementar aplicaciones de Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 8a5ac5c58e7ceab07e55da8b61ebb01f7ac984bc
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153199"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="88e93-103">Hospedaje e implementación de ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="88e93-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="88e93-104">Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="88e93-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="88e93-105">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="88e93-105">Publish the app</span></span>

<span data-ttu-id="88e93-106">Las aplicaciones se publican para implementación en la configuración de versión.</span><span class="sxs-lookup"><span data-stu-id="88e93-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="88e93-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="88e93-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="88e93-108">Seleccione **Compilar** > **Publicar {aplicación}** en la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="88e93-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="88e93-109">Seleccione el *destino de publicación*.</span><span class="sxs-lookup"><span data-stu-id="88e93-109">Select the *publish target*.</span></span> <span data-ttu-id="88e93-110">Para publicar localmente, seleccione **Carpeta**.</span><span class="sxs-lookup"><span data-stu-id="88e93-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="88e93-111">Acepte la ubicación predeterminada del campo **Elegir una carpeta** o especifique una ubicación diferente.</span><span class="sxs-lookup"><span data-stu-id="88e93-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="88e93-112">Seleccione el botón **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="88e93-112">Select the **Publish** button.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="88e93-113">Visual Studio Code y CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="88e93-113">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="88e93-114">Use el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) para publicar la aplicación con una configuración de versión:</span><span class="sxs-lookup"><span data-stu-id="88e93-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```console
dotnet publish -c Release
```

---

<span data-ttu-id="88e93-115">Al publicar la aplicación se desencadena una [restauración](/dotnet/core/tools/dotnet-restore) de las dependencias del proyecto y se [compila](/dotnet/core/tools/dotnet-build) el proyecto antes de crear los recursos para la implementación.</span><span class="sxs-lookup"><span data-stu-id="88e93-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="88e93-116">Como parte del proceso de compilación, se quitan los ensamblados y métodos que no se usan para reducir los tiempos de carga y el tamaño de descarga de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="88e93-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="88e93-117">Una aplicación cliente de Blazor se publica en la carpeta */bin/Release/{RED DE DESTINO}/publish/{NOMBRE DE ENSAMBLADO}/dist*.</span><span class="sxs-lookup"><span data-stu-id="88e93-117">A Blazor client-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span> <span data-ttu-id="88e93-118">Una aplicación de servidor de Blazor se publica en la carpeta */bin/Release/{TARGET FRAMEWORK}/publish*.</span><span class="sxs-lookup"><span data-stu-id="88e93-118">A Blazor server-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="88e93-119">Los recursos de la carpeta se implementan en el servidor web.</span><span class="sxs-lookup"><span data-stu-id="88e93-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="88e93-120">La implementación puede ser un proceso manual o automatizado, en función de las herramientas de desarrollo que se usen.</span><span class="sxs-lookup"><span data-stu-id="88e93-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="88e93-121">Implementación</span><span class="sxs-lookup"><span data-stu-id="88e93-121">Deployment</span></span>

<span data-ttu-id="88e93-122">Para una guía sobre la implementación, consulte los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="88e93-122">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a><span data-ttu-id="88e93-123">Hospedaje sin servidor de Blazor con Azure Storage</span><span class="sxs-lookup"><span data-stu-id="88e93-123">Blazor serverless hosting with Azure Storage</span></span>

<span data-ttu-id="88e93-124">Las aplicaciones de cliente de Blazor pueden obtenerse de [Azure Storage](https://azure.microsoft.com/services/storage/) como contenido estático directamente desde un contenedor de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="88e93-124">Blazor client-side apps can be served from [Azure Storage](https://azure.microsoft.com/services/storage/) as static content directly from a storage container.</span></span>

<span data-ttu-id="88e93-125">Para más información, consulte [Hospedaje e implementación de ASP.NET Core Blazor del lado cliente (implementación independiente): Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage).</span><span class="sxs-lookup"><span data-stu-id="88e93-125">For more information, see [Host and deploy ASP.NET Core Blazor client-side (Standalone deployment): Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage).</span></span>
