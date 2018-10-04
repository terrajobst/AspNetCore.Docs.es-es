---
title: Usar Gulp en ASP.NET Core
author: rick-anderson
description: Aprenda a usar Gulp en ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: 4f383be0498b5b861bd43cc0f0685b1e62c7571b
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795530"
---
# <a name="use-gulp-in-aspnet-core"></a><span data-ttu-id="31bff-103">Usar Gulp en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31bff-103">Use Gulp in ASP.NET Core</span></span>

<span data-ttu-id="31bff-104">Por [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), y [David Borovice](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="31bff-104">By [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="31bff-105">En una aplicación web moderna típica, es posible que el proceso de compilación:</span><span class="sxs-lookup"><span data-stu-id="31bff-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="31bff-106">Agrupar y minimizar los archivos JavaScript y CSS.</span><span class="sxs-lookup"><span data-stu-id="31bff-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="31bff-107">Ejecutar herramientas para llamar a las tareas de unión y minificación antes de cada compilación.</span><span class="sxs-lookup"><span data-stu-id="31bff-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="31bff-108">Archivos de compilar menos o SASS en CSS.</span><span class="sxs-lookup"><span data-stu-id="31bff-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="31bff-109">Compilar archivos CoffeeScript o TypeScript para JavaScript.</span><span class="sxs-lookup"><span data-stu-id="31bff-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="31bff-110">Un *ejecutor de tareas* es una herramienta que automatiza estas tareas de desarrollo de rutinas y mucho más.</span><span class="sxs-lookup"><span data-stu-id="31bff-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="31bff-111">Visual Studio proporciona compatibilidad integrada para dos ejecutores de tareas basadas en JavaScript populares: [Gulp](https://gulpjs.com/) y [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="31bff-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="31bff-112">Gulp</span><span class="sxs-lookup"><span data-stu-id="31bff-112">Gulp</span></span>

<span data-ttu-id="31bff-113">Gulp es un toolkit de compilación streaming basado en JavaScript para el código del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="31bff-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="31bff-114">Normalmente, sirve para transmitir los archivos de cliente a través de una serie de procesos cuando se desencadena un evento específico en un entorno de compilación.</span><span class="sxs-lookup"><span data-stu-id="31bff-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="31bff-115">Por ejemplo, Gulp puede utilizarse para automatizar [unión y minificación](bundling-and-minification.md) o la limpieza de un entorno de desarrollo antes de una nueva compilación.</span><span class="sxs-lookup"><span data-stu-id="31bff-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="31bff-116">Se define un conjunto de tareas de Gulp en *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="31bff-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="31bff-117">El siguiente código de JavaScript incluye módulos de Gulp y especifica las rutas de acceso de archivo que se haga referencia dentro de las tareas disponibles próximamente:</span><span class="sxs-lookup"><span data-stu-id="31bff-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="31bff-118">El código anterior especifica qué módulos de nodo son necesarios.</span><span class="sxs-lookup"><span data-stu-id="31bff-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="31bff-119">El `require` cada módulo importaciones de función para que las tareas dependientes pueden utilizar sus características.</span><span class="sxs-lookup"><span data-stu-id="31bff-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="31bff-120">Cada uno de los módulos importados se asigna a una variable.</span><span class="sxs-lookup"><span data-stu-id="31bff-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="31bff-121">Los módulos pueden encontrarse por nombre o ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="31bff-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="31bff-122">En este ejemplo, los módulos denominan `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, y `gulp-uglify` se recuperan por su nombre.</span><span class="sxs-lookup"><span data-stu-id="31bff-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="31bff-123">Además, se crean una serie de rutas de acceso para que se pueden volver a usar las ubicaciones de archivos CSS y JavaScript y hacer referencia en las tareas.</span><span class="sxs-lookup"><span data-stu-id="31bff-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="31bff-124">En la tabla siguiente se proporciona descripciones de los módulos incluidos en *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="31bff-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="31bff-125">Nombre del módulo</span><span class="sxs-lookup"><span data-stu-id="31bff-125">Module Name</span></span> | <span data-ttu-id="31bff-126">Descripción</span><span class="sxs-lookup"><span data-stu-id="31bff-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="31bff-127">gulp</span><span class="sxs-lookup"><span data-stu-id="31bff-127">gulp</span></span>        | <span data-ttu-id="31bff-128">La transmisión por secuencias del sistema de compilación Gulp.</span><span class="sxs-lookup"><span data-stu-id="31bff-128">The Gulp streaming build system.</span></span> <span data-ttu-id="31bff-129">Para obtener más información, consulte [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="31bff-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="31bff-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="31bff-130">rimraf</span></span>      | <span data-ttu-id="31bff-131">Un módulo de eliminación de nodo.</span><span class="sxs-lookup"><span data-stu-id="31bff-131">A Node deletion module.</span></span> <span data-ttu-id="31bff-132">Para obtener más información, consulte [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="31bff-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="31bff-133">gulp-concat</span><span class="sxs-lookup"><span data-stu-id="31bff-133">gulp-concat</span></span> | <span data-ttu-id="31bff-134">Un módulo que concatena los archivos según el carácter de nueva línea del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="31bff-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="31bff-135">Para obtener más información, consulte [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="31bff-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="31bff-136">gulp cssmin</span><span class="sxs-lookup"><span data-stu-id="31bff-136">gulp-cssmin</span></span> | <span data-ttu-id="31bff-137">Un módulo que minifica objeto archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="31bff-137">A module that minifies CSS files.</span></span> <span data-ttu-id="31bff-138">Para obtener más información, consulte [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="31bff-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="31bff-139">a que uglify de gulp</span><span class="sxs-lookup"><span data-stu-id="31bff-139">gulp-uglify</span></span> | <span data-ttu-id="31bff-140">Un módulo que minifica objeto *.js* archivos.</span><span class="sxs-lookup"><span data-stu-id="31bff-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="31bff-141">Para obtener más información, consulte [a que uglify gulp](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="31bff-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="31bff-142">Una vez que se importan los módulos necesarios, se pueden especificar las tareas.</span><span class="sxs-lookup"><span data-stu-id="31bff-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="31bff-143">Aquí hay seis tareas registrado, representado por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="31bff-143">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="31bff-144">En la tabla siguiente proporciona una explicación de las tareas especificadas en el código anterior:</span><span class="sxs-lookup"><span data-stu-id="31bff-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="31bff-145">Nombre de tarea</span><span class="sxs-lookup"><span data-stu-id="31bff-145">Task Name</span></span>|<span data-ttu-id="31bff-146">Descripción</span><span class="sxs-lookup"><span data-stu-id="31bff-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="31bff-147">limpiar: js</span><span class="sxs-lookup"><span data-stu-id="31bff-147">clean:js</span></span>|<span data-ttu-id="31bff-148">Una tarea que usa el módulo de eliminación de nodo rimraf para quitar la versión del archivo site.js minimizada.</span><span class="sxs-lookup"><span data-stu-id="31bff-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="31bff-149">limpiar: css</span><span class="sxs-lookup"><span data-stu-id="31bff-149">clean:css</span></span>|<span data-ttu-id="31bff-150">Una tarea que usa el módulo de eliminación de nodo rimraf para quitar la versión del archivo site.css minimizada.</span><span class="sxs-lookup"><span data-stu-id="31bff-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="31bff-151">Limpiar</span><span class="sxs-lookup"><span data-stu-id="31bff-151">clean</span></span>|<span data-ttu-id="31bff-152">Una tarea que llama a la `clean:js` tarea, seguido por la `clean:css` tarea.</span><span class="sxs-lookup"><span data-stu-id="31bff-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="31bff-153">min:js</span><span class="sxs-lookup"><span data-stu-id="31bff-153">min:js</span></span>|<span data-ttu-id="31bff-154">Una tarea que minifica objeto y se concatena todos los archivos .js en la carpeta para js.</span><span class="sxs-lookup"><span data-stu-id="31bff-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="31bff-155">El. min.js archivos se excluyen.</span><span class="sxs-lookup"><span data-stu-id="31bff-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="31bff-156">min:CSS</span><span class="sxs-lookup"><span data-stu-id="31bff-156">min:css</span></span>|<span data-ttu-id="31bff-157">Una tarea que minifica objeto y se concatena todos los archivos .css dentro de la carpeta de css.</span><span class="sxs-lookup"><span data-stu-id="31bff-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="31bff-158">El. min.css archivos se excluyen.</span><span class="sxs-lookup"><span data-stu-id="31bff-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="31bff-159">min</span><span class="sxs-lookup"><span data-stu-id="31bff-159">min</span></span>|<span data-ttu-id="31bff-160">Una tarea que llama a la `min:js` tarea, seguido por la `min:css` tarea.</span><span class="sxs-lookup"><span data-stu-id="31bff-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="31bff-161">Ejecución de tareas predeterminado</span><span class="sxs-lookup"><span data-stu-id="31bff-161">Running default tasks</span></span>

<span data-ttu-id="31bff-162">Si ya no ha creado una nueva aplicación Web, cree un nuevo proyecto de aplicación Web ASP.NET en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="31bff-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="31bff-163">Abra el *package.json* archivo (agregar si no existe) y agregue lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="31bff-163">Open the *package.json* file (add if not there) and add the following.</span></span>

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

2.  <span data-ttu-id="31bff-164">Agregar un nuevo archivo JavaScript al proyecto y asígnele el nombre *gulpfile.js*, a continuación, copie el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="31bff-164">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

3.  <span data-ttu-id="31bff-165">En **el Explorador de soluciones**, haga clic en *gulpfile.js*y seleccione **Task Runner Explorer**.</span><span class="sxs-lookup"><span data-stu-id="31bff-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![Abra el Explorador de ejecutores de tareas desde el Explorador de soluciones](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="31bff-167">**Task Runner Explorer** muestra la lista de las tareas de Gulp.</span><span class="sxs-lookup"><span data-stu-id="31bff-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="31bff-168">(Tendrá que hacer clic en el **actualizar** botón que aparece a la izquierda del nombre del proyecto.)</span><span class="sxs-lookup"><span data-stu-id="31bff-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![Explorador de ejecutores de tareas](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="31bff-170">El **Task Runner Explorer** elemento de menú contextual aparece sólo si *gulpfile.js* está en el directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="31bff-170">The **Task Runner Explorer** context menu item appears only if *gulpfile.js* is in the root project directory.</span></span>

4.  <span data-ttu-id="31bff-171">Debajo de la directiva **tareas** en **Task Runner Explorer**, haga clic en **limpia**y seleccione **ejecutar** en el menú emergente.</span><span class="sxs-lookup"><span data-stu-id="31bff-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Tarea de limpieza de explorador del ejecutador de tareas](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="31bff-173">**Task Runner Explorer** creará una nueva ficha denominada **limpia** y ejecutar la tarea clean según se define en *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="31bff-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="31bff-174">Haga clic en el **limpia** de tareas y, después, seleccione **enlaces** > **antes de compilar**.</span><span class="sxs-lookup"><span data-stu-id="31bff-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Task Runner Explorer BeforeBuild de enlace](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="31bff-176">El **antes de compilar** enlace configura la tarea clean se ejecute automáticamente antes de cada compilación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="31bff-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="31bff-177">Los enlaces que configuró con **Task Runner Explorer** se almacenan en forma de un comentario en la parte superior de su *gulpfile.js* y son eficaces solo en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="31bff-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="31bff-178">Es una alternativa que no requiere Visual Studio configurar la ejecución automática de las tareas de gulp en su *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="31bff-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="31bff-179">Por ejemplo, poner esto en su *.csproj* archivo:</span><span class="sxs-lookup"><span data-stu-id="31bff-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="31bff-180">Ahora la tarea clean se ejecute cuando se ejecuta el proyecto en Visual Studio o desde un símbolo del sistema mediante la [dotnet ejecutar](/dotnet/core/tools/dotnet-run) comando (ejecutar `npm install` primero).</span><span class="sxs-lookup"><span data-stu-id="31bff-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="31bff-181">Definir y ejecutar una nueva tarea</span><span class="sxs-lookup"><span data-stu-id="31bff-181">Defining and running a new task</span></span>

<span data-ttu-id="31bff-182">Para definir una nueva tarea de Gulp, modificar *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="31bff-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="31bff-183">Agregue el siguiente código de JavaScript al final de *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="31bff-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    <span data-ttu-id="31bff-184">Esta tarea se denomina `first`, y simplemente muestra una cadena.</span><span class="sxs-lookup"><span data-stu-id="31bff-184">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="31bff-185">Guardar *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="31bff-185">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="31bff-186">En **el Explorador de soluciones**, haga clic en *gulpfile.js*y seleccione *Task Runner Explorer*.</span><span class="sxs-lookup"><span data-stu-id="31bff-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="31bff-187">En **Task Runner Explorer**, haga clic en **primera**y seleccione **ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="31bff-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Ejecute la primera tarea Task Runner Explorer](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="31bff-189">Se muestra el texto de salida.</span><span class="sxs-lookup"><span data-stu-id="31bff-189">The output text is displayed.</span></span> <span data-ttu-id="31bff-190">Para ver ejemplos basados en escenarios comunes, consulte [Gulp recetas](#gulp-recipes).</span><span class="sxs-lookup"><span data-stu-id="31bff-190">To see examples based on common scenarios, see [Gulp Recipes](#gulp-recipes).</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="31bff-191">Definir y ejecutar tareas en una serie</span><span class="sxs-lookup"><span data-stu-id="31bff-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="31bff-192">Al ejecutar varias tareas, las tareas se ejecutarán simultáneamente de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="31bff-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="31bff-193">Sin embargo, si tiene que ejecutar tareas en un orden específico, debe especificar cuando cada tarea está completa, así como las tareas que dependen de la finalización de otra tarea.</span><span class="sxs-lookup"><span data-stu-id="31bff-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="31bff-194">Para definir una serie de tareas que se ejecutarán en orden, reemplace el `first` tarea que agregó anteriormente en *gulpfile.js* con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="31bff-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

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
 
    <span data-ttu-id="31bff-195">Ahora tiene tres tareas: `series:first`, `series:second`, y `series`.</span><span class="sxs-lookup"><span data-stu-id="31bff-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="31bff-196">El `series:second` tarea incluye un segundo parámetro que especifica una matriz de tareas que se ejecutarán y se complete antes de la `series:second` se ejecutará la tarea.</span><span class="sxs-lookup"><span data-stu-id="31bff-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="31bff-197">Como se especifica en el código anterior, solo la `series:first` tareas deben completarse antes de la `series:second` se ejecutará la tarea.</span><span class="sxs-lookup"><span data-stu-id="31bff-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="31bff-198">Guardar *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="31bff-198">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="31bff-199">En **el Explorador de soluciones**, haga clic en *gulpfile.js* y seleccione **Task Runner Explorer** si no está abierta.</span><span class="sxs-lookup"><span data-stu-id="31bff-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4.  <span data-ttu-id="31bff-200">En **Task Runner Explorer**, haga clic en **serie** y seleccione **ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="31bff-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Task Runner Explorer Ejecutar tarea de serie](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="31bff-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="31bff-202">IntelliSense</span></span>

<span data-ttu-id="31bff-203">IntelliSense proporciona la finalización de código, descripciones de parámetros y otras características para aumentar la productividad y reducir los errores.</span><span class="sxs-lookup"><span data-stu-id="31bff-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="31bff-204">Las tareas de gulp están escritas en JavaScript; por lo tanto, IntelliSense puede proporcionar asistencia durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="31bff-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="31bff-205">Cuando se trabaja con JavaScript, IntelliSense muestra los objetos, funciones, propiedades y parámetros que están disponibles según el contexto actual.</span><span class="sxs-lookup"><span data-stu-id="31bff-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="31bff-206">Seleccione una opción de codificación de la lista emergente proporcionada IntelliSense para completar el código.</span><span class="sxs-lookup"><span data-stu-id="31bff-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![IntelliSense de gulp](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="31bff-208">Para obtener más información acerca de IntelliSense, vea [IntelliSense para JavaScript](/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="31bff-208">For more information about IntelliSense, see [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="31bff-209">Entornos de desarrollo, ensayo y producción</span><span class="sxs-lookup"><span data-stu-id="31bff-209">Development, staging, and production environments</span></span>

<span data-ttu-id="31bff-210">Cuando se usa Gulp para optimizar los archivos de cliente de almacenamiento provisional y producción, se guardan los archivos procesados en una ubicación de ensayo y producción local.</span><span class="sxs-lookup"><span data-stu-id="31bff-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="31bff-211">El *_Layout.cshtml* archivo usa el **entorno** aplicación auxiliar para proporcionar dos versiones diferentes de los archivos CSS de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="31bff-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="31bff-212">Hay una versión de los archivos CSS para el desarrollo y la otra versión está optimizada para ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="31bff-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="31bff-213">En Visual Studio 2017, al cambiar el **ASPNETCORE_ENVIRONMENT** variable de entorno `Production`, Visual Studio compilará la aplicación Web y un vínculo a los archivos CSS minimizados.</span><span class="sxs-lookup"><span data-stu-id="31bff-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="31bff-214">El marcado siguiente se muestra el **entorno** etiquetar las aplicaciones auxiliares que contiene las etiquetas de vínculo a la `Development` CSS archivos y el minimizados `Staging, Production` archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="31bff-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="31bff-215">Cómo cambiar entre entornos</span><span class="sxs-lookup"><span data-stu-id="31bff-215">Switching between environments</span></span>

<span data-ttu-id="31bff-216">Para cambiar entre la compilación en diferentes entornos, modifique la **ASPNETCORE_ENVIRONMENT** valor de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="31bff-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="31bff-217">En **Task Runner Explorer**, compruebe que la **min** se ha establecido la tarea en ejecución **antes de compilar**.</span><span class="sxs-lookup"><span data-stu-id="31bff-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="31bff-218">En **el Explorador de soluciones**, haga clic en el nombre del proyecto y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="31bff-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="31bff-219">Se muestra la hoja de propiedades de la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="31bff-219">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="31bff-220">Haga clic en la pestaña **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="31bff-220">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="31bff-221">Establezca el valor de la **: entorno de hospedaje** variable de entorno `Production`.</span><span class="sxs-lookup"><span data-stu-id="31bff-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="31bff-222">Presione **F5** para ejecutar la aplicación en un explorador.</span><span class="sxs-lookup"><span data-stu-id="31bff-222">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="31bff-223">En la ventana del explorador, haga clic en la página y seleccione **ver código fuente** para ver el código HTML de la página.</span><span class="sxs-lookup"><span data-stu-id="31bff-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="31bff-224">Tenga en cuenta que los vínculos de hoja de estilos apuntan a los archivos CSS minificados.</span><span class="sxs-lookup"><span data-stu-id="31bff-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="31bff-225">Cierre el explorador para detener la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="31bff-225">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="31bff-226">En Visual Studio, vuelva a la hoja de propiedades de la aplicación Web y cambie el **: entorno de hospedaje** hacer una copia de la variable de entorno para `Development`.</span><span class="sxs-lookup"><span data-stu-id="31bff-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="31bff-227">Presione **F5** volver a ejecutar la aplicación en un explorador.</span><span class="sxs-lookup"><span data-stu-id="31bff-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="31bff-228">En la ventana del explorador, haga clic en la página y seleccione **ver código fuente** para ver el código HTML de la página.</span><span class="sxs-lookup"><span data-stu-id="31bff-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="31bff-229">Tenga en cuenta que los vínculos de hoja de estilos apuntan a las versiones de los archivos CSS unminified.</span><span class="sxs-lookup"><span data-stu-id="31bff-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="31bff-230">Para obtener más información relacionada con los entornos en ASP.NET Core, consulte [usar varios entornos](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="31bff-230">For more information related to environments in ASP.NET Core, see [Use multiple environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="31bff-231">Detalles de tarea y módulo</span><span class="sxs-lookup"><span data-stu-id="31bff-231">Task and module details</span></span>

<span data-ttu-id="31bff-232">Una tarea de Gulp está registrada con un nombre de función.</span><span class="sxs-lookup"><span data-stu-id="31bff-232">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="31bff-233">Puede especificar las dependencias si deben ejecutar otras tareas antes de la tarea actual.</span><span class="sxs-lookup"><span data-stu-id="31bff-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="31bff-234">Funciones adicionales que le permiten ejecutar y ver las tareas de Gulp, así como para establecer el origen (*src*) y de destino (*dest*) de los archivos que se está modificando.</span><span class="sxs-lookup"><span data-stu-id="31bff-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="31bff-235">Estas son las principales funciones de API de Gulp:</span><span class="sxs-lookup"><span data-stu-id="31bff-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="31bff-236">Gulp (función)</span><span class="sxs-lookup"><span data-stu-id="31bff-236">Gulp Function</span></span>|<span data-ttu-id="31bff-237">Sintaxis</span><span class="sxs-lookup"><span data-stu-id="31bff-237">Syntax</span></span>|<span data-ttu-id="31bff-238">Descripción</span><span class="sxs-lookup"><span data-stu-id="31bff-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="31bff-239">tarea</span><span class="sxs-lookup"><span data-stu-id="31bff-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="31bff-240">El `task` función crea una tarea.</span><span class="sxs-lookup"><span data-stu-id="31bff-240">The `task` function creates a task.</span></span> <span data-ttu-id="31bff-241">El `name` parámetro define el nombre de la tarea.</span><span class="sxs-lookup"><span data-stu-id="31bff-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="31bff-242">El `deps` parámetro contiene una matriz de tareas que deben completarse antes de que se ejecuta esta tarea.</span><span class="sxs-lookup"><span data-stu-id="31bff-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="31bff-243">El `fn` parámetro representa una función de devolución de llamada que realiza las operaciones de la tarea.</span><span class="sxs-lookup"><span data-stu-id="31bff-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="31bff-244">Inspección</span><span class="sxs-lookup"><span data-stu-id="31bff-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="31bff-245">El `watch` función supervisa las tareas de archivos y se ejecuta cuando se produce un cambio de archivo.</span><span class="sxs-lookup"><span data-stu-id="31bff-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="31bff-246">El `glob` parámetro es un `string` o `array` que determina qué archivos a inspeccionar.</span><span class="sxs-lookup"><span data-stu-id="31bff-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="31bff-247">El `opts` parámetro proporciona inspeccionando las opciones de archivo adicionales.</span><span class="sxs-lookup"><span data-stu-id="31bff-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="31bff-248">src</span><span class="sxs-lookup"><span data-stu-id="31bff-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="31bff-249">El `src` función proporciona archivos que coinciden con los valores de elemento glob.</span><span class="sxs-lookup"><span data-stu-id="31bff-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="31bff-250">El `glob` parámetro es un `string` o `array` que determina qué archivos para leer.</span><span class="sxs-lookup"><span data-stu-id="31bff-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="31bff-251">El `options` parámetro proporciona opciones de archivo adicionales.</span><span class="sxs-lookup"><span data-stu-id="31bff-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="31bff-252">dest</span><span class="sxs-lookup"><span data-stu-id="31bff-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="31bff-253">El `dest` función define una ubicación a la que se pueden escribir archivos.</span><span class="sxs-lookup"><span data-stu-id="31bff-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="31bff-254">El `path` parámetro es una cadena o una función que determina la carpeta de destino.</span><span class="sxs-lookup"><span data-stu-id="31bff-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="31bff-255">El `options` parámetro es un objeto que especifica las opciones de carpeta de salida.</span><span class="sxs-lookup"><span data-stu-id="31bff-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="31bff-256">Para obtener información adicional sobre la referencia de API de Gulp, consulte [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span><span class="sxs-lookup"><span data-stu-id="31bff-256">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="31bff-257">Recetas de gulp</span><span class="sxs-lookup"><span data-stu-id="31bff-257">Gulp recipes</span></span>

<span data-ttu-id="31bff-258">La Comunidad de Gulp proporciona Gulp [recetas](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="31bff-258">The Gulp community provides Gulp [Recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="31bff-259">Estas recetas constan de las tareas de Gulp para abordar escenarios comunes.</span><span class="sxs-lookup"><span data-stu-id="31bff-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="31bff-260">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="31bff-260">Additional resources</span></span>

* [<span data-ttu-id="31bff-261">Documentación de gulp</span><span class="sxs-lookup"><span data-stu-id="31bff-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="31bff-262">Unión y minificación en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31bff-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="31bff-263">Uso de Grunt en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31bff-263">Use Grunt in ASP.NET Core</span></span>](using-grunt.md)
