---
title: Unión y minimización de recursos estáticos en ASP.NET Core
author: scottaddie
description: Obtenga información sobre cómo optimizar recursos estáticos en una aplicación web de ASP.NET Core aplicando técnicas de unión y minimización.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: a7a5c40d6c31c4416212c02c1b491dd794f2a1d3
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78646787"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="e24e3-103">Unión y minimización de recursos estáticos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e24e3-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="e24e3-104">Por [Scott Addie](https://twitter.com/Scott_Addie) y [David Pine](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="e24e3-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="e24e3-105">En este artículo se explican las ventajas de aplicar la unión y la minimización, así como la forma en que estas características se pueden utilizar con aplicaciones web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e24e3-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="e24e3-106">¿Qué son la unión y la minimización?</span><span class="sxs-lookup"><span data-stu-id="e24e3-106">What is bundling and minification</span></span>

<span data-ttu-id="e24e3-107">La unión y la minimización son dos optimizaciones de rendimiento distintas que se pueden aplicar en una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="e24e3-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="e24e3-108">Usadas de forma conjunta, la unión y la minimización mejoran el rendimiento al reducir el número de solicitudes de servidor y el tamaño de los recursos estáticos solicitados.</span><span class="sxs-lookup"><span data-stu-id="e24e3-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="e24e3-109">La unión y la minimización mejoran principalmente el tiempo de carga de la solicitud de la primera página.</span><span class="sxs-lookup"><span data-stu-id="e24e3-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="e24e3-110">Una vez solicitada una página web, el explorador almacena en caché los recursos estáticos (JavaScript, CSS e imágenes).</span><span class="sxs-lookup"><span data-stu-id="e24e3-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="e24e3-111">Por lo tanto, la unión y la minimización no mejoran el rendimiento cuando solicitan la misma página, o páginas, en el mismo sitio que solicita los mismos recursos.</span><span class="sxs-lookup"><span data-stu-id="e24e3-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="e24e3-112">Si el encabezado Expires no se establece correctamente en los recursos y no se usa la unión y la minimización, la heurística de actualización del explorador marca los recursos obsoletos pasados unos días.</span><span class="sxs-lookup"><span data-stu-id="e24e3-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="e24e3-113">Además, el explorador requiere una solicitud de validación para cada recurso.</span><span class="sxs-lookup"><span data-stu-id="e24e3-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="e24e3-114">En este caso, la unión y la minimización proporcionan una mejora del rendimiento incluso después de la solicitud de la primera página.</span><span class="sxs-lookup"><span data-stu-id="e24e3-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="e24e3-115">Unión</span><span class="sxs-lookup"><span data-stu-id="e24e3-115">Bundling</span></span>

<span data-ttu-id="e24e3-116">La unión combina varios archivos en un único archivo.</span><span class="sxs-lookup"><span data-stu-id="e24e3-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="e24e3-117">La unión reduce el número de solicitudes de servidor necesarias para representar un recurso web, como una página web.</span><span class="sxs-lookup"><span data-stu-id="e24e3-117">Bundling reduces the number of server requests that are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="e24e3-118">Puede crear cualquier número de conjuntos individuales específicamente para CSS, JavaScript, etc. Menos archivos significa menos solicitudes HTTP desde el explorador al servidor o desde el servicio que proporciona la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e24e3-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="e24e3-119">Esto tiene como resultado una mejora en el rendimiento de la carga de la primera página.</span><span class="sxs-lookup"><span data-stu-id="e24e3-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="e24e3-120">Minimización</span><span class="sxs-lookup"><span data-stu-id="e24e3-120">Minification</span></span>

<span data-ttu-id="e24e3-121">La minimización quita caracteres innecesarios del código sin modificar la funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="e24e3-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="e24e3-122">El resultado es una reducción significativa del tamaño de los recursos solicitados (tales como CSS, imágenes y archivos de JavaScript).</span><span class="sxs-lookup"><span data-stu-id="e24e3-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="e24e3-123">Los efectos secundarios habituales de la minimización incluyen acortar nombres de variable a un carácter y quitar comentarios y espacios en blanco innecesarios.</span><span class="sxs-lookup"><span data-stu-id="e24e3-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="e24e3-124">Considere la siguiente función de JavaScript:</span><span class="sxs-lookup"><span data-stu-id="e24e3-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="e24e3-125">La minimización reduce la función a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e24e3-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="e24e3-126">Además de quitar los comentarios y los espacios en blanco innecesarios, se ha cambiado el nombre de los siguientes parámetros y variables de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="e24e3-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="e24e3-127">Original</span><span class="sxs-lookup"><span data-stu-id="e24e3-127">Original</span></span> | <span data-ttu-id="e24e3-128">Se cambia el nombre</span><span class="sxs-lookup"><span data-stu-id="e24e3-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="e24e3-129">Impacto de la unión y la minimización</span><span class="sxs-lookup"><span data-stu-id="e24e3-129">Impact of bundling and minification</span></span>

