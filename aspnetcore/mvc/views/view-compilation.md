---
title: Precompilación y compilación de vistas de Razor en ASP.NET Core
author: rick-anderson
description: Aprenda a habilitar la compilación y precompilación de vistas de MVC Razor en aplicaciones de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="2095c-103">Precompilación y compilación de vistas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2095c-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="2095c-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2095c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2095c-105">Las vistas de Razor se compilan en tiempo de ejecución cuando se invoca la vista.</span><span class="sxs-lookup"><span data-stu-id="2095c-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="2095c-106">ASP.NET Core 1.1.0 y versiones posteriores pueden, opcionalmente, compilar las vistas de Razor e implementarlas con la aplicación (un proceso conocido como precompilación).</span><span class="sxs-lookup"><span data-stu-id="2095c-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="2095c-107">Las plantillas de proyecto de ASP.NET Core 2.x habilitan la precompilación de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="2095c-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2095c-108">La precompilación de vistas de Razor no está disponible actualmente al realizar una [implementación independiente (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="2095c-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="2095c-109">La característica estará disponible para las implementaciones independientes en la versión 2.1.</span><span class="sxs-lookup"><span data-stu-id="2095c-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="2095c-110">Para más información, vea [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102) (Error de compilación de vistas al hacer varias compilaciones para Linux en Windows).</span><span class="sxs-lookup"><span data-stu-id="2095c-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="2095c-111">Consideraciones para la precompilación:</span><span class="sxs-lookup"><span data-stu-id="2095c-111">Precompilation considerations:</span></span>

* <span data-ttu-id="2095c-112">Al precompilar vistas se obtiene como resultado un conjunto publicado más pequeño y un tiempo de inicio más rápido.</span><span class="sxs-lookup"><span data-stu-id="2095c-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="2095c-113">No se pueden editar archivos de Razor después de precompilar vistas.</span><span class="sxs-lookup"><span data-stu-id="2095c-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="2095c-114">Las vistas editadas no estarán presentes en el conjunto publicado.</span><span class="sxs-lookup"><span data-stu-id="2095c-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="2095c-115">Para implementar vistas precompiladas:</span><span class="sxs-lookup"><span data-stu-id="2095c-115">To deploy precompiled views:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2095c-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2095c-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="2095c-117">Si el proyecto tiene como destino .NET Framework, incluya una referencia de paquete a [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="2095c-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="2095c-118">Si el proyecto tiene como destino .NET Core, no es necesario hacer cambios.</span><span class="sxs-lookup"><span data-stu-id="2095c-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="2095c-119">Las plantillas de proyecto de ASP.NET Core 2.x establecen implícitamente `MvcRazorCompileOnPublish` en `true` de forma predeterminada, lo que significa que este nodo se puede quitar de forma segura del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="2095c-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="2095c-120">Si prefiere ser explícito, no hay peligro en configurar la propiedad `MvcRazorCompileOnPublish` en `true`.</span><span class="sxs-lookup"><span data-stu-id="2095c-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="2095c-121">El siguiente ejemplo de *.csproj* resalta esta configuración:</span><span class="sxs-lookup"><span data-stu-id="2095c-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2095c-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2095c-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="2095c-123">Establezca `MvcRazorCompileOnPublish` en `true` e incluya una referencia de paquete a `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="2095c-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="2095c-124">El siguiente ejemplo de *.csproj* resalta estas opciones:</span><span class="sxs-lookup"><span data-stu-id="2095c-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

<span data-ttu-id="2095c-125">Prepare la aplicación para una [implementación independiente de la plataforma](/dotnet/core/deploying/#framework-dependent-deployments-fdd) con el [comando de publicación de la CLI de .NET Core](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="2095c-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="2095c-126">Por ejemplo, ejecute el siguiente comando en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="2095c-126">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="2095c-127">Cuando se realiza correctamente la precompilación, se genera un archivo *<nombre_de_proyecto>.PrecompiledViews.dll* que contiene las vistas de Razor compiladas.</span><span class="sxs-lookup"><span data-stu-id="2095c-127">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="2095c-128">Por ejemplo, la captura de pantalla de abajo muestra el contenido de *Index.cshtml* dentro de *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="2095c-128">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Vistas de Razor dentro de DLL](view-compilation/_static/razor-views-in-dll.png)
