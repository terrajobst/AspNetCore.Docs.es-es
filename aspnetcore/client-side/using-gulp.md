---
title: Usar Gulp en ASP.NET Core
author: rick-anderson
description: Aprenda a usar Gulp en ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: 43277dc5910971374187f49031e74769c9e29e1f
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665631"
---
# <a name="use-gulp-in-aspnet-core"></a>Usar Gulp en ASP.NET Core

Por [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), y [David Borovice](https://twitter.com/davidpine7)

En una aplicación web moderna típica, es posible que el proceso de compilación:

* Agrupar y minimizar los archivos JavaScript y CSS.
* Ejecutar herramientas para llamar a las tareas de unión y minificación antes de cada compilación.
* Archivos de compilar menos o SASS en CSS.
* Compilar archivos CoffeeScript o TypeScript para JavaScript.

Un *ejecutor de tareas* es una herramienta que automatiza estas tareas de desarrollo de rutinas y mucho más. Visual Studio proporciona compatibilidad integrada para dos ejecutores de tareas basadas en JavaScript populares: [Gulp](https://gulpjs.com/) y [Grunt](using-grunt.md).

## <a name="gulp"></a>Gulp

Gulp es un toolkit de compilación streaming basado en JavaScript para el código del lado cliente. Normalmente, sirve para transmitir los archivos de cliente a través de una serie de procesos cuando se desencadena un evento específico en un entorno de compilación. Por ejemplo, Gulp puede utilizarse para automatizar [unión y minificación](bundling-and-minification.md) o la limpieza de un entorno de desarrollo antes de una nueva compilación.

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

El código anterior especifica qué módulos de nodo son necesarios. El `require` cada módulo importaciones de función para que las tareas dependientes pueden utilizar sus características. Cada uno de los módulos importados se asigna a una variable. Los módulos pueden encontrarse por nombre o ruta de acceso. En este ejemplo, los módulos denominan `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, y `gulp-uglify` se recuperan por su nombre. Además, se crean una serie de rutas de acceso para que se pueden volver a usar las ubicaciones de archivos CSS y JavaScript y hacer referencia en las tareas. En la tabla siguiente se proporciona descripciones de los módulos incluidos en *gulpfile.js*.

| Nombre de módulo | Descripción |
| ----------- | ----------- |
| Gulp        | La transmisión por secuencias del sistema de compilación Gulp. Para obtener más información, consulte [gulp](https://www.npmjs.com/package/gulp). |
| rimraf      | Un módulo de eliminación de nodo. Para obtener más información, consulte [rimraf](https://www.npmjs.com/package/rimraf). |
| gulp-concat | Un módulo que concatena los archivos según el carácter de nueva línea del sistema operativo. Para obtener más información, consulte [gulp-concat](https://www.npmjs.com/package/gulp-concat). |
| gulp-cssmin | Un módulo que minifica objeto archivos CSS. Para obtener más información, consulte [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin). |
| gulp-uglify | Un módulo que minifica objeto *.js* archivos. Para obtener más información, consulte [a que uglify gulp](https://www.npmjs.com/package/gulp-uglify). |

Una vez que se importan los módulos necesarios, se pueden especificar las tareas. Aquí hay seis tareas registrado, representado por el código siguiente:

```javascript
gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

gulp.task("min:js", () => {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", () => {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", gulp.series(["min:js", "min:css"]));
    
// A 'default' task is required by Gulp v4
gulp.task("default", gulp.series(["min"]));
```

---

En la tabla siguiente proporciona una explicación de las tareas especificadas en el código anterior:

|Nombre de tarea|Descripción|
|--- |--- |
|clean:js|Una tarea que usa el módulo de eliminación de nodo rimraf para quitar la versión del archivo site.js minimizada.|
|clean:css|Una tarea que usa el módulo de eliminación de nodo rimraf para quitar la versión del archivo site.css minimizada.|
|clean|Una tarea que llama a la `clean:js` tarea, seguido por la `clean:css` tarea.|
|min:js|Una tarea que minifica objeto y se concatena todos los archivos .js en la carpeta para js. El. min.js archivos se excluyen.|
|min:css|Una tarea que minifica objeto y se concatena todos los archivos .css dentro de la carpeta de css. El. min.css archivos se excluyen.|
|min|Una tarea que llama a la `min:js` tarea, seguido por la `min:css` tarea.|

## <a name="running-default-tasks"></a>Ejecución de tareas predeterminado

Si ya no ha creado una nueva aplicación Web, cree un nuevo proyecto de aplicación Web ASP.NET en Visual Studio.

1.  Abra el *package.json* archivo (agregar si no existe) y agregue lo siguiente.

    ```json
    {
      "devDependencies": {
        "gulp": "^4.0.0",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.2.0",
        "gulp-uglify": "3.0.0",
        "rimraf": "2.6.1"
      }
    }
    ```

2.  Agregar un nuevo archivo JavaScript al proyecto y asígnele el nombre *gulpfile.js*, a continuación, copie el código siguiente.

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    const gulp = require("gulp"),
          rimraf = require("rimraf"),
          concat = require("gulp-concat"),
          cssmin = require("gulp-cssmin"),
          uglify = require("gulp-uglify");
    
    const paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
    gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
    gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

    gulp.task("min:js", () => {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });

    gulp.task("min:css", () => {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });

    gulp.task("min", gulp.series(["min:js", "min:css"]));
    
    // A 'default' task is required by Gulp v4
    gulp.task("default", gulp.series(["min"]));
    ```

3.  En **el Explorador de soluciones**, haga clic en *gulpfile.js*y seleccione **Task Runner Explorer**.
    
    ![Abra el Explorador de ejecutores de tareas desde el Explorador de soluciones](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **Task Runner Explorer** muestra la lista de las tareas de Gulp. (Tendrá que hacer clic en el **actualizar** botón que aparece a la izquierda del nombre del proyecto.)
    
    ![Explorador de ejecutores de tareas](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > El **Task Runner Explorer** elemento de menú contextual aparece sólo si *gulpfile.js* está en el directorio raíz del proyecto.

4.  Debajo de la directiva **tareas** en **Task Runner Explorer**, haga clic en **limpia**y seleccione **ejecutar** en el menú emergente.

    ![Tarea de limpieza de explorador del ejecutador de tareas](using-gulp/_static/04-TaskRunner-clean.png)

    **Task Runner Explorer** creará una nueva ficha denominada **limpia** y ejecutar la tarea clean según se define en *gulpfile.js*.

5.  Haga clic en el **limpia** de tareas y, después, seleccione **enlaces** > **antes de compilar**.

    ![Task Runner Explorer BeforeBuild de enlace](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    El **antes de compilar** enlace configura la tarea clean se ejecute automáticamente antes de cada compilación del proyecto.

Los enlaces que configuró con **Task Runner Explorer** se almacenan en forma de un comentario en la parte superior de su *gulpfile.js* y son eficaces solo en Visual Studio. Es una alternativa que no requiere Visual Studio configurar la ejecución automática de las tareas de gulp en su *.csproj* archivo. Por ejemplo, poner esto en su *.csproj* archivo:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

Ahora la tarea clean se ejecute cuando se ejecuta el proyecto en Visual Studio o desde un símbolo del sistema mediante la [dotnet ejecutar](/dotnet/core/tools/dotnet-run) comando (ejecutar `npm install` primero).

## <a name="defining-and-running-a-new-task"></a>Definir y ejecutar una nueva tarea

Para definir una nueva tarea de Gulp, modificar *gulpfile.js*.

1.  Agregue el siguiente código de JavaScript al final de *gulpfile.js*:

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    Esta tarea se denomina `first`, y simplemente muestra una cadena.

2.  Guardar *gulpfile.js*.

3.  En **el Explorador de soluciones**, haga clic en *gulpfile.js*y seleccione *Task Runner Explorer*.

4.  En **Task Runner Explorer**, haga clic en **primera**y seleccione **ejecutar**.

    ![Ejecute la primera tarea Task Runner Explorer](using-gulp/_static/06-TaskRunner-First.png)

    Se muestra el texto de salida. Para ver ejemplos basados en escenarios comunes, consulte [Gulp recetas](#gulp-recipes).

## <a name="defining-and-running-tasks-in-a-series"></a>Definir y ejecutar tareas en una serie

Al ejecutar varias tareas, las tareas se ejecutarán simultáneamente de forma predeterminada. Sin embargo, si tiene que ejecutar tareas en un orden específico, debe especificar cuando cada tarea está completa, así como las tareas que dependen de la finalización de otra tarea.

1.  Para definir una serie de tareas que se ejecutarán en orden, reemplace el `first` tarea que agregó anteriormente en *gulpfile.js* con lo siguiente:

    ```javascript
    gulp.task('series:first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    gulp.task('series:second', done => {
      console.log('second task! <-----');
      done(); // signal completion
    });

    gulp.task('series', gulp.series(['series:first', 'series:second']), () => { });

    // A 'default' task is required by Gulp v4
    gulp.task('default', gulp.series('series'));
    ```
 
    Ahora tiene tres tareas: `series:first`, `series:second`, y `series`. El `series:second` tarea incluye un segundo parámetro que especifica una matriz de tareas que se ejecutarán y se complete antes de la `series:second` se ejecutará la tarea. Como se especifica en el código anterior, solo la `series:first` tareas deben completarse antes de la `series:second` se ejecutará la tarea.

2.  Guardar *gulpfile.js*.

3.  En **el Explorador de soluciones**, haga clic en *gulpfile.js* y seleccione **Task Runner Explorer** si no está abierta.

4.  En **Task Runner Explorer**, haga clic en **serie** y seleccione **ejecutar**.

    ![Task Runner Explorer Ejecutar tarea de serie](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense proporciona la finalización de código, descripciones de parámetros y otras características para aumentar la productividad y reducir los errores. Las tareas de gulp están escritas en JavaScript; por lo tanto, IntelliSense puede proporcionar asistencia durante el desarrollo. Cuando se trabaja con JavaScript, IntelliSense muestra los objetos, funciones, propiedades y parámetros que están disponibles según el contexto actual. Seleccione una opción de codificación de la lista emergente proporcionada IntelliSense para completar el código.

![IntelliSense de gulp](using-gulp/_static/08-IntelliSense.png)

Para obtener más información acerca de IntelliSense, vea [IntelliSense para JavaScript](/visualstudio/ide/javascript-intellisense).

## <a name="development-staging-and-production-environments"></a>Entornos de desarrollo, ensayo y producción

Cuando se usa Gulp para optimizar los archivos de cliente de almacenamiento provisional y producción, se guardan los archivos procesados en una ubicación de ensayo y producción local. El *_Layout.cshtml* archivo usa el **entorno** aplicación auxiliar para proporcionar dos versiones diferentes de los archivos CSS de etiquetas. Hay una versión de los archivos CSS para el desarrollo y la otra versión está optimizada para ensayo y producción. En Visual Studio 2017, al cambiar el **ASPNETCORE_ENVIRONMENT** variable de entorno `Production`, Visual Studio compilará la aplicación Web y un vínculo a los archivos CSS minimizados. El marcado siguiente se muestra el **entorno** etiquetar las aplicaciones auxiliares que contiene las etiquetas de vínculo a la `Development` CSS archivos y el minimizados `Staging, Production` archivos CSS.

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

## <a name="switching-between-environments"></a>Cómo cambiar entre entornos

Para cambiar entre la compilación en diferentes entornos, modifique la **ASPNETCORE_ENVIRONMENT** valor de la variable de entorno.

1.  En **Task Runner Explorer**, compruebe que la **min** se ha establecido la tarea en ejecución **antes de compilar**.

2.  En **el Explorador de soluciones**, haga clic en el nombre del proyecto y seleccione **propiedades**.

    Se muestra la hoja de propiedades de la aplicación Web.

3.  Haga clic en la pestaña **Depurar**.

4.  Establezca el valor de la **: entorno de hospedaje** variable de entorno `Production`.

5.  Presione **F5** para ejecutar la aplicación en un explorador.

6.  En la ventana del explorador, haga clic en la página y seleccione **ver código fuente** para ver el código HTML de la página.

    Tenga en cuenta que los vínculos de hoja de estilos apuntan a los archivos CSS minificados.

7.  Cierre el explorador para detener la aplicación Web.

8.  En Visual Studio, vuelva a la hoja de propiedades de la aplicación Web y cambie el **: entorno de hospedaje** hacer una copia de la variable de entorno para `Development`.

9.  Presione **F5** volver a ejecutar la aplicación en un explorador.

10. En la ventana del explorador, haga clic en la página y seleccione **ver código fuente** para ver el código HTML de la página.

    Tenga en cuenta que los vínculos de hoja de estilos apuntan a las versiones de los archivos CSS unminified.

Para obtener más información relacionada con los entornos en ASP.NET Core, consulte [usar varios entornos](../fundamentals/environments.md).

## <a name="task-and-module-details"></a>Detalles de tarea y módulo

Una tarea de Gulp está registrada con un nombre de función. Puede especificar las dependencias si deben ejecutar otras tareas antes de la tarea actual. Funciones adicionales que le permiten ejecutar y ver las tareas de Gulp, así como para establecer el origen (*src*) y de destino (*dest*) de los archivos que se está modificando. Estas son las principales funciones de API de Gulp:

|Gulp (función)|Sintaxis|Descripción|
|---   |--- |--- |
|tarea  |`gulp.task(name[, deps], fn) { }`|El `task` función crea una tarea. El `name` parámetro define el nombre de la tarea. El `deps` parámetro contiene una matriz de tareas que deben completarse antes de que se ejecuta esta tarea. El `fn` parámetro representa una función de devolución de llamada que realiza las operaciones de la tarea.|
|Inspección |`gulp.watch(glob [, opts], tasks) { }`|El `watch` función supervisa las tareas de archivos y se ejecuta cuando se produce un cambio de archivo. El `glob` parámetro es un `string` o `array` que determina qué archivos a inspeccionar. El `opts` parámetro proporciona inspeccionando las opciones de archivo adicionales.|
|src   |`gulp.src(globs[, options]) { }`|El `src` función proporciona archivos que coinciden con los valores de elemento glob. El `glob` parámetro es un `string` o `array` que determina qué archivos para leer. El `options` parámetro proporciona opciones de archivo adicionales.|
|dest  |`gulp.dest(path[, options]) { }`|El `dest` función define una ubicación a la que se pueden escribir archivos. El `path` parámetro es una cadena o una función que determina la carpeta de destino. El `options` parámetro es un objeto que especifica las opciones de carpeta de salida.|

Para obtener información adicional sobre la referencia de API de Gulp, consulte [Gulp Docs API](https://gulpjs.org/API.html).

## <a name="gulp-recipes"></a>Recetas de gulp

La Comunidad de Gulp proporciona Gulp [recetas](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md). Estas recetas constan de las tareas de Gulp para abordar escenarios comunes.

## <a name="additional-resources"></a>Recursos adicionales

* [Documentación de gulp](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [Unión y minificación en ASP.NET Core](bundling-and-minification.md)
* [Uso de Grunt en ASP.NET Core](using-grunt.md)
