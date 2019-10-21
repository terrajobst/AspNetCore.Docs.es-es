---
title: SDK de Razor de ASP.NET Core
author: Rick-Anderson
description: Obtenga información sobre cómo las páginas de Razor de ASP.NET Core facilitan la programación de escenarios centrados en páginas y hacen que resulte más productiva que con MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/23/2019
uid: razor-pages/sdk
ms.openlocfilehash: 606d2bdca3fa4fb1c81df73ac697d2175c3ab633
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334033"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="77890-103">SDK de Razor de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77890-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="77890-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="77890-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="77890-105">Información general</span><span class="sxs-lookup"><span data-stu-id="77890-105">Overview</span></span>

<span data-ttu-id="77890-106">El [!INCLUDE[](~/includes/2.1-SDK.md)] incluye el SDK de MSBuild `Microsoft.NET.Sdk.Razor` (SDK de Razor).</span><span class="sxs-lookup"><span data-stu-id="77890-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="77890-107">El SDK de Razor:</span><span class="sxs-lookup"><span data-stu-id="77890-107">The Razor SDK:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="77890-108">Se requiere para compilar, empaquetar y publicar proyectos que contengan archivos [Razor](xref:mvc/views/razor) para proyectos de ASP.net Core basado en MVC o [increíbles](xref:blazor/index) .</span><span class="sxs-lookup"><span data-stu-id="77890-108">Is required to build, package, and publish projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based or [Blazor](xref:blazor/index) projects.</span></span>
* <span data-ttu-id="77890-109">Incluye un conjunto de destinos, propiedades y elementos predefinidos que permiten personalizar la compilación de archivos Razor ( *. cshtml* o *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="77890-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor (*.cshtml* or *.razor*) files.</span></span>

<span data-ttu-id="77890-110">El SDK de Razor incluye `Content` elementos con `Include` atributos establecidos en los patrones `**\*.cshtml` y `**\*.razor` comodines.</span><span class="sxs-lookup"><span data-stu-id="77890-110">The Razor SDK includes `Content` items with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="77890-111">Se publican los archivos coincidentes.</span><span class="sxs-lookup"><span data-stu-id="77890-111">Matching files are published.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="77890-112">Normaliza la experiencia de creación, empaquetado y publicación de proyectos que contienen archivos de [Razor](xref:mvc/views/razor) relativos a proyectos basados en ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="77890-112">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="77890-113">Incluye un conjunto de destinos, propiedades y elementos predefinidos que permiten personalizar la compilación de archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="77890-113">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

<span data-ttu-id="77890-114">El SDK de Razor incluye un elemento `Content` con un atributo `Include` establecido en el patrón `**\*.cshtml` comodines.</span><span class="sxs-lookup"><span data-stu-id="77890-114">The Razor SDK includes a `Content` item with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="77890-115">Se publican los archivos coincidentes.</span><span class="sxs-lookup"><span data-stu-id="77890-115">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="77890-116">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="77890-116">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="77890-117">Usar el SDK de Razor</span><span class="sxs-lookup"><span data-stu-id="77890-117">Use the Razor SDK</span></span>

<span data-ttu-id="77890-118">No es necesario que la mayoría de las aplicaciones web hagan referencia explícitamente al SDK de Razor.</span><span class="sxs-lookup"><span data-stu-id="77890-118">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="77890-119">Para usar el SDK de Razor para compilar bibliotecas de clases que contengan vistas de Razor o Razor Pages, se recomienda comenzar con la plantilla de proyecto de la biblioteca de clases de Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="77890-119">To use the Razor SDK to build class libraries containing Razor views or Razor Pages, we recommend starting with the Razor class library (RCL) project template.</span></span> <span data-ttu-id="77890-120">Un RCL que se usa para compilar los archivos ( *. Razor*) mínimos requiere una referencia al paquete [Microsoft. AspNetCore. Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) .</span><span class="sxs-lookup"><span data-stu-id="77890-120">An RCL that's used to build Blazor (*.razor*) files minimally requires a reference to the [Microsoft.AspNetCore.Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) package.</span></span> <span data-ttu-id="77890-121">Un RCL que se usa para compilar vistas o páginas de Razor (archivos *. cshtml* ) requiere mínimamente el destino `netcoreapp3.0` o posterior y tiene un `FrameworkReference` al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) en su archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="77890-121">An RCL that's used to build Razor views or pages (*.cshtml* files) minimally requires targeting `netcoreapp3.0` or later and has a `FrameworkReference` to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in its project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="77890-122">Para usar el SDK de Razor para crear bibliotecas de clases que contengan vistas de Razor o páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="77890-122">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="77890-123">Use `Microsoft.NET.Sdk.Razor` en lugar de `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="77890-123">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="77890-124">Normalmente, se requiere una referencia de paquete a `Microsoft.AspNetCore.Mvc` para recibir dependencias adicionales necesarias para compilar y compilar vistas de Razor Pages y de Razor.</span><span class="sxs-lookup"><span data-stu-id="77890-124">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="77890-125">Como mínimo, el proyecto debe agregar referencias de paquete a:</span><span class="sxs-lookup"><span data-stu-id="77890-125">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="77890-126">El paquete de `Microsoft.AspNetCore.Razor.Design` proporciona las tareas y destinos de compilación de Razor para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="77890-126">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="77890-127">Los paquetes anteriores están incluidos en `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="77890-127">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="77890-128">En el marcado siguiente se muestra un archivo de proyecto que utiliza el SDK de Razor para compilar archivos Razor para una aplicación ASP.NET Core Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="77890-128">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]
  
