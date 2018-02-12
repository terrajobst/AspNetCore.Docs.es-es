---
title: Metapaquete Microsoft.AspNetCore.All para ASP.NET Core 2.x y versiones posteriores
author: Rick-Anderson
description: El metapaquete Microsoft.AspNetCore.All incluye todos los paquetes de ASP.NET Core y Entity Framework Core, junto con sus dependencias.
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 07220fdae299723088fa85e452cedff5e5685bd7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="b889a-103">Metapaquete Microsoft.AspNetCore.All para ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b889a-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="b889a-104">Esta característica requiere ASP.NET Core 2.x con .NET Core 2.x como destino.</span><span class="sxs-lookup"><span data-stu-id="b889a-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="b889a-105">El metapaquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b889a-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="b889a-106">Todos los paquetes admitidos por el equipo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b889a-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="b889a-107">Todos los paquetes admitidos por Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b889a-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="b889a-108">Dependencias internas y de terceros usadas por ASP.NET Core y Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b889a-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="b889a-109">Todas las características de ASP.NET Core 2.x y Entity Framework Core 2.x están incluidas en el paquete `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="b889a-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="b889a-110">Las plantillas de proyecto predeterminadas usan este paquete.</span><span class="sxs-lookup"><span data-stu-id="b889a-110">The default project templates use this package.</span></span>

<span data-ttu-id="b889a-111">El número de versión del metapaquete `Microsoft.AspNetCore.All` representa la versión de ASP.NET Core y la versión de Entity Framework Core (alineado con la versión de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="b889a-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="b889a-112">Las aplicaciones que usan el metapaquete `Microsoft.AspNetCore.All` pueden aprovechar automáticamente el [almacén en tiempo de ejecución de .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="b889a-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="b889a-113">El almacén en tiempo de ejecución contiene todos los recursos en tiempo de ejecución necesarios para ejecutar aplicaciones de ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="b889a-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="b889a-114">Al usar el metapaquete `Microsoft.AspNetCore.All`, **no** se implementa ningún recurso de los paquetes NuGet de ASP.NET Core a los que se hace referencia con la aplicación, porque el almacén en tiempo de ejecución de .NET Core ya contiene esos recursos.</span><span class="sxs-lookup"><span data-stu-id="b889a-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="b889a-115">Los recursos del almacén en tiempo de ejecución se precompilan para mejorar el tiempo de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b889a-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="b889a-116">Puede usar el proceso de recorte de paquetes para quitar los paquetes que no se usan.</span><span class="sxs-lookup"><span data-stu-id="b889a-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="b889a-117">Los paquetes recortados se excluyen de la salida de la aplicación publicada.</span><span class="sxs-lookup"><span data-stu-id="b889a-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="b889a-118">El siguiente archivo *.csproj* hace referencia al metapaquete `Microsoft.AspNetCore.All` de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b889a-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
