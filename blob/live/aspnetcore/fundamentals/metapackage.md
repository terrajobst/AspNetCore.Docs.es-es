---
title: Microsoft.AspNetCore.All metapackage para ASP.NET Core 2.x y versiones posteriores
author: Rick-Anderson
description: El metapackage Microsoft.AspNetCore.All incluye todos los paquetes de ASP.NET Core y Entity Framework Core, junto con sus dependencias.
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 8a44ee7ebb7e6b0112000429f1f080bceb7dc895
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="4f892-103">Microsoft.AspNetCore.All metapackage para ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4f892-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="4f892-104">Esta característica requiere ASP.NET Core 2.x destinatarios .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="4f892-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="4f892-105">El metapaquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4f892-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="4f892-106">Todos los paquetes admitidos por el equipo de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f892-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="4f892-107">Todos los paquetes admitidos por Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4f892-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="4f892-108">Dependencias internas y de terceros usadas por ASP.NET Core y Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4f892-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="4f892-109">Todas las características de ASP.NET Core 2.x y Entity Framework Core 2.x se incluyen en el `Microsoft.AspNetCore.All` paquete.</span><span class="sxs-lookup"><span data-stu-id="4f892-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="4f892-110">Las plantillas de proyecto predeterminadas usan este paquete.</span><span class="sxs-lookup"><span data-stu-id="4f892-110">The default project templates use this package.</span></span>

<span data-ttu-id="4f892-111">El número de versión de la `Microsoft.AspNetCore.All` metapackage representa la versión de ASP.NET Core y la versión de Entity Framework Core (alineado con la versión de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="4f892-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="4f892-112">Las aplicaciones que utilizan el `Microsoft.AspNetCore.All` metapackage automáticamente aprovechar la [almacén de tiempo de ejecución de .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="4f892-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="4f892-113">El almacén de tiempo de ejecución contiene todos los activos en tiempo de ejecución necesarios para ejecutar ASP.NET Core aplicaciones 2.x.</span><span class="sxs-lookup"><span data-stu-id="4f892-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="4f892-114">Cuando se usa el `Microsoft.AspNetCore.All` metapackage, **no** activos de los paquetes de NuGet de núcleo de ASP.NET que se hace referencia se implementan con la aplicación &mdash; el almacén de tiempo de ejecución de .NET Core contiene estos activos.</span><span class="sxs-lookup"><span data-stu-id="4f892-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="4f892-115">Los activos en el almacén de tiempo de ejecución se precompilan para mejorar el tiempo de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4f892-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="4f892-116">Puede usar el proceso de recorte de paquete para quitar los paquetes que no se usan.</span><span class="sxs-lookup"><span data-stu-id="4f892-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="4f892-117">Se excluyen recortados paquetes de salida de la aplicación publicada.</span><span class="sxs-lookup"><span data-stu-id="4f892-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="4f892-118">El siguiente *.csproj* referencias de archivo la `Microsoft.AspNetCore.All` metapackage principales de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="4f892-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
