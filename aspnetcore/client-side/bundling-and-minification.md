---
title: Agrupar y minificar recursos estáticos en ASP.NET Core
author: scottaddie
description: Aprenda a optimizar los recursos estáticos en una aplicación web ASP.NET Core aplicando técnicas de unión y minificación.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/10/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: ba01d365a25dfbd13fed89263d7489b2ce2a8771
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/11/2019
ms.locfileid: "65535927"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="879a2-103">Agrupar y minificar recursos estáticos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="879a2-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="879a2-104">Por [Scott Addie](https://twitter.com/Scott_Addie) y [David Borovice](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="879a2-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="879a2-105">En este artículo explica las ventajas de la aplicación de unión y minificación, incluido cómo se pueden usar estas características con aplicaciones web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="879a2-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="879a2-106">¿Qué es la unión y minificación</span><span class="sxs-lookup"><span data-stu-id="879a2-106">What is bundling and minification</span></span>

<span data-ttu-id="879a2-107">Unión y minificación son dos optimizaciones de rendimiento distintas que se pueden aplicar en una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="879a2-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="879a2-108">Se usan juntos, unión y minificación de mejoran el rendimiento mediante la reducción del número de solicitudes de servidor y reducir el tamaño de los activos estáticos solicitados.</span><span class="sxs-lookup"><span data-stu-id="879a2-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="879a2-109">Unión y minificación principalmente mejoran el tiempo de carga de solicitud de primera página.</span><span class="sxs-lookup"><span data-stu-id="879a2-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="879a2-110">Una vez que se ha solicitado una página web, el explorador almacena en caché los recursos estáticos (imágenes, CSS y JavaScript).</span><span class="sxs-lookup"><span data-stu-id="879a2-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="879a2-111">Por lo tanto, unión y minificación no mejoran el rendimiento cuando se solicita la misma página o páginas, en el mismo sitio que solicita los mismos recursos.</span><span class="sxs-lookup"><span data-stu-id="879a2-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="879a2-112">Si el expira encabezado no está configurado correctamente en los activos y si no se usa la unión y minificación, la heurística de actualización del explorador marca los recursos obsoletos después de unos días.</span><span class="sxs-lookup"><span data-stu-id="879a2-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="879a2-113">Además, el explorador requiere una solicitud de validación para cada recurso.</span><span class="sxs-lookup"><span data-stu-id="879a2-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="879a2-114">En este caso, unión y minificación de proporcionan una mejora del rendimiento incluso después de la primera solicitud de página.</span><span class="sxs-lookup"><span data-stu-id="879a2-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="879a2-115">La Unión</span><span class="sxs-lookup"><span data-stu-id="879a2-115">Bundling</span></span>

<span data-ttu-id="879a2-116">La Unión combina varios archivos en un único archivo.</span><span class="sxs-lookup"><span data-stu-id="879a2-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="879a2-117">La unión, reduce el número de solicitudes del servidor que son necesarios para representar un activo de web, como una página web.</span><span class="sxs-lookup"><span data-stu-id="879a2-117">Bundling reduces the number of server requests that are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="879a2-118">Puede crear cualquier número de paquetes individuales específicamente para CSS, JavaScript, etcetera. Menos archivos significa menos solicitudes HTTP desde el explorador al servidor o desde el servicio que proporciona la aplicación.</span><span class="sxs-lookup"><span data-stu-id="879a2-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="879a2-119">Este se produce en el mejor rendimiento de carga de primera página.</span><span class="sxs-lookup"><span data-stu-id="879a2-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="879a2-120">Minificación</span><span class="sxs-lookup"><span data-stu-id="879a2-120">Minification</span></span>

