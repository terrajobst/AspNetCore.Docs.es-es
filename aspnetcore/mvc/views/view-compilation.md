---
title: "Precompilación y compilación de vistas de Razor"
author: rick-anderson
description: "Documento de referencia que explica cómo habilitar la precompilación y la compilación de vistas de MVC Razor en aplicaciones ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: bd3f4470035b0375fc79aa7caa73b60ba6fc4f53
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="71258-103">Precompilación y compilación de vistas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="71258-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="71258-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="71258-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="71258-105">Las vistas de Razor se compilan en tiempo de ejecución cuando se invoca la vista.</span><span class="sxs-lookup"><span data-stu-id="71258-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="71258-106">ASP.NET Core 1.1.0 y versiones posteriores pueden, opcionalmente, compilar las vistas de Razor e implementarlas con la aplicación (un proceso conocido como precompilación).</span><span class="sxs-lookup"><span data-stu-id="71258-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="71258-107">Las plantillas de proyecto de ASP.NET Core 2.x habilitan la precompilación de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="71258-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="71258-108">La precompilación de vistas de Razor no está disponible actualmente al realizar una [implementación independiente (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="71258-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="71258-109">La característica estará disponible para las implementaciones independientes en la versión 2.1.</span><span class="sxs-lookup"><span data-stu-id="71258-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="71258-110">Para más información, vea [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102) (Error de compilación de vistas al hacer varias compilaciones para Linux en Windows).</span><span class="sxs-lookup"><span data-stu-id="71258-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="71258-111">Consideraciones para la precompilación:</span><span class="sxs-lookup"><span data-stu-id="71258-111">Precompilation considerations:</span></span>

* <span data-ttu-id="71258-112">Al precompilar vistas se obtiene como resultado un conjunto publicado más pequeño y un tiempo de inicio más rápido.</span><span class="sxs-lookup"><span data-stu-id="71258-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="71258-113">No se pueden editar archivos de Razor después de precompilar vistas.</span><span class="sxs-lookup"><span data-stu-id="71258-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="71258-114">Las vistas editadas no estarán presentes en el conjunto publicado.</span><span class="sxs-lookup"><span data-stu-id="71258-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="71258-115">Para implementar vistas precompiladas:</span><span class="sxs-lookup"><span data-stu-id="71258-115">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="71258-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="71258-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="71258-117">Si el proyecto tiene como destino .NET Framework, incluya una referencia de paquete a [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="71258-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="71258-118">Si el proyecto tiene como destino .NET Core, no es necesario hacer cambios.</span><span class="sxs-lookup"><span data-stu-id="71258-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="71258-119">Las plantillas de proyecto de ASP.NET Core 2.x establecen implícitamente `MvcRazorCompileOnPublish` en `true` de forma predeterminada, lo que significa que este nodo se puede quitar de forma segura del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="71258-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="71258-120">Si prefiere ser explícito, no hay peligro en configurar la propiedad `MvcRazorCompileOnPublish` en `true`.</span><span class="sxs-lookup"><span data-stu-id="71258-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="71258-121">El siguiente ejemplo de *.csproj* resalta esta configuración:</span><span class="sxs-lookup"><span data-stu-id="71258-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="71258-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="71258-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="71258-123">Establezca `MvcRazorCompileOnPublish` en `true` e incluya una referencia de paquete a `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="71258-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="71258-124">El siguiente ejemplo de *.csproj* resalta estas opciones:</span><span class="sxs-lookup"><span data-stu-id="71258-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="71258-125">Prepare la aplicación para una [implementación dependiente de marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd) mediante la ejecución de un comando como el siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="71258-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) by executing a command such as the following at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="71258-126">Cuando se realiza correctamente la precompilación, se genera un archivo *<nombre_de_proyecto>.PrecompiledViews.dll* que contiene las vistas de Razor compiladas.</span><span class="sxs-lookup"><span data-stu-id="71258-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="71258-127">Por ejemplo, la captura de pantalla de abajo muestra el contenido de *Index.cshtml* dentro de *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="71258-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Vistas de Razor dentro de DLL](view-compilation/_static/razor-views-in-dll.png)
