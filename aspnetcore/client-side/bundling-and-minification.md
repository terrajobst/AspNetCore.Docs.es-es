---
title: Agrupación y minimizar de recursos estáticos en ASP.NET Core
author: scottaddie
description: Obtenga información sobre cómo optimizar los recursos estáticos en una aplicación Web de ASP.NET Core aplicando técnicas de agrupación y minificación.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: 7499381a24a2513a4fbd1205f245e624c86647c3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080556"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a>Agrupación y minimizar de recursos estáticos en ASP.NET Core

Por [Scott Addie](https://twitter.com/Scott_Addie) y [David pino](https://twitter.com/davidpine7)

En este artículo se explican las ventajas de aplicar la agrupación y la minificación, incluida la forma en que estas características se pueden utilizar con ASP.NET Core Web Apps.

## <a name="what-is-bundling-and-minification"></a>Qué es la agrupación y minificación

La Unión y minificación son dos optimizaciones de rendimiento distintas que se pueden aplicar en una aplicación Web. Se usan juntos, el empaquetado y la minificación mejoran el rendimiento al reducir el número de solicitudes de servidor y reducir el tamaño de los recursos estáticos solicitados.

La agrupación y minificación mejoran principalmente el tiempo de carga de la primera solicitud de página. Una vez solicitada una página web, el explorador almacena en caché los recursos estáticos (JavaScript, CSS e imágenes). Por lo tanto, la Unión y minificación no mejoran el rendimiento cuando solicitan la misma página, o páginas, en el mismo sitio que solicita los mismos recursos. Si el encabezado Expires no se establece correctamente en los activos y no se usa la agrupación y minificación, la heurística de actualización del explorador marca los recursos obsoletos después de unos días. Además, el explorador requiere una solicitud de validación para cada activo. En este caso, la agrupación y la minificación proporcionan una mejora del rendimiento incluso después de la primera solicitud de la página.

### <a name="bundling"></a>Unión

La Unión combina varios archivos en un único archivo. La agrupación reduce el número de solicitudes de servidor necesarias para representar un recurso Web, como una página web. Puede crear cualquier número de paquetes individuales específicamente para CSS, JavaScript, etc. Menos archivos significa menos solicitudes HTTP desde el explorador al servidor o desde el servicio que proporciona la aplicación. Como resultado, se mejora el rendimiento de la carga de la primera página.

### <a name="minification"></a>Minificación

Minificación quita los caracteres innecesarios del código sin modificar la funcionalidad. El resultado es una reducción significativa del tamaño de los activos solicitados (como CSS, imágenes y archivos JavaScript). Los efectos secundarios comunes de minificación incluyen acortar nombres de variable a un carácter y quitar comentarios y espacios en blanco innecesarios.

Considere la siguiente función de JavaScript:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Minificación reduce la función a lo siguiente:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Además de quitar los comentarios y los espacios en blanco innecesarios, se ha cambiado el nombre de los siguientes nombres de parámetros y variables de la siguiente manera:

Original | Se cambia el nombre
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Impacto de la agrupación y minificación

En la tabla siguiente se describen las diferencias entre la carga individual de recursos y el uso de la agrupación y la minificación:

. | Con B/M | Sin B/M | Cambio
--- | :---: | :---: | :---:
Solicitudes de archivo  | 7   | 18     | 157%
KB transferidos | 156 | 264.68 | 70%
Tiempo de carga (MS) | 885 | 2360   | 167%

Los exploradores son bastante detallados con respecto a los encabezados de solicitud HTTP. La métrica total de bytes enviados vio una reducción significativa en la agrupación. El tiempo de carga muestra una mejora significativa, pero este ejemplo se ejecutó localmente. Se obtienen mayores mejoras de rendimiento al usar la agrupación y la minificación con recursos transferidos a través de una red.

## <a name="choose-a-bundling-and-minification-strategy"></a>Elegir una estrategia de agrupación y minificación

Las plantillas de proyecto de MVC y Razor Pages proporcionan una solución integrada para la agrupación y el minificación de archivos que se componen de un archivo de configuración JSON. Las herramientas de terceros, [como la tarea del Ejecutor de tareas](xref:client-side/using-grunt) , realizan las mismas tareas con un poco más de complejidad. Una herramienta de terceros es una buena opción cuando el flujo de trabajo de desarrollo requiere un procesamiento más allá&mdash;de la agrupación y la minificación, como la detección de errores y la optimización de imágenes. Mediante el uso de la Unión en tiempo de diseño y minificación, los archivos reducida se crean antes de la implementación de la aplicación. La agrupación y minificar antes de la implementación proporciona la ventaja de reducir la carga del servidor. Sin embargo, es importante reconocer que la agrupación en tiempo de diseño y minificación aumenta la complejidad de la compilación y solo funciona con archivos estáticos.

## <a name="configure-bundling-and-minification"></a>Configuración de la agrupación y minificación

::: moniker range="<= aspnetcore-2.0"

En ASP.NET Core 2,0 o versiones anteriores, las plantillas de proyecto MVC y Razor Pages proporcionan un archivo de configuración *bundleconfig. JSON* que define las opciones de cada paquete:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

En ASP.NET Core 2,1 o posterior, agregue un nuevo archivo JSON denominado *bundleconfig. JSON*a la raíz del proyecto MVC o Razor pages. Incluya el siguiente código JSON en ese archivo como punto de partida:

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

El archivo *bundleconfig. JSON* define las opciones de cada paquete. En el ejemplo anterior, se define una configuración de agrupación única para los archivos personalizados de JavaScript (*wwwroot/JS/site. js*) y de hoja de estilos (*wwwroot/CSS/site. CSS*).

Las opciones de configuración incluyen:

* `outputFileName`: Nombre del archivo de paquete que se va a generar. Puede contener una ruta de acceso relativa del archivo *bundleconfig. JSON* . **required**
* `inputFiles`: Matriz de archivos que se van a agrupar. Se trata de rutas de acceso relativas al archivo de configuración. **opcional**, * un valor vacío da como resultado un archivo de salida vacío. se admiten patrones [comodines](https://www.tldp.org/LDP/abs/html/globbingref.html) .
* `minify`: Opciones de minificación para el tipo de salida. **opcional**, *valor predeterminado `minify: { enabled: true }` :*
  * Las opciones de configuración están disponibles por tipo de archivo de salida.
    * [Minifier CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Minifier de JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [Minifier HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Marca que indica si se van a agregar los archivos generados al archivo de proyecto. **opcional**, *el valor predeterminado es false* .
* `sourceMap`: Marca que indica si se debe generar un mapa de origen para el archivo agrupado. **opcional**, *el valor predeterminado es false* .
* `sourceMapRootPath`: Ruta de acceso raíz para almacenar el archivo de asignación de origen generado.

## <a name="build-time-execution-of-bundling-and-minification"></a>Ejecución en tiempo de compilación de la agrupación y minificación

El paquete NuGet de [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) permite la ejecución de la agrupación y minificación en tiempo de compilación. El paquete inserta [destinos de MSBuild](/visualstudio/msbuild/msbuild-targets) que se ejecutan en tiempo de compilación y limpieza. El proceso de compilación analiza el archivo *bundleconfig. JSON* para generar los archivos de salida basados en la configuración definida.

> [!NOTE]
> BuildBundlerMinifier pertenece a un proyecto controlado por la comunidad en GitHub para el que Microsoft no proporciona soporte técnico. [Aquí](https://github.com/madskristensen/BundlerMinifier/issues)se deben incluir los problemas.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Agregue el paquete *BuildBundlerMinifier* al proyecto.

Compile el proyecto. La siguiente información aparece en la ventana salida:

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

Limpie el proyecto. La siguiente información aparece en la ventana salida:

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Agregue el paquete *BuildBundlerMinifier* al proyecto:

```dotnetcli
dotnet add package BuildBundlerMinifier
```

Si usa ASP.NET Core 1. x, restaure el paquete recién agregado:

```dotnetcli
dotnet restore
```

Compile el proyecto:

```dotnetcli
dotnet build
```

Aparece lo siguiente:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

Limpie el proyecto:

```dotnetcli
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

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Ejecución ad hoc de la agrupación y minificación

Es posible ejecutar las tareas de agrupación y minificación de forma ad hoc, sin necesidad de compilar el proyecto. Agregue el paquete NuGet [BundlerMinifier. Core](https://www.nuget.org/packages/BundlerMinifier.Core/) al proyecto:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier. Core pertenece a un proyecto controlado por la comunidad en GitHub para el que Microsoft no proporciona soporte técnico. [Aquí](https://github.com/madskristensen/BundlerMinifier/issues)se deben incluir los problemas.

Este paquete amplía el CLI de .NET Core para incluir la herramienta *dotnet-bundle* . El siguiente comando se puede ejecutar en la ventana de la consola del administrador de paquetes (PMC) o en un shell de comandos:

```dotnetcli
dotnet bundle
```

> [!IMPORTANT]
> El administrador de paquetes NuGet agrega dependencias al archivo *. csproj `<PackageReference />` como nodos. El `dotnet bundle` comando se registra con la CLI de .net Core solo cuando se `<DotNetCliToolReference />` usa un nodo. Modifique el archivo *. csproj en consecuencia.

## <a name="add-files-to-workflow"></a>Agregar archivos al flujo de trabajo

Considere un ejemplo en el que se agrega un archivo *. CSS personalizado* adicional similar al siguiente:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

Para minimizar *custom. CSS* y agruparlo con *site. CSS* en un archivo *site. min. CSS* , agregue la ruta de acceso relativa a *bundleconfig. JSON*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Como alternativa, podría usarse el siguiente patrón comodines:
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> Este patrón comodines coincide con todos los archivos CSS y excluye el patrón de archivo reducida.

Compile la aplicación. Abra *site. min. CSS* y observe que el contenido de *custom. CSS* se anexa al final del archivo.

## <a name="environment-based-bundling-and-minification"></a>Agrupación y minificación basados en entornos

Como procedimiento recomendado, los archivos agrupados y reducida de la aplicación deben usarse en un entorno de producción. Durante el desarrollo, los archivos originales facilitan la depuración de la aplicación.

Especifique los archivos que se van a incluir en las páginas mediante la [aplicación auxiliar de etiquetas de entorno](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) en las vistas. La aplicación auxiliar de etiquetas de entorno solo representa su contenido cuando se ejecuta en [entornos](xref:fundamentals/environments)específicos.

La siguiente `environment` etiqueta representa los archivos CSS no procesados cuando se ejecuta en el `Development` entorno:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

La etiqueta `environment` siguiente representa los archivos CSS agrupados y reducida cuando se ejecuta en un entorno distinto de `Development`. Por ejemplo, la ejecución `Production` de `Staging` en o desencadena la representación de estas hojas de estilos:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a>Usar bundleconfig. JSON de Gulp

Hay casos en los que el flujo de trabajo de empaquetado y minificación de una aplicación requiere un procesamiento adicional. Algunos ejemplos son la optimización de imágenes, el desprocesamiento de memoria caché y el procesamiento de recursos de red CDN. Para cumplir estos requisitos, puede convertir el flujo de trabajo de Unión y minificación para usar Gulp.

### <a name="use-the-bundler--minifier-extension"></a>Uso de la extensión bundler & Minifier

El [empaquetador](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) de Visual Studio & extensión Minifier controla la conversión a Gulp.

> [!NOTE]
> El agrupador & extensión Minifier pertenece a un proyecto controlado por la comunidad en GitHub para el que Microsoft no proporciona soporte técnico. [Aquí](https://github.com/madskristensen/BundlerMinifier/issues)se deben incluir los problemas.

Haga clic con el botón derecho en el archivo *bundleconfig. JSON* en explorador de soluciones y seleccione **agrupador & Minifier** > **convertir a Gulp...** :

![Convertir en elemento de menú contextual de Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

Los archivos *gulpfile. js* y *Package. JSON* se agregan al proyecto. Se instalan los paquetes de [NPM](https://www.npmjs.com/) compatibles que se enumeran `devDependencies` en la sección del archivo *Package. JSON* .

Ejecute el siguiente comando en la ventana PMC para instalar la CLI de Gulp como una dependencia global:

```console
npm i -g gulp-cli
```

El archivo *gulpfile. js* lee el archivo *bundleconfig. JSON* para las entradas, salidas y configuraciones.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Convertir manualmente

Si Visual Studio y/o el paquete & extensión Minifier no están disponibles, convierta manualmente.

Agregue un archivo *Package. JSON* , con lo siguiente `devDependencies`, a la raíz del proyecto:

> [!WARNING]
> El `gulp-uglify` módulo no es compatible con ECMAScript (es) 2015/ES6 y versiones posteriores. Instale [Gulp-terser](https://www.npmjs.com/package/gulp-terser) en lugar de `gulp-uglify` usar ES2015/ES6 o posterior.

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Instale las dependencias ejecutando el comando siguiente en el mismo nivel que *Package. JSON*:

```console
npm i
```

Instale la CLI de Gulp como una dependencia global:

```console
npm i -g gulp-cli
```

Copie el archivo *gulpfile. js* siguiente en la raíz del proyecto:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Ejecutar tareas de Gulp

Para desencadenar la tarea de minificación de Gulp antes de que el proyecto se compile en Visual Studio, agregue el siguiente [destino de MSBuild](/visualstudio/msbuild/msbuild-targets) al archivo *. csproj:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

En este ejemplo, las tareas definidas dentro del `MyPreCompileTarget` destino se ejecutan antes del `Build` destino predefinido. Aparece una salida similar a la siguiente en la ventana de salida de Visual Studio:

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


## <a name="additional-resources"></a>Recursos adicionales

* [Uso de Grunt](xref:client-side/using-grunt)
* [Uso de varios entornos](xref:fundamentals/environments)
* [Asistentes de etiquetas](xref:mvc/views/tag-helpers/intro)