<span data-ttu-id="879a2-121">Minificación quita caracteres innecesarios de código sin modificar la funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="879a2-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="879a2-122">El resultado es una reducción de tamaño considerable en los activos solicitados (como CSS, imágenes y archivos de JavaScript).</span><span class="sxs-lookup"><span data-stu-id="879a2-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="879a2-123">Efectos secundarios comunes de minificación incluyen acortar los nombres de variable a un carácter y quitar los comentarios y espacios en blanco innecesarios.</span><span class="sxs-lookup"><span data-stu-id="879a2-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="879a2-124">Tenga en cuenta la siguiente función de JavaScript:</span><span class="sxs-lookup"><span data-stu-id="879a2-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="879a2-125">Minificación reduce la función a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="879a2-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="879a2-126">Además de quitar los comentarios y espacios en blanco innecesarios, los siguientes nombres de parámetro y la variable se cambió el nombre como sigue:</span><span class="sxs-lookup"><span data-stu-id="879a2-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="879a2-127">Original</span><span class="sxs-lookup"><span data-stu-id="879a2-127">Original</span></span> | <span data-ttu-id="879a2-128">Se cambia el nombre</span><span class="sxs-lookup"><span data-stu-id="879a2-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="879a2-129">Impacto de la unión y minificación</span><span class="sxs-lookup"><span data-stu-id="879a2-129">Impact of bundling and minification</span></span>

<span data-ttu-id="879a2-130">En la tabla siguiente se describe las diferencias entre cargar activos y uso de unión y minificación individualmente:</span><span class="sxs-lookup"><span data-stu-id="879a2-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="879a2-131">Acción</span><span class="sxs-lookup"><span data-stu-id="879a2-131">Action</span></span> | <span data-ttu-id="879a2-132">Con B/M</span><span class="sxs-lookup"><span data-stu-id="879a2-132">With B/M</span></span> | <span data-ttu-id="879a2-133">Sin B/M</span><span class="sxs-lookup"><span data-stu-id="879a2-133">Without B/M</span></span> | <span data-ttu-id="879a2-134">Cambio</span><span class="sxs-lookup"><span data-stu-id="879a2-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="879a2-135">Solicitudes de archivos</span><span class="sxs-lookup"><span data-stu-id="879a2-135">File Requests</span></span>  | <span data-ttu-id="879a2-136">7</span><span class="sxs-lookup"><span data-stu-id="879a2-136">7</span></span>   | <span data-ttu-id="879a2-137">18</span><span class="sxs-lookup"><span data-stu-id="879a2-137">18</span></span>     | <span data-ttu-id="879a2-138">157%</span><span class="sxs-lookup"><span data-stu-id="879a2-138">157%</span></span>
<span data-ttu-id="879a2-139">KB transferido</span><span class="sxs-lookup"><span data-stu-id="879a2-139">KB Transferred</span></span> | <span data-ttu-id="879a2-140">156</span><span class="sxs-lookup"><span data-stu-id="879a2-140">156</span></span> | <span data-ttu-id="879a2-141">264.68</span><span class="sxs-lookup"><span data-stu-id="879a2-141">264.68</span></span> | <span data-ttu-id="879a2-142">70%</span><span class="sxs-lookup"><span data-stu-id="879a2-142">70%</span></span>
<span data-ttu-id="879a2-143">Tiempo de carga (ms)</span><span class="sxs-lookup"><span data-stu-id="879a2-143">Load Time (ms)</span></span> | <span data-ttu-id="879a2-144">885</span><span class="sxs-lookup"><span data-stu-id="879a2-144">885</span></span> | <span data-ttu-id="879a2-145">2360</span><span class="sxs-lookup"><span data-stu-id="879a2-145">2360</span></span>   | <span data-ttu-id="879a2-146">167%</span><span class="sxs-lookup"><span data-stu-id="879a2-146">167%</span></span>

