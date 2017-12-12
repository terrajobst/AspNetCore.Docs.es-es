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
# <a name="bundling-and-minification"></a>Agrupar y minificar

Por [Scott Addie](https://twitter.com/Scott_Addie)

Este artículo explica las ventajas de aplicar la agrupación y minificación, incluido cómo estas características pueden utilizarse con las aplicaciones web de ASP.NET Core.

## <a name="what-is-bundling-and-minification"></a>¿Qué es agrupar y minificar?

Unión y minificación son dos optimizaciones de rendimiento distintas que puede aplicar en una aplicación web. Se usan juntos, agrupar y minificar mejoran el rendimiento reduciendo el número de solicitudes de servidor y reducir el tamaño de los activos estáticos solicitados.

Agrupar y minificar mejoran principalmente el primer tiempo de carga de solicitud de página. Una vez que se ha solicitado una página web, el explorador almacena en caché los activos estáticos (JavaScript, CSS e imágenes). Por lo tanto, agrupar y minificar no mejoran el rendimiento cuando se solicita la misma página o páginas, en el mismo sitio que solicita los activos de la mismos. Si no establece la expira encabezado correctamente en los activos, y si no utiliza la agrupación y minificación, heurística de actualización del explorador marca los activos obsoletas después de unos días. Además, el explorador requiere que una solicitud de validación para cada activo. En este caso, agrupar y minificar proporcionan una mejora del rendimiento incluso después de la primera solicitud de página.

### <a name="bundling"></a>Cómo agrupar

Cómo agrupar combina varios archivos en un único archivo. Cómo agrupar, reduce el número de solicitudes de servidor que son necesarios para representar un recurso web, por ejemplo, una página web. Puede crear cualquier número de paquetes individuales específicamente para CSS, JavaScript, etcetera. Menos archivos significa menos solicitudes HTTP desde el explorador hasta el servidor o desde el servicio que proporciona la aplicación. El resultado de la mejora del rendimiento de carga de primera página.

### <a name="minification"></a>Minificación

Minificación quita caracteres innecesarios de código sin modificar la funcionalidad. El resultado es una reducción de un tamaño considerable en activos solicitados (por ejemplo, CSS, imágenes y archivos de JavaScript). Los efectos secundarios comunes de minificación incluyen acortar los nombres de variable a un carácter y quitar los comentarios y espacios en blanco innecesarios.

Tenga en cuenta la siguiente función de JavaScript:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Minificación reduce la función a la siguiente:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Además de quitar los comentarios y espacios en blanco innecesarios, los siguientes nombres de parámetro y la variable se cambió el nombre como sigue:

Original | Se cambia el nombre
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Impacto de agrupar y minificar

En la tabla siguiente se describe las diferencias entre individualmente carga activos y el uso de agrupación y minificación:

Acción | Con B/M | Sin B/M | Cambio
--- | :---: | :---: | :---:
Solicitudes de archivos  | 7   | 18     | 157%
KB transferido | 156 | 264.68 | 70%
Tiempo de carga (ms) | 885 | 2360   | 167%

Los exploradores son bastante detallados con respecto a los encabezados de solicitud HTTP. El número total de bytes enviados métrica vio una reducción significativa cuando se agrupa. El tiempo de carga muestra una mejora considerable, sin embargo, en este ejemplo se ejecutaba localmente. Mayores mejoras de rendimiento se percibe cuando el uso de agrupación y minificación con activos transfiere a través de una red.

## <a name="choose-a-bundling-and-minification-strategy"></a>Elegir una estrategia de agrupación y minificación

Las plantillas de proyecto MVC y las páginas de Razor proporcionan una solución para agrupar y minificar que consta de un archivo de configuración de JSON. Herramientas de terceros, como el [Gulp](xref:client-side/using-gulp) y [Grunt](xref:client-side/using-grunt) corredores de tareas, realizar las mismas tareas con un poco más compleja. Una herramienta de terceros es una buena elección cuando el flujo de trabajo de desarrollo requiere un procesamiento más allá de la agrupación y minificación&mdash;como optimización linting e imagen. Mediante el uso de agrupación y minificación de tiempo de diseño, se crean los archivos reducidos antes de la implementación de la aplicación. Agrupar y minificar antes de la implementación proporcionan la ventaja de carga reducida del servidor. Sin embargo, es importante reconocer que agrupación de tiempo de diseño y minificación aumenta la complejidad de la compilación y solo funciona con archivos estáticos.

## <a name="configure-bundling-and-minification"></a>Configurar la agrupación y minificación

Las plantillas de proyecto MVC y las páginas de Razor proporcionan una *bundleconfig.json* archivo de configuración que define las opciones para cada paquete. De forma predeterminada, se define una configuración de agrupación única para el código de JavaScript personalizado (*wwwroot/js/site.js*) y hojas de estilo (*wwwroot/css/site.css*) archivos:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

Opciones de agrupación se incluyen:

* `outputFileName`: El nombre del archivo de paquete para la salida. Puede contener una ruta de acceso relativa desde la *bundleconfig.json* archivo. **Obligatorio**
* `inputFiles`: Una matriz de archivos que se va a agrupar. Estas son las rutas de acceso relativas al archivo de configuración. **opcional**, * da como resultado un valor vacío en un archivo de resultados vacío. [uso de comodines](http://www.tldp.org/LDP/abs/html/globbingref.html) patrones son compatibles.
* `minify`: Las opciones de reducción para el tipo de salida. **opcional**, *predeterminado:`minify: { enabled: true }`*
  * Opciones de configuración están disponibles por tipo de archivo de salida.
    * [Minificador CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Minificador de JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [Minificador de HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Marca que indica si se debe agregar los archivos generados al archivo del proyecto. **opcional**, *predeterminado: false*
* `sourceMap`: Marca que indica si se debe generar un mapa de código fuente para el archivo agrupado. **opcional**, *predeterminado: false*
* `sourceMapRootPath`: La ruta de acceso raíz para almacenar el archivo de mapa de código fuente generado.

## <a name="build-time-execution-of-bundling-and-minification"></a>Ejecución en tiempo de compilación de agrupar y minificar

El [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) paquete NuGet permite la ejecución de agrupación y minificación en tiempo de compilación. Inserta el paquete [destinos de MSBuild](/visualstudio/msbuild/msbuild-targets) que ejecutar en la compilación y tiempo de limpieza. El *bundleconfig.json* archivo se analiza el proceso de compilación para generar los archivos de salida en función de la configuración definida.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Agregar el *BuildBundlerMinifier* paquete al proyecto.

Compile el proyecto. Aparecerá el siguiente mensaje en la ventana de salida:

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

Limpie el proyecto. Aparecerá el siguiente mensaje en la ventana de salida:

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli) 

Agregar el *BuildBundlerMinifier* paquete al proyecto:

```console
dotnet add package BuildBundlerMinifier
```

Si usa ASP.NET Core 1.x, restaurar el paquete recién agregado:

```console
dotnet restore
```

Compile el proyecto:

```console
dotnet build
```

Aparecerá el siguiente mensaje:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

Limpie el proyecto:

```console
dotnet clean
```

Aparecerá el siguiente resultado:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Ejecución ad hoc de agrupar y minificar

Es posible ejecutar las tareas de agrupación y minificación de manera ad hoc, sin compilar el proyecto. Agregar el [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) paquete NuGet para el proyecto:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

Este paquete extiende la CLI de núcleo de .NET para incluir la *dotnet agrupación* herramienta. En la ventana de consola de administrador de paquetes (PMC) o en un shell de comandos, se puede ejecutar el comando siguiente:

```console
dotnet bundle
```

> [!IMPORTANT]
> Administrador de paquetes de NuGet agrega las dependencias en el archivo *.csproj como `<PackageReference />` nodos. El `dotnet bundle` comando está registrado con la CLI de .NET Core solo cuando un `<DotNetCliToolReference />` nodo se utiliza. Modifique el archivo *.csproj según corresponda.

## <a name="add-files-to-workflow"></a>Agregar archivos al flujo de trabajo

Considere un ejemplo en el que más *custom.css* archivo se agrega similar a lo siguiente:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

A minificar *custom.css* y agrupar con *site.css* en un *site.min.css* , agregue la ruta de acceso relativa a *bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Como alternativa, podría utilizar el siguiente patrón de uso de comodines en:
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> Este patrón de uso de comodines coincide con todos los archivos CSS y excluye el patrón de archivo reducida.

Compile la aplicación. Abra *site.min.css* y observe el contenido de *custom.css* se anexa al final del archivo.

## <a name="environment-based-bundling-and-minification"></a>Agrupar y minificar basado en el entorno

Como práctica recomendada, los archivos integrados y reducidos de la aplicación deben usarse en un entorno de producción. Durante el desarrollo, los archivos originales se realizan para una depuración más sencilla de la aplicación.

Especificar qué archivos desea incluir en las páginas mediante el [auxiliar de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) en las vistas. La aplicación auxiliar de etiquetas de entorno solo representa su contenido cuando se ejecuta en específico [entornos](xref:fundamentals/environments).

El siguiente `environment` etiqueta representa los archivos sin procesar de CSS cuando se ejecuta en el `Development` entorno:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

El siguiente `environment` etiqueta representa los archivos CSS agrupados y reducidos cuando se ejecuta en un entorno distinto de `Development`. Por ejemplo, que se ejecutan `Production` o `Staging` desencadena el procesamiento de estas hojas de estilos:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a>Consumir bundleconfig.json desde Gulp

Hay casos en los que el flujo de trabajo de una aplicación agrupar y minificar requiere un procesamiento adicional. Algunos ejemplos son la optimización de la imagen, la desactivación de caché y el procesamiento del recurso de red CDN. Para satisfacer estos requisitos, que puede convertir el flujo de trabajo de agrupación y minificación para usar Gulp.

### <a name="use-the-bundler--minifier-extension"></a>Utilizar la extensión de paquete de instalación & Minificador

Visual Studio [paquete de instalación & Minificador](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extensión controla la conversión a Gulp.

Haga clic en el *bundleconfig.json* un archivo en el Explorador de soluciones y seleccione **paquete de instalación & Minificador** > **convertir a Gulp...** :

![Convertir a Gulp elemento del menú contextual](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

El *gulpfile.js* y *package.json* archivos se agregan al proyecto. La compatibilidad con [npm](https://www.npmjs.com/) paquetes incluidos en el *package.json* del archivo `devDependencies` sección están instalados.

Ejecute el siguiente comando en la ventana PMC para instalar la CLI Gulp como una dependencia global:

```console
npm i -g gulp-cli
```

El *gulpfile.js* lecturas de archivos la *bundleconfig.json* archivo de entradas, salidas y configuración.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Convertir manualmente

Si Visual Studio o la extensión del paquete de instalación & Minificador no está disponible, convertir manualmente.

Agregar un *package.json* archivo, con el siguiente `devDependencies`, a la raíz del proyecto:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Instalar las dependencias ejecutando el siguiente comando en el mismo nivel que *package.json*:

```console
npm i
```

Instale la CLI Gulp como una dependencia global:

```console
npm i -g gulp-cli
```

Copia la *gulpfile.js* por debajo del archivo a la raíz del proyecto:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Ejecutar tareas de Gulp

Para desencadenar la tarea de preparación de minificación Gulp antes de que el proyecto se compila en Visual Studio, agregue las siguientes [destino de MSBuild](/visualstudio/msbuild/msbuild-targets) al archivo *.csproj:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

En este ejemplo, las tareas se definen en el `MyPreCompileTarget` destino ejecutar antes predefinido `Build` destino. Aparecerá un resultado similar al siguiente en la ventana de salida de Visual Studio:

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

También se puede usar el explorador del ejecutor de tareas de Visual Studio para enlazar Gulp tareas a eventos específicos de Visual Studio. Vea [ejecutando tareas predeterminadas](xref:client-side/using-gulp#running-default-tasks) para obtener instrucciones sobre cómo hacer que.

## <a name="additional-resources"></a>Recursos adicionales

* [Uso de Gulp](xref:client-side/using-gulp)
* [Uso de Grunt](xref:client-side/using-grunt)
* [Trabajar con varios entornos](xref:fundamentals/environments)
* [Aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro)
