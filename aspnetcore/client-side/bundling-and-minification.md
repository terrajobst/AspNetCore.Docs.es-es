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
ms.openlocfilehash: 11528cb2067ced79a09845f9ff78d897da033438
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a>Agrupar y minificar en ASP.NET Core

Agrupar y minificar son dos técnicas puede usar en ASP.NET para mejorar el rendimiento de la carga de página para la aplicación web. Cómo agrupar combina varios archivos en un único archivo. Minificación realiza una serie de optimizaciones de código diferente para las secuencias de comandos y CSS, que da lugar a unas cargas menores. Usan juntos, agrupar y minificar mejora el rendimiento en tiempo de carga, lo que reduce el número de solicitudes al servidor y reducir el tamaño de los activos solicitados (como archivos CSS y JavaScript).

Este artículo explica las ventajas de usar la agrupación y minificación, incluido cómo estas características pueden utilizarse con aplicaciones de ASP.NET Core.

## <a name="overview"></a>Información general

En las aplicaciones ASP.NET Core, existen varias opciones para agrupar y minificar recursos del lado cliente. Las plantillas de principal para MVC proporcionan una solución de fábrica mediante un archivo de configuración y el paquete BuildBundlerMinifier NuGet. Herramientas de terceros, como [Gulp](using-gulp.md) y [Grunt](using-grunt.md) también están disponibles para realizar las mismas tareas deben requerir los procesos de flujo de trabajo adicional o complejidades. Mediante el uso de agrupación y minificación de tiempo de diseño, se crean los archivos reducidos antes de la implementación de la aplicación. Agrupar y minificar antes de la implementación proporcionan la ventaja de carga reducida del servidor. Sin embargo, es importante reconocer que agrupación de tiempo de diseño y minificación aumenta la complejidad de la compilación y solo funciona con archivos estáticos.

Agrupar y minificar mejoran principalmente el primer tiempo de carga de solicitud de página. Una vez que se ha solicitado una página web, el explorador se almacena en memoria caché los activos (JavaScript, CSS e imágenes) para agrupar y minificar no proporcionan ningún aumento del rendimiento cuando se solicita la misma página o páginas en el mismo sitio que solicita los activos de la mismos. Si no establece la expira encabezado correctamente en los activos y no use agrupar y minificar, heurística de actualización del explorador, se marcarán como los recursos obsoletos después de unos días y el explorador requerirá una solicitud de validación para cada recurso. En este caso, agrupar y minificar proporcionan un aumento del rendimiento incluso después de la primera solicitud de página.

### <a name="bundling"></a>Cómo agrupar

Cómo agrupar es una característica que facilita el proceso combinar o agrupar varios archivos en un único archivo. Porque agrupación combina varios archivos en un único archivo, reduce el número de solicitudes al servidor que se necesitan para recuperar y mostrar un recurso web, por ejemplo, una página web. Puede crear CSS, JavaScript y otros paquetes. Menos archivos significa menos solicitudes HTTP desde el explorador hasta el servidor o desde el servicio que proporciona la aplicación. El resultado de la mejora del rendimiento de carga de primera página.

### <a name="minification"></a>Minificación

Minificación realiza una serie de optimizaciones de código diferente para reducir el tamaño de los activos solicitados (por ejemplo, CSS, imágenes, archivos de JavaScript). Resultados comunes de reducción incluyen quitando el espacio en blanco innecesario y comentarios y reducir los nombres de variable con un único carácter.

Tenga en cuenta la siguiente función de JavaScript:

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

Después de la reducción, la función se reduce a lo siguiente:

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

Además de quitar los comentarios y espacios en blanco innecesarios, los siguientes parámetros y nombres de variables se cambió el nombre (abreviar) como se indica a continuación:

Original | Se cambia el nombre
--- | :---:
imageTagAndImageID | m
imageContext | a
imageElement | c

## <a name="impact-of-bundling-and-minification"></a>Impacto de agrupar y minificar

La siguiente tabla muestra algunas diferencias importantes entre enumerar todos los activos de forma individual y el uso de agrupación y minificación en una página web sencilla:

Acción | Con B/M | Sin B/M | Cambio
--- | :---: | :---: | :---:
Solicitudes de archivos |7 | 18 | 157%
KB transferido | 156 | 264.68 | 70%
Tiempo de carga (MS) | 885 | 2360 | 167%

Los bytes enviados tenían una reducción significativa de la agrupación sea bastante detallados con los encabezados HTTP que se aplican en las solicitudes de los exploradores. El tiempo de carga muestra una gran mejora, sin embargo, en este ejemplo se ejecute localmente. Obtendrá mayores ganancias de rendimiento al uso de agrupación y minificación con activos transfiere a través de una red.

## <a name="using-bundling-and-minification-in-a-project"></a>Utilizar agrupar y minificar en un proyecto

La plantilla de proyecto MVC proporciona una `bundleconfig.json` archivo de configuración que define las opciones para cada paquete. De forma predeterminada, se define una configuración de agrupación única para el código de JavaScript personalizado (`wwwroot/js/site.js`) y hojas de estilo (`wwwroot/css/site.css`) archivos.

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]

Opciones de agrupación se incluyen:

* outputFileName - nombre del archivo de paquete para la salida. Puede contener una ruta de acceso relativa desde la `bundleconfig.json` archivo. **Obligatorio**
* inputFiles - matriz de archivos que se va a agrupar. Estas son las rutas de acceso relativas al archivo de configuración. **opcional**, * da como resultado un valor vacío en un archivo de resultados vacío. [uso de comodines](http://www.tldp.org/LDP/abs/html/globbingref.html) patrones son compatibles.
* minificar - escriba opciones de reducción de la salida. **opcional**, *predeterminado:`minify: { enabled: true }`*
  * Opciones de configuración están disponibles por tipo de archivo de salida.
    * [Minificador CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Minificador de JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki)
    * [Minificador de HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* includeInProject - agregar archivos generados al archivo del proyecto. **opcional**, *predeterminado: false*
* sourceMaps - generar mapas de código fuente para el archivo agrupado. **opcional**, *predeterminado: false*

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 / 2017

Abra `bundleconfig.json` en Visual Studio, si su entorno no tiene la extensión instalada; un símbolo del sistema se presenta lo que sugiere que hay uno que puede ayudar a este tipo de archivo.

![Sugerencia de extensión BuildBundlerMinifier](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

Seleccione ver extensiones e instalar la **paquete de instalación & Minificador** extensión (reiniciar requiere Visual Studio).

![Sugerencia de extensión BuildBundlerMinifier](../client-side/bundling-and-minification/_static/view-extension.png)

Una vez completado el reinicio, debe configurar la compilación para ejecutar los procesos de minificación y agrupación de los activos de cliente. Haga clic en el `bundleconfig.json` de archivos y seleccione *habilitar agrupación en la compilación... *.

Compile el proyecto y el `bundleconfig.json` se incluye en el proceso de compilación para generar los archivos de salida en función de la configuración.

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a>Código de Visual Studio o la línea de comandos

Visual Studio y la extensión de unidad el proceso de agrupación y minificación con movimientos de GUI; Sin embargo, las mismas capacidades están disponibles con el `dotnet` paquete CLI y BuildBundlerMinifier NuGet.

Agregue el paquete NuGet al proyecto:

```console
dotnet add package BuildBundlerMinifier
```

Restaurar las dependencias:

```console
dotnet restore
```

Compile la aplicación:

```console
dotnet build
```

La salida del comando de compilación muestra los resultados de la minificación y cómo agrupar según la configuración establecida.

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a>Agregar archivos

En este ejemplo, se agrega un archivo CSS adicional llamado `custom.css` y configurado para agrupación y minificación con `site.css`, da lugar a una sola `site.min.css`.

Custom.CSS

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

Agregar la ruta de acceso relativa a `bundleconfig.json`.

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]

> [!NOTE]
> También se puede usar el patrón de uso de comodines - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` que obtiene CSS todos los archivos y se excluye el patrón de archivo reducida.

Compilar la aplicación y si abre `site.min.css`, ahora observará que el contenido de `custom.css` se ha anexado al final del archivo.

## <a name="controlling-bundling-and-minification"></a>Controlar la agrupación y minificación

En general, desea utilizar los archivos integrados y reducidos de la aplicación únicamente en un entorno de producción. Durante el desarrollo, desea utilizar los archivos originales, por lo que es más fácil de depurar la aplicación.

Puede especificar qué secuencias de comandos y archivos CSS que se incluirán en las páginas utilizando la aplicación auxiliar de etiquetas de entorno en las páginas de diseño (vea [aplicaciones auxiliares de etiquetas](../mvc/views/tag-helpers/index.md)). La aplicación auxiliar de etiquetas de entorno solo representará su contenido cuando se ejecuta en entornos específicos. Vea [trabajar con varios entornos](../fundamentals/environments.md) para obtener más información sobre cómo especificar el entorno actual.

La siguiente etiqueta de entorno representará los archivos sin procesar de CSS cuando se ejecuta en el `Development` entorno:

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]

Esta etiqueta de entorno representará los archivos CSS agrupados y reducidos solo cuando se ejecuta en `Production` o `Staging`:

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]

## <a name="consuming-bundleconfigjson-from-gulp"></a>Consumir bundleconfig.json desde Gulp

Si el flujo de trabajo de agrupación y minificación de aplicación requiere procesos adicionales, como el procesamiento de imágenes, la desactivación de caché, el procesamiento de red CDN assest, etc., puede convertir el proceso de agrupación y Minify a Gulp.

> [!NOTE]
> Opción de conversión solo está disponible en Visual Studio 2015 y de 2017.

Haga clic en el `bundleconfig.json` y seleccione **convertir en Gulp... **. Esto generará el `gulpfile.js` e instalar los paquetes de npm necesarios.

![Convertir en Gulp](../client-side/bundling-and-minification/_static/convert-togulp.png)

El `gulpfile.js` generado lee la `bundleconfig.json` archivo para la configuración, por lo tanto puede continuar que se usará para las entradas/salidas y la configuración.

[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]

Para habilitar Gulp cuando se compila el proyecto en Visual Studio de 2017, agregue lo siguiente al archivo *.csproj:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

Para habilitar Gulp cuando se compila el proyecto en Visual Studio 2015, agregue lo siguiente a la `project.json` archivo:

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a>Recursos adicionales

* [Uso de Gulp](using-gulp.md)
* [Uso de Grunt](using-grunt.md)
* [Trabajar con varios entornos](../fundamentals/environments.md)
* [Aplicaciones auxiliares de etiquetas](../mvc/views/tag-helpers/index.md)