<span data-ttu-id="879a2-147">Los exploradores son bastante detallados con respecto a los encabezados de solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="879a2-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="879a2-148">El número total de bytes enviados métrica vio una reducción significativa cuando se agrupa.</span><span class="sxs-lookup"><span data-stu-id="879a2-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="879a2-149">El tiempo de carga muestra una mejora considerable, aunque en este ejemplo se ejecutó localmente.</span><span class="sxs-lookup"><span data-stu-id="879a2-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="879a2-150">Mejoras de rendimiento mayores se llevan a cabo cuando el uso de unión y minificación con activos transfiere a través de una red.</span><span class="sxs-lookup"><span data-stu-id="879a2-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="879a2-151">Elegir una estrategia de unión y minificación</span><span class="sxs-lookup"><span data-stu-id="879a2-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="879a2-152">Las plantillas de proyecto MVC y páginas de Razor proporcionan una solución para la unión y minificación que consta de un archivo de configuración de JSON.</span><span class="sxs-lookup"><span data-stu-id="879a2-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="879a2-153">Herramientas de terceros, como el [Grunt](xref:client-side/using-grunt) ejecutor de tareas, realizar las mismas tareas con un poco más compleja.</span><span class="sxs-lookup"><span data-stu-id="879a2-153">Third-party tools, such as the [Grunt](xref:client-side/using-grunt) task runner, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="879a2-154">Una herramienta de terceros es una opción ideal cuando el flujo de trabajo de desarrollo requiere un procesamiento más allá de unión y minificación&mdash;como la optimización de la detección de errores y la imagen.</span><span class="sxs-lookup"><span data-stu-id="879a2-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="879a2-155">Mediante el uso de unión y minificación de tiempo de diseño, se crean los archivos minimizados antes de la implementación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="879a2-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="879a2-156">Agrupar y minificar antes de la implementación ofrece la ventaja de carga reducida del servidor.</span><span class="sxs-lookup"><span data-stu-id="879a2-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="879a2-157">Sin embargo, es importante reconocer ese tiempo de diseño la unión y minificación aumenta la complejidad de la compilación y solo funciona con archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="879a2-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="879a2-158">Configurar la unión y minificación</span><span class="sxs-lookup"><span data-stu-id="879a2-158">Configure bundling and minification</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="879a2-159">En ASP.NET Core 2.0 o versiones anteriores, las plantillas de proyecto MVC y páginas de Razor proporcionan una *bundleconfig.json* archivo de configuración que define las opciones para cada lote:</span><span class="sxs-lookup"><span data-stu-id="879a2-159">In ASP.NET Core 2.0 or earlier, the MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file that defines the options for each bundle:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="879a2-160">En ASP.NET Core 2.1 o posterior, agregue un nuevo archivo JSON, denominado *bundleconfig.json*, a la raíz del proyecto MVC o las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="879a2-160">In ASP.NET Core 2.1 or later, add a new JSON file, named *bundleconfig.json*, to the MVC or Razor Pages project root.</span></span> <span data-ttu-id="879a2-161">Incluya el siguiente JSON en ese archivo como un punto de partida:</span><span class="sxs-lookup"><span data-stu-id="879a2-161">Include the following JSON in that file as a starting point:</span></span>

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="879a2-162">El *bundleconfig.json* archivo define las opciones para cada paquete.</span><span class="sxs-lookup"><span data-stu-id="879a2-162">The *bundleconfig.json* file defines the options for each bundle.</span></span> <span data-ttu-id="879a2-163">En el ejemplo anterior, se define una configuración de lote único para el código JavaScript personalizado (*wwwroot/js/site.js*) y hojas de estilo (*wwwroot/css/site.css*) los archivos.</span><span class="sxs-lookup"><span data-stu-id="879a2-163">In the preceding example, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files.</span></span>

<span data-ttu-id="879a2-164">Opciones de configuración incluyen:</span><span class="sxs-lookup"><span data-stu-id="879a2-164">Configuration options include:</span></span>

