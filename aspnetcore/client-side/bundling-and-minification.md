---
title: Agrupar y minificar en ASP.NET Core
author: scottaddie
description: "Obtenga información acerca de cómo optimizar recursos estáticos en una aplicación web de ASP.NET Core aplicando técnicas de agrupación y minificación."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/01/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: c271b7ef386bacedbd45fbe9f62c9c486db55b36
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2017
---
# <a name="bundling-and-minification"></a><span data-ttu-id="8a28c-103">Agrupar y minificar</span><span class="sxs-lookup"><span data-stu-id="8a28c-103">Bundling and minification</span></span>

<span data-ttu-id="8a28c-104">Por [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="8a28c-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="8a28c-105">Este artículo explica las ventajas de aplicar la agrupación y minificación, incluido cómo estas características pueden utilizarse con las aplicaciones web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8a28c-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="8a28c-106">¿Qué es agrupar y minificar?</span><span class="sxs-lookup"><span data-stu-id="8a28c-106">What is bundling and minification?</span></span>

<span data-ttu-id="8a28c-107">Unión y minificación son dos optimizaciones de rendimiento distintas que puede aplicar en una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="8a28c-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="8a28c-108">Se usan juntos, agrupar y minificar mejoran el rendimiento reduciendo el número de solicitudes de servidor y reducir el tamaño de los activos estáticos solicitados.</span><span class="sxs-lookup"><span data-stu-id="8a28c-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="8a28c-109">Agrupar y minificar mejoran principalmente el primer tiempo de carga de solicitud de página.</span><span class="sxs-lookup"><span data-stu-id="8a28c-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="8a28c-110">Una vez que se ha solicitado una página web, el explorador almacena en caché los activos estáticos (JavaScript, CSS e imágenes).</span><span class="sxs-lookup"><span data-stu-id="8a28c-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="8a28c-111">Por lo tanto, agrupar y minificar no mejoran el rendimiento cuando se solicita la misma página o páginas, en el mismo sitio que solicita los activos de la mismos.</span><span class="sxs-lookup"><span data-stu-id="8a28c-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="8a28c-112">Si no establece la expira encabezado correctamente en los activos, y si no utiliza la agrupación y minificación, heurística de actualización del explorador marca los activos obsoletas después de unos días.</span><span class="sxs-lookup"><span data-stu-id="8a28c-112">If you don't set the expires header correctly on your assets, and if you don’t use bundling and minification, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="8a28c-113">Además, el explorador requiere que una solicitud de validación para cada activo.</span><span class="sxs-lookup"><span data-stu-id="8a28c-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="8a28c-114">En este caso, agrupar y minificar proporcionan una mejora del rendimiento incluso después de la primera solicitud de página.</span><span class="sxs-lookup"><span data-stu-id="8a28c-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="8a28c-115">Cómo agrupar</span><span class="sxs-lookup"><span data-stu-id="8a28c-115">Bundling</span></span>

<span data-ttu-id="8a28c-116">Cómo agrupar combina varios archivos en un único archivo.</span><span class="sxs-lookup"><span data-stu-id="8a28c-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="8a28c-117">Cómo agrupar, reduce el número de solicitudes de servidor que son necesarios para representar un recurso web, por ejemplo, una página web.</span><span class="sxs-lookup"><span data-stu-id="8a28c-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="8a28c-118">Puede crear cualquier número de paquetes individuales específicamente para CSS, JavaScript, etcetera. Menos archivos significa menos solicitudes HTTP desde el explorador hasta el servidor o desde el servicio que proporciona la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8a28c-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="8a28c-119">El resultado de la mejora del rendimiento de carga de primera página.</span><span class="sxs-lookup"><span data-stu-id="8a28c-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="8a28c-120">Minificación</span><span class="sxs-lookup"><span data-stu-id="8a28c-120">Minification</span></span>

<span data-ttu-id="8a28c-121">Minificación quita caracteres innecesarios de código sin modificar la funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="8a28c-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="8a28c-122">El resultado es una reducción de un tamaño considerable en activos solicitados (por ejemplo, CSS, imágenes y archivos de JavaScript).</span><span class="sxs-lookup"><span data-stu-id="8a28c-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="8a28c-123">Los efectos secundarios comunes de minificación incluyen acortar los nombres de variable a un carácter y quitar los comentarios y espacios en blanco innecesarios.</span><span class="sxs-lookup"><span data-stu-id="8a28c-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="8a28c-124">Tenga en cuenta la siguiente función de JavaScript:</span><span class="sxs-lookup"><span data-stu-id="8a28c-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="8a28c-125">Minificación reduce la función a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="8a28c-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="8a28c-126">Además de quitar los comentarios y espacios en blanco innecesarios, los siguientes nombres de parámetro y la variable se cambió el nombre como sigue:</span><span class="sxs-lookup"><span data-stu-id="8a28c-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="8a28c-127">Original</span><span class="sxs-lookup"><span data-stu-id="8a28c-127">Original</span></span> | <span data-ttu-id="8a28c-128">Se cambia el nombre</span><span class="sxs-lookup"><span data-stu-id="8a28c-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="8a28c-129">Impacto de agrupar y minificar</span><span class="sxs-lookup"><span data-stu-id="8a28c-129">Impact of bundling and minification</span></span>

<span data-ttu-id="8a28c-130">En la tabla siguiente se describe las diferencias entre individualmente carga activos y el uso de agrupación y minificación:</span><span class="sxs-lookup"><span data-stu-id="8a28c-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="8a28c-131">Acción</span><span class="sxs-lookup"><span data-stu-id="8a28c-131">Action</span></span> | <span data-ttu-id="8a28c-132">Con B/M</span><span class="sxs-lookup"><span data-stu-id="8a28c-132">With B/M</span></span> | <span data-ttu-id="8a28c-133">Sin B/M</span><span class="sxs-lookup"><span data-stu-id="8a28c-133">Without B/M</span></span> | <span data-ttu-id="8a28c-134">Cambio</span><span class="sxs-lookup"><span data-stu-id="8a28c-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="8a28c-135">Solicitudes de archivos</span><span class="sxs-lookup"><span data-stu-id="8a28c-135">File Requests</span></span>  | <span data-ttu-id="8a28c-136">7</span><span class="sxs-lookup"><span data-stu-id="8a28c-136">7</span></span>   | <span data-ttu-id="8a28c-137">18</span><span class="sxs-lookup"><span data-stu-id="8a28c-137">18</span></span>     | <span data-ttu-id="8a28c-138">157%</span><span class="sxs-lookup"><span data-stu-id="8a28c-138">157%</span></span>
<span data-ttu-id="8a28c-139">KB transferido</span><span class="sxs-lookup"><span data-stu-id="8a28c-139">KB Transferred</span></span> | <span data-ttu-id="8a28c-140">156</span><span class="sxs-lookup"><span data-stu-id="8a28c-140">156</span></span> | <span data-ttu-id="8a28c-141">264.68</span><span class="sxs-lookup"><span data-stu-id="8a28c-141">264.68</span></span> | <span data-ttu-id="8a28c-142">70%</span><span class="sxs-lookup"><span data-stu-id="8a28c-142">70%</span></span>
<span data-ttu-id="8a28c-143">Tiempo de carga (ms)</span><span class="sxs-lookup"><span data-stu-id="8a28c-143">Load Time (ms)</span></span> | <span data-ttu-id="8a28c-144">885</span><span class="sxs-lookup"><span data-stu-id="8a28c-144">885</span></span> | <span data-ttu-id="8a28c-145">2360</span><span class="sxs-lookup"><span data-stu-id="8a28c-145">2360</span></span>   | <span data-ttu-id="8a28c-146">167%</span><span class="sxs-lookup"><span data-stu-id="8a28c-146">167%</span></span>

<span data-ttu-id="8a28c-147">Los exploradores son bastante detallados con respecto a los encabezados de solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="8a28c-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="8a28c-148">El número total de bytes enviados métrica vio una reducción significativa cuando se agrupa.</span><span class="sxs-lookup"><span data-stu-id="8a28c-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="8a28c-149">El tiempo de carga muestra una mejora considerable, sin embargo, en este ejemplo se ejecutaba localmente.</span><span class="sxs-lookup"><span data-stu-id="8a28c-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="8a28c-150">Mayores mejoras de rendimiento se percibe cuando el uso de agrupación y minificación con activos transfiere a través de una red.</span><span class="sxs-lookup"><span data-stu-id="8a28c-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="8a28c-151">Elegir una estrategia de agrupación y minificación</span><span class="sxs-lookup"><span data-stu-id="8a28c-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="8a28c-152">Las plantillas de proyecto MVC y las páginas de Razor proporcionan una solución para agrupar y minificar que consta de un archivo de configuración de JSON.</span><span class="sxs-lookup"><span data-stu-id="8a28c-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="8a28c-153">Herramientas de terceros, como el [Gulp](xref:client-side/using-gulp) y [Grunt](xref:client-side/using-grunt) corredores de tareas, realizar las mismas tareas con un poco más compleja.</span><span class="sxs-lookup"><span data-stu-id="8a28c-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="8a28c-154">Una herramienta de terceros es una buena elección cuando el flujo de trabajo de desarrollo requiere un procesamiento más allá de la agrupación y minificación&mdash;como optimización linting e imagen.</span><span class="sxs-lookup"><span data-stu-id="8a28c-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="8a28c-155">Mediante el uso de agrupación y minificación de tiempo de diseño, se crean los archivos reducidos antes de la implementación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8a28c-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="8a28c-156">Agrupar y minificar antes de la implementación proporcionan la ventaja de carga reducida del servidor.</span><span class="sxs-lookup"><span data-stu-id="8a28c-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="8a28c-157">Sin embargo, es importante reconocer que agrupación de tiempo de diseño y minificación aumenta la complejidad de la compilación y solo funciona con archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="8a28c-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="8a28c-158">Configurar la agrupación y minificación</span><span class="sxs-lookup"><span data-stu-id="8a28c-158">Configure bundling and minification</span></span>

<span data-ttu-id="8a28c-159">Las plantillas de proyecto MVC y las páginas de Razor proporcionan una *bundleconfig.json* archivo de configuración que define las opciones para cada paquete.</span><span class="sxs-lookup"><span data-stu-id="8a28c-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="8a28c-160">De forma predeterminada, se define una configuración de agrupación única para el código de JavaScript personalizado (*wwwroot/js/site.js*) y hojas de estilo (*wwwroot/css/site.css*) archivos:</span><span class="sxs-lookup"><span data-stu-id="8a28c-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="8a28c-161">Opciones de agrupación se incluyen:</span><span class="sxs-lookup"><span data-stu-id="8a28c-161">Bundle options include:</span></span>

* <span data-ttu-id="8a28c-162">`outputFileName`: El nombre del archivo de paquete para la salida.</span><span class="sxs-lookup"><span data-stu-id="8a28c-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="8a28c-163">Puede contener una ruta de acceso relativa desde la *bundleconfig.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="8a28c-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="8a28c-164">**Obligatorio**</span><span class="sxs-lookup"><span data-stu-id="8a28c-164">**required**</span></span>
* <span data-ttu-id="8a28c-165">`inputFiles`: Una matriz de archivos que se va a agrupar.</span><span class="sxs-lookup"><span data-stu-id="8a28c-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="8a28c-166">Estas son las rutas de acceso relativas al archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="8a28c-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="8a28c-167">**opcional**, * da como resultado un valor vacío en un archivo de resultados vacío.</span><span class="sxs-lookup"><span data-stu-id="8a28c-167">**optional**, *an empty value results in an empty output file.</span></span> <span data-ttu-id="8a28c-168">[uso de comodines](http://www.tldp.org/LDP/abs/html/globbingref.html) patrones son compatibles.</span><span class="sxs-lookup"><span data-stu-id="8a28c-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="8a28c-169">`minify`: Las opciones de reducción para el tipo de salida.</span><span class="sxs-lookup"><span data-stu-id="8a28c-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="8a28c-170">**opcional**, *predeterminado:`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="8a28c-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="8a28c-171">Opciones de configuración están disponibles por tipo de archivo de salida.</span><span class="sxs-lookup"><span data-stu-id="8a28c-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="8a28c-172">Minificador CSS</span><span class="sxs-lookup"><span data-stu-id="8a28c-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="8a28c-173">Minificador de JavaScript</span><span class="sxs-lookup"><span data-stu-id="8a28c-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="8a28c-174">Minificador de HTML</span><span class="sxs-lookup"><span data-stu-id="8a28c-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="8a28c-175">`includeInProject`: Marca que indica si se debe agregar los archivos generados al archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="8a28c-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="8a28c-176">**opcional**, *predeterminado: false*</span><span class="sxs-lookup"><span data-stu-id="8a28c-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="8a28c-177">`sourceMap`: Marca que indica si se debe generar un mapa de código fuente para el archivo agrupado.</span><span class="sxs-lookup"><span data-stu-id="8a28c-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="8a28c-178">**opcional**, *predeterminado: false*</span><span class="sxs-lookup"><span data-stu-id="8a28c-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="8a28c-179">`sourceMapRootPath`: La ruta de acceso raíz para almacenar el archivo de mapa de código fuente generado.</span><span class="sxs-lookup"><span data-stu-id="8a28c-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="8a28c-180">Ejecución en tiempo de compilación de agrupar y minificar</span><span class="sxs-lookup"><span data-stu-id="8a28c-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="8a28c-181">El [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) paquete NuGet permite la ejecución de agrupación y minificación en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="8a28c-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="8a28c-182">Inserta el paquete [destinos de MSBuild](/visualstudio/msbuild/msbuild-targets) que ejecutar en la compilación y tiempo de limpieza.</span><span class="sxs-lookup"><span data-stu-id="8a28c-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="8a28c-183">El *bundleconfig.json* archivo se analiza el proceso de compilación para generar los archivos de salida en función de la configuración definida.</span><span class="sxs-lookup"><span data-stu-id="8a28c-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8a28c-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8a28c-184">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="8a28c-185">Agregar el *BuildBundlerMinifier* paquete al proyecto.</span><span class="sxs-lookup"><span data-stu-id="8a28c-185">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="8a28c-186">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="8a28c-186">Build the project.</span></span> <span data-ttu-id="8a28c-187">Aparecerá el siguiente mensaje en la ventana de salida:</span><span class="sxs-lookup"><span data-stu-id="8a28c-187">The following appears in the Output window:</span></span>

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

<span data-ttu-id="8a28c-188">Limpie el proyecto.</span><span class="sxs-lookup"><span data-stu-id="8a28c-188">Clean the project.</span></span> <span data-ttu-id="8a28c-189">Aparecerá el siguiente mensaje en la ventana de salida:</span><span class="sxs-lookup"><span data-stu-id="8a28c-189">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8a28c-190">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="8a28c-190">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="8a28c-191">Agregar el *BuildBundlerMinifier* paquete al proyecto:</span><span class="sxs-lookup"><span data-stu-id="8a28c-191">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="8a28c-192">Si usa ASP.NET Core 1.x, restaurar el paquete recién agregado:</span><span class="sxs-lookup"><span data-stu-id="8a28c-192">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="8a28c-193">Compile el proyecto:</span><span class="sxs-lookup"><span data-stu-id="8a28c-193">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="8a28c-194">Aparecerá el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="8a28c-194">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="8a28c-195">Limpie el proyecto:</span><span class="sxs-lookup"><span data-stu-id="8a28c-195">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="8a28c-196">Aparecerá el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="8a28c-196">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="8a28c-197">Ejecución ad hoc de agrupar y minificar</span><span class="sxs-lookup"><span data-stu-id="8a28c-197">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="8a28c-198">Es posible ejecutar las tareas de agrupación y minificación de manera ad hoc, sin compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="8a28c-198">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="8a28c-199">Agregar el [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) paquete NuGet para el proyecto:</span><span class="sxs-lookup"><span data-stu-id="8a28c-199">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

<span data-ttu-id="8a28c-200">Este paquete extiende la CLI de núcleo de .NET para incluir la *dotnet agrupación* herramienta.</span><span class="sxs-lookup"><span data-stu-id="8a28c-200">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="8a28c-201">En la ventana de consola de administrador de paquetes (PMC) o en un shell de comandos, se puede ejecutar el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="8a28c-201">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="8a28c-202">Administrador de paquetes de NuGet agrega las dependencias en el archivo *.csproj como `<PackageReference />` nodos.</span><span class="sxs-lookup"><span data-stu-id="8a28c-202">NuGet Package Manager adds dependencies to the *.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="8a28c-203">El `dotnet bundle` comando está registrado con la CLI de .NET Core solo cuando un `<DotNetCliToolReference />` nodo se utiliza.</span><span class="sxs-lookup"><span data-stu-id="8a28c-203">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="8a28c-204">Modifique el archivo *.csproj según corresponda.</span><span class="sxs-lookup"><span data-stu-id="8a28c-204">Modify the *.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="8a28c-205">Agregar archivos al flujo de trabajo</span><span class="sxs-lookup"><span data-stu-id="8a28c-205">Add files to workflow</span></span>

<span data-ttu-id="8a28c-206">Considere un ejemplo en el que más *custom.css* archivo se agrega similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8a28c-206">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="8a28c-207">A minificar *custom.css* y agrupar con *site.css* en un *site.min.css* , agregue la ruta de acceso relativa a *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="8a28c-207">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="8a28c-208">Como alternativa, podría utilizar el siguiente patrón de uso de comodines en:</span><span class="sxs-lookup"><span data-stu-id="8a28c-208">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="8a28c-209">Este patrón de uso de comodines coincide con todos los archivos CSS y excluye el patrón de archivo reducida.</span><span class="sxs-lookup"><span data-stu-id="8a28c-209">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="8a28c-210">Compile la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8a28c-210">Build the application.</span></span> <span data-ttu-id="8a28c-211">Abra *site.min.css* y observe el contenido de *custom.css* se anexa al final del archivo.</span><span class="sxs-lookup"><span data-stu-id="8a28c-211">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="8a28c-212">Agrupar y minificar basado en el entorno</span><span class="sxs-lookup"><span data-stu-id="8a28c-212">Environment-based bundling and minification</span></span>

<span data-ttu-id="8a28c-213">Como práctica recomendada, los archivos integrados y reducidos de la aplicación deben usarse en un entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="8a28c-213">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="8a28c-214">Durante el desarrollo, los archivos originales se realizan para una depuración más sencilla de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8a28c-214">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="8a28c-215">Especificar qué archivos desea incluir en las páginas mediante el [auxiliar de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) en las vistas.</span><span class="sxs-lookup"><span data-stu-id="8a28c-215">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="8a28c-216">La aplicación auxiliar de etiquetas de entorno solo representa su contenido cuando se ejecuta en específico [entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="8a28c-216">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="8a28c-217">El siguiente `environment` etiqueta representa los archivos sin procesar de CSS cuando se ejecuta en el `Development` entorno:</span><span class="sxs-lookup"><span data-stu-id="8a28c-217">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8a28c-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8a28c-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8a28c-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8a28c-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

<span data-ttu-id="8a28c-220">El siguiente `environment` etiqueta representa los archivos CSS agrupados y reducidos cuando se ejecuta en un entorno distinto de `Development`.</span><span class="sxs-lookup"><span data-stu-id="8a28c-220">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="8a28c-221">Por ejemplo, que se ejecutan `Production` o `Staging` desencadena el procesamiento de estas hojas de estilos:</span><span class="sxs-lookup"><span data-stu-id="8a28c-221">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8a28c-222">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8a28c-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8a28c-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8a28c-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="8a28c-224">Consumir bundleconfig.json desde Gulp</span><span class="sxs-lookup"><span data-stu-id="8a28c-224">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="8a28c-225">Hay casos en los que el flujo de trabajo de una aplicación agrupar y minificar requiere un procesamiento adicional.</span><span class="sxs-lookup"><span data-stu-id="8a28c-225">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="8a28c-226">Algunos ejemplos son la optimización de la imagen, la desactivación de caché y el procesamiento del recurso de red CDN.</span><span class="sxs-lookup"><span data-stu-id="8a28c-226">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="8a28c-227">Para satisfacer estos requisitos, que puede convertir el flujo de trabajo de agrupación y minificación para usar Gulp.</span><span class="sxs-lookup"><span data-stu-id="8a28c-227">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="8a28c-228">Utilizar la extensión de paquete de instalación & Minificador</span><span class="sxs-lookup"><span data-stu-id="8a28c-228">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="8a28c-229">Visual Studio [paquete de instalación & Minificador](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extensión controla la conversión a Gulp.</span><span class="sxs-lookup"><span data-stu-id="8a28c-229">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

<span data-ttu-id="8a28c-230">Haga clic en el *bundleconfig.json* un archivo en el Explorador de soluciones y seleccione **paquete de instalación & Minificador** > **convertir a Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="8a28c-230">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Convertir a Gulp elemento del menú contextual](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="8a28c-232">El *gulpfile.js* y *package.json* archivos se agregan al proyecto.</span><span class="sxs-lookup"><span data-stu-id="8a28c-232">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="8a28c-233">La compatibilidad con [npm](https://www.npmjs.com/) paquetes incluidos en el *package.json* del archivo `devDependencies` sección están instalados.</span><span class="sxs-lookup"><span data-stu-id="8a28c-233">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="8a28c-234">Ejecute el siguiente comando en la ventana PMC para instalar la CLI Gulp como una dependencia global:</span><span class="sxs-lookup"><span data-stu-id="8a28c-234">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="8a28c-235">El *gulpfile.js* lecturas de archivos la *bundleconfig.json* archivo de entradas, salidas y configuración.</span><span class="sxs-lookup"><span data-stu-id="8a28c-235">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="8a28c-236">Convertir manualmente</span><span class="sxs-lookup"><span data-stu-id="8a28c-236">Convert manually</span></span>

<span data-ttu-id="8a28c-237">Si Visual Studio o la extensión del paquete de instalación & Minificador no está disponible, convertir manualmente.</span><span class="sxs-lookup"><span data-stu-id="8a28c-237">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="8a28c-238">Agregar un *package.json* archivo, con el siguiente `devDependencies`, a la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="8a28c-238">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="8a28c-239">Instalar las dependencias ejecutando el siguiente comando en el mismo nivel que *package.json*:</span><span class="sxs-lookup"><span data-stu-id="8a28c-239">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="8a28c-240">Instale la CLI Gulp como una dependencia global:</span><span class="sxs-lookup"><span data-stu-id="8a28c-240">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="8a28c-241">Copia la *gulpfile.js* por debajo del archivo a la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="8a28c-241">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="8a28c-242">Ejecutar tareas de Gulp</span><span class="sxs-lookup"><span data-stu-id="8a28c-242">Run Gulp tasks</span></span>

<span data-ttu-id="8a28c-243">Para desencadenar la tarea de preparación de minificación Gulp antes de que el proyecto se compila en Visual Studio, agregue las siguientes [destino de MSBuild](/visualstudio/msbuild/msbuild-targets) al archivo *.csproj:</span><span class="sxs-lookup"><span data-stu-id="8a28c-243">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the *.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="8a28c-244">En este ejemplo, las tareas se definen en el `MyPreCompileTarget` destino ejecutar antes predefinido `Build` destino.</span><span class="sxs-lookup"><span data-stu-id="8a28c-244">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="8a28c-245">Aparecerá un resultado similar al siguiente en la ventana de salida de Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="8a28c-245">Output similar to the following appears in Visual Studio's Output window:</span></span>

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

<span data-ttu-id="8a28c-246">También se puede usar el explorador del ejecutor de tareas de Visual Studio para enlazar Gulp tareas a eventos específicos de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8a28c-246">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="8a28c-247">Vea [ejecutando tareas predeterminadas](xref:client-side/using-gulp#running-default-tasks) para obtener instrucciones sobre cómo hacer que.</span><span class="sxs-lookup"><span data-stu-id="8a28c-247">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8a28c-248">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8a28c-248">Additional resources</span></span>

* [<span data-ttu-id="8a28c-249">Uso de Gulp</span><span class="sxs-lookup"><span data-stu-id="8a28c-249">Using Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="8a28c-250">Uso de Grunt</span><span class="sxs-lookup"><span data-stu-id="8a28c-250">Using Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="8a28c-251">Trabajar con varios entornos</span><span class="sxs-lookup"><span data-stu-id="8a28c-251">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="8a28c-252">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="8a28c-252">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
