---
title: SDK de Razor de ASP.NET Core
author: Rick-Anderson
description: Obtenga información sobre cómo las páginas de Razor de ASP.NET Core facilitan la programación de escenarios centrados en páginas y hacen que resulte más productiva que con MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/18/2019
uid: razor-pages/sdk
ms.openlocfilehash: fa69e4840377e0c1c8291c7ba9305a27bd3e6b82
ms.sourcegitcommit: 516f166c5f7cec54edf3d9c71e6e2ba53fb3b0e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67196364"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="f3872-103">SDK de Razor de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f3872-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="f3872-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f3872-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="f3872-105">Información general</span><span class="sxs-lookup"><span data-stu-id="f3872-105">Overview</span></span>

<span data-ttu-id="f3872-106">El [!INCLUDE[](~/includes/2.1-SDK.md)] incluye la `Microsoft.NET.Sdk.Razor` MSBuild SDK (SDK de Razor).</span><span class="sxs-lookup"><span data-stu-id="f3872-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="f3872-107">El SDK de Razor:</span><span class="sxs-lookup"><span data-stu-id="f3872-107">The Razor SDK:</span></span>

* <span data-ttu-id="f3872-108">Normaliza la experiencia de creación, empaquetado y publicación de proyectos que contienen archivos de [Razor](xref:mvc/views/razor) relativos a proyectos basados en ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="f3872-108">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="f3872-109">Incluye un conjunto de destinos, propiedades y elementos predefinidos que permiten personalizar la compilación de archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="f3872-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="f3872-110">El SDK de Razor incluye un `<Content>` elemento con un `Include` atributo establecido en el `**\*.cshtml` patrón global.</span><span class="sxs-lookup"><span data-stu-id="f3872-110">The Razor SDK includes a `<Content>` element with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="f3872-111">Se publican los archivos coincidentes.</span><span class="sxs-lookup"><span data-stu-id="f3872-111">Matching files are published.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f3872-112">El SDK de Razor incluye `<Content>` elementos con `Include` atributos establecidos en el `**\*.cshtml` y `**\*.razor` patrones de comodines.</span><span class="sxs-lookup"><span data-stu-id="f3872-112">The Razor SDK includes `<Content>` elements with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="f3872-113">Se publican los archivos coincidentes.</span><span class="sxs-lookup"><span data-stu-id="f3872-113">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="f3872-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f3872-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="f3872-115">Usar el SDK de Razor</span><span class="sxs-lookup"><span data-stu-id="f3872-115">Use the Razor SDK</span></span>

<span data-ttu-id="f3872-116">La mayoría de las aplicaciones web no están necesario hacer referencia explícitamente el SDK de Razor.</span><span class="sxs-lookup"><span data-stu-id="f3872-116">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

<span data-ttu-id="f3872-117">Para usar el SDK de Razor para crear bibliotecas de clases que contengan vistas de Razor o páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="f3872-117">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="f3872-118">Use `Microsoft.NET.Sdk.Razor` en lugar de `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="f3872-118">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="f3872-119">Normalmente, una referencia de paquete para `Microsoft.AspNetCore.Mvc` se requiere para recibir las dependencias adicionales que son necesarios para generar y compilar las páginas de Razor y las vistas de Razor.</span><span class="sxs-lookup"><span data-stu-id="f3872-119">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="f3872-120">Como mínimo, el proyecto debe agregar referencias de paquete para:</span><span class="sxs-lookup"><span data-stu-id="f3872-120">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="f3872-121">El `Microsoft.AspNetCore.Razor.Design` paquete proporciona las tareas de compilación de Razor y destinos para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f3872-121">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="f3872-122">Los paquetes anteriores están incluidos en `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="f3872-122">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="f3872-123">El marcado siguiente muestra un archivo de proyecto que usa el SDK de Razor para generar archivos de Razor para una aplicación de páginas de Razor de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f3872-123">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="f3872-124">El `Microsoft.AspNetCore.Razor.Design` y `Microsoft.AspNetCore.Mvc.Razor.Extensions` paquetes se incluyen en el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="f3872-124">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="f3872-125">Sin embargo, la versión de menor `Microsoft.AspNetCore.App` referencia de paquete proporciona un metapaquete a la aplicación que no incluye la versión más reciente de `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="f3872-125">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="f3872-126">Deben hacer referencia a una versión coherente de los proyectos `Microsoft.AspNetCore.Razor.Design` (o `Microsoft.AspNetCore.Mvc`) para que se incluyen las correcciones más recientes de tiempo de compilación para Razor.</span><span class="sxs-lookup"><span data-stu-id="f3872-126">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="f3872-127">Para obtener más información, consulte [este problema de GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="f3872-127">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="f3872-128">Propiedades</span><span class="sxs-lookup"><span data-stu-id="f3872-128">Properties</span></span>

