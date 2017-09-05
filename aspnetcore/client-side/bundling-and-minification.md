---
title: Agrupar y minificar en ASP.NET Core
author: spboyer
description: 
keywords: "ASP.NET básicos, agrupación y Minificación, CSS, JavaScript, Minificar, BuildBundlerMinifier"
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.assetid: d54230f9-8e5f-4861-a29c-1d3a14e0b0d9
ms.technology: aspnet
ms.prod: aspnet-core
uid: client-side/bundling-and-minification
ms.openlocfilehash: 810d89798330aeb1c387ec85eb05b1f4efde167a
ms.sourcegitcommit: 275a5381b6172b4f0b5fcd1d252aff03d3dae166
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a><span data-ttu-id="fd09f-103">Agrupar y minificar en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fd09f-103">Bundling and minification in ASP.NET Core</span></span>

<span data-ttu-id="fd09f-104">Agrupar y minificar son dos técnicas puede usar en ASP.NET para mejorar el rendimiento de la carga de página para la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="fd09f-104">Bundling and minification are two techniques you can use in ASP.NET to improve page load performance for your web application.</span></span> <span data-ttu-id="fd09f-105">Cómo agrupar combina varios archivos en un único archivo.</span><span class="sxs-lookup"><span data-stu-id="fd09f-105">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="fd09f-106">Minificación realiza una serie de optimizaciones de código diferente para las secuencias de comandos y CSS, que da lugar a unas cargas menores.</span><span class="sxs-lookup"><span data-stu-id="fd09f-106">Minification performs a variety of different code optimizations to scripts and CSS, which results in smaller payloads.</span></span> <span data-ttu-id="fd09f-107">Usan juntos, agrupar y minificar mejora el rendimiento en tiempo de carga, lo que reduce el número de solicitudes al servidor y reducir el tamaño de los activos solicitados (como archivos CSS y JavaScript).</span><span class="sxs-lookup"><span data-stu-id="fd09f-107">Used together, bundling and minification improves load time performance by reducing the number of requests to the server and reducing the size of the requested assets (such as CSS and JavaScript files).</span></span>

<span data-ttu-id="fd09f-108">Este artículo explica las ventajas de usar la agrupación y minificación, incluido cómo estas características pueden utilizarse con aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd09f-108">This article explains the benefits of using bundling and minification, including how these features can be used with ASP.NET Core applications.</span></span>

## <a name="overview"></a><span data-ttu-id="fd09f-109">Información general</span><span class="sxs-lookup"><span data-stu-id="fd09f-109">Overview</span></span>

<span data-ttu-id="fd09f-110">En las aplicaciones ASP.NET Core, existen varias opciones para agrupar y minificar recursos del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="fd09f-110">In ASP.NET Core apps, there are multiple options for bundling and minifying client-side resources.</span></span> <span data-ttu-id="fd09f-111">Las plantillas de principal para MVC proporcionan una solución de fábrica mediante un archivo de configuración y el paquete BuildBundlerMinifier NuGet.</span><span class="sxs-lookup"><span data-stu-id="fd09f-111">The core templates for MVC provide an out-of-the-box solution using a configuration file and BuildBundlerMinifier NuGet package.</span></span> <span data-ttu-id="fd09f-112">Herramientas de terceros, como [Gulp](using-gulp.md) y [Grunt](using-grunt.md) también están disponibles para realizar las mismas tareas deben requerir los procesos de flujo de trabajo adicional o complejidades.</span><span class="sxs-lookup"><span data-stu-id="fd09f-112">Third party tools, such as [Gulp](using-gulp.md) and [Grunt](using-grunt.md) are also available to accomplish the same tasks should your processes require additional workflow or complexities.</span></span> <span data-ttu-id="fd09f-113">Mediante el uso de agrupación y minificación de tiempo de diseño, se crean los archivos reducidos antes de la implementación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fd09f-113">By using design-time bundling and minification, the minified files are created prior to the application’s deployment.</span></span> <span data-ttu-id="fd09f-114">Agrupar y minificar antes de la implementación proporcionan la ventaja de carga reducida del servidor.</span><span class="sxs-lookup"><span data-stu-id="fd09f-114">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="fd09f-115">Sin embargo, es importante reconocer que agrupación de tiempo de diseño y minificación aumenta la complejidad de la compilación y solo funciona con archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="fd09f-115">However, it’s important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

