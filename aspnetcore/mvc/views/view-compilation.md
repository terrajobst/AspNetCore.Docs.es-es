---
title: Compilación y precompilación de archivos de Razor en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre las ventajas de precompilar archivos Razor y cómo lograr la precompilación de estos archivos en una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: c4e8f722fdf3d3f64807cc35ff9f349af7f32abd
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248191"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="f37a5-103">Compilación de archivos de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f37a5-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="f37a5-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f37a5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="f37a5-105">Un archivo de Razor se compila en tiempo de ejecución, cuando se invoca la vista MVC asociada.</span><span class="sxs-lookup"><span data-stu-id="f37a5-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="f37a5-106">No se admite la publicación de archivos de Razor en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="f37a5-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="f37a5-107">Opcionalmente, los archivos de Razor se pueden compilar en el momento de la publicación e implementar con la aplicación, mediante la herramienta de precompilación.</span><span class="sxs-lookup"><span data-stu-id="f37a5-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f37a5-108">Un archivo de Razor se compila en tiempo de ejecución, cuando se invoca la vista MVC o la página de Razor asociada.</span><span class="sxs-lookup"><span data-stu-id="f37a5-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="f37a5-109">No se admite la publicación de archivos de Razor en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="f37a5-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="f37a5-110">Opcionalmente, los archivos de Razor se pueden compilar en el momento de la publicación e implementar con la aplicación, mediante la herramienta de precompilación.</span><span class="sxs-lookup"><span data-stu-id="f37a5-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f37a5-111">Un archivo de Razor se compila en tiempo de ejecución, cuando se invoca la vista MVC o la página de Razor asociada.</span><span class="sxs-lookup"><span data-stu-id="f37a5-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="f37a5-112">Los archivos de Razor se compilan en tiempo de compilación y publicación mediante el [SDK de Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="f37a5-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="f37a5-113">Consideraciones de precompilación</span><span class="sxs-lookup"><span data-stu-id="f37a5-113">Precompilation considerations</span></span>

<span data-ttu-id="f37a5-114">Los siguientes son los efectos secundarios de la precompilación de los archivos de Razor:</span><span class="sxs-lookup"><span data-stu-id="f37a5-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="f37a5-115">Un paquete publicado más pequeño.</span><span class="sxs-lookup"><span data-stu-id="f37a5-115">A smaller published bundle</span></span>
* <span data-ttu-id="f37a5-116">Un menor tiempo de inicio.</span><span class="sxs-lookup"><span data-stu-id="f37a5-116">A faster startup time</span></span>
* <span data-ttu-id="f37a5-117">Los archivos de Razor no se pueden editar; el contenido asociado no está presente en el paquete publicado.</span><span class="sxs-lookup"><span data-stu-id="f37a5-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="f37a5-118">Implementar archivos precompilados</span><span class="sxs-lookup"><span data-stu-id="f37a5-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f37a5-119">La compilación de los archivos de Razor en tiempo de publicación y compilación está habilitada de manera predeterminada en el SDK de Razor.</span><span class="sxs-lookup"><span data-stu-id="f37a5-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="f37a5-120">La edición de los archivos de Razor después de que se actualicen se admite en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="f37a5-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="f37a5-121">De forma predeterminada, solo se implementa con la aplicación el archivo *Views.dll* compilado y ningún archivo *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f37a5-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f37a5-122">La herramienta de precompilación se eliminará en ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="f37a5-122">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="f37a5-123">Se recomienda migrar al [SDK de Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="f37a5-123">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="f37a5-124">El SDK de Razor solo es efectivo cuando no hay propiedades específicas de la precompilación establecidas en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="f37a5-124">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="f37a5-125">Por ejemplo, establecer la propiedad `MvcRazorCompileOnPublish` del archivo *.csproj* en `true` deshabilita el SDK de Razor.</span><span class="sxs-lookup"><span data-stu-id="f37a5-125">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f37a5-126">Si el proyecto tiene como destino .NET Framework, instale el paquete NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="f37a5-126">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="f37a5-127">Si el proyecto tiene como destino .NET Core, no es necesario hacer cambios.</span><span class="sxs-lookup"><span data-stu-id="f37a5-127">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="f37a5-128">Las plantillas de proyecto de ASP.NET Core 2.x establecen implícitamente la propiedad `MvcRazorCompileOnPublish` en `true` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f37a5-128">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="f37a5-129">Por tanto, este elemento se puede quitar de manera segura del archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="f37a5-129">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f37a5-130">La herramienta de precompilación se eliminará en ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="f37a5-130">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="f37a5-131">Se recomienda migrar al [SDK de Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="f37a5-131">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="f37a5-132">La precompilación de los archivos de Razor no está disponible cuando se realiza una [implementación independiente (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="f37a5-132">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="f37a5-133">Establezca la propiedad `MvcRazorCompileOnPublish` en `true` e instale el paquete NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/).</span><span class="sxs-lookup"><span data-stu-id="f37a5-133">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="f37a5-134">El siguiente ejemplo de *.csproj* resalta estas opciones:</span><span class="sxs-lookup"><span data-stu-id="f37a5-134">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="f37a5-135">Prepare la aplicación para una [implementación independiente de la plataforma](/dotnet/core/deploying/#framework-dependent-deployments-fdd) con el [comando de publicación de la CLI de .NET Core](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="f37a5-135">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="f37a5-136">Por ejemplo, ejecute el siguiente comando en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="f37a5-136">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="f37a5-137">Se crea un archivo *<Nombre_proyecto>.PrecompiledViews.dll*, que contiene los archivos de Razor compilados, cuando la precompilación se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="f37a5-137">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="f37a5-138">Por ejemplo, en la captura de pantalla siguiente se muestra el contenido de *Index.cshtml* en *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="f37a5-138">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Vistas de Razor dentro de DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="recompile-razor-files-on-change"></a><span data-ttu-id="f37a5-140">Recompilación de archivos de Razor al cambiar</span><span class="sxs-lookup"><span data-stu-id="f37a5-140">Recompile Razor files on change</span></span>

<span data-ttu-id="f37a5-141"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> obtiene o establece un valor que determina si los archivos de Razor (vistas y Razor Pages) se vuelven a compilar y actualizar si cambian los archivos en el disco.</span><span class="sxs-lookup"><span data-stu-id="f37a5-141">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="f37a5-142">Cuando se establece en `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) observa los cambios realizados en los archivos de Razor en las instancias de <xref:Microsoft.Extensions.FileProviders.IFileProvider> configuradas.</span><span class="sxs-lookup"><span data-stu-id="f37a5-142">When set to `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) watches for changes to Razor files in configured <xref:Microsoft.Extensions.FileProviders.IFileProvider> instances.</span></span>