<span data-ttu-id="e24e3-130">En la tabla siguiente se describen las diferencias entre la carga de recursos de forma individual y el uso de la unión y la minimización:</span><span class="sxs-lookup"><span data-stu-id="e24e3-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="e24e3-131">Acción</span><span class="sxs-lookup"><span data-stu-id="e24e3-131">Action</span></span> | <span data-ttu-id="e24e3-132">Con U/M</span><span class="sxs-lookup"><span data-stu-id="e24e3-132">With B/M</span></span> | <span data-ttu-id="e24e3-133">Sin U/M</span><span class="sxs-lookup"><span data-stu-id="e24e3-133">Without B/M</span></span> | <span data-ttu-id="e24e3-134">Cambio</span><span class="sxs-lookup"><span data-stu-id="e24e3-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="e24e3-135">Solicitudes de archivos</span><span class="sxs-lookup"><span data-stu-id="e24e3-135">File Requests</span></span>  | <span data-ttu-id="e24e3-136">7</span><span class="sxs-lookup"><span data-stu-id="e24e3-136">7</span></span>   | <span data-ttu-id="e24e3-137">18</span><span class="sxs-lookup"><span data-stu-id="e24e3-137">18</span></span>     | <span data-ttu-id="e24e3-138">157 %</span><span class="sxs-lookup"><span data-stu-id="e24e3-138">157%</span></span>
<span data-ttu-id="e24e3-139">KB transferidos</span><span class="sxs-lookup"><span data-stu-id="e24e3-139">KB Transferred</span></span> | <span data-ttu-id="e24e3-140">156</span><span class="sxs-lookup"><span data-stu-id="e24e3-140">156</span></span> | <span data-ttu-id="e24e3-141">264,68</span><span class="sxs-lookup"><span data-stu-id="e24e3-141">264.68</span></span> | <span data-ttu-id="e24e3-142">70%</span><span class="sxs-lookup"><span data-stu-id="e24e3-142">70%</span></span>
<span data-ttu-id="e24e3-143">Tiempo de carga (ms)</span><span class="sxs-lookup"><span data-stu-id="e24e3-143">Load Time (ms)</span></span> | <span data-ttu-id="e24e3-144">885</span><span class="sxs-lookup"><span data-stu-id="e24e3-144">885</span></span> | <span data-ttu-id="e24e3-145">2360</span><span class="sxs-lookup"><span data-stu-id="e24e3-145">2360</span></span>   | <span data-ttu-id="e24e3-146">167 %</span><span class="sxs-lookup"><span data-stu-id="e24e3-146">167%</span></span>