<span data-ttu-id="fd09f-116">Agrupar y minificar mejoran principalmente el primer tiempo de carga de solicitud de página.</span><span class="sxs-lookup"><span data-stu-id="fd09f-116">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="fd09f-117">Una vez que se ha solicitado una página web, el explorador se almacena en memoria caché los activos (JavaScript, CSS e imágenes) para agrupar y minificar no proporcionan ningún aumento del rendimiento cuando se solicita la misma página o páginas en el mismo sitio que solicita los activos de la mismos.</span><span class="sxs-lookup"><span data-stu-id="fd09f-117">Once a web page has been requested, the browser caches the assets (JavaScript, CSS and images) so bundling and minification won’t provide any performance boost when requesting the same page, or pages on the same site requesting the same assets.</span></span> <span data-ttu-id="fd09f-118">Si no establece la expira encabezado correctamente en los activos y no use agrupar y minificar, heurística de actualización del explorador, se marcarán como los recursos obsoletos después de unos días y el explorador requerirá una solicitud de validación para cada recurso.</span><span class="sxs-lookup"><span data-stu-id="fd09f-118">If you don’t set the expires header correctly on your assets, and you don’t use bundling and minification, the browser's freshness heuristics will mark the assets stale after a few days and the browser will require a validation request for each asset.</span></span> <span data-ttu-id="fd09f-119">En este caso, agrupar y minificar proporcionan un aumento del rendimiento incluso después de la primera solicitud de página.</span><span class="sxs-lookup"><span data-stu-id="fd09f-119">In this case, bundling and minification provide a performance increase even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="fd09f-120">Cómo agrupar</span><span class="sxs-lookup"><span data-stu-id="fd09f-120">Bundling</span></span>

<span data-ttu-id="fd09f-121">Cómo agrupar es una característica que facilita el proceso combinar o agrupar varios archivos en un único archivo.</span><span class="sxs-lookup"><span data-stu-id="fd09f-121">Bundling is a feature that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="fd09f-122">Porque agrupación combina varios archivos en un único archivo, reduce el número de solicitudes al servidor que se necesitan para recuperar y mostrar un recurso web, por ejemplo, una página web.</span><span class="sxs-lookup"><span data-stu-id="fd09f-122">Because bundling combines multiple files into a single file, it reduces the number of requests to the server that are required to retrieve and display a web asset, such as a web page.</span></span> <span data-ttu-id="fd09f-123">Puede crear CSS, JavaScript y otros paquetes.</span><span class="sxs-lookup"><span data-stu-id="fd09f-123">You can create CSS, JavaScript and other bundles.</span></span> <span data-ttu-id="fd09f-124">Menos archivos significa menos solicitudes HTTP desde el explorador hasta el servidor o desde el servicio que proporciona la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fd09f-124">Fewer files means fewer HTTP requests from your browser to the server or from the service providing your application.</span></span> <span data-ttu-id="fd09f-125">El resultado de la mejora del rendimiento de carga de primera página.</span><span class="sxs-lookup"><span data-stu-id="fd09f-125">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="fd09f-126">Minificación</span><span class="sxs-lookup"><span data-stu-id="fd09f-126">Minification</span></span>