<span data-ttu-id="f37a5-143">El valor predeterminado es `true` para:</span><span class="sxs-lookup"><span data-stu-id="f37a5-143">The default value is `true` for:</span></span>

* <span data-ttu-id="f37a5-144">Aplicaciones ASP.NET Core 2.1 o versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="f37a5-144">ASP.NET Core 2.1 or earlier apps.</span></span>
* <span data-ttu-id="f37a5-145">Aplicaciones ASP.NET Core 2.2 o versiones posteriores en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="f37a5-145">ASP.NET Core 2.2 or later apps in the Development environment.</span></span>

<span data-ttu-id="f37a5-146"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> se asocia con un modificador de compatibilidad y puede proporcionar un comportamiento diferente según la versión de compatibilidad configurada para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f37a5-146"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is associated with a compatibility switch and can provide different behavior depending on the configured compatibility version for the app.</span></span> <span data-ttu-id="f37a5-147">La configuración de la aplicación estableciendo <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> tiene prioridad sobre el valor implícito en la versión de compatibilidad de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f37a5-147">Configuring the app by setting <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> takes precedence over the value implied by the app's compatibility version.</span></span>

<span data-ttu-id="f37a5-148">Si se establece la versión de compatibilidad de la aplicación en <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> o versiones anteriores, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> se establece en `true`, a menos que se haya configurado explícitamente.</span><span class="sxs-lookup"><span data-stu-id="f37a5-148">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is set to `true` unless explicitly configured.</span></span>

<span data-ttu-id="f37a5-149">Si se establece la versión de compatibilidad de la aplicación en <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> o versiones posteriores, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> se establece en `false`, a menos que el entorno sea el de desarrollo o el valor se configure explícitamente.</span><span class="sxs-lookup"><span data-stu-id="f37a5-149">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> or later, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is set to `false` unless the environment is Development or the value is explicitly configured.</span></span>

<span data-ttu-id="f37a5-150">Para obtener instrucciones y ejemplos de configuración de la versión de compatibilidad de la aplicación, consulte <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="f37a5-150">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f37a5-151">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f37a5-151">Additional resources</span></span>

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