::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="77890-129">Los paquetes de `Microsoft.AspNetCore.Razor.Design` y `Microsoft.AspNetCore.Mvc.Razor.Extensions` se incluyen en el [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="77890-129">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="77890-130">No obstante, la referencia de paquete de `Microsoft.AspNetCore.App` sin versión proporciona un metapaquete a la aplicación que no incluye la versión más reciente de `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="77890-130">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="77890-131">Los proyectos deben hacer referencia a una versión coherente de `Microsoft.AspNetCore.Razor.Design` (o `Microsoft.AspNetCore.Mvc`), de modo que se incluyan las últimas correcciones de tiempo de compilación de Razor.</span><span class="sxs-lookup"><span data-stu-id="77890-131">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="77890-132">Para más información, consulte [este problema de GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="77890-132">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="77890-133">Propiedades</span><span class="sxs-lookup"><span data-stu-id="77890-133">Properties</span></span>

<span data-ttu-id="77890-134">Las siguientes propiedades controlan el comportamiento del SDK de Razor como parte de la generación de un proyecto:</span><span class="sxs-lookup"><span data-stu-id="77890-134">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="77890-135">`RazorCompileOnBuild` &ndash; cuando `true`, compila y emite el ensamblado Razor como parte de la compilación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="77890-135">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="77890-136">Tiene como valor predeterminado `true`.</span><span class="sxs-lookup"><span data-stu-id="77890-136">Defaults to `true`.</span></span>
* <span data-ttu-id="77890-137">`RazorCompileOnPublish` &ndash; cuando `true`, compila y emite el ensamblado Razor como parte de la publicación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="77890-137">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="77890-138">Tiene como valor predeterminado `true`.</span><span class="sxs-lookup"><span data-stu-id="77890-138">Defaults to `true`.</span></span>

<span data-ttu-id="77890-139">Las propiedades y los elementos de la tabla siguiente se usan para configurar entradas y salidas en el SDK de Razor.</span><span class="sxs-lookup"><span data-stu-id="77890-139">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="77890-140">A partir de ASP.NET Core 3,0, las vistas o Razor Pages de MVC no se atienden de forma predeterminada si las propiedades de MSBuild `RazorCompileOnBuild` o `RazorCompileOnPublish` del archivo de proyecto están deshabilitadas.</span><span class="sxs-lookup"><span data-stu-id="77890-140">Starting with ASP.NET Core 3.0, MVC Views or Razor Pages aren't served by default if the `RazorCompileOnBuild` or `RazorCompileOnPublish` MSBuild properties in the project file are disabled.</span></span> <span data-ttu-id="77890-141">Las aplicaciones deben agregar una referencia explícita al paquete [Microsoft. AspNetCore. Mvc. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) si la aplicación se basa en la compilación en tiempo de ejecución para procesar archivos *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="77890-141">Applications must add an explicit reference to the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) package if the app relies on runtime compilation to process *.cshtml* files.</span></span>

::: moniker-end

| <span data-ttu-id="77890-142">Elementos</span><span class="sxs-lookup"><span data-stu-id="77890-142">Items</span></span> | <span data-ttu-id="77890-143">Descripción</span><span class="sxs-lookup"><span data-stu-id="77890-143">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="77890-144">Elementos Item (archivos *. cshtml* ) que son entradas para la generación de código.</span><span class="sxs-lookup"><span data-stu-id="77890-144">Item elements (*.cshtml* files) that are inputs to code generation.</span></span> |
| `RazorComponent` | <span data-ttu-id="77890-145">Elementos Item (archivos *. Razor* ) que son entradas para la generación de código de componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="77890-145">Item elements (*.razor* files) that are inputs to Razor component code generation.</span></span> |
| `RazorCompile` | <span data-ttu-id="77890-146">Elementos de elemento (archivos *. CS* ) que son entradas para los destinos de compilación de Razor.</span><span class="sxs-lookup"><span data-stu-id="77890-146">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="77890-147">Use este `ItemGroup` para especificar archivos adicionales que se van a compilar en el ensamblado de Razor.</span><span class="sxs-lookup"><span data-stu-id="77890-147">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="77890-148">Elementos que se usan para generar atributos de código para el ensamblado de Razor.</span><span class="sxs-lookup"><span data-stu-id="77890-148">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="77890-149">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="77890-149">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="77890-150">Elementos de elemento que se agregan como recursos incrustados al ensamblado de Razor generado.</span><span class="sxs-lookup"><span data-stu-id="77890-150">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="77890-151">Propiedad.</span><span class="sxs-lookup"><span data-stu-id="77890-151">Property</span></span> | <span data-ttu-id="77890-152">Descripción</span><span class="sxs-lookup"><span data-stu-id="77890-152">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="77890-153">Nombre de archivo (sin extensión) del ensamblado generado por Razor.</span><span class="sxs-lookup"><span data-stu-id="77890-153">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="77890-154">Directorio de salida de Razor.</span><span class="sxs-lookup"><span data-stu-id="77890-154">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="77890-155">Sirve para determinar el conjunto de herramientas que se va a usar para compilar el ensamblado de Razor.</span><span class="sxs-lookup"><span data-stu-id="77890-155">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="77890-156">Valores válidos son `Implicit`, `RazorSDK` y `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="77890-156">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="77890-157">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="77890-157">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="77890-158">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="77890-158">Default is `true`.</span></span> <span data-ttu-id="77890-159">Cuando `true`, incluye los archivos *Web. config*, *. JSON*y *. cshtml* como contenido en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="77890-159">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="77890-160">Cuando se hace referencia a través de `Microsoft.NET.Sdk.Web`, también se incluyen los archivos de los archivos *wwwroot* y config.</span><span class="sxs-lookup"><span data-stu-id="77890-160">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="77890-161">Si es `true`, incluye los archivos *.cshtml* de los elementos `Content` en elementos `RazorGenerate`.</span><span class="sxs-lookup"><span data-stu-id="77890-161">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="77890-162">Cuando `true`, genera un archivo *. CS* que contiene los atributos especificados por `RazorAssemblyAttribute` e incluye el archivo en la salida de compilación.</span><span class="sxs-lookup"><span data-stu-id="77890-162">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="77890-163">Si es `true`, agrega a `RazorAssemblyAttribute` un conjunto predeterminado de atributos de ensamblado.</span><span class="sxs-lookup"><span data-stu-id="77890-163">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="77890-164">Cuando `true`, copia archivos de `RazorGenerate` elementos ( *. cshtml*) en el directorio de publicación.</span><span class="sxs-lookup"><span data-stu-id="77890-164">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="77890-165">Normalmente, los archivos de Razor no son necesarios para una aplicación publicada si participan en la compilación en tiempo de compilación o en tiempo de publicación.</span><span class="sxs-lookup"><span data-stu-id="77890-165">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="77890-166">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="77890-166">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="77890-167">Si es `true`, copia los elementos de referencia del ensamblado en el directorio de publicación.</span><span class="sxs-lookup"><span data-stu-id="77890-167">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="77890-168">Normalmente, los ensamblados de referencia no son necesarios para una aplicación publicada si la compilación de Razor se produce en tiempo de compilación o en tiempo de publicación.</span><span class="sxs-lookup"><span data-stu-id="77890-168">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="77890-169">Establezca en `true` si la aplicación publicada requiere compilación en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="77890-169">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="77890-170">Por ejemplo, establezca el valor en `true` si la aplicación modifica los archivos *. cshtml* en tiempo de ejecución o utiliza vistas incrustadas.</span><span class="sxs-lookup"><span data-stu-id="77890-170">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="77890-171">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="77890-171">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="77890-172">Cuando se `true`, todos los elementos de contenido de Razor (archivos *. cshtml* ) se marcan para su inclusión en el paquete de NuGet generado.</span><span class="sxs-lookup"><span data-stu-id="77890-172">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="77890-173">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="77890-173">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="77890-174">Si es `true`, agrega los elementos de RazorGenerate ( *.cshtml*) como archivos incrustados al ensamblado de Razor generado.</span><span class="sxs-lookup"><span data-stu-id="77890-174">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="77890-175">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="77890-175">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="77890-176">Si es `true`, usa un proceso de servidor de compilación persistente para descargar el trabajo de generación de código.</span><span class="sxs-lookup"><span data-stu-id="77890-176">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="77890-177">El valor predeterminado es `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="77890-177">Defaults to the value of `UseSharedCompilation`.</span></span> |
| `GenerateMvcApplicationPartsAssemblyAttributes` | <span data-ttu-id="77890-178">Cuando `true`, el SDK genera atributos adicionales usados por MVC en tiempo de ejecución para realizar la detección de la parte de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="77890-178">When `true`, the SDK generates additional attributes used by MVC at runtime to perform application part discovery.</span></span> |

<span data-ttu-id="77890-179">Para más información sobre las propiedades, consulte [Propiedades de MSBuild](/visualstudio/msbuild/msbuild-properties).</span><span class="sxs-lookup"><span data-stu-id="77890-179">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="77890-180">Destinos</span><span class="sxs-lookup"><span data-stu-id="77890-180">Targets</span></span>

<span data-ttu-id="77890-181">El SDK de Razor establece dos objetivos principales:</span><span class="sxs-lookup"><span data-stu-id="77890-181">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="77890-182">`RazorGenerate` código de &ndash; genera archivos *. CS* a partir de elementos de `RazorGenerate` elemento.</span><span class="sxs-lookup"><span data-stu-id="77890-182">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="77890-183">Use la propiedad `RazorGenerateDependsOn` para especificar destinos adicionales que se pueden ejecutar antes o después de este destino.</span><span class="sxs-lookup"><span data-stu-id="77890-183">Use the `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="77890-184">`RazorCompile` &ndash; compila los archivos *. CS* generados en un ensamblado de Razor.</span><span class="sxs-lookup"><span data-stu-id="77890-184">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="77890-185">Use el `RazorCompileDependsOn` para especificar destinos adicionales que se pueden ejecutar antes o después de este destino.</span><span class="sxs-lookup"><span data-stu-id="77890-185">Use the `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="77890-186">`RazorComponentGenerate` código de &ndash; genera archivos *. CS* para elementos de `RazorComponent` elemento.</span><span class="sxs-lookup"><span data-stu-id="77890-186">`RazorComponentGenerate` &ndash; Code generates *.cs* files for `RazorComponent` item elements.</span></span> <span data-ttu-id="77890-187">Use la propiedad `RazorComponentGenerateDependsOn` para especificar destinos adicionales que se pueden ejecutar antes o después de este destino.</span><span class="sxs-lookup"><span data-stu-id="77890-187">Use the `RazorComponentGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="77890-188">Compilación en tiempo de ejecución de vistas de Razor</span><span class="sxs-lookup"><span data-stu-id="77890-188">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="77890-189">El SDK de Razor no publica de forma predeterminada los ensamblados de referencia necesarios para realizar una compilación en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="77890-189">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="77890-190">Como consecuencia, se producen errores de compilación cuando el modelo de aplicación se basa en la compilación en tiempo de ejecución; por ejemplo, la aplicación usa vistas incrustadas o cambia vistas tras haber sido publicada.</span><span class="sxs-lookup"><span data-stu-id="77890-190">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="77890-191">Establezca `CopyRefAssembliesToPublishDirectory` en `true` para seguir publicando ensamblados de referencia.</span><span class="sxs-lookup"><span data-stu-id="77890-191">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="77890-192">En el caso de una aplicación Web, asegúrese de que su aplicación tiene como destino el SDK de `Microsoft.NET.Sdk.Web`.</span><span class="sxs-lookup"><span data-stu-id="77890-192">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="77890-193">Versión de lenguaje Razor</span><span class="sxs-lookup"><span data-stu-id="77890-193">Razor language version</span></span>

<span data-ttu-id="77890-194">Cuando el destino es el SDK de `Microsoft.NET.Sdk.Web`, la versión de lenguaje Razor se deduce de la versión de .NET Framework de destino de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="77890-194">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the app's target framework version.</span></span> <span data-ttu-id="77890-195">Para los proyectos que tienen como destino el SDK de `Microsoft.NET.Sdk.Razor` o en el caso excepcional de que la aplicación requiera una versión de lenguaje Razor diferente que el valor deducido, se puede configurar una versión estableciendo la propiedad `<RazorLangVersion>` en el archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="77890-195">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

<span data-ttu-id="77890-196">La versión de lenguaje de Razor se integra estrechamente con la versión del Runtime para el que se compiló.</span><span class="sxs-lookup"><span data-stu-id="77890-196">Razor's language version is tightly integrated with the version of the runtime that it was built for.</span></span> <span data-ttu-id="77890-197">El destino de una versión de idioma que no está diseñada para el tiempo de ejecución no se admite y es probable que genere errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="77890-197">Targeting a language version that isn't designed for the runtime is unsupported and likely produces build errors.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="77890-198">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="77890-198">Additional resources</span></span>

* [<span data-ttu-id="77890-199">Adiciones al formato csproj para .NET Core</span><span class="sxs-lookup"><span data-stu-id="77890-199">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="77890-200">Elementos comunes de proyectos de MSBuild</span><span class="sxs-lookup"><span data-stu-id="77890-200">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
