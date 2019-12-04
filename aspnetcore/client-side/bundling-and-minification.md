---
title: Agrupación y minimizar de recursos estáticos en ASP.NET Core
author: scottaddie
description: Obtenga información sobre cómo optimizar los recursos estáticos en una aplicación Web de ASP.NET Core aplicando técnicas de agrupación y minificación.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: a7a5c40d6c31c4416212c02c1b491dd794f2a1d3
ms.sourcegitcommit: b3e1e31e5d8bdd94096cf27444594d4a7b065525
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74803284"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="74a21-103">Agrupación y minimizar de recursos estáticos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="74a21-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="74a21-104">Por [Scott Addie](https://twitter.com/Scott_Addie) y [David pino](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="74a21-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="74a21-105">En este artículo se explican las ventajas de aplicar la agrupación y la minificación, incluida la forma en que estas características se pueden utilizar con ASP.NET Core Web Apps.</span><span class="sxs-lookup"><span data-stu-id="74a21-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="74a21-106">Qué es la agrupación y minificación</span><span class="sxs-lookup"><span data-stu-id="74a21-106">What is bundling and minification</span></span>

<span data-ttu-id="74a21-107">La Unión y minificación son dos optimizaciones de rendimiento distintas que se pueden aplicar en una aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="74a21-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="74a21-108">Se usan juntos, el empaquetado y la minificación mejoran el rendimiento al reducir el número de solicitudes de servidor y reducir el tamaño de los recursos estáticos solicitados.</span><span class="sxs-lookup"><span data-stu-id="74a21-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="74a21-109">La agrupación y minificación mejoran principalmente el tiempo de carga de la primera solicitud de página.</span><span class="sxs-lookup"><span data-stu-id="74a21-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="74a21-110">Una vez solicitada una página web, el explorador almacena en caché los recursos estáticos (JavaScript, CSS e imágenes).</span><span class="sxs-lookup"><span data-stu-id="74a21-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="74a21-111">Por lo tanto, la Unión y minificación no mejoran el rendimiento cuando solicitan la misma página, o páginas, en el mismo sitio que solicita los mismos recursos.</span><span class="sxs-lookup"><span data-stu-id="74a21-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="74a21-112">Si el encabezado Expires no se establece correctamente en los activos y no se usa la agrupación y minificación, la heurística de actualización del explorador marca los recursos obsoletos después de unos días.</span><span class="sxs-lookup"><span data-stu-id="74a21-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="74a21-113">Además, el explorador requiere una solicitud de validación para cada activo.</span><span class="sxs-lookup"><span data-stu-id="74a21-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="74a21-114">En este caso, la agrupación y la minificación proporcionan una mejora del rendimiento incluso después de la primera solicitud de la página.</span><span class="sxs-lookup"><span data-stu-id="74a21-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="74a21-115">Unión</span><span class="sxs-lookup"><span data-stu-id="74a21-115">Bundling</span></span>

<span data-ttu-id="74a21-116">La unión combina varios archivos en un único archivo.</span><span class="sxs-lookup"><span data-stu-id="74a21-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="74a21-117">La agrupación reduce el número de solicitudes de servidor necesarias para representar un recurso Web, como una página web.</span><span class="sxs-lookup"><span data-stu-id="74a21-117">Bundling reduces the number of server requests that are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="74a21-118">Puede crear cualquier número de paquetes individuales específicamente para CSS, JavaScript, etc. Menos archivos significa menos solicitudes HTTP desde el explorador al servidor o desde el servicio que proporciona la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74a21-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="74a21-119">Como resultado, se mejora el rendimiento de la carga de la primera página.</span><span class="sxs-lookup"><span data-stu-id="74a21-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="74a21-120">Minificación</span><span class="sxs-lookup"><span data-stu-id="74a21-120">Minification</span></span>

<span data-ttu-id="74a21-121">Minificación quita los caracteres innecesarios del código sin modificar la funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="74a21-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="74a21-122">El resultado es una reducción significativa del tamaño de los activos solicitados (como CSS, imágenes y archivos JavaScript).</span><span class="sxs-lookup"><span data-stu-id="74a21-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="74a21-123">Los efectos secundarios comunes de minificación incluyen acortar nombres de variable a un carácter y quitar comentarios y espacios en blanco innecesarios.</span><span class="sxs-lookup"><span data-stu-id="74a21-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="74a21-124">Considere la siguiente función de JavaScript:</span><span class="sxs-lookup"><span data-stu-id="74a21-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="74a21-125">Minificación reduce la función a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="74a21-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="74a21-126">Además de quitar los comentarios y los espacios en blanco innecesarios, se ha cambiado el nombre de los siguientes nombres de parámetros y variables de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="74a21-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="74a21-127">Original</span><span class="sxs-lookup"><span data-stu-id="74a21-127">Original</span></span> | <span data-ttu-id="74a21-128">Se cambia el nombre</span><span class="sxs-lookup"><span data-stu-id="74a21-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="74a21-129">Impacto de la agrupación y minificación</span><span class="sxs-lookup"><span data-stu-id="74a21-129">Impact of bundling and minification</span></span>

<span data-ttu-id="74a21-130">En la tabla siguiente se describen las diferencias entre la carga individual de recursos y el uso de la agrupación y la minificación:</span><span class="sxs-lookup"><span data-stu-id="74a21-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="74a21-131">Acción de</span><span class="sxs-lookup"><span data-stu-id="74a21-131">Action</span></span> | <span data-ttu-id="74a21-132">Con B/M</span><span class="sxs-lookup"><span data-stu-id="74a21-132">With B/M</span></span> | <span data-ttu-id="74a21-133">Sin B/M</span><span class="sxs-lookup"><span data-stu-id="74a21-133">Without B/M</span></span> | <span data-ttu-id="74a21-134">Cambio</span><span class="sxs-lookup"><span data-stu-id="74a21-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="74a21-135">Solicitudes de archivo</span><span class="sxs-lookup"><span data-stu-id="74a21-135">File Requests</span></span>  | <span data-ttu-id="74a21-136">7</span><span class="sxs-lookup"><span data-stu-id="74a21-136">7</span></span>   | <span data-ttu-id="74a21-137">18</span><span class="sxs-lookup"><span data-stu-id="74a21-137">18</span></span>     | <span data-ttu-id="74a21-138">157%</span><span class="sxs-lookup"><span data-stu-id="74a21-138">157%</span></span>
<span data-ttu-id="74a21-139">KB transferidos</span><span class="sxs-lookup"><span data-stu-id="74a21-139">KB Transferred</span></span> | <span data-ttu-id="74a21-140">156</span><span class="sxs-lookup"><span data-stu-id="74a21-140">156</span></span> | <span data-ttu-id="74a21-141">264.68</span><span class="sxs-lookup"><span data-stu-id="74a21-141">264.68</span></span> | <span data-ttu-id="74a21-142">70%</span><span class="sxs-lookup"><span data-stu-id="74a21-142">70%</span></span>
<span data-ttu-id="74a21-143">Tiempo de carga (MS)</span><span class="sxs-lookup"><span data-stu-id="74a21-143">Load Time (ms)</span></span> | <span data-ttu-id="74a21-144">885</span><span class="sxs-lookup"><span data-stu-id="74a21-144">885</span></span> | <span data-ttu-id="74a21-145">2360</span><span class="sxs-lookup"><span data-stu-id="74a21-145">2360</span></span>   | <span data-ttu-id="74a21-146">167%</span><span class="sxs-lookup"><span data-stu-id="74a21-146">167%</span></span>

<span data-ttu-id="74a21-147">Los exploradores son bastante detallados con respecto a los encabezados de solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="74a21-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="74a21-148">La métrica total de bytes enviados vio una reducción significativa en la agrupación.</span><span class="sxs-lookup"><span data-stu-id="74a21-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="74a21-149">El tiempo de carga muestra una mejora significativa, pero este ejemplo se ejecutó localmente.</span><span class="sxs-lookup"><span data-stu-id="74a21-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="74a21-150">Se obtienen mayores mejoras de rendimiento al usar la agrupación y la minificación con recursos transferidos a través de una red.</span><span class="sxs-lookup"><span data-stu-id="74a21-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="74a21-151">Elegir una estrategia de agrupación y minificación</span><span class="sxs-lookup"><span data-stu-id="74a21-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="74a21-152">Las plantillas de proyecto de MVC y Razor Pages proporcionan una solución integrada para la agrupación y el minificación de archivos que se componen de un archivo de configuración JSON.</span><span class="sxs-lookup"><span data-stu-id="74a21-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="74a21-153">Las herramientas de terceros, [como la tarea del Ejecutor de tareas](xref:client-side/using-grunt) , realizan las mismas tareas con un poco más de complejidad.</span><span class="sxs-lookup"><span data-stu-id="74a21-153">Third-party tools, such as the [Grunt](xref:client-side/using-grunt) task runner, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="74a21-154">Una herramienta de terceros es una buena opción cuando el flujo de trabajo de desarrollo requiere un procesamiento más allá de la agrupación y la minificación&mdash;como la detección de errores y la optimización de imágenes.</span><span class="sxs-lookup"><span data-stu-id="74a21-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="74a21-155">Mediante el uso de la Unión en tiempo de diseño y minificación, los archivos reducida se crean antes de la implementación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74a21-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="74a21-156">La agrupación y minificar antes de la implementación proporciona la ventaja de reducir la carga del servidor.</span><span class="sxs-lookup"><span data-stu-id="74a21-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="74a21-157">Sin embargo, es importante reconocer que la agrupación en tiempo de diseño y minificación aumenta la complejidad de la compilación y solo funciona con archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="74a21-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="74a21-158">Configuración de la agrupación y minificación</span><span class="sxs-lookup"><span data-stu-id="74a21-158">Configure bundling and minification</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="74a21-159">En ASP.NET Core 2,0 o versiones anteriores, las plantillas de proyecto MVC y Razor Pages proporcionan un archivo de configuración *bundleconfig. JSON* que define las opciones de cada paquete:</span><span class="sxs-lookup"><span data-stu-id="74a21-159">In ASP.NET Core 2.0 or earlier, the MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file that defines the options for each bundle:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="74a21-160">En ASP.NET Core 2,1 o posterior, agregue un nuevo archivo JSON denominado *bundleconfig. JSON*a la raíz del proyecto MVC o Razor pages.</span><span class="sxs-lookup"><span data-stu-id="74a21-160">In ASP.NET Core 2.1 or later, add a new JSON file, named *bundleconfig.json*, to the MVC or Razor Pages project root.</span></span> <span data-ttu-id="74a21-161">Incluya el siguiente código JSON en ese archivo como punto de partida:</span><span class="sxs-lookup"><span data-stu-id="74a21-161">Include the following JSON in that file as a starting point:</span></span>

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="74a21-162">El archivo *bundleconfig. JSON* define las opciones de cada paquete.</span><span class="sxs-lookup"><span data-stu-id="74a21-162">The *bundleconfig.json* file defines the options for each bundle.</span></span> <span data-ttu-id="74a21-163">En el ejemplo anterior, se define una configuración de agrupación única para los archivos personalizados de JavaScript (*wwwroot/JS/site. js*) y de hoja de estilos (*wwwroot/CSS/site. CSS*).</span><span class="sxs-lookup"><span data-stu-id="74a21-163">In the preceding example, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files.</span></span>

<span data-ttu-id="74a21-164">Las opciones de configuración incluyen:</span><span class="sxs-lookup"><span data-stu-id="74a21-164">Configuration options include:</span></span>

* <span data-ttu-id="74a21-165">`outputFileName`: el nombre del archivo de paquete que se va a generar.</span><span class="sxs-lookup"><span data-stu-id="74a21-165">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="74a21-166">Puede contener una ruta de acceso relativa del archivo *bundleconfig. JSON* .</span><span class="sxs-lookup"><span data-stu-id="74a21-166">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="74a21-167">**required**</span><span class="sxs-lookup"><span data-stu-id="74a21-167">**required**</span></span>
* <span data-ttu-id="74a21-168">`inputFiles`: una matriz de archivos para agrupar.</span><span class="sxs-lookup"><span data-stu-id="74a21-168">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="74a21-169">Se trata de rutas de acceso relativas al archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="74a21-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="74a21-170">**opcional**, \* un valor vacío da como resultado un archivo de salida vacío.</span><span class="sxs-lookup"><span data-stu-id="74a21-170">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="74a21-171">se admiten patrones [comodines](https://www.tldp.org/LDP/abs/html/globbingref.html) .</span><span class="sxs-lookup"><span data-stu-id="74a21-171">[globbing](https://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="74a21-172">`minify`: opciones de minificación para el tipo de salida.</span><span class="sxs-lookup"><span data-stu-id="74a21-172">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="74a21-173">**opcional**, *valor predeterminado: `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="74a21-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="74a21-174">Las opciones de configuración están disponibles por tipo de archivo de salida.</span><span class="sxs-lookup"><span data-stu-id="74a21-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="74a21-175">Minifier CSS</span><span class="sxs-lookup"><span data-stu-id="74a21-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="74a21-176">Minifier de JavaScript</span><span class="sxs-lookup"><span data-stu-id="74a21-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="74a21-177">Minifier HTML</span><span class="sxs-lookup"><span data-stu-id="74a21-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="74a21-178">`includeInProject`: marca que indica si se van a agregar los archivos generados al archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="74a21-178">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="74a21-179">**opcional**, *el valor predeterminado es false* .</span><span class="sxs-lookup"><span data-stu-id="74a21-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="74a21-180">`sourceMap`: marca que indica si se debe generar un mapa de origen para el archivo agrupado.</span><span class="sxs-lookup"><span data-stu-id="74a21-180">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="74a21-181">**opcional**, *el valor predeterminado es false* .</span><span class="sxs-lookup"><span data-stu-id="74a21-181">**optional**, *default - false*</span></span>
* <span data-ttu-id="74a21-182">`sourceMapRootPath`: ruta de acceso raíz para almacenar el archivo de asignación de origen generado.</span><span class="sxs-lookup"><span data-stu-id="74a21-182">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="74a21-183">Ejecución en tiempo de compilación de la agrupación y minificación</span><span class="sxs-lookup"><span data-stu-id="74a21-183">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="74a21-184">El paquete NuGet de [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) permite la ejecución de la agrupación y minificación en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="74a21-184">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="74a21-185">El paquete inserta [destinos de MSBuild](/visualstudio/msbuild/msbuild-targets) que se ejecutan en tiempo de compilación y limpieza.</span><span class="sxs-lookup"><span data-stu-id="74a21-185">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="74a21-186">El proceso de compilación analiza el archivo *bundleconfig. JSON* para generar los archivos de salida basados en la configuración definida.</span><span class="sxs-lookup"><span data-stu-id="74a21-186">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="74a21-187">BuildBundlerMinifier pertenece a un proyecto controlado por la comunidad en GitHub para el que Microsoft no proporciona soporte técnico.</span><span class="sxs-lookup"><span data-stu-id="74a21-187">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="74a21-188">[Aquí](https://github.com/madskristensen/BundlerMinifier/issues)se deben incluir los problemas.</span><span class="sxs-lookup"><span data-stu-id="74a21-188">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="74a21-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="74a21-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="74a21-190">Agregue el paquete *BuildBundlerMinifier* al proyecto.</span><span class="sxs-lookup"><span data-stu-id="74a21-190">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="74a21-191">Generar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="74a21-191">Build the project.</span></span> <span data-ttu-id="74a21-192">La siguiente información aparece en la ventana salida:</span><span class="sxs-lookup"><span data-stu-id="74a21-192">The following appears in the Output window:</span></span>

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

<span data-ttu-id="74a21-193">Limpie el proyecto.</span><span class="sxs-lookup"><span data-stu-id="74a21-193">Clean the project.</span></span> <span data-ttu-id="74a21-194">La siguiente información aparece en la ventana salida:</span><span class="sxs-lookup"><span data-stu-id="74a21-194">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="74a21-195">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="74a21-195">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="74a21-196">Agregue el paquete *BuildBundlerMinifier* al proyecto:</span><span class="sxs-lookup"><span data-stu-id="74a21-196">Add the *BuildBundlerMinifier* package to your project:</span></span>

```dotnetcli
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="74a21-197">Si usa ASP.NET Core 1. x, restaure el paquete recién agregado:</span><span class="sxs-lookup"><span data-stu-id="74a21-197">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```dotnetcli
dotnet restore
```

<span data-ttu-id="74a21-198">Compile el proyecto:</span><span class="sxs-lookup"><span data-stu-id="74a21-198">Build the project:</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="74a21-199">Aparece lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="74a21-199">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="74a21-200">Limpie el proyecto:</span><span class="sxs-lookup"><span data-stu-id="74a21-200">Clean the project:</span></span>

```dotnetcli
dotnet clean
```

<span data-ttu-id="74a21-201">Aparece el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="74a21-201">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="74a21-202">Ejecución ad hoc de la agrupación y minificación</span><span class="sxs-lookup"><span data-stu-id="74a21-202">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="74a21-203">Es posible ejecutar las tareas de agrupación y minificación de forma ad hoc, sin necesidad de compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="74a21-203">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="74a21-204">Agregue el paquete NuGet [BundlerMinifier. Core](https://www.nuget.org/packages/BundlerMinifier.Core/) al proyecto:</span><span class="sxs-lookup"><span data-stu-id="74a21-204">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="74a21-205">BundlerMinifier. Core pertenece a un proyecto controlado por la comunidad en GitHub para el que Microsoft no proporciona soporte técnico.</span><span class="sxs-lookup"><span data-stu-id="74a21-205">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="74a21-206">[Aquí](https://github.com/madskristensen/BundlerMinifier/issues)se deben incluir los problemas.</span><span class="sxs-lookup"><span data-stu-id="74a21-206">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="74a21-207">Este paquete amplía el CLI de .NET Core para incluir la herramienta *dotnet-bundle* .</span><span class="sxs-lookup"><span data-stu-id="74a21-207">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="74a21-208">El siguiente comando se puede ejecutar en la ventana de la consola del administrador de paquetes (PMC) o en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="74a21-208">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```dotnetcli
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="74a21-209">El administrador de paquetes NuGet agrega dependencias al archivo \*. csproj como nodos `<PackageReference />`.</span><span class="sxs-lookup"><span data-stu-id="74a21-209">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="74a21-210">El comando `dotnet bundle` se registra con el CLI de .NET Core solo cuando se utiliza un nodo de `<DotNetCliToolReference />`.</span><span class="sxs-lookup"><span data-stu-id="74a21-210">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="74a21-211">Modifique el archivo \*. csproj en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="74a21-211">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="74a21-212">Agregar archivos al flujo de trabajo</span><span class="sxs-lookup"><span data-stu-id="74a21-212">Add files to workflow</span></span>

<span data-ttu-id="74a21-213">Considere un ejemplo en el que se agrega un archivo *. CSS personalizado* adicional similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="74a21-213">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="74a21-214">Para minimizar *custom. CSS* y agruparlo con *site. CSS* en un archivo *site. min. CSS* , agregue la ruta de acceso relativa a *bundleconfig. JSON*:</span><span class="sxs-lookup"><span data-stu-id="74a21-214">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="74a21-215">Como alternativa, podría usarse el siguiente patrón comodines:</span><span class="sxs-lookup"><span data-stu-id="74a21-215">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/!(*.min).css" ]
> ```
>
> <span data-ttu-id="74a21-216">Este patrón comodines coincide con todos los archivos CSS y excluye el patrón de archivo reducida.</span><span class="sxs-lookup"><span data-stu-id="74a21-216">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="74a21-217">Genere la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74a21-217">Build the application.</span></span> <span data-ttu-id="74a21-218">Abra *site. min. CSS* y observe que el contenido de *custom. CSS* se anexa al final del archivo.</span><span class="sxs-lookup"><span data-stu-id="74a21-218">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="74a21-219">Agrupación y minificación basados en entornos</span><span class="sxs-lookup"><span data-stu-id="74a21-219">Environment-based bundling and minification</span></span>

<span data-ttu-id="74a21-220">Como procedimiento recomendado, los archivos agrupados y reducida de la aplicación deben usarse en un entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="74a21-220">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="74a21-221">Durante el desarrollo, los archivos originales facilitan la depuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74a21-221">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="74a21-222">Especifique los archivos que se van a incluir en las páginas mediante la [aplicación auxiliar de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) en las vistas.</span><span class="sxs-lookup"><span data-stu-id="74a21-222">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="74a21-223">La aplicación auxiliar de etiquetas de entorno solo representa su contenido cuando se ejecuta en [entornos](xref:fundamentals/environments)específicos.</span><span class="sxs-lookup"><span data-stu-id="74a21-223">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="74a21-224">La siguiente etiqueta de `environment` representa los archivos CSS no procesados cuando se ejecuta en el entorno de `Development`:</span><span class="sxs-lookup"><span data-stu-id="74a21-224">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

<span data-ttu-id="74a21-225">La siguiente etiqueta de `environment` representa los archivos CSS agrupados y reducida cuando se ejecuta en un entorno que no sea `Development`.</span><span class="sxs-lookup"><span data-stu-id="74a21-225">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="74a21-226">Por ejemplo, la ejecución de en `Production` o `Staging` desencadena la representación de estas hojas de estilos:</span><span class="sxs-lookup"><span data-stu-id="74a21-226">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="74a21-227">Usar bundleconfig. JSON de Gulp</span><span class="sxs-lookup"><span data-stu-id="74a21-227">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="74a21-228">Hay casos en los que el flujo de trabajo de empaquetado y minificación de una aplicación requiere un procesamiento adicional.</span><span class="sxs-lookup"><span data-stu-id="74a21-228">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="74a21-229">Algunos ejemplos son la optimización de imágenes, el desprocesamiento de memoria caché y el procesamiento de recursos de red CDN.</span><span class="sxs-lookup"><span data-stu-id="74a21-229">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="74a21-230">Para cumplir estos requisitos, puede convertir el flujo de trabajo de Unión y minificación para usar Gulp.</span><span class="sxs-lookup"><span data-stu-id="74a21-230">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="74a21-231">Uso de la extensión bundler & Minifier</span><span class="sxs-lookup"><span data-stu-id="74a21-231">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="74a21-232">El [empaquetador](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) de Visual Studio & extensión Minifier controla la conversión a Gulp.</span><span class="sxs-lookup"><span data-stu-id="74a21-232">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="74a21-233">El agrupador & extensión Minifier pertenece a un proyecto controlado por la comunidad en GitHub para el que Microsoft no proporciona soporte técnico.</span><span class="sxs-lookup"><span data-stu-id="74a21-233">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="74a21-234">[Aquí](https://github.com/madskristensen/BundlerMinifier/issues)se deben incluir los problemas.</span><span class="sxs-lookup"><span data-stu-id="74a21-234">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="74a21-235">Haga clic con el botón derecho en el archivo *bundleconfig. JSON* en explorador de soluciones y seleccione **agrupador & Minifier** > **convertir a Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="74a21-235">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Convertir en elemento de menú contextual de Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="74a21-237">Los archivos *gulpfile. js* y *Package. JSON* se agregan al proyecto.</span><span class="sxs-lookup"><span data-stu-id="74a21-237">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="74a21-238">Se instalan los paquetes de [NPM](https://www.npmjs.com/) compatibles que se enumeran en la sección `devDependencies` del archivo *Package. JSON* .</span><span class="sxs-lookup"><span data-stu-id="74a21-238">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="74a21-239">Ejecute el siguiente comando en la ventana PMC para instalar la CLI de Gulp como una dependencia global:</span><span class="sxs-lookup"><span data-stu-id="74a21-239">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="74a21-240">El archivo *gulpfile. js* lee el archivo *bundleconfig. JSON* para las entradas, salidas y configuraciones.</span><span class="sxs-lookup"><span data-stu-id="74a21-240">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="74a21-241">Convertir manualmente</span><span class="sxs-lookup"><span data-stu-id="74a21-241">Convert manually</span></span>

<span data-ttu-id="74a21-242">Si Visual Studio y/o el paquete & extensión Minifier no están disponibles, convierta manualmente.</span><span class="sxs-lookup"><span data-stu-id="74a21-242">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="74a21-243">Agregue un archivo *Package. JSON* , con la siguiente `devDependencies`, a la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="74a21-243">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

> [!WARNING]
> <span data-ttu-id="74a21-244">El módulo `gulp-uglify` no es compatible con ECMAScript (ES) 2015/ES6 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="74a21-244">The `gulp-uglify` module doesn't support ECMAScript (ES) 2015 / ES6 and later.</span></span> <span data-ttu-id="74a21-245">Instale [Gulp-terser](https://www.npmjs.com/package/gulp-terser) en lugar de `gulp-uglify` para usar ES2015/ES6 o posterior.</span><span class="sxs-lookup"><span data-stu-id="74a21-245">Install [gulp-terser](https://www.npmjs.com/package/gulp-terser) instead of `gulp-uglify` to use ES2015 / ES6 or later.</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="74a21-246">Instale las dependencias ejecutando el comando siguiente en el mismo nivel que *Package. JSON*:</span><span class="sxs-lookup"><span data-stu-id="74a21-246">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="74a21-247">Instale la CLI de Gulp como una dependencia global:</span><span class="sxs-lookup"><span data-stu-id="74a21-247">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="74a21-248">Copie el archivo *gulpfile. js* siguiente en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="74a21-248">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="74a21-249">Ejecutar tareas de Gulp</span><span class="sxs-lookup"><span data-stu-id="74a21-249">Run Gulp tasks</span></span>

<span data-ttu-id="74a21-250">Para desencadenar la tarea de minificación de Gulp antes de que el proyecto se compile en Visual Studio, agregue el siguiente [destino de MSBuild](/visualstudio/msbuild/msbuild-targets) al archivo \*. csproj:</span><span class="sxs-lookup"><span data-stu-id="74a21-250">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="74a21-251">En este ejemplo, las tareas definidas dentro de la `MyPreCompileTarget` destino se ejecutan antes del destino `Build` predefinido.</span><span class="sxs-lookup"><span data-stu-id="74a21-251">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="74a21-252">Aparece una salida similar a la siguiente en la ventana de salida de Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="74a21-252">Output similar to the following appears in Visual Studio's Output window:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="74a21-253">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="74a21-253">Additional resources</span></span>

* [<span data-ttu-id="74a21-254">Uso de Grunt</span><span class="sxs-lookup"><span data-stu-id="74a21-254">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="74a21-255">Uso de varios entornos</span><span class="sxs-lookup"><span data-stu-id="74a21-255">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="74a21-256">Asistentes de etiquetas</span><span class="sxs-lookup"><span data-stu-id="74a21-256">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
