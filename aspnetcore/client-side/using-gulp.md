---
title: Usar Gulp en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo usar Gulp en ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-gulp
ms.openlocfilehash: 13f30be7670983bd65f8402404b841039bdacb09
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
---
# <a name="use-gulp-in-aspnet-core"></a>Usar Gulp en ASP.NET Core

Por [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), y [Shayne Boyer](https://twitter.com/spboyer)

En una aplicación web moderna típica, el proceso de compilación puede:

* Agrupar y minificar archivos JavaScript y CSS.
* Ejecutar herramientas para llamar a las tareas de agrupación y minificación antes de cada compilación.
* Compilar menos o SASS archivos CSS.
* Compila los archivos de CoffeeScript o TypeScript a JavaScript.

A *ejecutor de tareas* es una herramienta que automatiza estas tareas de desarrollo de rutinas y mucho más. Visual Studio proporciona compatibilidad integrada para dos corredores populares tareas basadas en JavaScript: [Gulp](https://gulpjs.com/) y [Grunt](using-grunt.md).

## <a name="gulp"></a>gulp

Gulp es un toolkit de compilación streaming basadas en JavaScript para el código de cliente. Normalmente se utiliza para secuenciar los archivos de cliente a través de una serie de procesos cuando se desencadena un evento específico en un entorno de compilación. Por ejemplo, Gulp puede utilizarse para automatizar [agrupar y minificar](bundling-and-minification.md) o la limpieza de un entorno de desarrollo antes de una nueva compilación.

Se define un conjunto de tareas de Gulp en *gulpfile.js*. El siguiente código de JavaScript incluye módulos de Gulp y especifica las rutas de acceso de archivo que se haga referencia dentro de las tareas disponibles próximamente:

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
  rimraf = require("rimraf"),
  concat = require("gulp-concat"),
  cssmin = require("gulp-cssmin"),
  uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

El código anterior especifica qué módulos de nodo se necesitan. El `require` función importa cada módulo para que las tareas dependientes pueden utilizar sus características. Cada uno de los módulos importados se asigna a una variable. Los módulos pueden encontrarse por nombre o ruta de acceso. En este ejemplo, los módulos denominan `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, y `gulp-uglify` se recuperan por su nombre. Además, se crean una serie de rutas de acceso de modo que las ubicaciones de los archivos CSS y JavaScript se pueden volver a usar y hace referencia en las tareas. En la tabla siguiente se proporciona descripciones de los módulos incluidos en *gulpfile.js*.

| Nombre del módulo | Descripción |
| ----------- | ----------- |
| gulp        | El sistema de compilación streaming Gulp. Para obtener más información, consulte [gulp](https://www.npmjs.com/package/gulp). |
| rimraf      | Un módulo de eliminación de nodo. Para obtener más información, consulte [rimraf](https://www.npmjs.com/package/rimraf). |
| gulp concat | Un módulo que concatena los archivos según su carácter de nueva línea del sistema operativo. Para obtener más información, consulte [gulp concat](https://www.npmjs.com/package/gulp-concat). |
| gulp cssmin | Un módulo que minifica objeto archivos CSS. Para obtener más información, consulte [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin). |
| uglify gulp | Un módulo que minifica objeto *.js* archivos. Para obtener más información, consulte [uglify gulp](https://www.npmjs.com/package/gulp-uglify). |

Una vez que se importan los módulos necesarios, se pueden especificar las tareas. Hay seis tareas registrado, representado por el código siguiente:

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

En la tabla siguiente proporciona una explicación de las tareas especificadas en el código anterior:

|Nombre de tarea|Descripción|
|--- |--- |
|limpiar: js|Una tarea que usa el módulo de eliminación de nodo rimraf para quitar la versión reducida del archivo site.js.|
|limpiar: css|Una tarea que usa el módulo de eliminación de nodo rimraf para quitar la versión reducida del archivo site.css.|
|Limpiar|Una tarea que requiera el `clean:js` tarea, seguido por la `clean:css` tarea.|
|min:js|Una tarea que minifica objeto y los concatena todos los archivos .js dentro de la carpeta para js. El. se excluyen min.js archivos.|
|min:CSS|Una tarea que minifica objeto y los concatena todos los archivos .css dentro de la carpeta de css. El. se excluyen min.css archivos.|
|min|Una tarea que requiera el `min:js` tarea, seguido por la `min:css` tarea.|

## <a name="running-default-tasks"></a>Ejecución de tareas de manera predeterminada

Si aún no ha creado una nueva aplicación Web, cree un nuevo proyecto de aplicación Web ASP.NET en Visual Studio.

1.  Agregue un nuevo archivo de JavaScript a su proyecto y asígnele el nombre *gulpfile.js*, a continuación, copie el código siguiente.

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    var gulp = require("gulp"),
      rimraf = require("rimraf"),
      concat = require("gulp-concat"),
      cssmin = require("gulp-cssmin"),
      uglify = require("gulp-uglify");
    
    var paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", function (cb) {
      rimraf(paths.concatJsDest, cb);
    });
    
    gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  Abra la *package.json* archivo (agregar si no existe) y agregue lo siguiente.

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
        "rimraf": "2.6.1"
      }
    }
    ```

3.  En **el Explorador de soluciones**, haga clic en *gulpfile.js*y seleccione **explorador del ejecutor de tareas**.
    
    ![Abrir el explorador del ejecutor de tareas desde el Explorador de soluciones](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **Explorador del ejecutor de tareas** muestra la lista de tareas de Gulp. (Tendrá que hacer clic en el **actualizar** botón que aparece a la izquierda del nombre del proyecto.)
    
    ![Explorador del ejecutor de tareas](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > El **explorador del ejecutor de tareas** elemento de menú contextual aparece sólo si *gulpfile.js* está en el directorio raíz del proyecto.

4.  Debajo de la directiva **tareas** en **explorador del ejecutor de tareas**, haga clic en **limpia**y seleccione **ejecutar** en el menú emergente.

    ![Tarea de limpieza de explorador del ejecutor de tareas](using-gulp/_static/04-TaskRunner-clean.png)

    **Explorador del ejecutor de tareas** creará una nueva ficha denominada **limpia** y ejecute la tarea clean tal y como se define en *gulpfile.js*.

5.  Haga clic en el **limpia** de tareas, a continuación, seleccione **enlaces** > **antes de compilar**.

    ![Enlace BeforeBuild explorador del ejecutor de tareas](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    El **antes de compilar** enlace configura la tarea clean se ejecute automáticamente antes de cada compilación del proyecto.

Los enlaces configura con **explorador del ejecutor de tareas** se almacenan en forma de un comentario en la parte superior de su *gulpfile.js* y son eficaces solo en Visual Studio. Es una alternativa que no requiere Visual Studio configurar la ejecución automática de tareas gulp en su *.csproj* archivo. Por ejemplo, analizando estos datos con su *.csproj* archivo:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

Ahora la tarea clean se ejecuta cuando se ejecuta el proyecto en Visual Studio o desde un símbolo del sistema mediante la [dotnet ejecutar](/dotnet/core/tools/dotnet-run) comando (ejecutar `npm install` primera).

## <a name="defining-and-running-a-new-task"></a>Definir y ejecutar una nueva tarea

Para definir una nueva tarea de Gulp, modificar *gulpfile.js*.

1.  Agregue el siguiente código de JavaScript al final de *gulpfile.js*:

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    Esta tarea se denomina `first`, y simplemente muestra una cadena.

2.  Guardar *gulpfile.js*.

3.  En **el Explorador de soluciones**, haga clic en *gulpfile.js*y seleccione *explorador del ejecutor de tareas*.

4.  En **explorador del ejecutor de tareas**, haga clic en **primer**y seleccione **ejecutar**.

    ![Explorador del ejecutor de tareas ejecute la primera tarea](using-gulp/_static/06-TaskRunner-First.png)

    Se muestra el texto de salida. Para ver ejemplos basados en los escenarios comunes, consulte [Gulp recetas](#gulp-recipes).

## <a name="defining-and-running-tasks-in-a-series"></a>Definir y ejecutar tareas en una serie

Al ejecutar varias tareas, las tareas se ejecutan simultáneamente de forma predeterminada. Sin embargo, si tiene que ejecutar tareas en un orden específico, debe especificar cada tarea una vez haya finalizado, así como las tareas que dependen de la finalización de otra tarea.

1.  Para definir una serie de tareas que se ejecutan en orden, reemplace la `first` tareas que agregó anteriormente en *gulpfile.js* con lo siguiente:

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    Ahora que tiene tres tareas: `series:first`, `series:second`, y `series`. El `series:second` tarea incluye un segundo parámetro que especifica una matriz de tareas que se ejecutarán y se complete antes de la `series:second` se ejecutará la tarea. Como se especifica en el código anterior, solo la `series:first` tarea debe completarse antes de la `series:second` se ejecutará la tarea.

2.  Guardar *gulpfile.js*.

3.  En **el Explorador de soluciones**, haga clic en *gulpfile.js* y seleccione **explorador del ejecutor de tareas** si no está abierta.

4.  En **explorador del ejecutor de tareas**, haga clic en **serie** y seleccione **ejecutar**.

    ![Explorador del ejecutor de tareas Ejecutar tarea de serie](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense proporciona la finalización de código, descripciones de parámetros y otras características para aumentar la productividad y reducir los errores. Tareas de gulp se escriben en JavaScript; por lo tanto, IntelliSense puede proporcionar asistencia durante el desarrollo. Cuando se trabaja con JavaScript, IntelliSense enumera los objetos, funciones, propiedades y parámetros que están disponibles según el contexto actual. Seleccione una opción de codificación de la lista desplegable proporcionada IntelliSense para completar el código.

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

Para obtener más información acerca de IntelliSense, vea [IntelliSense para JavaScript](/visualstudio/ide/javascript-intellisense).

## <a name="development-staging-and-production-environments"></a>Entornos de desarrollo, ensayo y producción

Cuando Gulp se utiliza para optimizar los archivos de cliente de ensayo y producción, se guardan los archivos procesados en una ubicación local de ensayo y producción. El *_Layout.cshtml* archivo usa el **entorno** etiqueta auxiliar para proporcionar dos versiones diferentes de archivos CSS. Es una versión de los archivos CSS para el desarrollo y la otra versión está optimizada para ensayo y producción. En Visual Studio 2017 al cambiar la **ASPNETCORE_ENVIRONMENT** variable de entorno `Production`, Visual Studio compilará la aplicación Web y un vínculo a los archivos CSS minimizados. El marcado siguiente se muestra la **entorno** etiqueta aplicaciones auxiliares que contiene las etiquetas de vínculo a la `Development` CSS archivos y la reducida `Staging, Production` archivos CSS.

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a>Cambiar entre entornos

Para cambiar entre la compilación para los entornos diferentes, modifique la **ASPNETCORE_ENVIRONMENT** valor de la variable de entorno.

1.  En **explorador del ejecutor de tareas**, compruebe que la **min** tarea se ha establecido para ejecutarse **antes de compilar**.

2.  En **el Explorador de soluciones**, haga clic en el nombre del proyecto y seleccione **propiedades**.

    Se muestra la hoja de propiedades de la aplicación Web.

3.  Haga clic en la pestaña **Depurar**.

4.  Establezca el valor de la **: entorno de hospedaje** variable de entorno `Production`.

5.  Presione **F5** para ejecutar la aplicación en un explorador.

6.  En la ventana del explorador, haga clic en la página y seleccione **ver código fuente** para ver el código HTML de la página.

    Tenga en cuenta que los vínculos de hoja de estilos apuntan a los archivos CSS reducidos.

7.  Cierre el explorador para detener la aplicación Web.

8.  En Visual Studio, vuelva a la hoja de propiedades de la aplicación Web y cambiar la **: entorno de hospedaje** hacia la variable de entorno `Development`.

9.  Presione **F5** volver a ejecutar la aplicación en un explorador.

10. En la ventana del explorador, haga clic en la página y seleccione **ver código fuente** para ver el código HTML de la página.

    Tenga en cuenta que los vínculos de hoja de estilos apuntan a las versiones de los archivos CSS unminified.

Para obtener más información relacionada con los entornos en ASP.NET Core, vea [usar varios entornos](../fundamentals/environments.md).

## <a name="task-and-module-details"></a>Detalles de la tarea y módulo

Una tarea de Gulp está registrada con un nombre de función. Puede especificar las dependencias si deben ejecutar otras tareas antes de la tarea actual. Funciones adicionales que permiten ejecutar y ver las tareas de Gulp, así como para establecer el origen (*src*) y de destino (*dest*) de los archivos que se está modificaciones. Éstas son las funciones de API Gulp principales:

|Gulp (función)|Sintaxis|Descripción|
|---   |--- |--- |
|tarea  |`gulp.task(name[, deps], fn) { }`|El `task` función crea una tarea. El `name` parámetro define el nombre de la tarea. El `deps` parámetro contiene una matriz de tareas se complete antes de que se ejecuta esta tarea. El `fn` parámetro representa una función de devolución de llamada que realiza las operaciones de la tarea.|
|Inspección |`gulp.watch(glob [, opts], tasks) { }`|El `watch` función supervisa las tareas de archivos y se ejecuta cuando se produce un cambio de archivo. El `glob` parámetro es un `string` o `array` que determina los archivos que para ver. El `opts` parámetro proporciona viendo opciones de archivo adicionales.|
|src   |`gulp.src(globs[, options]) { }`|El `src` función proporciona archivos que coinciden con los valores de glob. El `glob` parámetro es un `string` o `array` que determina los archivos que para leer. El `options` parámetro proporciona opciones de archivo adicionales.|
|dest  |`gulp.dest(path[, options]) { }`|El `dest` función define una ubicación a la que se pueden escribir archivos. El `path` parámetro es una cadena o una función que determina la carpeta de destino. El `options` parámetro es un objeto que especifica las opciones de carpeta de salida.|

Para obtener información adicional sobre la referencia de API Gulp, consulte [Gulp API de documentos](https://github.com/gulpjs/gulp/blob/master/docs/API.md).

## <a name="gulp-recipes"></a>Gulp recetas

La Comunidad Gulp proporciona Gulp [recetas](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md). Estas recetas constan de tareas de Gulp para dirigirse a escenarios comunes.

## <a name="additional-resources"></a>Recursos adicionales

* [Documentación de gulp](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [Agrupar y minificar en ASP.NET Core](bundling-and-minification.md)
* [Usar Grunt en ASP.NET Core](using-grunt.md)