* <span data-ttu-id="879a2-165">`outputFileName`: El nombre del archivo de paquete para la salida.</span><span class="sxs-lookup"><span data-stu-id="879a2-165">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="879a2-166">Puede contener una ruta de acceso relativa desde la *bundleconfig.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="879a2-166">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="879a2-167">**required**</span><span class="sxs-lookup"><span data-stu-id="879a2-167">**required**</span></span>
* <span data-ttu-id="879a2-168">`inputFiles`: Una matriz de archivos que se va a agrupar.</span><span class="sxs-lookup"><span data-stu-id="879a2-168">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="879a2-169">Estas son las rutas de acceso relativas al archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="879a2-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="879a2-170">**opcional**, \* da como resultado un valor vacío en un archivo de resultados vacío.</span><span class="sxs-lookup"><span data-stu-id="879a2-170">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="879a2-171">[uso de comodines](http://www.tldp.org/LDP/abs/html/globbingref.html) se admiten patrones.</span><span class="sxs-lookup"><span data-stu-id="879a2-171">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="879a2-172">`minify`: Las opciones de reducción para el tipo de salida.</span><span class="sxs-lookup"><span data-stu-id="879a2-172">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="879a2-173">**opcional**, *predeterminado: `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="879a2-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="879a2-174">Las opciones de configuración están disponibles por tipo de archivo de salida.</span><span class="sxs-lookup"><span data-stu-id="879a2-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="879a2-175">Minificador CSS</span><span class="sxs-lookup"><span data-stu-id="879a2-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="879a2-176">Minificador de JavaScript</span><span class="sxs-lookup"><span data-stu-id="879a2-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="879a2-177">Minificador de HTML</span><span class="sxs-lookup"><span data-stu-id="879a2-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="879a2-178">`includeInProject`: Marca que indica si se debe agregar los archivos generados en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="879a2-178">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="879a2-179">**optional**, *default - false*</span><span class="sxs-lookup"><span data-stu-id="879a2-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="879a2-180">`sourceMap`: Marca que indica si se debe generar un mapa de origen para el archivo agrupado.</span><span class="sxs-lookup"><span data-stu-id="879a2-180">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="879a2-181">**optional**, *default - false*</span><span class="sxs-lookup"><span data-stu-id="879a2-181">**optional**, *default - false*</span></span>
* <span data-ttu-id="879a2-182">`sourceMapRootPath`: La ruta de acceso raíz para almacenar el archivo de mapa de código fuente generado.</span><span class="sxs-lookup"><span data-stu-id="879a2-182">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="879a2-183">Compilación en tiempo de ejecución de la unión y minificación</span><span class="sxs-lookup"><span data-stu-id="879a2-183">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="879a2-184">El [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) paquete NuGet permite la ejecución de la unión y minificación en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="879a2-184">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="879a2-185">Inserta el paquete [destinos de MSBuild](/visualstudio/msbuild/msbuild-targets) que se ejecutan en la compilación y tiempo de limpieza.</span><span class="sxs-lookup"><span data-stu-id="879a2-185">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="879a2-186">El *bundleconfig.json* archivo analizado por el proceso de compilación para generar los archivos de salida según la configuración definida.</span><span class="sxs-lookup"><span data-stu-id="879a2-186">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="879a2-187">BuildBundlerMinifier pertenece a un proyecto controlado por la Comunidad en GitHub para el que Microsoft no proporciona soporte.</span><span class="sxs-lookup"><span data-stu-id="879a2-187">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="879a2-188">Se deben notificar problemas [aquí](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="879a2-188">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="879a2-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="879a2-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="879a2-190">Agregar el *BuildBundlerMinifier* paquete al proyecto.</span><span class="sxs-lookup"><span data-stu-id="879a2-190">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="879a2-191">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="879a2-191">Build the project.</span></span> <span data-ttu-id="879a2-192">Aparecerá el siguiente mensaje en la ventana de salida:</span><span class="sxs-lookup"><span data-stu-id="879a2-192">The following appears in the Output window:</span></span>

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

<span data-ttu-id="879a2-193">Limpie el proyecto.</span><span class="sxs-lookup"><span data-stu-id="879a2-193">Clean the project.</span></span> <span data-ttu-id="879a2-194">Aparecerá el siguiente mensaje en la ventana de salida:</span><span class="sxs-lookup"><span data-stu-id="879a2-194">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="879a2-195">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="879a2-195">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="879a2-196">Agregar el *BuildBundlerMinifier* paquete al proyecto:</span><span class="sxs-lookup"><span data-stu-id="879a2-196">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="879a2-197">Si usa ASP.NET Core 1.x, restaurar el paquete recién agregado:</span><span class="sxs-lookup"><span data-stu-id="879a2-197">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="879a2-198">Compile el proyecto:</span><span class="sxs-lookup"><span data-stu-id="879a2-198">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="879a2-199">Aparecerá el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="879a2-199">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="879a2-200">Limpie el proyecto:</span><span class="sxs-lookup"><span data-stu-id="879a2-200">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="879a2-201">Aparece el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="879a2-201">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="879a2-202">Ejecución ad hoc de unión y minificación</span><span class="sxs-lookup"><span data-stu-id="879a2-202">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="879a2-203">Es posible ejecutar las tareas de unión y minificación en forma ad-hoc, sin compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="879a2-203">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="879a2-204">Agregar el [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) paquete NuGet al proyecto:</span><span class="sxs-lookup"><span data-stu-id="879a2-204">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="879a2-205">BundlerMinifier.Core pertenece a un proyecto controlado por la Comunidad en GitHub para el que Microsoft no proporciona soporte.</span><span class="sxs-lookup"><span data-stu-id="879a2-205">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="879a2-206">Se deben notificar problemas [aquí](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="879a2-206">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="879a2-207">Este paquete amplía la CLI de .NET Core para incluir el *dotnet agrupación* herramienta.</span><span class="sxs-lookup"><span data-stu-id="879a2-207">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="879a2-208">En la ventana de consola de administrador de paquetes (PMC) o en un shell de comandos, se puede ejecutar el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="879a2-208">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="879a2-209">Administrador de paquetes NuGet agrega las dependencias al archivo \*.csproj como `<PackageReference />` nodos.</span><span class="sxs-lookup"><span data-stu-id="879a2-209">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="879a2-210">El `dotnet bundle` comando está registrado con la CLI de .NET Core solo cuando un `<DotNetCliToolReference />` nodo se utiliza.</span><span class="sxs-lookup"><span data-stu-id="879a2-210">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="879a2-211">Modifique el archivo \*.csproj según corresponda.</span><span class="sxs-lookup"><span data-stu-id="879a2-211">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="879a2-212">Agregar archivos al flujo de trabajo</span><span class="sxs-lookup"><span data-stu-id="879a2-212">Add files to workflow</span></span>

<span data-ttu-id="879a2-213">Considere un ejemplo en el que más *custom.css* archivo se agrega similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="879a2-213">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="879a2-214">Para minimizar *custom.css* y empaquetarla con *site.css* en un *site.min.css* , agregue la ruta de acceso relativa *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="879a2-214">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="879a2-215">Como alternativa, podría utilizar el siguiente patrón de uso de comodines:</span><span class="sxs-lookup"><span data-stu-id="879a2-215">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> <span data-ttu-id="879a2-216">Este patrón global coincide con todos los archivos CSS y excluye el patrón de archivo minimizado.</span><span class="sxs-lookup"><span data-stu-id="879a2-216">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="879a2-217">Compile la aplicación.</span><span class="sxs-lookup"><span data-stu-id="879a2-217">Build the application.</span></span> <span data-ttu-id="879a2-218">Abra *site.min.css* y observe el contenido de *custom.css* se anexa al final del archivo.</span><span class="sxs-lookup"><span data-stu-id="879a2-218">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="879a2-219">Unión y minificación basado en el entorno</span><span class="sxs-lookup"><span data-stu-id="879a2-219">Environment-based bundling and minification</span></span>

<span data-ttu-id="879a2-220">Como práctica recomendada, se deben usar los archivos y minificados de la aplicación en un entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="879a2-220">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="879a2-221">Durante el desarrollo, asegúrese de los archivos originales para una depuración más sencilla de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="879a2-221">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="879a2-222">Especificar qué archivos incluir en las páginas mediante el [aplicación auxiliar de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) en las vistas.</span><span class="sxs-lookup"><span data-stu-id="879a2-222">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="879a2-223">La aplicación auxiliar de etiquetas de entorno solo procesa su contenido cuando se ejecuta en específico [entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="879a2-223">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="879a2-224">La siguiente `environment` etiqueta representa los archivos CSS sin procesar cuando se ejecuta en el `Development` entorno:</span><span class="sxs-lookup"><span data-stu-id="879a2-224">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

<span data-ttu-id="879a2-225">La siguiente `environment` etiqueta representa los archivos CSS y minificados cuando se ejecuta en un entorno distinto `Development`.</span><span class="sxs-lookup"><span data-stu-id="879a2-225">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="879a2-226">Por ejemplo, que se ejecutan en `Production` o `Staging` desencadena el procesamiento de estas hojas de estilo:</span><span class="sxs-lookup"><span data-stu-id="879a2-226">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="879a2-227">Consumir bundleconfig.json de Gulp</span><span class="sxs-lookup"><span data-stu-id="879a2-227">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="879a2-228">Hay casos en que el flujo de trabajo de unión y minificación de una aplicación requiere un procesamiento adicional.</span><span class="sxs-lookup"><span data-stu-id="879a2-228">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="879a2-229">Algunos ejemplos son la optimización de la imagen, limpieza de caché y el procesamiento de activos de red CDN.</span><span class="sxs-lookup"><span data-stu-id="879a2-229">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="879a2-230">Para satisfacer estos requisitos, que puede convertir el flujo de trabajo de unión y minificación para usar Gulp.</span><span class="sxs-lookup"><span data-stu-id="879a2-230">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="879a2-231">Use la extensión Bundler & Minifier</span><span class="sxs-lookup"><span data-stu-id="879a2-231">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="879a2-232">Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extensión controla la conversión a Gulp.</span><span class="sxs-lookup"><span data-stu-id="879a2-232">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="879a2-233">La extensión Bundler & Minifier pertenece a un proyecto controlado por la Comunidad en GitHub para el que Microsoft no proporciona soporte.</span><span class="sxs-lookup"><span data-stu-id="879a2-233">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="879a2-234">Se deben notificar problemas [aquí](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="879a2-234">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="879a2-235">Haga clic en el *bundleconfig.json* de archivos en el Explorador de soluciones y seleccione **Bundler & Minifier** > **convertir a Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="879a2-235">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Elemento de menú contextual de convertir a Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="879a2-237">El *gulpfile.js* y *package.json* archivos se agregan al proyecto.</span><span class="sxs-lookup"><span data-stu-id="879a2-237">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="879a2-238">La compatibilidad con [npm](https://www.npmjs.com/) paquetes que aparecen en la *package.json* del archivo `devDependencies` sección están instalados.</span><span class="sxs-lookup"><span data-stu-id="879a2-238">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="879a2-239">Ejecute el siguiente comando en la ventana PMC para instalar la CLI de Gulp como una dependencia global:</span><span class="sxs-lookup"><span data-stu-id="879a2-239">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="879a2-240">El *gulpfile.js* lecturas de archivos el *bundleconfig.json* archivo para las entradas, salidas y la configuración.</span><span class="sxs-lookup"><span data-stu-id="879a2-240">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="879a2-241">Convertir manualmente</span><span class="sxs-lookup"><span data-stu-id="879a2-241">Convert manually</span></span>

<span data-ttu-id="879a2-242">Si Visual Studio o la extensión Bundler & Minifier no está disponible, convertir manualmente.</span><span class="sxs-lookup"><span data-stu-id="879a2-242">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="879a2-243">Agregar un *package.json* archivo, por lo siguiente `devDependencies`, a la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="879a2-243">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="879a2-244">Instalar las dependencias ejecutando el comando siguiente en el mismo nivel que *package.json*:</span><span class="sxs-lookup"><span data-stu-id="879a2-244">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="879a2-245">Instale la CLI de Gulp como una dependencia global:</span><span class="sxs-lookup"><span data-stu-id="879a2-245">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="879a2-246">Copia el *gulpfile.js* por debajo del archivo a la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="879a2-246">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="879a2-247">Ejecutar las tareas de Gulp</span><span class="sxs-lookup"><span data-stu-id="879a2-247">Run Gulp tasks</span></span>

<span data-ttu-id="879a2-248">Para desencadenar la tarea de Gulp minificación antes de que el proyecto se compila en Visual Studio, agregue el siguiente [destino de MSBuild](/visualstudio/msbuild/msbuild-targets) al archivo \*.csproj:</span><span class="sxs-lookup"><span data-stu-id="879a2-248">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="879a2-249">En este ejemplo, las tareas se definen en el `MyPreCompileTarget` destino ejecutar antes predefinido `Build` destino.</span><span class="sxs-lookup"><span data-stu-id="879a2-249">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="879a2-250">Aparecerá un resultado similar al siguiente en la ventana de salida de Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="879a2-250">Output similar to the following appears in Visual Studio's Output window:</span></span>

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


## <a name="additional-resources"></a><span data-ttu-id="879a2-251">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="879a2-251">Additional resources</span></span>

* [<span data-ttu-id="879a2-252">Uso de Grunt</span><span class="sxs-lookup"><span data-stu-id="879a2-252">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="879a2-253">Uso de varios entornos</span><span class="sxs-lookup"><span data-stu-id="879a2-253">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="879a2-254">Asistentes de etiquetas</span><span class="sxs-lookup"><span data-stu-id="879a2-254">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