<span data-ttu-id="fd09f-127">Minificación realiza una serie de optimizaciones de código diferente para reducir el tamaño de los activos solicitados (por ejemplo, CSS, imágenes, archivos de JavaScript).</span><span class="sxs-lookup"><span data-stu-id="fd09f-127">Minification performs a variety of different code optimizations to reduce the size of requested assets (such as CSS, images, JavaScript files).</span></span> <span data-ttu-id="fd09f-128">Resultados comunes de reducción incluyen quitando el espacio en blanco innecesario y comentarios y reducir los nombres de variable con un único carácter.</span><span class="sxs-lookup"><span data-stu-id="fd09f-128">Common results of minification include removing unnecessary white space and comments, and shortening variable names to one character.</span></span>

<span data-ttu-id="fd09f-129">Tenga en cuenta la siguiente función de JavaScript:</span><span class="sxs-lookup"><span data-stu-id="fd09f-129">Consider the following JavaScript function:</span></span>

```javascript
AddAltToImg = function (imageTagAndImageID, imageContext) {
  ///<signature>
  ///<summary> Adds an alt tab to the image
  // </summary>
  //<param name="imgElement" type="String">The image selector.</param>
  //<param name="ContextForImage" type="String">The image context.</param>
  ///</signature>
  var imageElement = $(imageTagAndImageID, imageContext);
  imageElement.attr('alt', imageElement.attr('id').replace(/ID/, ''));
}
```

<span data-ttu-id="fd09f-130">Después de la reducción, la función se reduce a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="fd09f-130">After minification, the function is reduced to the following:</span></span>

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

<span data-ttu-id="fd09f-131">Además de quitar los comentarios y espacios en blanco innecesarios, los siguientes parámetros y nombres de variables se cambió el nombre (abreviar) como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="fd09f-131">In addition to removing the comments and unnecessary whitespace, the following parameters and variable names were renamed (shortened) as follows:</span></span>

<span data-ttu-id="fd09f-132">Original</span><span class="sxs-lookup"><span data-stu-id="fd09f-132">Original</span></span> | <span data-ttu-id="fd09f-133">Se cambia el nombre</span><span class="sxs-lookup"><span data-stu-id="fd09f-133">Renamed</span></span>
--- | :---:
<span data-ttu-id="fd09f-134">imageTagAndImageID</span><span class="sxs-lookup"><span data-stu-id="fd09f-134">imageTagAndImageID</span></span> | <span data-ttu-id="fd09f-135">m</span><span class="sxs-lookup"><span data-stu-id="fd09f-135">t</span></span>
<span data-ttu-id="fd09f-136">imageContext</span><span class="sxs-lookup"><span data-stu-id="fd09f-136">imageContext</span></span> | <span data-ttu-id="fd09f-137">a</span><span class="sxs-lookup"><span data-stu-id="fd09f-137">a</span></span>
<span data-ttu-id="fd09f-138">imageElement</span><span class="sxs-lookup"><span data-stu-id="fd09f-138">imageElement</span></span> | <span data-ttu-id="fd09f-139">c</span><span class="sxs-lookup"><span data-stu-id="fd09f-139">r</span></span>

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="fd09f-140">Impacto de agrupar y minificar</span><span class="sxs-lookup"><span data-stu-id="fd09f-140">Impact of bundling and minification</span></span>

<span data-ttu-id="fd09f-141">La siguiente tabla muestra algunas diferencias importantes entre enumerar todos los activos de forma individual y el uso de agrupación y minificación en una página web sencilla:</span><span class="sxs-lookup"><span data-stu-id="fd09f-141">The following table shows several important differences between listing all the assets individually and using bundling and minification on a simple web page:</span></span>

