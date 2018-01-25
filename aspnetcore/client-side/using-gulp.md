---
title: Usar Gulp en ASP.NET Core
author: rick-anderson
description: "Obtenga información acerca de cómo usar Gulp en ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-gulp
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2ccfed42d66ea49c5f2745bc8653d8fb12bf707a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-using-gulp-in-aspnet-core"></a><span data-ttu-id="2830a-103">Introducción al uso de Gulp en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2830a-103">Introduction to using Gulp in ASP.NET Core</span></span> 

<span data-ttu-id="2830a-104">Por [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), y [Shayne Boyer](https://twitter.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="2830a-104">By [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), and [Shayne Boyer](https://twitter.com/spboyer)</span></span>

<span data-ttu-id="2830a-105">En una aplicación web moderna típica, el proceso de compilación puede:</span><span class="sxs-lookup"><span data-stu-id="2830a-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="2830a-106">Agrupar y minificar archivos JavaScript y CSS.</span><span class="sxs-lookup"><span data-stu-id="2830a-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="2830a-107">Ejecutar herramientas para llamar a las tareas de agrupación y minificación antes de cada compilación.</span><span class="sxs-lookup"><span data-stu-id="2830a-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="2830a-108">Compilar menos o SASS archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="2830a-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="2830a-109">Compila los archivos de CoffeeScript o TypeScript a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2830a-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="2830a-110">A *ejecutor de tareas* es una herramienta que automatiza estas tareas de desarrollo de rutinas y mucho más.</span><span class="sxs-lookup"><span data-stu-id="2830a-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="2830a-111">Visual Studio proporciona compatibilidad integrada para dos corredores populares tareas basadas en JavaScript: [Gulp](https://gulpjs.com/) y [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="2830a-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="2830a-112">gulp</span><span class="sxs-lookup"><span data-stu-id="2830a-112">Gulp</span></span>

<span data-ttu-id="2830a-113">Gulp es un toolkit de compilación streaming basadas en JavaScript para el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="2830a-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="2830a-114">Normalmente se utiliza para secuenciar los archivos de cliente a través de una serie de procesos cuando se desencadena un evento específico en un entorno de compilación.</span><span class="sxs-lookup"><span data-stu-id="2830a-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="2830a-115">Por ejemplo, Gulp puede utilizarse para automatizar [agrupar y minificar](bundling-and-minification.md) o la limpieza de un entorno de desarrollo antes de una nueva compilación.</span><span class="sxs-lookup"><span data-stu-id="2830a-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="2830a-116">Se define un conjunto de tareas de Gulp en *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="2830a-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="2830a-117">El siguiente código de JavaScript incluye módulos de Gulp y especifica las rutas de acceso de archivo que se haga referencia dentro de las tareas disponibles próximamente:</span><span class="sxs-lookup"><span data-stu-id="2830a-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="2830a-118">El código anterior especifica qué módulos de nodo se necesitan.</span><span class="sxs-lookup"><span data-stu-id="2830a-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="2830a-119">El `require` función importa cada módulo para que las tareas dependientes pueden utilizar sus características.</span><span class="sxs-lookup"><span data-stu-id="2830a-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="2830a-120">Cada uno de los módulos importados se asigna a una variable.</span><span class="sxs-lookup"><span data-stu-id="2830a-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="2830a-121">Los módulos pueden encontrarse por nombre o ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="2830a-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="2830a-122">En este ejemplo, los módulos denominan `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, y `gulp-uglify` se recuperan por su nombre.</span><span class="sxs-lookup"><span data-stu-id="2830a-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="2830a-123">Además, se crean una serie de rutas de acceso de modo que las ubicaciones de los archivos CSS y JavaScript se pueden volver a usar y hace referencia en las tareas.</span><span class="sxs-lookup"><span data-stu-id="2830a-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="2830a-124">En la tabla siguiente se proporciona descripciones de los módulos incluidos en *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="2830a-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

|<span data-ttu-id="2830a-125">Nombre del módulo</span><span class="sxs-lookup"><span data-stu-id="2830a-125">Module Name</span></span>|<span data-ttu-id="2830a-126">Descripción</span><span class="sxs-lookup"><span data-stu-id="2830a-126">Description</span></span>|
|---|---|
|<span data-ttu-id="2830a-127">gulp</span><span class="sxs-lookup"><span data-stu-id="2830a-127">gulp</span></span>|<span data-ttu-id="2830a-128">El sistema de compilación streaming Gulp.</span><span class="sxs-lookup"><span data-stu-id="2830a-128">The Gulp streaming build system.</span></span> <span data-ttu-id="2830a-129">Para obtener más información, consulte [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="2830a-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span>|
|<span data-ttu-id="2830a-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="2830a-130">rimraf</span></span>|<span data-ttu-id="2830a-131">Un módulo de eliminación de nodo.</span><span class="sxs-lookup"><span data-stu-id="2830a-131">A Node deletion module.</span></span> <span data-ttu-id="2830a-132">Para obtener más información, consulte [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="2830a-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span>|
|<span data-ttu-id="2830a-133">gulp concat</span><span class="sxs-lookup"><span data-stu-id="2830a-133">gulp-concat</span></span>|<span data-ttu-id="2830a-134">Un módulo que concatena los archivos según su carácter de nueva línea del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="2830a-134">A module that concatenates files based on the operating system’s newline character.</span></span> <span data-ttu-id="2830a-135">Para obtener más información, consulte [gulp concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="2830a-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span>|
|<span data-ttu-id="2830a-136">gulp cssmin</span><span class="sxs-lookup"><span data-stu-id="2830a-136">gulp-cssmin</span></span>|<span data-ttu-id="2830a-137">Un módulo que minifica objeto archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="2830a-137">A module that minifies CSS files.</span></span> <span data-ttu-id="2830a-138">Para obtener más información, consulte [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="2830a-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span>|
|<span data-ttu-id="2830a-139">uglify gulp</span><span class="sxs-lookup"><span data-stu-id="2830a-139">gulp-uglify</span></span>|<span data-ttu-id="2830a-140">Un módulo que minifica objeto *.js* archivos.</span><span class="sxs-lookup"><span data-stu-id="2830a-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="2830a-141">Para obtener más información, consulte [uglify gulp](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="2830a-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span>|

<span data-ttu-id="2830a-142">Una vez que se importan los módulos necesarios, se pueden especificar las tareas.</span><span class="sxs-lookup"><span data-stu-id="2830a-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="2830a-143">Hay seis tareas registrado, representado por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2830a-143">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="2830a-144">En la tabla siguiente proporciona una explicación de las tareas especificadas en el código anterior:</span><span class="sxs-lookup"><span data-stu-id="2830a-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="2830a-145">Nombre de tarea</span><span class="sxs-lookup"><span data-stu-id="2830a-145">Task Name</span></span>|<span data-ttu-id="2830a-146">Descripción</span><span class="sxs-lookup"><span data-stu-id="2830a-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="2830a-147">limpiar: js</span><span class="sxs-lookup"><span data-stu-id="2830a-147">clean:js</span></span>|<span data-ttu-id="2830a-148">Una tarea que usa el módulo de eliminación de nodo rimraf para quitar la versión reducida del archivo site.js.</span><span class="sxs-lookup"><span data-stu-id="2830a-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="2830a-149">limpiar: css</span><span class="sxs-lookup"><span data-stu-id="2830a-149">clean:css</span></span>|<span data-ttu-id="2830a-150">Una tarea que usa el módulo de eliminación de nodo rimraf para quitar la versión reducida del archivo site.css.</span><span class="sxs-lookup"><span data-stu-id="2830a-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="2830a-151">Limpiar</span><span class="sxs-lookup"><span data-stu-id="2830a-151">clean</span></span>|<span data-ttu-id="2830a-152">Una tarea que requiera el `clean:js` tarea, seguido por la `clean:css` tarea.</span><span class="sxs-lookup"><span data-stu-id="2830a-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="2830a-153">min:js</span><span class="sxs-lookup"><span data-stu-id="2830a-153">min:js</span></span>|<span data-ttu-id="2830a-154">Una tarea que minifica objeto y los concatena todos los archivos .js dentro de la carpeta para js.</span><span class="sxs-lookup"><span data-stu-id="2830a-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="2830a-155">El. se excluyen min.js archivos.</span><span class="sxs-lookup"><span data-stu-id="2830a-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="2830a-156">min:CSS</span><span class="sxs-lookup"><span data-stu-id="2830a-156">min:css</span></span>|<span data-ttu-id="2830a-157">Una tarea que minifica objeto y los concatena todos los archivos .css dentro de la carpeta de css.</span><span class="sxs-lookup"><span data-stu-id="2830a-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="2830a-158">El. se excluyen min.css archivos.</span><span class="sxs-lookup"><span data-stu-id="2830a-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="2830a-159">min</span><span class="sxs-lookup"><span data-stu-id="2830a-159">min</span></span>|<span data-ttu-id="2830a-160">Una tarea que requiera el `min:js` tarea, seguido por la `min:css` tarea.</span><span class="sxs-lookup"><span data-stu-id="2830a-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="2830a-161">Ejecución de tareas de manera predeterminada</span><span class="sxs-lookup"><span data-stu-id="2830a-161">Running default tasks</span></span>

<span data-ttu-id="2830a-162">Si aún no ha creado una nueva aplicación Web, cree un nuevo proyecto de aplicación Web ASP.NET en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2830a-162">If you haven’t already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="2830a-163">Agregue un nuevo archivo de JavaScript a su proyecto y asígnele el nombre *gulpfile.js*, a continuación, copie el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="2830a-163">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

2.  <span data-ttu-id="2830a-164">Abra la *package.json* archivo (agregar si no existe) y agregue lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="2830a-164">Open the *package.json* file (add if not there) and add the following.</span></span>

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

3.  <span data-ttu-id="2830a-165">En **el Explorador de soluciones**, haga clic en *gulpfile.js*y seleccione **explorador del ejecutor de tareas**.</span><span class="sxs-lookup"><span data-stu-id="2830a-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![Abrir el explorador del ejecutor de tareas desde el Explorador de soluciones](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="2830a-167">**Explorador del ejecutor de tareas** muestra la lista de tareas de Gulp.</span><span class="sxs-lookup"><span data-stu-id="2830a-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="2830a-168">(Tendrá que hacer clic en el **actualizar** botón que aparece a la izquierda del nombre del proyecto.)</span><span class="sxs-lookup"><span data-stu-id="2830a-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![Explorador del ejecutor de tareas](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  <span data-ttu-id="2830a-170">Debajo de la directiva **tareas** en **explorador del ejecutor de tareas**, haga clic en **limpia**y seleccione **ejecutar** en el menú emergente.</span><span class="sxs-lookup"><span data-stu-id="2830a-170">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Tarea de limpieza de explorador del ejecutor de tareas](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="2830a-172">**Explorador del ejecutor de tareas** creará una nueva ficha denominada **limpia** y ejecute la tarea clean tal y como se define en *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="2830a-172">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="2830a-173">Haga clic en el **limpia** de tareas, a continuación, seleccione **enlaces** > **antes de compilar**.</span><span class="sxs-lookup"><span data-stu-id="2830a-173">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Enlace BeforeBuild explorador del ejecutor de tareas](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="2830a-175">El **antes de compilar** enlace configura la tarea clean se ejecute automáticamente antes de cada compilación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="2830a-175">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="2830a-176">Los enlaces configura con **explorador del ejecutor de tareas** se almacenan en forma de un comentario en la parte superior de su *gulpfile.js* y son eficaces solo en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2830a-176">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="2830a-177">Es una alternativa que no requiere Visual Studio configurar la ejecución automática de tareas gulp en su *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="2830a-177">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="2830a-178">Por ejemplo, analizando estos datos con su *.csproj* archivo:</span><span class="sxs-lookup"><span data-stu-id="2830a-178">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="2830a-179">Ahora la tarea clean se ejecuta cuando se ejecuta el proyecto en Visual Studio o desde un símbolo del sistema mediante la `dotnet run` comando (ejecutar `npm install` primera).</span><span class="sxs-lookup"><span data-stu-id="2830a-179">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the `dotnet run` command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="2830a-180">Definir y ejecutar una nueva tarea</span><span class="sxs-lookup"><span data-stu-id="2830a-180">Defining and running a new task</span></span>

<span data-ttu-id="2830a-181">Para definir una nueva tarea de Gulp, modificar *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="2830a-181">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="2830a-182">Agregue el siguiente código de JavaScript al final de *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="2830a-182">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    <span data-ttu-id="2830a-183">Esta tarea se denomina `first`, y simplemente muestra una cadena.</span><span class="sxs-lookup"><span data-stu-id="2830a-183">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="2830a-184">Guardar *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="2830a-184">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="2830a-185">En **el Explorador de soluciones**, haga clic en *gulpfile.js*y seleccione *explorador del ejecutor de tareas*.</span><span class="sxs-lookup"><span data-stu-id="2830a-185">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="2830a-186">En **explorador del ejecutor de tareas**, haga clic en **primer**y seleccione **ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="2830a-186">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Explorador del ejecutor de tareas ejecute la primera tarea](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="2830a-188">Verá que se muestra el texto de salida.</span><span class="sxs-lookup"><span data-stu-id="2830a-188">You’ll see that the output text is displayed.</span></span> <span data-ttu-id="2830a-189">Si está interesado en ejemplos basados en un escenario común, consulte Gulp recetas.</span><span class="sxs-lookup"><span data-stu-id="2830a-189">If you are interested in examples based on a common scenario, see Gulp Recipes.</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="2830a-190">Definir y ejecutar tareas en una serie</span><span class="sxs-lookup"><span data-stu-id="2830a-190">Defining and running tasks in a series</span></span>

<span data-ttu-id="2830a-191">Al ejecutar varias tareas, las tareas se ejecutan simultáneamente de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="2830a-191">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="2830a-192">Sin embargo, si tiene que ejecutar tareas en un orden específico, debe especificar cada tarea una vez haya finalizado, así como las tareas que dependen de la finalización de otra tarea.</span><span class="sxs-lookup"><span data-stu-id="2830a-192">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="2830a-193">Para definir una serie de tareas que se ejecutan en orden, reemplace la `first` tareas que agregó anteriormente en *gulpfile.js* con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="2830a-193">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    <span data-ttu-id="2830a-194">Ahora que tiene tres tareas: `series:first`, `series:second`, y `series`.</span><span class="sxs-lookup"><span data-stu-id="2830a-194">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="2830a-195">El `series:second` tarea incluye un segundo parámetro que especifica una matriz de tareas que se ejecutarán y se complete antes de la `series:second` se ejecutará la tarea.</span><span class="sxs-lookup"><span data-stu-id="2830a-195">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="2830a-196">Como se especifica en el código anterior, solo la `series:first` tarea debe completarse antes de la `series:second` se ejecutará la tarea.</span><span class="sxs-lookup"><span data-stu-id="2830a-196">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="2830a-197">Guardar *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="2830a-197">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="2830a-198">En **el Explorador de soluciones**, haga clic en *gulpfile.js* y seleccione **explorador del ejecutor de tareas** si no está abierta.</span><span class="sxs-lookup"><span data-stu-id="2830a-198">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4.  <span data-ttu-id="2830a-199">En **explorador del ejecutor de tareas**, haga clic en **serie** y seleccione **ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="2830a-199">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Explorador del ejecutor de tareas Ejecutar tarea de serie](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="2830a-201">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="2830a-201">IntelliSense</span></span>

<span data-ttu-id="2830a-202">IntelliSense proporciona la finalización de código, descripciones de parámetros y otras características para aumentar la productividad y reducir los errores.</span><span class="sxs-lookup"><span data-stu-id="2830a-202">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="2830a-203">Tareas de gulp se escriben en JavaScript; por lo tanto, IntelliSense puede proporcionar asistencia durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="2830a-203">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="2830a-204">Cuando se trabaja con JavaScript, IntelliSense enumera los objetos, funciones, propiedades y parámetros que están disponibles según el contexto actual.</span><span class="sxs-lookup"><span data-stu-id="2830a-204">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="2830a-205">Seleccione una opción de codificación de la lista desplegable proporcionada IntelliSense para completar el código.</span><span class="sxs-lookup"><span data-stu-id="2830a-205">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="2830a-207">Para obtener más información acerca de IntelliSense, vea [IntelliSense para JavaScript](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="2830a-207">For more information about IntelliSense, see [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="2830a-208">Entornos de desarrollo, ensayo y producción</span><span class="sxs-lookup"><span data-stu-id="2830a-208">Development, staging, and production environments</span></span>

<span data-ttu-id="2830a-209">Cuando Gulp se utiliza para optimizar los archivos de cliente de ensayo y producción, se guardan los archivos procesados en una ubicación local de ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="2830a-209">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="2830a-210">El *_Layout.cshtml* archivo usa el **entorno** etiqueta auxiliar para proporcionar dos versiones diferentes de archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="2830a-210">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="2830a-211">Es una versión de los archivos CSS para el desarrollo y la otra versión está optimizada para ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="2830a-211">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="2830a-212">En Visual Studio 2017 al cambiar la **ASPNETCORE_ENVIRONMENT** variable de entorno `Production`, Visual Studio compilará la aplicación Web y un vínculo a los archivos CSS minimizados.</span><span class="sxs-lookup"><span data-stu-id="2830a-212">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="2830a-213">El marcado siguiente se muestra la **entorno** etiqueta aplicaciones auxiliares que contiene las etiquetas de vínculo a la `Development` CSS archivos y la reducida `Staging, Production` archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="2830a-213">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="2830a-214">Cambiar entre entornos</span><span class="sxs-lookup"><span data-stu-id="2830a-214">Switching between environments</span></span>

<span data-ttu-id="2830a-215">Para cambiar entre la compilación para los entornos diferentes, modifique la **ASPNETCORE_ENVIRONMENT** valor de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="2830a-215">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="2830a-216">En **explorador del ejecutor de tareas**, compruebe que la **min** tarea se ha establecido para ejecutarse **antes de compilar**.</span><span class="sxs-lookup"><span data-stu-id="2830a-216">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="2830a-217">En **el Explorador de soluciones**, haga clic en el nombre del proyecto y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="2830a-217">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="2830a-218">Se muestra la hoja de propiedades de la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="2830a-218">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="2830a-219">Haga clic en la pestaña **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="2830a-219">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="2830a-220">Establezca el valor de la **: entorno de hospedaje** variable de entorno `Production`.</span><span class="sxs-lookup"><span data-stu-id="2830a-220">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="2830a-221">Presione **F5** para ejecutar la aplicación en un explorador.</span><span class="sxs-lookup"><span data-stu-id="2830a-221">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="2830a-222">En la ventana del explorador, haga clic en la página y seleccione **ver código fuente** para ver el código HTML de la página.</span><span class="sxs-lookup"><span data-stu-id="2830a-222">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="2830a-223">Tenga en cuenta que los vínculos de hoja de estilos apuntan a los archivos CSS reducidos.</span><span class="sxs-lookup"><span data-stu-id="2830a-223">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="2830a-224">Cierre el explorador para detener la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="2830a-224">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="2830a-225">En Visual Studio, vuelva a la hoja de propiedades de la aplicación Web y cambiar la **: entorno de hospedaje** hacia la variable de entorno `Development`.</span><span class="sxs-lookup"><span data-stu-id="2830a-225">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="2830a-226">Presione **F5** volver a ejecutar la aplicación en un explorador.</span><span class="sxs-lookup"><span data-stu-id="2830a-226">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="2830a-227">En la ventana del explorador, haga clic en la página y seleccione **ver código fuente** para ver el código HTML de la página.</span><span class="sxs-lookup"><span data-stu-id="2830a-227">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="2830a-228">Tenga en cuenta que los vínculos de hoja de estilos apuntan a las versiones de los archivos CSS unminified.</span><span class="sxs-lookup"><span data-stu-id="2830a-228">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="2830a-229">Para obtener más información relacionada con los entornos en ASP.NET Core, vea [trabajar con varios entornos](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="2830a-229">For more information related to environments in ASP.NET Core, see [Working with Multiple Environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="2830a-230">Detalles de la tarea y módulo</span><span class="sxs-lookup"><span data-stu-id="2830a-230">Task and module details</span></span>

<span data-ttu-id="2830a-231">Una tarea de Gulp está registrada con un nombre de función.</span><span class="sxs-lookup"><span data-stu-id="2830a-231">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="2830a-232">Puede especificar las dependencias si deben ejecutar otras tareas antes de la tarea actual.</span><span class="sxs-lookup"><span data-stu-id="2830a-232">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="2830a-233">Funciones adicionales que permiten ejecutar y ver las tareas de Gulp, así como para establecer el origen (*src*) y de destino (*dest*) de los archivos que se está modificaciones.</span><span class="sxs-lookup"><span data-stu-id="2830a-233">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="2830a-234">Éstas son las funciones de API Gulp principales:</span><span class="sxs-lookup"><span data-stu-id="2830a-234">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="2830a-235">Gulp (función)</span><span class="sxs-lookup"><span data-stu-id="2830a-235">Gulp Function</span></span>|<span data-ttu-id="2830a-236">Sintaxis</span><span class="sxs-lookup"><span data-stu-id="2830a-236">Syntax</span></span>|<span data-ttu-id="2830a-237">Descripción</span><span class="sxs-lookup"><span data-stu-id="2830a-237">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="2830a-238">tarea</span><span class="sxs-lookup"><span data-stu-id="2830a-238">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="2830a-239">El `task` función crea una tarea.</span><span class="sxs-lookup"><span data-stu-id="2830a-239">The `task` function creates a task.</span></span> <span data-ttu-id="2830a-240">El `name` parámetro define el nombre de la tarea.</span><span class="sxs-lookup"><span data-stu-id="2830a-240">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="2830a-241">El `deps` parámetro contiene una matriz de tareas se complete antes de que se ejecuta esta tarea.</span><span class="sxs-lookup"><span data-stu-id="2830a-241">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="2830a-242">El `fn` parámetro representa una función de devolución de llamada que realiza las operaciones de la tarea.</span><span class="sxs-lookup"><span data-stu-id="2830a-242">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="2830a-243">Inspección</span><span class="sxs-lookup"><span data-stu-id="2830a-243">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="2830a-244">El `watch` función supervisa las tareas de archivos y se ejecuta cuando se produce un cambio de archivo.</span><span class="sxs-lookup"><span data-stu-id="2830a-244">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="2830a-245">El `glob` parámetro es un `string` o `array` que determina los archivos que para ver.</span><span class="sxs-lookup"><span data-stu-id="2830a-245">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="2830a-246">El `opts` parámetro proporciona viendo opciones de archivo adicionales.</span><span class="sxs-lookup"><span data-stu-id="2830a-246">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="2830a-247">src</span><span class="sxs-lookup"><span data-stu-id="2830a-247">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="2830a-248">El `src` función proporciona archivos que coinciden con los valores de glob.</span><span class="sxs-lookup"><span data-stu-id="2830a-248">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="2830a-249">El `glob` parámetro es un `string` o `array` que determina los archivos que para leer.</span><span class="sxs-lookup"><span data-stu-id="2830a-249">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="2830a-250">El `options` parámetro proporciona opciones de archivo adicionales.</span><span class="sxs-lookup"><span data-stu-id="2830a-250">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="2830a-251">dest</span><span class="sxs-lookup"><span data-stu-id="2830a-251">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="2830a-252">El `dest` función define una ubicación a la que se pueden escribir archivos.</span><span class="sxs-lookup"><span data-stu-id="2830a-252">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="2830a-253">El `path` parámetro es una cadena o una función que determina la carpeta de destino.</span><span class="sxs-lookup"><span data-stu-id="2830a-253">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="2830a-254">El `options` parámetro es un objeto que especifica las opciones de carpeta de salida.</span><span class="sxs-lookup"><span data-stu-id="2830a-254">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="2830a-255">Para obtener información adicional sobre la referencia de API Gulp, consulte [Gulp API de documentos](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span><span class="sxs-lookup"><span data-stu-id="2830a-255">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="2830a-256">Gulp recetas</span><span class="sxs-lookup"><span data-stu-id="2830a-256">Gulp recipes</span></span>

<span data-ttu-id="2830a-257">La Comunidad Gulp proporciona Gulp [recetas](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="2830a-257">The Gulp community provides Gulp [recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="2830a-258">Estas recetas constan de tareas de Gulp para dirigirse a escenarios comunes.</span><span class="sxs-lookup"><span data-stu-id="2830a-258">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2830a-259">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2830a-259">Additional resources</span></span>

* [<span data-ttu-id="2830a-260">Documentación de gulp</span><span class="sxs-lookup"><span data-stu-id="2830a-260">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="2830a-261">Agrupar y minificar en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2830a-261">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="2830a-262">Usar Grunt en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2830a-262">Using Grunt in ASP.NET Core</span></span>](using-grunt.md)
