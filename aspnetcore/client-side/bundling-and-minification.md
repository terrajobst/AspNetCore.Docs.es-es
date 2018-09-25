---
title: Agrupar y minificar recursos estáticos en ASP.NET Core
author: scottaddie
description: Aprenda a optimizar los recursos estáticos en una aplicación web ASP.NET Core aplicando técnicas de unión y minificación.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
uid: client-side/bundling-and-minification
ms.openlocfilehash: 45200d34974cbbb44787616eba7508458882416c
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028146"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a>Agrupar y minificar recursos estáticos en ASP.NET Core

Por [Scott Addie](https://twitter.com/Scott_Addie)

En este artículo explica las ventajas de la aplicación de unión y minificación, incluido cómo se pueden usar estas características con aplicaciones web ASP.NET Core.

## <a name="what-is-bundling-and-minification"></a>¿Qué es la unión y minificación

Unión y minificación son dos optimizaciones de rendimiento distintas que se pueden aplicar en una aplicación web. Se usan juntos, unión y minificación de mejoran el rendimiento mediante la reducción del número de solicitudes de servidor y reducir el tamaño de los activos estáticos solicitados.

Unión y minificación principalmente mejoran el tiempo de carga de solicitud de primera página. Una vez que se ha solicitado una página web, el explorador almacena en caché los recursos estáticos (imágenes, CSS y JavaScript). Por lo tanto, unión y minificación no mejoran el rendimiento cuando se solicita la misma página o páginas, en el mismo sitio que solicita los mismos recursos. Si el expira encabezado no está configurado correctamente en los activos y si no se usa la unión y minificación, la heurística de actualización del explorador marca los recursos obsoletos después de unos días. Además, el explorador requiere una solicitud de validación para cada recurso. En este caso, unión y minificación de proporcionan una mejora del rendimiento incluso después de la primera solicitud de página.

### <a name="bundling"></a>La Unión

La Unión combina varios archivos en un único archivo. La unión, reduce el número de solicitudes del servidor que son necesarios para representar un activo de web, como una página web. Puede crear cualquier número de paquetes individuales específicamente para CSS, JavaScript, etcetera. Menos archivos significa menos solicitudes HTTP desde el explorador al servidor o desde el servicio que proporciona la aplicación. Este se produce en el mejor rendimiento de carga de primera página.

### <a name="minification"></a>Minificación

Minificación quita caracteres innecesarios de código sin modificar la funcionalidad. El resultado es una reducción de tamaño considerable en los activos solicitados (como CSS, imágenes y archivos de JavaScript). Efectos secundarios comunes de minificación incluyen acortar los nombres de variable a un carácter y quitar los comentarios y espacios en blanco innecesarios.

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

## <a name="impact-of-bundling-and-minification"></a>Impacto de la unión y minificación

En la tabla siguiente se describe las diferencias entre cargar activos y uso de unión y minificación individualmente:

Acción | Con B/M | Sin B/M | Cambio
--- | :---: | :---: | :---:
Solicitudes de archivos  | 7   | 18     | 157%
KB transferido | 156 | 264.68 | 70%
Tiempo de carga (ms) | 885 | 2360   | 167%

Los exploradores son bastante detallados con respecto a los encabezados de solicitud HTTP. El número total de bytes enviados métrica vio una reducción significativa cuando se agrupa. El tiempo de carga muestra una mejora considerable, aunque en este ejemplo se ejecutó localmente. Mejoras de rendimiento mayores se llevan a cabo cuando el uso de unión y minificación con activos transfiere a través de una red.

## <a name="choose-a-bundling-and-minification-strategy"></a>Elegir una estrategia de unión y minificación

Las plantillas de proyecto MVC y páginas de Razor proporcionan una solución para la unión y minificación que consta de un archivo de configuración de JSON. Herramientas de terceros, como el [Gulp](xref:client-side/using-gulp) y [Grunt](xref:client-side/using-grunt) ejecutores de tareas, realizar las mismas tareas con un poco más compleja. Una herramienta de terceros es una opción ideal cuando el flujo de trabajo de desarrollo requiere un procesamiento más allá de unión y minificación&mdash;como la optimización de la detección de errores y la imagen. Mediante el uso de unión y minificación de tiempo de diseño, se crean los archivos minimizados antes de la implementación de la aplicación. Agrupar y minificar antes de la implementación ofrece la ventaja de carga reducida del servidor. Sin embargo, es importante reconocer ese tiempo de diseño la unión y minificación aumenta la complejidad de la compilación y solo funciona con archivos estáticos.

## <a name="configure-bundling-and-minification"></a>Configurar la unión y minificación

Las plantillas de proyecto MVC y páginas de Razor proporcionan una *bundleconfig.json* archivo de configuración que define las opciones para cada paquete. De forma predeterminada, se define una configuración de lote único para el código JavaScript personalizado (*wwwroot/js/site.js*) y hojas de estilo (*wwwroot/css/site.css*) archivos:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

Opciones de configuración incluyen:

* `outputFileName`: El nombre del archivo de paquete para la salida. Puede contener una ruta de acceso relativa desde la *bundleconfig.json* archivo. **Obligatorio**
* `inputFiles`: Una matriz de archivos que se va a agrupar. Estas son las rutas de acceso relativas al archivo de configuración. **opcional**, * da como resultado un valor vacío en un archivo de resultados vacío. [uso de comodines](http://www.tldp.org/LDP/abs/html/globbingref.html) se admiten patrones.
* `minify`: Las opciones de reducción para el tipo de salida. **opcional**, *predeterminado: `minify: { enabled: true }`*
  * Las opciones de configuración están disponibles por tipo de archivo de salida.
    * [Minificador CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Minificador de JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [Minificador de HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Marca que indica si se debe agregar los archivos generados en el archivo de proyecto. **opcional**, *predeterminado: false*
* `sourceMap`: Marca que indica si se debe generar un mapa de origen para el archivo agrupado. **opcional**, *predeterminado: false*
* `sourceMapRootPath`: La ruta de acceso raíz para almacenar el archivo de mapa de código fuente generado.

## <a name="build-time-execution-of-bundling-and-minification"></a>Compilación en tiempo de ejecución de la unión y minificación

El [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) paquete NuGet permite la ejecución de la unión y minificación en tiempo de compilación. Inserta el paquete [destinos de MSBuild](/visualstudio/msbuild/msbuild-targets) que se ejecutan en la compilación y tiempo de limpieza. El *bundleconfig.json* archivo analizado por el proceso de compilación para generar los archivos de salida según la configuración definida.

> [!NOTE]
> BuildBundlerMinifier pertenece a un proyecto controlado por la Comunidad en GitHub para el que Microsoft no proporciona soporte. Se deben notificar problemas [aquí](https://github.com/madskristensen/BundlerMinifier/issues).

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

Aparece el siguiente resultado:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Ejecución ad hoc de unión y minificación

Es posible ejecutar las tareas de unión y minificación en forma ad-hoc, sin compilar el proyecto. Agregar el [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) paquete NuGet al proyecto:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier.Core pertenece a un proyecto controlado por la Comunidad en GitHub para el que Microsoft no proporciona soporte. Se deben notificar problemas [aquí](https://github.com/madskristensen/BundlerMinifier/issues).

Este paquete amplía la CLI de .NET Core para incluir el *dotnet agrupación* herramienta. En la ventana de consola de administrador de paquetes (PMC) o en un shell de comandos, se puede ejecutar el comando siguiente:

```console
dotnet bundle
```

> [!IMPORTANT]
> Administrador de paquetes NuGet agrega las dependencias al archivo *.csproj como `<PackageReference />` nodos. El `dotnet bundle` comando está registrado con la CLI de .NET Core solo cuando un `<DotNetCliToolReference />` nodo se utiliza. Modifique el archivo *.csproj según corresponda.

## <a name="add-files-to-workflow"></a>Agregar archivos al flujo de trabajo

Considere un ejemplo en el que más *custom.css* archivo se agrega similar al siguiente:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

Para minimizar *custom.css* y empaquetarla con *site.css* en un *site.min.css* , agregue la ruta de acceso relativa *bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Como alternativa, podría utilizar el siguiente patrón de uso de comodines:
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> Este patrón global coincide con todos los archivos CSS y excluye el patrón de archivo minimizado.

Compile la aplicación. Abra *site.min.css* y observe el contenido de *custom.css* se anexa al final del archivo.

## <a name="environment-based-bundling-and-minification"></a>Unión y minificación basado en el entorno

Como práctica recomendada, se deben usar los archivos y minificados de la aplicación en un entorno de producción. Durante el desarrollo, asegúrese de los archivos originales para una depuración más sencilla de la aplicación.

Especificar qué archivos incluir en las páginas mediante el [aplicación auxiliar de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) en las vistas. La aplicación auxiliar de etiquetas de entorno solo procesa su contenido cuando se ejecuta en específico [entornos](xref:fundamentals/environments).

La siguiente `environment` etiqueta representa los archivos CSS sin procesar cuando se ejecuta en el `Development` entorno:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

La siguiente `environment` etiqueta representa los archivos CSS y minificados cuando se ejecuta en un entorno distinto `Development`. Por ejemplo, que se ejecutan en `Production` o `Staging` desencadena el procesamiento de estas hojas de estilo:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a>Consumir bundleconfig.json de Gulp

Hay casos en que el flujo de trabajo de unión y minificación de una aplicación requiere un procesamiento adicional. Algunos ejemplos son la optimización de la imagen, limpieza de caché y el procesamiento de activos de red CDN. Para satisfacer estos requisitos, que puede convertir el flujo de trabajo de unión y minificación para usar Gulp.

### <a name="use-the-bundler--minifier-extension"></a>Use la extensión Bundler & Minifier

Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extensión controla la conversión a Gulp.

> [!NOTE]
> La extensión Bundler & Minifier pertenece a un proyecto controlado por la Comunidad en GitHub para el que Microsoft no proporciona soporte. Se deben notificar problemas [aquí](https://github.com/madskristensen/BundlerMinifier/issues).

Haga clic en el *bundleconfig.json* de archivos en el Explorador de soluciones y seleccione **Bundler & Minifier** > **convertir a Gulp...** :

![Elemento de menú contextual de convertir a Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

El *gulpfile.js* y *package.json* archivos se agregan al proyecto. La compatibilidad con [npm](https://www.npmjs.com/) paquetes que aparecen en la *package.json* del archivo `devDependencies` sección están instalados.

Ejecute el siguiente comando en la ventana PMC para instalar la CLI de Gulp como una dependencia global:

```console
npm i -g gulp-cli
```

El *gulpfile.js* lecturas de archivos el *bundleconfig.json* archivo para las entradas, salidas y la configuración.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Convertir manualmente

Si Visual Studio o la extensión Bundler & Minifier no está disponible, convertir manualmente.

Agregar un *package.json* archivo, por lo siguiente `devDependencies`, a la raíz del proyecto:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Instalar las dependencias ejecutando el comando siguiente en el mismo nivel que *package.json*:

```console
npm i
```

Instale la CLI de Gulp como una dependencia global:

```console
npm i -g gulp-cli
```

Copia el *gulpfile.js* por debajo del archivo a la raíz del proyecto:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Ejecutar las tareas de Gulp

Para desencadenar la tarea de Gulp minificación antes de que el proyecto se compila en Visual Studio, agregue el siguiente [destino de MSBuild](/visualstudio/msbuild/msbuild-targets) al archivo *.csproj:

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

Como alternativa, Visual Studio Task Runner Explorer puede utilizarse para enlazar las tareas de Gulp a eventos específicos de Visual Studio. Consulte [ejecutando tareas predeterminadas](xref:client-side/using-gulp#running-default-tasks) para obtener instrucciones sobre cómo hacerlo.

## <a name="additional-resources"></a>Recursos adicionales

* [Uso de Gulp](xref:client-side/using-gulp)
* [Uso de Grunt](xref:client-side/using-grunt)
* [Uso de varios entornos](xref:fundamentals/environments)
* [Asistentes de etiquetas](xref:mvc/views/tag-helpers/intro)
