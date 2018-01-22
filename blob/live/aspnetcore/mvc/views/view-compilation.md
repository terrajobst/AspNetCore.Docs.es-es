---
title: "Precompilación y compilación de vista razor"
author: rick-anderson
description: "Un documento de referencia que se explica cómo habilitar la compilación de la vista de MVC Razor y precompilación en aplicaciones de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 12/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 87989455c2fb6b5a922c7fb6133aa3e8cef42c88
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="8ddfe-103">Compilación de vista Razor y precompilación en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ddfe-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="8ddfe-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8ddfe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8ddfe-105">Vistas de Razor se compilan en tiempo de ejecución cuando se invoca la vista.</span><span class="sxs-lookup"><span data-stu-id="8ddfe-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="8ddfe-106">ASP.NET principales 1.1.0 y versiones posteriores puede opcionalmente compilar vistas Razor e implementarlas con la aplicación&mdash;un proceso conocido como precompilación.</span><span class="sxs-lookup"><span data-stu-id="8ddfe-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="8ddfe-107">Las plantillas de proyecto de ASP.NET Core 2.x habilitan precompilación de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="8ddfe-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8ddfe-108">Precompilación de vista Razor no está disponible actualmente al realizar una [implementación independiente (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) de núcleo de ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="8ddfe-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="8ddfe-109">La característica estará disponible para DVL cuando vaya a versiones 2.1.</span><span class="sxs-lookup"><span data-stu-id="8ddfe-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="8ddfe-110">Para obtener más información, consulte [se produce un error en la compilación de la vista cuando se compila entre para Linux en Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="8ddfe-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="8ddfe-111">Consideraciones para la precompilación:</span><span class="sxs-lookup"><span data-stu-id="8ddfe-111">Precompilation considerations:</span></span>

* <span data-ttu-id="8ddfe-112">Precompilar vistas da como resultado un menor publicado agrupación y el tiempo de inicio.</span><span class="sxs-lookup"><span data-stu-id="8ddfe-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="8ddfe-113">No se puede editar archivos de Razor después de precompilar vistas.</span><span class="sxs-lookup"><span data-stu-id="8ddfe-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="8ddfe-114">Las vistas editadas no estará presentes en el paquete publicado.</span><span class="sxs-lookup"><span data-stu-id="8ddfe-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="8ddfe-115">Para implementar vistas precompiladas:</span><span class="sxs-lookup"><span data-stu-id="8ddfe-115">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8ddfe-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8ddfe-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8ddfe-117">Si el proyecto tiene como destino .NET Framework, incluir una referencia de paquete a [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="8ddfe-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="8ddfe-118">Si el proyecto tiene como destino .NET Core, no son necesarios cambios.</span><span class="sxs-lookup"><span data-stu-id="8ddfe-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="8ddfe-119">Las plantillas de proyecto de ASP.NET Core 2.x establece implícitamente `MvcRazorCompileOnPublish` a `true` de forma predeterminada, lo que significa que este nodo se puede quitar de forma segura desde el *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="8ddfe-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="8ddfe-120">Si prefiere sea explícito, no hay peligro en configuración de la `MvcRazorCompileOnPublish` propiedad `true`.</span><span class="sxs-lookup"><span data-stu-id="8ddfe-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="8ddfe-121">El siguiente *.csproj* ejemplo resalta esta configuración:</span><span class="sxs-lookup"><span data-stu-id="8ddfe-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8ddfe-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8ddfe-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8ddfe-123">Establecer `MvcRazorCompileOnPublish` a `true`e incluir una referencia de paquete para `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="8ddfe-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="8ddfe-124">El siguiente *.csproj* ejemplo resalta estas opciones:</span><span class="sxs-lookup"><span data-stu-id="8ddfe-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="8ddfe-125">Preparar la aplicación para una [implementación dependiente de framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) al ejecutar un comando como el siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="8ddfe-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) by executing a command such as the following at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="8ddfe-126">Un *< Nombre_proyecto >. PrecompiledViews.dll* archivo, que contiene las vistas de Razor compiladas, se produce cuando se realiza correctamente de precompilación.</span><span class="sxs-lookup"><span data-stu-id="8ddfe-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="8ddfe-127">Por ejemplo, la captura de pantalla siguiente muestra el contenido de *Index.cshtml* dentro de *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="8ddfe-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Vistas de Razor dentro de DLL](view-compilation/_static/razor-views-in-dll.png)