<span data-ttu-id="e24e3-147">Los exploradores son bastante detallados con respecto a los encabezados de solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="e24e3-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="e24e3-148">La métrica del número total de bytes enviados ha mostrado una reducción considerable al aplicar la unión.</span><span class="sxs-lookup"><span data-stu-id="e24e3-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="e24e3-149">El tiempo de carga muestra una mejora considerable, si bien este ejemplo se ha ejecutado localmente.</span><span class="sxs-lookup"><span data-stu-id="e24e3-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="e24e3-150">Se obtienen mayores mejoras de rendimiento al usar la unión y la minimización con recursos transferidos a través de una red.</span><span class="sxs-lookup"><span data-stu-id="e24e3-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="e24e3-151">Elección de una estrategia de unión y minimización</span><span class="sxs-lookup"><span data-stu-id="e24e3-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="e24e3-152">Las plantillas de proyecto de MVC y Razor Pages proporcionan una solución para la unión y la minimización lista para su uso que consta de un archivo de configuración JSON.</span><span class="sxs-lookup"><span data-stu-id="e24e3-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="e24e3-153">Las herramientas de terceros, como el ejecutor de tareas [Grunt](xref:client-side/using-grunt), realizan las mismas tareas con un poco más de complejidad.</span><span class="sxs-lookup"><span data-stu-id="e24e3-153">Third-party tools, such as the [Grunt](xref:client-side/using-grunt) task runner, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="e24e3-154">Una herramienta de terceros es una buena opción cuando el flujo de trabajo de desarrollo requiere un procesamiento más allá de la unión y la minimización, como la detección de errores y la optimización de imágenes.</span><span class="sxs-lookup"><span data-stu-id="e24e3-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="e24e3-155">Mediante el uso de la unión y la minimización en tiempo de diseño, los archivos minimizados se crean con anterioridad a la implementación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e24e3-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="e24e3-156">La unión y la minimización antes de la implementación proporcionan la ventaja de reducir la carga del servidor.</span><span class="sxs-lookup"><span data-stu-id="e24e3-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="e24e3-157">Sin embargo, es importante reconocer que la unión y la minimización en tiempo de diseño aumentan la complejidad de la compilación y solo funcionan con archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="e24e3-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="e24e3-158">Configuración de la unión y la minimización</span><span class="sxs-lookup"><span data-stu-id="e24e3-158">Configure bundling and minification</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="e24e3-159">En ASP.NET Core 2.0 o versiones anteriores, las plantillas de proyecto de MVC y Razor Pages proporcionan un archivo de configuración *bundleconfig.json* que define las opciones de cada conjunto:</span><span class="sxs-lookup"><span data-stu-id="e24e3-159">In ASP.NET Core 2.0 or earlier, the MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file that defines the options for each bundle:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e24e3-160">En ASP.NET Core 2.1 o versiones posteriores, agregue un archivo JSON, denominado *bundleconfig.json*, a la raíz del proyecto de MVC o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e24e3-160">In ASP.NET Core 2.1 or later, add a new JSON file, named *bundleconfig.json*, to the MVC or Razor Pages project root.</span></span> <span data-ttu-id="e24e3-161">Incluya el código JSON siguiente en ese archivo como punto inicial:</span><span class="sxs-lookup"><span data-stu-id="e24e3-161">Include the following JSON in that file as a starting point:</span></span>

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="e24e3-162">El archivo *bundleconfig.json* define las opciones de cada conjunto.</span><span class="sxs-lookup"><span data-stu-id="e24e3-162">The *bundleconfig.json* file defines the options for each bundle.</span></span> <span data-ttu-id="e24e3-163">En el ejemplo anterior, se define una configuración de conjunto única para los archivos personalizados de JavaScript (*wwwroot/JS/site.js*) y de hoja de estilo (*wwwroot/CSS/site.css*).</span><span class="sxs-lookup"><span data-stu-id="e24e3-163">In the preceding example, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files.</span></span>

<span data-ttu-id="e24e3-164">Las opciones de configuración incluyen lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e24e3-164">Configuration options include:</span></span>