<span data-ttu-id="fd09f-142">Acción</span><span class="sxs-lookup"><span data-stu-id="fd09f-142">Action</span></span> | <span data-ttu-id="fd09f-143">Con B/M</span><span class="sxs-lookup"><span data-stu-id="fd09f-143">With B/M</span></span> | <span data-ttu-id="fd09f-144">Sin B/M</span><span class="sxs-lookup"><span data-stu-id="fd09f-144">Without B/M</span></span> | <span data-ttu-id="fd09f-145">Cambio</span><span class="sxs-lookup"><span data-stu-id="fd09f-145">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="fd09f-146">Solicitudes de archivos</span><span class="sxs-lookup"><span data-stu-id="fd09f-146">File Requests</span></span> |<span data-ttu-id="fd09f-147">7</span><span class="sxs-lookup"><span data-stu-id="fd09f-147">7</span></span> | <span data-ttu-id="fd09f-148">18</span><span class="sxs-lookup"><span data-stu-id="fd09f-148">18</span></span> | <span data-ttu-id="fd09f-149">157%</span><span class="sxs-lookup"><span data-stu-id="fd09f-149">157%</span></span>
<span data-ttu-id="fd09f-150">KB transferido</span><span class="sxs-lookup"><span data-stu-id="fd09f-150">KB Transferred</span></span> | <span data-ttu-id="fd09f-151">156</span><span class="sxs-lookup"><span data-stu-id="fd09f-151">156</span></span> | <span data-ttu-id="fd09f-152">264.68</span><span class="sxs-lookup"><span data-stu-id="fd09f-152">264.68</span></span> | <span data-ttu-id="fd09f-153">70%</span><span class="sxs-lookup"><span data-stu-id="fd09f-153">70%</span></span>
<span data-ttu-id="fd09f-154">Tiempo de carga (MS)</span><span class="sxs-lookup"><span data-stu-id="fd09f-154">Load Time (MS)</span></span> | <span data-ttu-id="fd09f-155">885</span><span class="sxs-lookup"><span data-stu-id="fd09f-155">885</span></span> | <span data-ttu-id="fd09f-156">2360</span><span class="sxs-lookup"><span data-stu-id="fd09f-156">2360</span></span> | <span data-ttu-id="fd09f-157">167%</span><span class="sxs-lookup"><span data-stu-id="fd09f-157">167%</span></span>

<span data-ttu-id="fd09f-158">Los bytes enviados tenían una reducción significativa de la agrupación sea bastante detallados con los encabezados HTTP que se aplican en las solicitudes de los exploradores.</span><span class="sxs-lookup"><span data-stu-id="fd09f-158">The bytes sent had a significant reduction with bundling as browsers are fairly verbose with the HTTP headers that they apply on requests.</span></span> <span data-ttu-id="fd09f-159">El tiempo de carga muestra una gran mejora, sin embargo, en este ejemplo se ejecute localmente.</span><span class="sxs-lookup"><span data-stu-id="fd09f-159">The load time shows a big improvement, however this example was run locally.</span></span> <span data-ttu-id="fd09f-160">Obtendrá mayores ganancias de rendimiento al uso de agrupación y minificación con activos transfiere a través de una red.</span><span class="sxs-lookup"><span data-stu-id="fd09f-160">You will get greater gains in performance when using bundling and minification with assets transferred over a network.</span></span>

## <a name="using-bundling-and-minification-in-a-project"></a><span data-ttu-id="fd09f-161">Utilizar agrupar y minificar en un proyecto</span><span class="sxs-lookup"><span data-stu-id="fd09f-161">Using bundling and minification in a project</span></span>

<span data-ttu-id="fd09f-162">La plantilla de proyecto MVC proporciona una `bundleconfig.json` archivo de configuración que define las opciones para cada paquete.</span><span class="sxs-lookup"><span data-stu-id="fd09f-162">The MVC project template provides a `bundleconfig.json` configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="fd09f-163">De forma predeterminada, se define una configuración de agrupación única para el código de JavaScript personalizado (`wwwroot/js/site.js`) y hojas de estilo (`wwwroot/css/site.css`) archivos.</span><span class="sxs-lookup"><span data-stu-id="fd09f-163">By default, a single bundle configuration is defined for the custom JavaScript (`wwwroot/js/site.js`) and Stylesheet (`wwwroot/css/site.css`) files.</span></span>