<span data-ttu-id="f3872-129">Las siguientes propiedades controlan el comportamiento del SDK de Razor como parte de la generación de un proyecto:</span><span class="sxs-lookup"><span data-stu-id="f3872-129">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="f3872-130">`RazorCompileOnBuild` &ndash; Cuando `true`, compila y se emite el ensamblado de Razor como parte de la creación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f3872-130">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="f3872-131">Tiene como valor predeterminado `true`.</span><span class="sxs-lookup"><span data-stu-id="f3872-131">Defaults to `true`.</span></span>
* <span data-ttu-id="f3872-132">`RazorCompileOnPublish` &ndash; Cuando `true`, compila y se emite el ensamblado de Razor como parte de la publicación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f3872-132">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="f3872-133">Tiene como valor predeterminado `true`.</span><span class="sxs-lookup"><span data-stu-id="f3872-133">Defaults to `true`.</span></span>

<span data-ttu-id="f3872-134">Las propiedades y elementos en la tabla siguiente se utilizan para configurar las entradas y salida para el SDK de Razor.</span><span class="sxs-lookup"><span data-stu-id="f3872-134">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

| <span data-ttu-id="f3872-135">Elementos</span><span class="sxs-lookup"><span data-stu-id="f3872-135">Items</span></span> | <span data-ttu-id="f3872-136">Descripción</span><span class="sxs-lookup"><span data-stu-id="f3872-136">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="f3872-137">Elementos (archivos *.cshtml*) que son entradas para los destinos de generación de código.</span><span class="sxs-lookup"><span data-stu-id="f3872-137">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| `RazorCompile` | <span data-ttu-id="f3872-138">Elementos de elemento ( *.cs* archivos) que son entradas para los destinos de compilación de Razor.</span><span class="sxs-lookup"><span data-stu-id="f3872-138">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="f3872-139">Use esta `ItemGroup` para especificar los archivos adicionales que se compilarán en el ensamblado de Razor.</span><span class="sxs-lookup"><span data-stu-id="f3872-139">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="f3872-140">Elementos que se usan para generar atributos de código para el ensamblado de Razor.</span><span class="sxs-lookup"><span data-stu-id="f3872-140">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="f3872-141">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f3872-141">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="f3872-142">Elementos que se agregan como recursos incrustados en el ensamblado generado de Razor.</span><span class="sxs-lookup"><span data-stu-id="f3872-142">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="f3872-143">Property</span><span class="sxs-lookup"><span data-stu-id="f3872-143">Property</span></span> | <span data-ttu-id="f3872-144">Descripción</span><span class="sxs-lookup"><span data-stu-id="f3872-144">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="f3872-145">Nombre de archivo (sin extensión) del ensamblado generado por Razor.</span><span class="sxs-lookup"><span data-stu-id="f3872-145">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="f3872-146">Directorio de salida de Razor.</span><span class="sxs-lookup"><span data-stu-id="f3872-146">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="f3872-147">Sirve para determinar el conjunto de herramientas que se va a usar para compilar el ensamblado de Razor.</span><span class="sxs-lookup"><span data-stu-id="f3872-147">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="f3872-148">Valores válidos son `Implicit`, `RazorSDK` y `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="f3872-148">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="f3872-149">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="f3872-149">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="f3872-150">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="f3872-150">Default is `true`.</span></span> <span data-ttu-id="f3872-151">Cuando `true`, incluye *web.config*, *.json*, y *.cshtml* archivos como contenido en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f3872-151">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="f3872-152">Cuando se hace referencia a través de `Microsoft.NET.Sdk.Web`, los archivos en *wwwroot* y también se incluyen los archivos de configuración.</span><span class="sxs-lookup"><span data-stu-id="f3872-152">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="f3872-153">Si es `true`, incluye los archivos *.cshtml* de los elementos `Content` en elementos `RazorGenerate`.</span><span class="sxs-lookup"><span data-stu-id="f3872-153">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="f3872-154">Cuando `true`, genera un *.cs* archivo que contiene los atributos especificados por `RazorAssemblyAttribute` e incluye el archivo en la salida de compilación.</span><span class="sxs-lookup"><span data-stu-id="f3872-154">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="f3872-155">Si es `true`, agrega a `RazorAssemblyAttribute` un conjunto predeterminado de atributos de ensamblado.</span><span class="sxs-lookup"><span data-stu-id="f3872-155">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="f3872-156">Cuando `true`, copias `RazorGenerate` elementos ( *.cshtml*) archivos en el directorio de publicación.</span><span class="sxs-lookup"><span data-stu-id="f3872-156">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="f3872-157">Normalmente, los archivos de Razor no son necesarios para una aplicación publicada si participa en la compilación en tiempo de compilación o tiempo de publicación.</span><span class="sxs-lookup"><span data-stu-id="f3872-157">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="f3872-158">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="f3872-158">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="f3872-159">Si es `true`, copia los elementos de referencia del ensamblado en el directorio de publicación.</span><span class="sxs-lookup"><span data-stu-id="f3872-159">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="f3872-160">Normalmente, los ensamblados de referencia no son necesarios para una aplicación publicada si la compilación de Razor se produce en tiempo de compilación o tiempo de publicación.</span><span class="sxs-lookup"><span data-stu-id="f3872-160">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="f3872-161">Establecido en `true` si la aplicación publicada requiere compilación en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="f3872-161">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="f3872-162">Por ejemplo, establezca el valor en `true` si modifica la aplicación *.cshtml* archivos en tiempo de ejecución o usa vistas incrustadas.</span><span class="sxs-lookup"><span data-stu-id="f3872-162">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="f3872-163">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="f3872-163">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="f3872-164">Cuando `true`, todos los elementos de contenido de Razor ( *.cshtml* archivos) se marcan para su inclusión en el paquete NuGet generado.</span><span class="sxs-lookup"><span data-stu-id="f3872-164">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="f3872-165">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="f3872-165">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="f3872-166">Si es `true`, agrega los elementos de RazorGenerate ( *.cshtml*) como archivos incrustados al ensamblado de Razor generado.</span><span class="sxs-lookup"><span data-stu-id="f3872-166">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="f3872-167">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="f3872-167">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="f3872-168">Si es `true`, usa un proceso de servidor de compilación persistente para descargar el trabajo de generación de código.</span><span class="sxs-lookup"><span data-stu-id="f3872-168">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="f3872-169">El valor predeterminado es `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="f3872-169">Defaults to the value of `UseSharedCompilation`.</span></span> |

<span data-ttu-id="f3872-170">Para más información sobre las propiedades, consulte [Propiedades de MSBuild](/visualstudio/msbuild/msbuild-properties).</span><span class="sxs-lookup"><span data-stu-id="f3872-170">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="f3872-171">Destinos</span><span class="sxs-lookup"><span data-stu-id="f3872-171">Targets</span></span>

<span data-ttu-id="f3872-172">El SDK de Razor establece dos objetivos principales:</span><span class="sxs-lookup"><span data-stu-id="f3872-172">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="f3872-173">`RazorGenerate` &ndash; Genera código *.cs* archivos desde `RazorGenerate` elementos de elemento.</span><span class="sxs-lookup"><span data-stu-id="f3872-173">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="f3872-174">Use la propiedad `RazorGenerateDependsOn` para especificar más destinos que puedan ejecutarse antes o después de este destino.</span><span class="sxs-lookup"><span data-stu-id="f3872-174">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="f3872-175">`RazorCompile` &ndash; Compila genera *.cs* archivos en un ensamblado de Razor.</span><span class="sxs-lookup"><span data-stu-id="f3872-175">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="f3872-176">Use `RazorCompileDependsOn` para especificar más destinos que puedan ejecutarse antes o después de este destino.</span><span class="sxs-lookup"><span data-stu-id="f3872-176">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="f3872-177">Compilación en tiempo de ejecución de vistas de Razor</span><span class="sxs-lookup"><span data-stu-id="f3872-177">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="f3872-178">El SDK de Razor no publica de forma predeterminada los ensamblados de referencia necesarios para realizar una compilación en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="f3872-178">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="f3872-179">Como consecuencia, se producen errores de compilación cuando el modelo de aplicación se basa en la compilación en tiempo de ejecución; por ejemplo, la aplicación usa vistas incrustadas o cambia vistas tras haber sido publicada.</span><span class="sxs-lookup"><span data-stu-id="f3872-179">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="f3872-180">Establezca `CopyRefAssembliesToPublishDirectory` en `true` para seguir publicando ensamblados de referencia.</span><span class="sxs-lookup"><span data-stu-id="f3872-180">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="f3872-181">Para una aplicación web, asegúrese de que su aplicación tiene como destino el `Microsoft.NET.Sdk.Web` SDK.</span><span class="sxs-lookup"><span data-stu-id="f3872-181">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="f3872-182">Versión de lenguaje Razor</span><span class="sxs-lookup"><span data-stu-id="f3872-182">Razor language version</span></span>

<span data-ttu-id="f3872-183">Cuando el destino es el `Microsoft.NET.Sdk.Web` SDK, la versión de lenguaje de Razor se deduce de la versión de framework de destino de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f3872-183">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the the app's target framework version.</span></span> <span data-ttu-id="f3872-184">Para los proyectos destinados a la `Microsoft.NET.Sdk.Razor` SDK o en el caso excepcional de que la aplicación requiere una versión de lenguaje Razor diferente que el valor deducido, se puede configurar una versión estableciendo el `<RazorLangVersion>` propiedad en el archivo de proyecto de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="f3872-184">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

## <a name="additional-resources"></a><span data-ttu-id="f3872-185">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f3872-185">Additional resources</span></span>

* [<span data-ttu-id="f3872-186">Adiciones al formato csproj para .NET Core</span><span class="sxs-lookup"><span data-stu-id="f3872-186">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="f3872-187">Elementos comunes de proyectos de MSBuild</span><span class="sxs-lookup"><span data-stu-id="f3872-187">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