* <span data-ttu-id="e24e3-165">`outputFileName`: nombre del archivo de conjunto que se va a generar.</span><span class="sxs-lookup"><span data-stu-id="e24e3-165">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="e24e3-166">Puede contener una ruta de acceso relativa del archivo *bundleconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="e24e3-166">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="e24e3-167">**requerido**</span><span class="sxs-lookup"><span data-stu-id="e24e3-167">**required**</span></span>
* <span data-ttu-id="e24e3-168">`inputFiles`: matriz de archivos que se van a unir.</span><span class="sxs-lookup"><span data-stu-id="e24e3-168">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="e24e3-169">Se trata de rutas de acceso relativas al archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="e24e3-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="e24e3-170">**opcional**, \*un valor vacío da como resultado un archivo de salida vacío.</span><span class="sxs-lookup"><span data-stu-id="e24e3-170">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="e24e3-171">Se admiten los patrones [globales](https://www.tldp.org/LDP/abs/html/globbingref.html).</span><span class="sxs-lookup"><span data-stu-id="e24e3-171">[globbing](https://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="e24e3-172">`minify`: opciones de minimización para el tipo de salida.</span><span class="sxs-lookup"><span data-stu-id="e24e3-172">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="e24e3-173">**opcional**, *valor predeterminado: `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="e24e3-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="e24e3-174">Las opciones de configuración están disponibles por tipo de archivo de salida.</span><span class="sxs-lookup"><span data-stu-id="e24e3-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="e24e3-175">Minimizador CSS</span><span class="sxs-lookup"><span data-stu-id="e24e3-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="e24e3-176">Minimizador de JavaScript</span><span class="sxs-lookup"><span data-stu-id="e24e3-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="e24e3-177">Minimizador HTML</span><span class="sxs-lookup"><span data-stu-id="e24e3-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="e24e3-178">`includeInProject`: marca que indica si se van a agregar los archivos generados al archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="e24e3-178">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="e24e3-179">**opcional**, *valor predeterminado: false*</span><span class="sxs-lookup"><span data-stu-id="e24e3-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="e24e3-180">`sourceMap`: marca que indica si se debe generar un mapa de origen para el archivo unido.</span><span class="sxs-lookup"><span data-stu-id="e24e3-180">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="e24e3-181">**opcional**, *valor predeterminado: false*</span><span class="sxs-lookup"><span data-stu-id="e24e3-181">**optional**, *default - false*</span></span>
* <span data-ttu-id="e24e3-182">`sourceMapRootPath`: ruta de acceso raíz para almacenar el archivo de asignación de origen generado.</span><span class="sxs-lookup"><span data-stu-id="e24e3-182">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="e24e3-183">Ejecución en tiempo de compilación de la unión y la minimización</span><span class="sxs-lookup"><span data-stu-id="e24e3-183">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="e24e3-184">El paquete NuGet [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) permite la ejecución de la unión y la minimización en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="e24e3-184">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="e24e3-185">El paquete inserta [destinos de MSBuild](/visualstudio/msbuild/msbuild-targets) que se ejecutan en tiempo de compilación y de limpieza.</span><span class="sxs-lookup"><span data-stu-id="e24e3-185">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="e24e3-186">El proceso de compilación analiza el archivo *bundleconfig.json* para generar los archivos de salida en función de la configuración definida.</span><span class="sxs-lookup"><span data-stu-id="e24e3-186">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="e24e3-187">BuildBundlerMinifier pertenece a un proyecto impulsado por la comunidad en GitHub para el que Microsoft no proporciona soporte técnico.</span><span class="sxs-lookup"><span data-stu-id="e24e3-187">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="e24e3-188">Las incidencias se deben notificar [aquí](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="e24e3-188">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e24e3-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e24e3-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e24e3-190">Agregue el paquete *BuildBundlerMinifier* al proyecto.</span><span class="sxs-lookup"><span data-stu-id="e24e3-190">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="e24e3-191">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="e24e3-191">Build the project.</span></span> <span data-ttu-id="e24e3-192">En la ventana de salida aparece lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e24e3-192">The following appears in the Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="e24e3-193">Limpie el proyecto.</span><span class="sxs-lookup"><span data-stu-id="e24e3-193">Clean the project.</span></span> <span data-ttu-id="e24e3-194">En la ventana de salida aparece lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e24e3-194">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-cli"></a>[<span data-ttu-id="e24e3-195">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="e24e3-195">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e24e3-196">Agregue el paquete *BuildBundlerMinifier* al proyecto:</span><span class="sxs-lookup"><span data-stu-id="e24e3-196">Add the *BuildBundlerMinifier* package to your project:</span></span>

```dotnetcli
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="e24e3-197">Si se usa ASP.NET Core 1.x, restaure el paquete recientemente agregado:</span><span class="sxs-lookup"><span data-stu-id="e24e3-197">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```dotnetcli
dotnet restore
```

<span data-ttu-id="e24e3-198">Compile el proyecto:</span><span class="sxs-lookup"><span data-stu-id="e24e3-198">Build the project:</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="e24e3-199">Aparece lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e24e3-199">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="e24e3-200">Limpie el proyecto:</span><span class="sxs-lookup"><span data-stu-id="e24e3-200">Clean the project:</span></span>

```dotnetcli
dotnet clean
```

<span data-ttu-id="e24e3-201">Aparece el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="e24e3-201">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="e24e3-202">Ejecución ad hoc de la unión y la minimización</span><span class="sxs-lookup"><span data-stu-id="e24e3-202">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="e24e3-203">Es posible ejecutar las tareas de unión y minimización ad hoc, sin necesidad de compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="e24e3-203">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="e24e3-204">Agregue el paquete NuGet [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) al proyecto:</span><span class="sxs-lookup"><span data-stu-id="e24e3-204">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="e24e3-205">BundlerMinifier.Core pertenece a un proyecto impulsado por la comunidad en GitHub para el que Microsoft no proporciona soporte técnico.</span><span class="sxs-lookup"><span data-stu-id="e24e3-205">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="e24e3-206">Las incidencias se deben notificar [aquí](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="e24e3-206">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="e24e3-207">Este paquete amplía la CLI de .NET Core para incluir la herramienta *dotnet-bundle*.</span><span class="sxs-lookup"><span data-stu-id="e24e3-207">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="e24e3-208">El comando siguiente se puede ejecutar en la ventana de la Consola del Administrador de paquetes (PMC) o en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="e24e3-208">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```dotnetcli
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="e24e3-209">El Administrador de paquetes NuGet agrega dependencias al archivo \*.csproj como nodos de `<PackageReference />`.</span><span class="sxs-lookup"><span data-stu-id="e24e3-209">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="e24e3-210">El comando `dotnet bundle` se registra con la CLI de .NET Core solo cuando se utiliza un nodo de `<DotNetCliToolReference />`.</span><span class="sxs-lookup"><span data-stu-id="e24e3-210">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="e24e3-211">Modifique el archivo \*.csproj en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="e24e3-211">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="e24e3-212">Incorporación de archivos al flujo de trabajo</span><span class="sxs-lookup"><span data-stu-id="e24e3-212">Add files to workflow</span></span>

<span data-ttu-id="e24e3-213">Considere un ejemplo en el que se agrega un archivo *custom.css* adicional similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="e24e3-213">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="e24e3-214">Para minimizar *custom.css* y unirlo con *site.css* en un archivo *site.min.css*, agregue la ruta de acceso relativa a *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="e24e3-214">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="e24e3-215">También podría usarse el patrón global siguiente:</span><span class="sxs-lookup"><span data-stu-id="e24e3-215">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/!(*.min).css" ]
> ```
>
> <span data-ttu-id="e24e3-216">Este patrón global coincide con todos los archivos CSS y excluye el patrón de archivo minimizado.</span><span class="sxs-lookup"><span data-stu-id="e24e3-216">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="e24e3-217">Compile la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e24e3-217">Build the application.</span></span> <span data-ttu-id="e24e3-218">Abra *site.min.css* y observe que el contenido de *custom.css* se anexa al final del archivo.</span><span class="sxs-lookup"><span data-stu-id="e24e3-218">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="e24e3-219">Unión y minimización basadas en entorno</span><span class="sxs-lookup"><span data-stu-id="e24e3-219">Environment-based bundling and minification</span></span>

<span data-ttu-id="e24e3-220">Como procedimiento recomendado, los archivos unidos y minimizados de la aplicación deben usarse en un entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="e24e3-220">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="e24e3-221">Durante el desarrollo, los archivos originales facilitan la depuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e24e3-221">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="e24e3-222">Especifique los archivos que se van a incluir en las páginas mediante el [Asistente de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) en las vistas.</span><span class="sxs-lookup"><span data-stu-id="e24e3-222">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="e24e3-223">El Asistente de etiquetas de entorno solo representa su contenido cuando se ejecuta en [entornos](xref:fundamentals/environments) concretos.</span><span class="sxs-lookup"><span data-stu-id="e24e3-223">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="e24e3-224">La etiqueta `environment` siguiente representa los archivos CSS no procesados cuando se ejecuta en el entorno `Development`:</span><span class="sxs-lookup"><span data-stu-id="e24e3-224">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

<span data-ttu-id="e24e3-225">La etiqueta `environment` siguiente representa los archivos CSS unidos y minimizados cuando se ejecuta en un entorno distinto de `Development`.</span><span class="sxs-lookup"><span data-stu-id="e24e3-225">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="e24e3-226">Por ejemplo, la ejecución en `Production` o `Staging` desencadena la representación de estas hojas de estilo:</span><span class="sxs-lookup"><span data-stu-id="e24e3-226">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="e24e3-227">Consumo de bundleconfig.json desde Gulp</span><span class="sxs-lookup"><span data-stu-id="e24e3-227">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="e24e3-228">Hay casos en los que el flujo de trabajo de unión y minimización de una aplicación requiere un procesamiento adicional.</span><span class="sxs-lookup"><span data-stu-id="e24e3-228">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="e24e3-229">Entre los ejemplos se incluyen la optimización de imágenes, la limpieza de memoria caché y el procesamiento de recursos de red CDN.</span><span class="sxs-lookup"><span data-stu-id="e24e3-229">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="e24e3-230">Para cumplir estos requisitos, se puede convertir el flujo de trabajo de unión y minimización para usar Gulp.</span><span class="sxs-lookup"><span data-stu-id="e24e3-230">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="e24e3-231">Uso de la extensión Bundler & Minifier</span><span class="sxs-lookup"><span data-stu-id="e24e3-231">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="e24e3-232">La extensión [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) de Visual Studio controla la conversión a Gulp.</span><span class="sxs-lookup"><span data-stu-id="e24e3-232">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="e24e3-233">La extensión Bundler & Minifier pertenece a un proyecto impulsado por la comunidad en GitHub para el que Microsoft no proporciona soporte técnico.</span><span class="sxs-lookup"><span data-stu-id="e24e3-233">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="e24e3-234">Las incidencias se deben notificar [aquí](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="e24e3-234">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="e24e3-235">En el Explorador de soluciones, haga clic con el botón derecho en el archivo *bundleconfig.json* y seleccione **Bundler & Minifier** > **Convertir a Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="e24e3-235">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Elemento de menú contextual Convertir a Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="e24e3-237">Los archivos *gulpfile.js* y *package.json* se agregan al proyecto.</span><span class="sxs-lookup"><span data-stu-id="e24e3-237">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="e24e3-238">Se instalan los paquetes [npm](https://www.npmjs.com/) compatibles que se muestran en la sección `devDependencies` del archivo *package.json*.</span><span class="sxs-lookup"><span data-stu-id="e24e3-238">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="e24e3-239">Ejecute el comando siguiente en la ventana de la PMC para instalar la CLI de Gulp como una dependencia global:</span><span class="sxs-lookup"><span data-stu-id="e24e3-239">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="e24e3-240">El archivo *gulpfile.js* lee el archivo *bundleconfig.json* para las entradas, salidas y configuraciones.</span><span class="sxs-lookup"><span data-stu-id="e24e3-240">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="e24e3-241">Conversión de forma manual</span><span class="sxs-lookup"><span data-stu-id="e24e3-241">Convert manually</span></span>

<span data-ttu-id="e24e3-242">Si Visual Studio y/o la extensión Bundler & Minifier no están disponibles, haga la conversión manualmente.</span><span class="sxs-lookup"><span data-stu-id="e24e3-242">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="e24e3-243">Agregue un archivo *package.json*, con el elemento `devDependencies` siguiente, a la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="e24e3-243">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

> [!WARNING]
> <span data-ttu-id="e24e3-244">El módulo `gulp-uglify` no es compatible con ECMAScript (ES) 2015/ES6 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="e24e3-244">The `gulp-uglify` module doesn't support ECMAScript (ES) 2015 / ES6 and later.</span></span> <span data-ttu-id="e24e3-245">Instale [gulp-terser](https://www.npmjs.com/package/gulp-terser) en lugar de `gulp-uglify` para usar ES2015/ES6 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="e24e3-245">Install [gulp-terser](https://www.npmjs.com/package/gulp-terser) instead of `gulp-uglify` to use ES2015 / ES6 or later.</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="e24e3-246">Ejecute el comando siguiente al mismo nivel que *package.json* para instalar las dependencias:</span><span class="sxs-lookup"><span data-stu-id="e24e3-246">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="e24e3-247">Instale la CLI de Gulp como una dependencia global:</span><span class="sxs-lookup"><span data-stu-id="e24e3-247">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="e24e3-248">Copie el archivo *gulpfile.js* siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="e24e3-248">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="e24e3-249">Ejecución de tareas de Gulp</span><span class="sxs-lookup"><span data-stu-id="e24e3-249">Run Gulp tasks</span></span>

<span data-ttu-id="e24e3-250">Para desencadenar la tarea de minimización de Gulp antes de que el proyecto se compile en Visual Studio, agregue el [destino de MSBuild](/visualstudio/msbuild/msbuild-targets) siguiente al archivo \*.csproj:</span><span class="sxs-lookup"><span data-stu-id="e24e3-250">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="e24e3-251">En este ejemplo, las tareas definidas en el destino `MyPreCompileTarget` se ejecutan antes que el destino `Build` predefinido.</span><span class="sxs-lookup"><span data-stu-id="e24e3-251">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="e24e3-252">Aparece una salida similar a la siguiente en la ventana de salida de Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="e24e3-252">Output similar to the following appears in Visual Studio's Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

## <a name="additional-resources"></a><span data-ttu-id="e24e3-253">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e24e3-253">Additional resources</span></span>

* [<span data-ttu-id="e24e3-254">Uso de Grunt</span><span class="sxs-lookup"><span data-stu-id="e24e3-254">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="e24e3-255">Uso de varios entornos</span><span class="sxs-lookup"><span data-stu-id="e24e3-255">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="e24e3-256">Asistentes de etiquetas</span><span class="sxs-lookup"><span data-stu-id="e24e3-256">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