<span data-ttu-id="fd09f-164">[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]</span><span class="sxs-lookup"><span data-stu-id="fd09f-164">[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]</span></span>

<span data-ttu-id="fd09f-165">Opciones de agrupación se incluyen:</span><span class="sxs-lookup"><span data-stu-id="fd09f-165">Bundle options include:</span></span>

* <span data-ttu-id="fd09f-166">outputFileName - nombre del archivo de paquete para la salida.</span><span class="sxs-lookup"><span data-stu-id="fd09f-166">outputFileName - name of the bundle file to output.</span></span> <span data-ttu-id="fd09f-167">Puede contener una ruta de acceso relativa desde la `bundleconfig.json` archivo.</span><span class="sxs-lookup"><span data-stu-id="fd09f-167">Can contain a relative path from the `bundleconfig.json` file.</span></span> <span data-ttu-id="fd09f-168">**Obligatorio**</span><span class="sxs-lookup"><span data-stu-id="fd09f-168">**required**</span></span>
* <span data-ttu-id="fd09f-169">inputFiles - matriz de archivos que se va a agrupar.</span><span class="sxs-lookup"><span data-stu-id="fd09f-169">inputFiles - array of files to bundle together.</span></span> <span data-ttu-id="fd09f-170">Estas son las rutas de acceso relativas al archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="fd09f-170">These are relative paths to the configuration file.</span></span> <span data-ttu-id="fd09f-171">**opcional**, * da como resultado un valor vacío en un archivo de resultados vacío.</span><span class="sxs-lookup"><span data-stu-id="fd09f-171">**optional**, *an empty value results in an empty output file.</span></span> <span data-ttu-id="fd09f-172">[uso de comodines](http://www.tldp.org/LDP/abs/html/globbingref.html) patrones son compatibles.</span><span class="sxs-lookup"><span data-stu-id="fd09f-172">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="fd09f-173">minificar - escriba opciones de reducción de la salida.</span><span class="sxs-lookup"><span data-stu-id="fd09f-173">minify - minification options for the output type.</span></span> <span data-ttu-id="fd09f-174">**opcional**, *predeterminado:`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="fd09f-174">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="fd09f-175">Opciones de configuración están disponibles por tipo de archivo de salida.</span><span class="sxs-lookup"><span data-stu-id="fd09f-175">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="fd09f-176">Minificador CSS</span><span class="sxs-lookup"><span data-stu-id="fd09f-176">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="fd09f-177">Minificador de JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd09f-177">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/jsminifier)
    * [<span data-ttu-id="fd09f-178">Minificador de HTML</span><span class="sxs-lookup"><span data-stu-id="fd09f-178">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/htmlminifier)
* <span data-ttu-id="fd09f-179">includeInProject - agregar archivos generados al archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="fd09f-179">includeInProject - add generated files to project file.</span></span> <span data-ttu-id="fd09f-180">**opcional**, *predeterminado: false*</span><span class="sxs-lookup"><span data-stu-id="fd09f-180">**optional**, *default - false*</span></span>
* <span data-ttu-id="fd09f-181">sourceMaps - generar mapas de código fuente para el archivo agrupado.</span><span class="sxs-lookup"><span data-stu-id="fd09f-181">sourceMaps - generate source maps for the bundled file.</span></span> <span data-ttu-id="fd09f-182">**opcional**, *predeterminado: false*</span><span class="sxs-lookup"><span data-stu-id="fd09f-182">**optional**, *default - false*</span></span>

### <a name="visual-studio-2015--2017"></a><span data-ttu-id="fd09f-183">Visual Studio 2015 / 2017</span><span class="sxs-lookup"><span data-stu-id="fd09f-183">Visual Studio 2015 / 2017</span></span>

<span data-ttu-id="fd09f-184">Abra `bundleconfig.json` en Visual Studio, si su entorno no tiene la extensión instalada; un símbolo del sistema se presenta lo que sugiere que hay uno que puede ayudar a este tipo de archivo.</span><span class="sxs-lookup"><span data-stu-id="fd09f-184">Open `bundleconfig.json` in Visual Studio, if your environment does not have the extension installed; a prompt is presented suggesting that there is one that could assist with this file type.</span></span>

![Sugerencia de extensión BuildBundlerMinifier](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

<span data-ttu-id="fd09f-186">Seleccione ver extensiones e instalar la **paquete de instalación & Minificador** extensión (reiniciar requiere Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="fd09f-186">Select View Extensions, and install the **Bundler & Minifier** extension (Requires Visual Studio restart).</span></span>

![Sugerencia de extensión BuildBundlerMinifier](../client-side/bundling-and-minification/_static/view-extension.png)

<span data-ttu-id="fd09f-188">Una vez completado el reinicio, debe configurar la compilación para ejecutar los procesos de minificación y agrupación de los activos de cliente.</span><span class="sxs-lookup"><span data-stu-id="fd09f-188">When the restart is complete, you need to configure the build to run the processes of minifying and bundling the client-side assets.</span></span> <span data-ttu-id="fd09f-189">Haga clic en el `bundleconfig.json` de archivos y seleccione *habilitar agrupación en la compilación...* .</span><span class="sxs-lookup"><span data-stu-id="fd09f-189">Right-click the `bundleconfig.json` file and select *Enable bundle on build...*.</span></span>

<span data-ttu-id="fd09f-190">Compile el proyecto y el `bundleconfig.json` se incluye en el proceso de compilación para generar los archivos de salida en función de la configuración.</span><span class="sxs-lookup"><span data-stu-id="fd09f-190">Build the project, and the `bundleconfig.json` is included in the build process to produce the output files based on the configuration.</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a><span data-ttu-id="fd09f-191">Código de Visual Studio o la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="fd09f-191">Visual Studio Code or Command Line</span></span>

<span data-ttu-id="fd09f-192">Visual Studio y la extensión de unidad el proceso de agrupación y minificación con movimientos de GUI; Sin embargo, las mismas capacidades están disponibles con el `dotnet` paquete CLI y BuildBundlerMinifier NuGet.</span><span class="sxs-lookup"><span data-stu-id="fd09f-192">Visual Studio and the extension drive the bundling and minification process using GUI gestures; however, the same capabilities are available with the `dotnet` CLI and BuildBundlerMinifier NuGet package.</span></span>

<span data-ttu-id="fd09f-193">Agregue el paquete NuGet al proyecto:</span><span class="sxs-lookup"><span data-stu-id="fd09f-193">Add the NuGet package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="fd09f-194">Restaurar las dependencias:</span><span class="sxs-lookup"><span data-stu-id="fd09f-194">Restore the dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="fd09f-195">Compile la aplicación:</span><span class="sxs-lookup"><span data-stu-id="fd09f-195">Build the app:</span></span>

```console
dotnet build
```

<span data-ttu-id="fd09f-196">La salida del comando de compilación muestra los resultados de la minificación y cómo agrupar según la configuración establecida.</span><span class="sxs-lookup"><span data-stu-id="fd09f-196">The output from the build command shows the results of the minification and/or bundling according to what is configured.</span></span>

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a><span data-ttu-id="fd09f-197">Agregar archivos</span><span class="sxs-lookup"><span data-stu-id="fd09f-197">Adding files</span></span>

<span data-ttu-id="fd09f-198">En este ejemplo, se agrega un archivo CSS adicional llamado `custom.css` y configurado para agrupación y minificación con `site.css`, da lugar a una sola `site.min.css`.</span><span class="sxs-lookup"><span data-stu-id="fd09f-198">In this example, an additional CSS file is added called `custom.css` and configured for bundling and minification with `site.css`, resulting in a single `site.min.css`.</span></span>

<span data-ttu-id="fd09f-199">Custom.CSS</span><span class="sxs-lookup"><span data-stu-id="fd09f-199">custom.css</span></span>

```css
.about, [role=main], [role=complementary]
{
    margin-top: 60px;
}

footer
{
    margin-top: 10px;
}
```

<span data-ttu-id="fd09f-200">Agregar la ruta de acceso relativa a `bundleconfig.json`.</span><span class="sxs-lookup"><span data-stu-id="fd09f-200">Add the relative path to `bundleconfig.json`.</span></span>

<span data-ttu-id="fd09f-201">[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]</span><span class="sxs-lookup"><span data-stu-id="fd09f-201">[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]</span></span>

> [!NOTE]
> <span data-ttu-id="fd09f-202">También se puede usar el patrón de uso de comodines - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` que obtiene CSS todos los archivos y se excluye el patrón de archivo reducida.</span><span class="sxs-lookup"><span data-stu-id="fd09f-202">Alternatively, the globbing pattern could be used - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` which gets all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="fd09f-203">Compilar la aplicación y si abre `site.min.css`, ahora observará que el contenido de `custom.css` se ha anexado al final del archivo.</span><span class="sxs-lookup"><span data-stu-id="fd09f-203">Build the application and if you open `site.min.css`, you'll now notice that contents of `custom.css` has been appended to the end of the file.</span></span>

## <a name="controlling-bundling-and-minification"></a><span data-ttu-id="fd09f-204">Controlar la agrupación y minificación</span><span class="sxs-lookup"><span data-stu-id="fd09f-204">Controlling bundling and minification</span></span>

<span data-ttu-id="fd09f-205">En general, desea utilizar los archivos integrados y reducidos de la aplicación únicamente en un entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="fd09f-205">In general, you want to use the bundled and minified files of your app only in a production environment.</span></span> <span data-ttu-id="fd09f-206">Durante el desarrollo, desea utilizar los archivos originales, por lo que es más fácil de depurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fd09f-206">During development, you want to use your original files so your app is easier to debug.</span></span>

<span data-ttu-id="fd09f-207">Puede especificar qué secuencias de comandos y archivos CSS que se incluirán en las páginas utilizando la aplicación auxiliar de etiquetas de entorno en las páginas de diseño (vea [aplicaciones auxiliares de etiquetas](../mvc/views/tag-helpers/index.md)).</span><span class="sxs-lookup"><span data-stu-id="fd09f-207">You can specify which scripts and CSS files to include in your pages using the environment tag helper in your layout pages (see [Tag Helpers](../mvc/views/tag-helpers/index.md)).</span></span> <span data-ttu-id="fd09f-208">La aplicación auxiliar de etiquetas de entorno solo representará su contenido cuando se ejecuta en entornos específicos.</span><span class="sxs-lookup"><span data-stu-id="fd09f-208">The environment tag helper will only render its contents when running in specific environments.</span></span> <span data-ttu-id="fd09f-209">Vea [trabajar con varios entornos](../fundamentals/environments.md) para obtener más información sobre cómo especificar el entorno actual.</span><span class="sxs-lookup"><span data-stu-id="fd09f-209">See [Working with Multiple Environments](../fundamentals/environments.md) for details on specifying the current environment.</span></span>

<span data-ttu-id="fd09f-210">La siguiente etiqueta de entorno representará los archivos sin procesar de CSS cuando se ejecuta en el `Development` entorno:</span><span class="sxs-lookup"><span data-stu-id="fd09f-210">The following environment tag will render the unprocessed CSS files when running in the `Development` environment:</span></span>

<span data-ttu-id="fd09f-211">[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]</span><span class="sxs-lookup"><span data-stu-id="fd09f-211">[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]</span></span>

<span data-ttu-id="fd09f-212">Esta etiqueta de entorno representará los archivos CSS agrupados y reducidos solo cuando se ejecuta en `Production` o `Staging`:</span><span class="sxs-lookup"><span data-stu-id="fd09f-212">This environment tag will render the bundled and minified CSS files only when running in `Production` or `Staging`:</span></span>

<span data-ttu-id="fd09f-213">[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]</span><span class="sxs-lookup"><span data-stu-id="fd09f-213">[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]</span></span>

## <a name="consuming-bundleconfigjson-from-gulp"></a><span data-ttu-id="fd09f-214">Consumir bundleconfig.json desde Gulp</span><span class="sxs-lookup"><span data-stu-id="fd09f-214">Consuming bundleconfig.json from Gulp</span></span>

<span data-ttu-id="fd09f-215">Si el flujo de trabajo de agrupación y minificación de aplicación requiere procesos adicionales, como el procesamiento de imágenes, la desactivación de caché, el procesamiento de red CDN assest, etc., puede convertir el proceso de agrupación y Minify a Gulp.</span><span class="sxs-lookup"><span data-stu-id="fd09f-215">If your app bundling and minification workflow requires additional processes such as image processing, cache busting, CDN assest processing, etc., then you can convert the Bundle and Minify process to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="fd09f-216">Opción de conversión solo está disponible en Visual Studio 2015 y de 2017.</span><span class="sxs-lookup"><span data-stu-id="fd09f-216">Conversion option only available in Visual Studio 2015 and 2017.</span></span>

<span data-ttu-id="fd09f-217">Haga clic en el `bundleconfig.json` y seleccione **convertir en Gulp...** . Esto generará el `gulpfile.js` e instalar los paquetes de npm necesarios.</span><span class="sxs-lookup"><span data-stu-id="fd09f-217">Right-click the `bundleconfig.json` and select **Convert to Gulp...**. This will generate the `gulpfile.js` and install the necessary npm packages.</span></span>

![Convertir en Gulp](../client-side/bundling-and-minification/_static/convert-togulp.png)

<span data-ttu-id="fd09f-219">El `gulpfile.js` generado lee la `bundleconfig.json` archivo para la configuración, por lo tanto puede continuar que se usará para las entradas/salidas y la configuración.</span><span class="sxs-lookup"><span data-stu-id="fd09f-219">The `gulpfile.js` produced reads the `bundleconfig.json` file for the configuration, therefore it can continue to be used for the inputs/outputs and settings.</span></span>

<span data-ttu-id="fd09f-220">[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]</span><span class="sxs-lookup"><span data-stu-id="fd09f-220">[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]</span></span>

<span data-ttu-id="fd09f-221">Para habilitar Gulp cuando se compila el proyecto en Visual Studio de 2017, agregue lo siguiente al archivo *.csproj:</span><span class="sxs-lookup"><span data-stu-id="fd09f-221">To enable Gulp when the project builds in Visual Studio 2017, add the following to the *.csproj file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

<span data-ttu-id="fd09f-222">Para habilitar Gulp cuando se compila el proyecto en Visual Studio 2015, agregue lo siguiente a la `project.json` archivo:</span><span class="sxs-lookup"><span data-stu-id="fd09f-222">To enable Gulp when the project builds in Visual Studio 2015, add the following to the `project.json` file:</span></span>

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a><span data-ttu-id="fd09f-223">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="fd09f-223">Additional resources</span></span>

* [<span data-ttu-id="fd09f-224">Usar Gulp</span><span class="sxs-lookup"><span data-stu-id="fd09f-224">Using Gulp</span></span>](using-gulp.md)
* [<span data-ttu-id="fd09f-225">Usar Grunt</span><span class="sxs-lookup"><span data-stu-id="fd09f-225">Using Grunt</span></span>](using-grunt.md)
* [<span data-ttu-id="fd09f-226">Trabajar con varios entornos</span><span class="sxs-lookup"><span data-stu-id="fd09f-226">Working with Multiple Environments</span></span>](../fundamentals/environments.md)
* [<span data-ttu-id="fd09f-227">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="fd09f-227">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
