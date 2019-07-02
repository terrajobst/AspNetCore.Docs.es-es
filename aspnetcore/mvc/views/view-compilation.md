---
title: Compilación de archivos de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo se compilan los archivos de Razor en una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/20/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: ff66148fc9aad2871f9f55ce76b5a0dacb0ad10c
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313784"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="fc2b0-103">Compilación de archivos de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fc2b0-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="fc2b0-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fc2b0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="fc2b0-105">Un archivo de Razor se compila en tiempo de ejecución, cuando se invoca la vista MVC asociada.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="fc2b0-106">No se admite la publicación de archivos de Razor en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="fc2b0-107">Opcionalmente, los archivos de Razor se pueden compilar en el momento de la publicación e implementar con la aplicación, mediante la herramienta de precompilación.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="fc2b0-108">Un archivo de Razor se compila en tiempo de ejecución, cuando se invoca la vista MVC o la página de Razor asociada.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="fc2b0-109">No se admite la publicación de archivos de Razor en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="fc2b0-110">Opcionalmente, los archivos de Razor se pueden compilar en el momento de la publicación e implementar con la aplicación, mediante la herramienta de precompilación.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="fc2b0-111">Un archivo de Razor se compila en tiempo de ejecución, cuando se invoca la vista MVC o la página de Razor asociada.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="fc2b0-112">Los archivos de Razor se compilan en tiempo de compilación y publicación mediante el [SDK de Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="fc2b0-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fc2b0-113">Los archivos de Razor se compilan en tiempo de compilación y publicación mediante el [SDK de Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="fc2b0-113">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="fc2b0-114">La compilación en tiempo de ejecución se puede habilitar opcionalmente mediante la configuración de la aplicación</span><span class="sxs-lookup"><span data-stu-id="fc2b0-114">Runtime compilation may be optionally enabled by configuring your application.</span></span>

::: moniker-end

## <a name="razor-compilation"></a><span data-ttu-id="fc2b0-115">Compilación de Razor</span><span class="sxs-lookup"><span data-stu-id="fc2b0-115">Razor compilation</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="fc2b0-116">La compilación de los archivos de Razor en tiempo de publicación y compilación está habilitada de manera predeterminada en el SDK de Razor.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-116">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="fc2b0-117">Cuando se habilita la compilación en tiempo de ejecución, complementa a la de tiempo de compilación y permite que se actualicen los archivos de Razor si se modifican.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-117">When enabled, runtime compilation complements build-time compilation, allowing Razor files to be updated if they are edited.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="fc2b0-118">La compilación de los archivos de Razor en tiempo de publicación y compilación está habilitada de manera predeterminada en el SDK de Razor.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-118">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="fc2b0-119">La edición de los archivos de Razor después de que se actualicen se admite en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-119">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="fc2b0-120">De forma predeterminada, solo se implementan con la aplicación los archivos *Views.dll* y los que no son *.cshtml*, o las referencias de los ensamblados necesarios para compilar los archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-120">By default, only the compiled *Views.dll* and no *.cshtml* files or references assemblies required to compile Razor files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fc2b0-121">La herramienta de precompilación está en desuso y se eliminará en ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-121">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="fc2b0-122">Se recomienda migrar al [SDK de Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="fc2b0-122">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="fc2b0-123">El SDK de Razor solo es efectivo cuando no hay propiedades específicas de la precompilación establecidas en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-123">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="fc2b0-124">Por ejemplo, establecer la propiedad `MvcRazorCompileOnPublish` del archivo *.csproj* en `true` deshabilita el SDK de Razor.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-124">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="fc2b0-125">Si el proyecto tiene como destino .NET Framework, instale el paquete NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="fc2b0-125">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="fc2b0-126">Si el proyecto tiene como destino .NET Core, no es necesario hacer cambios.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="fc2b0-127">Las plantillas de proyecto de ASP.NET Core 2.x establecen implícitamente la propiedad `MvcRazorCompileOnPublish` en `true` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="fc2b0-128">Por tanto, este elemento se puede quitar de manera segura del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fc2b0-129">La herramienta de precompilación está en desuso y se eliminará en ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-129">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="fc2b0-130">Se recomienda migrar al [SDK de Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="fc2b0-130">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="fc2b0-131">La precompilación de los archivos de Razor no está disponible cuando se realiza una [implementación independiente (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-131">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="fc2b0-132">Establezca la propiedad `MvcRazorCompileOnPublish` en `true` e instale el paquete NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/).</span><span class="sxs-lookup"><span data-stu-id="fc2b0-132">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="fc2b0-133">El siguiente ejemplo de *.csproj* resalta estas opciones:</span><span class="sxs-lookup"><span data-stu-id="fc2b0-133">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="fc2b0-134">Prepare la aplicación para una [implementación independiente de la plataforma](/dotnet/core/deploying/#framework-dependent-deployments-fdd) con el [comando de publicación de la CLI de .NET Core](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="fc2b0-134">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="fc2b0-135">Por ejemplo, ejecute el siguiente comando en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="fc2b0-135">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="fc2b0-136">Se crea un archivo *\<nombre_del_proyecto>.PrecompiledViews.dll*, que contiene los archivos de Razor compilados, cuando la precompilación se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-136">A *\<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="fc2b0-137">Por ejemplo, en la captura de pantalla siguiente se muestra el contenido de *Index.cshtml* en *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="fc2b0-137">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Vistas de Razor dentro de DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a><span data-ttu-id="fc2b0-139">Compilación en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="fc2b0-139">Runtime compilation</span></span>

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="fc2b0-140">La compilación en tiempo de compilación se complementa con la compilación en tiempo de ejecución de archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-140">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="fc2b0-141">ASP.NET Core MVC volverá a compilar los archivos de Razor cuando cambie el contenido de un archivo *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-141">ASP.NET Core MVC will recompile Razor files when the contents of a *.cshtml* file change.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="fc2b0-142">La compilación en tiempo de compilación se complementa con la compilación en tiempo de ejecución de archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-142">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="fc2b0-143"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> obtiene o establece un valor que determina si los archivos de Razor (vistas y Razor Pages) se vuelven a compilar y actualizar si cambian los archivos en el disco.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-143">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="fc2b0-144">El valor predeterminado es `true` para:</span><span class="sxs-lookup"><span data-stu-id="fc2b0-144">The default value is `true` for:</span></span>

* <span data-ttu-id="fc2b0-145">Si la versión de compatibilidad de la aplicación se establece en <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> o una versión anterior</span><span class="sxs-lookup"><span data-stu-id="fc2b0-145">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier</span></span>
* <span data-ttu-id="fc2b0-146">Si la versión de compatibilidad de la aplicación se establece en <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> o una versión posterior, y la aplicación está en el entorno de desarrollo <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-146">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> or later and the app is in the Development environment <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span></span> <span data-ttu-id="fc2b0-147">En otras palabras, los archivos de Razor no se vuelven a compilar en el entorno que no es de desarrollo a menos que <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> se establezca de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-147">In other words, Razor files would not recompile in non-Development environment unless <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is explicitly set.</span></span>

<span data-ttu-id="fc2b0-148">Para obtener instrucciones y ejemplos de configuración de la versión de compatibilidad de la aplicación, consulte <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-148">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fc2b0-149">La compilación en tiempo de ejecución se habilita mediante el paquete `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation`.</span><span class="sxs-lookup"><span data-stu-id="fc2b0-149">Runtime compilation is enabled using the `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` package.</span></span> <span data-ttu-id="fc2b0-150">Para habilitar la compilación en tiempo de ejecución, las aplicaciones deben:</span><span class="sxs-lookup"><span data-stu-id="fc2b0-150">To enable runtime compilation, apps must:</span></span>

* <span data-ttu-id="fc2b0-151">Instalar el paquete NuGet [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/).</span><span class="sxs-lookup"><span data-stu-id="fc2b0-151">Install the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet package.</span></span>
* <span data-ttu-id="fc2b0-152">Actualice el método `Startup.ConfigureServices` del proyecto para incluir una llamada a `AddRazorRuntimeCompilation`:</span><span class="sxs-lookup"><span data-stu-id="fc2b0-152">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`:</span></span>

  ```csharp
  services
      .AddControllersWithViews()
      .AddRazorRuntimeCompilation();
  ```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="fc2b0-153">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="fc2b0-153">Additional resources</span></span>

::: moniker range="= aspnetcore-1.1"

* <xref:mvc/views/overview>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
