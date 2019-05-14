---
title: Uso de Grunt en ASP.NET Core
author: rick-anderson
description: Uso de Grunt en ASP.NET Core
ms.author: riande
ms.date: 05/10/2019
uid: client-side/using-grunt
ms.openlocfilehash: 718a1358c0474711b05bb2c90dc86ec9edacbf1e
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610221"
---
# <a name="use-grunt-in-aspnet-core"></a><span data-ttu-id="36313-103">Uso de Grunt en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="36313-103">Use Grunt in ASP.NET Core</span></span>

<span data-ttu-id="36313-104">Por [arroz Noel](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span><span class="sxs-lookup"><span data-stu-id="36313-104">By [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span></span>

<span data-ttu-id="36313-105">Grunt es un ejecutor de tareas de JavaScript que automatiza la minificación de secuencia de comandos, compilación de TypeScript, las herramientas "lint" de calidad de código, preprocesadores CSS y casi cualquier tarea repetitiva que necesita hacer para admitir el desarrollo cliente.</span><span class="sxs-lookup"><span data-stu-id="36313-105">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="36313-106">Grunt es totalmente compatible en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36313-106">Grunt is fully supported in Visual Studio.</span></span>

<span data-ttu-id="36313-107">Este ejemplo usa un proyecto vacío de ASP.NET Core como punto de partida, para mostrar cómo automatizar el proceso de compilación de cliente desde el principio.</span><span class="sxs-lookup"><span data-stu-id="36313-107">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="36313-108">El ejemplo finalizado limpia el directorio de implementación de destino, combina los archivos de JavaScript, comprueba la calidad del código, condensa el contenido del archivo de JavaScript y se implementa en la raíz de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="36313-108">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="36313-109">Usamos los siguientes paquetes:</span><span class="sxs-lookup"><span data-stu-id="36313-109">We will use the following packages:</span></span>

* <span data-ttu-id="36313-110">**grunt**: El paquete de ejecutor de tareas de Grunt.</span><span class="sxs-lookup"><span data-stu-id="36313-110">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="36313-111">**grunt-contrib-clean**: Un complemento que quita los archivos o directorios.</span><span class="sxs-lookup"><span data-stu-id="36313-111">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="36313-112">**grunt-contrib-jshint**: Un complemento que revisa la calidad del código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="36313-112">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="36313-113">**grunt-contrib-concat**: Un complemento que combina los archivos en un único archivo.</span><span class="sxs-lookup"><span data-stu-id="36313-113">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="36313-114">**grunt-contrib-uglify**: Un complemento que minifica el objeto de JavaScript para reducir el tamaño.</span><span class="sxs-lookup"><span data-stu-id="36313-114">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="36313-115">**grunt-contrib-watch**: Un complemento que supervisa la actividad de archivos.</span><span class="sxs-lookup"><span data-stu-id="36313-115">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="36313-116">Preparación de la aplicación</span><span class="sxs-lookup"><span data-stu-id="36313-116">Preparing the application</span></span>

<span data-ttu-id="36313-117">Para empezar, configure una nueva aplicación web vacía y agregar los archivos de ejemplo de TypeScript.</span><span class="sxs-lookup"><span data-stu-id="36313-117">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="36313-118">Archivos de TypeScript se compilan automáticamente en JavaScript con la configuración de Visual Studio de forma predeterminada y serán la prima para procesar el uso de Grunt.</span><span class="sxs-lookup"><span data-stu-id="36313-118">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1. <span data-ttu-id="36313-119">En Visual Studio, cree un nuevo `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="36313-119">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2. <span data-ttu-id="36313-120">En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione ASP.NET Core **vacía** plantilla y haga clic en el botón Aceptar.</span><span class="sxs-lookup"><span data-stu-id="36313-120">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3. <span data-ttu-id="36313-121">En el Explorador de soluciones, revise la estructura del proyecto.</span><span class="sxs-lookup"><span data-stu-id="36313-121">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="36313-122">El `\src` carpeta incluye vacía `wwwroot` y `Dependencies` nodos.</span><span class="sxs-lookup"><span data-stu-id="36313-122">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![solución de web vacía](using-grunt/_static/grunt-solution-explorer.png)

4. <span data-ttu-id="36313-124">Agregue una nueva carpeta denominada `TypeScript` al directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="36313-124">Add a new folder named `TypeScript` to your project directory.</span></span>

5. <span data-ttu-id="36313-125">Antes de agregar los archivos, asegúrese de que Visual Studio tiene la opción ' compilar al guardar ' para TypeScript se protegió ningún archivo.</span><span class="sxs-lookup"><span data-stu-id="36313-125">Before adding any files, make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="36313-126">Vaya a **herramientas** > **opciones** > **Editor de texto** > **Typescript**  >  **Proyecto**:</span><span class="sxs-lookup"><span data-stu-id="36313-126">Navigate to **Tools** > **Options** > **Text Editor** > **Typescript** > **Project**:</span></span>

    ![Opciones de configuración de compilación automática de archivos de TypeScript](using-grunt/_static/typescript-options.png)

6. <span data-ttu-id="36313-128">Haga clic en el `TypeScript` directorio y seleccione **Agregar > nuevo elemento** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="36313-128">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="36313-129">Seleccione el **archivo JavaScript** de elemento y asigne el nombre *Tastes.ts* (tenga en cuenta el \*extensión TS).</span><span class="sxs-lookup"><span data-stu-id="36313-129">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="36313-130">Copie la línea de código TypeScript a continuación en el archivo (cuando se guarda, un nuevo *Tastes.js* archivo aparecerá con el origen de JavaScript).</span><span class="sxs-lookup"><span data-stu-id="36313-130">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. <span data-ttu-id="36313-131">Agregar un segundo archivo en el **TypeScript** directorio y asígnele el nombre `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="36313-131">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="36313-132">Copie el código siguiente en el archivo.</span><span class="sxs-lookup"><span data-stu-id="36313-132">Copy the code below into the file.</span></span>

    ```typescript
    class Food {
      constructor(name: string, calories: number) {
        this._name = name;
        this._calories = calories;
      }

      private _name: string;
      get Name() {
        return this._name;
      }

      private _calories: number;
      get Calories() {
        return this._calories;
      }

      private _taste: Tastes;
      get Taste(): Tastes { return this._taste }
      set Taste(value: Tastes) {
        this._taste = value;
      }
    }
    ```

## <a name="configuring-npm"></a><span data-ttu-id="36313-133">Configuración de NPM</span><span class="sxs-lookup"><span data-stu-id="36313-133">Configuring NPM</span></span>

<span data-ttu-id="36313-134">A continuación, configure Network Performance Monitor para descargar grunt y tareas de grunt.</span><span class="sxs-lookup"><span data-stu-id="36313-134">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="36313-135">En el Explorador de soluciones, haga clic en el proyecto y seleccione **Agregar > nuevo elemento** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="36313-135">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="36313-136">Seleccione el **archivo de configuración de NPM** de elemento, deje el nombre predeterminado, *package.json*y haga clic en el **agregar** botón.</span><span class="sxs-lookup"><span data-stu-id="36313-136">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="36313-137">En el *package.json* de archivos, en el `devDependencies` objetos entre llaves, escriba "grunt".</span><span class="sxs-lookup"><span data-stu-id="36313-137">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="36313-138">Seleccione `grunt` en Intellisense lista y presione la tecla ENTRAR.</span><span class="sxs-lookup"><span data-stu-id="36313-138">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="36313-139">Visual Studio entrecomillar el nombre del paquete de grunt y agregue dos puntos.</span><span class="sxs-lookup"><span data-stu-id="36313-139">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="36313-140">A la derecha de los dos puntos, seleccione la última versión estable del paquete en la parte superior de la lista de Intellisense (presione `Ctrl-Space` si Intellisense no aparece).</span><span class="sxs-lookup"><span data-stu-id="36313-140">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense doesn't appear).</span></span>

    ![grunt Intellisense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > <span data-ttu-id="36313-142">NPM usa [versionamiento semántico](http://semver.org/) para organizar las dependencias.</span><span class="sxs-lookup"><span data-stu-id="36313-142">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="36313-143">Control de versiones semántico, también conocido como SemVer, identifica los paquetes con el esquema de numeración \<principal >.\< secundaria >. \<revisión >.</span><span class="sxs-lookup"><span data-stu-id="36313-143">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="36313-144">IntelliSense simplifica el control de versiones semántico mostrando solo algunas opciones comunes.</span><span class="sxs-lookup"><span data-stu-id="36313-144">Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="36313-145">El elemento superior de la lista de Intellisense (0.4.5 en el ejemplo anterior) se considera la última versión estable del paquete.</span><span class="sxs-lookup"><span data-stu-id="36313-145">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="36313-146">El símbolo de intercalación (^) coincide con la versión principal más reciente y la tilde (~) con la versión secundaria más reciente.</span><span class="sxs-lookup"><span data-stu-id="36313-146">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="36313-147">Consulte la [referencia del analizador de versión NPM semver](https://www.npmjs.com/package/semver) como guía para la expresividad completa que proporciona SemVer.</span><span class="sxs-lookup"><span data-stu-id="36313-147">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="36313-148">Agregar más dependencias para cargar grunt-contrib -\* empaqueta para su *limpia*, *jshint*, *concat*, *a que uglify*y *inspección* tal como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="36313-148">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="36313-149">Las versiones no es necesario para que coincida con el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="36313-149">The versions don't need to match the example.</span></span>

    ```json
    "devDependencies": {
      "grunt": "0.4.5",
      "grunt-contrib-clean": "0.6.0",
      "grunt-contrib-jshint": "0.11.0",
      "grunt-contrib-concat": "0.5.1",
      "grunt-contrib-uglify": "0.8.0",
      "grunt-contrib-watch": "0.6.1"
    }
    ```

4. <span data-ttu-id="36313-150">Guardar el *package.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="36313-150">Save the *package.json* file.</span></span>

<span data-ttu-id="36313-151">Se descargarán los paquetes para cada elemento devDependencies, junto con los archivos que requiere cada paquete.</span><span class="sxs-lookup"><span data-stu-id="36313-151">The packages for each devDependencies item will download, along with any files that each package requires.</span></span> <span data-ttu-id="36313-152">Puede encontrar los archivos del paquete en el `node_modules` directorio habilitando la **mostrar todos los archivos** botón en el Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="36313-152">You can find the package files in the `node_modules` directory by enabling the **Show All Files** button in the Solution Explorer.</span></span>

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="36313-154">Si necesita, puede restaurar manualmente las dependencias en el Explorador de soluciones con el botón secundario en `Dependencies\NPM` y seleccionando el **restaurar paquetes** opción de menú.</span><span class="sxs-lookup"><span data-stu-id="36313-154">If you need to, you can manually restore dependencies in Solution Explorer by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![restaurar paquetes](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="36313-156">Configuración de Grunt</span><span class="sxs-lookup"><span data-stu-id="36313-156">Configuring Grunt</span></span>

<span data-ttu-id="36313-157">Grunt se configura mediante un manifiesto llamado *Gruntfile.js* que define, carga y registra las tareas que se pueden ejecutar manualmente o configuradas para ejecutarse automáticamente basándose en eventos en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36313-157">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1. <span data-ttu-id="36313-158">Haga clic en el proyecto y seleccione **Agregar > nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="36313-158">Right-click the project and select **Add > New Item**.</span></span> <span data-ttu-id="36313-159">Seleccione el **archivo de configuración de Grunt** opción, deje el nombre predeterminado, *Gruntfile.js*y haga clic en el **agregar** botón.</span><span class="sxs-lookup"><span data-stu-id="36313-159">Select the **Grunt Configuration file** option, leave the default name, *Gruntfile.js*, and click the **Add** button.</span></span>

   <span data-ttu-id="36313-160">El código inicial incluye una definición de módulo y el `grunt.initConfig()` método.</span><span class="sxs-lookup"><span data-stu-id="36313-160">The initial code includes a module definition and the `grunt.initConfig()` method.</span></span> <span data-ttu-id="36313-161">El `initConfig()` se usa para establecer las opciones para cada paquete, y el resto del módulo se carga y registrar las tareas.</span><span class="sxs-lookup"><span data-stu-id="36313-161">The `initConfig()` is used to set options for each package, and the remainder of the module will load and register tasks.</span></span>

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

2. <span data-ttu-id="36313-162">Dentro de la `initConfig()` método, agregar opciones para la `clean` tareas tal como se muestra en el ejemplo *Gruntfile.js* a continuación.</span><span class="sxs-lookup"><span data-stu-id="36313-162">Inside the `initConfig()` method, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="36313-163">La tarea clean acepta una matriz de cadenas de directorio.</span><span class="sxs-lookup"><span data-stu-id="36313-163">The clean task accepts an array of directory strings.</span></span> <span data-ttu-id="36313-164">Esta tarea quita archivos wwwroot/lib y quita el directorio temp/todo.</span><span class="sxs-lookup"><span data-stu-id="36313-164">This task removes files from wwwroot/lib and removes the entire /temp directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. <span data-ttu-id="36313-165">Debajo del método initConfig(), agregue una llamada a `grunt.loadNpmTasks()`.</span><span class="sxs-lookup"><span data-stu-id="36313-165">Below the initConfig() method, add a call to `grunt.loadNpmTasks()`.</span></span> <span data-ttu-id="36313-166">Esto hará que la tarea se puede ejecutar desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36313-166">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. <span data-ttu-id="36313-167">Guardar *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="36313-167">Save *Gruntfile.js*.</span></span> <span data-ttu-id="36313-168">El archivo debe ser similar a la captura de pantalla siguiente.</span><span class="sxs-lookup"><span data-stu-id="36313-168">The file should look something like the screenshot below.</span></span>

    ![gruntfile inicial](using-grunt/_static/gruntfile-js-initial.png)

5. <span data-ttu-id="36313-170">Haga clic en *Gruntfile.js* y seleccione **Task Runner Explorer** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="36313-170">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="36313-171">Se abrirá la ventana de explorador del ejecutador de tareas.</span><span class="sxs-lookup"><span data-stu-id="36313-171">The Task Runner Explorer window will open.</span></span>

    ![menú del explorador de ejecutor de tareas](using-grunt/_static/task-runner-explorer-menu.png)

6. <span data-ttu-id="36313-173">Compruebe que `clean` se muestra bajo **tareas** en el Explorador de ejecutores de tareas.</span><span class="sxs-lookup"><span data-stu-id="36313-173">Verify that `clean` shows under **Tasks** in the Task Runner Explorer.</span></span>

    ![lista de tareas del explorador de ejecutor de tarea](using-grunt/_static/task-runner-explorer-tasks.png)

7. <span data-ttu-id="36313-175">Haga clic en la tarea clean y seleccione **ejecutar** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="36313-175">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="36313-176">Una ventana de comandos muestra el progreso de la tarea.</span><span class="sxs-lookup"><span data-stu-id="36313-176">A command window displays progress of the task.</span></span>

    ![tarea de limpieza de ejecutar el Explorador de ejecutor tarea](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > <span data-ttu-id="36313-178">No hay ningún archivos o directorios para limpiar todavía.</span><span class="sxs-lookup"><span data-stu-id="36313-178">There are no files or directories to clean yet.</span></span> <span data-ttu-id="36313-179">Si lo desea, puede crearlas manualmente en el Explorador de soluciones y, a continuación, ejecutar la tarea clean como prueba.</span><span class="sxs-lookup"><span data-stu-id="36313-179">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>

8. <span data-ttu-id="36313-180">En el método initConfig(), agregue una entrada para `concat` con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="36313-180">In the initConfig() method, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="36313-181">El `src` matriz de propiedades incluyen archivos para combinar en el orden en que deben combinarse.</span><span class="sxs-lookup"><span data-stu-id="36313-181">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="36313-182">El `dest` propiedad asigna la ruta de acceso al archivo combinado que se genera.</span><span class="sxs-lookup"><span data-stu-id="36313-182">The `dest` property assigns the path to the combined file that's produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="36313-183">El `all` propiedad en el código anterior es el nombre de un destino.</span><span class="sxs-lookup"><span data-stu-id="36313-183">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="36313-184">Los destinos se usan en algunas tareas de Grunt para permitir que varios entornos de compilación.</span><span class="sxs-lookup"><span data-stu-id="36313-184">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="36313-185">Puede ver los destinos integrados mediante Intellisense o asignar su propio.</span><span class="sxs-lookup"><span data-stu-id="36313-185">You can view the built-in targets using Intellisense or assign your own.</span></span>

9. <span data-ttu-id="36313-186">Agregue el `jshint` de tareas con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="36313-186">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="36313-187">Se ejecuta la utilidad de la calidad del código jshint en cada archivo de JavaScript que se encuentra en el directorio temporal.</span><span class="sxs-lookup"><span data-stu-id="36313-187">The jshint code-quality utility is run against every JavaScript file found in the temp directory.</span></span>

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="36313-188">La opción "-W069" es un error generado por jshint cuando JavaScript usa la sintaxis para asignar una propiedad en lugar de la notación de puntos, es decir, los corchetes `Tastes["Sweet"]` en lugar de `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="36313-188">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="36313-189">La opción desactiva la advertencia para permitir que el resto del proceso para continuar.</span><span class="sxs-lookup"><span data-stu-id="36313-189">The option turns off the warning to allow the rest of the process to continue.</span></span>

10. <span data-ttu-id="36313-190">Agregue el `uglify` de tareas con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="36313-190">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="36313-191">La tarea minifica objeto el *combined.js* archivo se encuentra en el directorio temporal y crea el archivo de resultados en wwwroot/lib siguiendo la convención de nomenclatura estándar  *\<nombre de archivo\>. min.js*.</span><span class="sxs-lookup"><span data-stu-id="36313-191">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

11. <span data-ttu-id="36313-192">En el grunt.loadNpmTasks() de llamada que se carga grunt-contrib-clean, incluir la misma llamada para jshint, concat y a que uglify con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="36313-192">Under the call grunt.loadNpmTasks() that loads grunt-contrib-clean, include the same call for jshint, concat and uglify using the code below.</span></span>

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. <span data-ttu-id="36313-193">Guardar *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="36313-193">Save *Gruntfile.js*.</span></span> <span data-ttu-id="36313-194">El archivo debe tener un aspecto similar al ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="36313-194">The file should look something like the example below.</span></span>

    ![ejemplo de archivo completa de grunt](using-grunt/_static/gruntfile-js-complete.png)

13. <span data-ttu-id="36313-196">Tenga en cuenta que la lista de tareas del explorador de ejecutor de tareas incluye `clean`, `concat`, `jshint` y `uglify` tareas.</span><span class="sxs-lookup"><span data-stu-id="36313-196">Notice that the Task Runner Explorer Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="36313-197">Cada tarea se ejecutan en orden y observar los resultados en el Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="36313-197">Run each task in order and observe the results in Solution Explorer.</span></span> <span data-ttu-id="36313-198">Cada tarea debe ejecutarse sin errores.</span><span class="sxs-lookup"><span data-stu-id="36313-198">Each task should run without errors.</span></span>

    ![Explorador de ejecutores de tareas ejecutar cada tarea.](using-grunt/_static/task-runner-explorer-run-each-task.png)

    <span data-ttu-id="36313-200">La tarea de concat crea un nuevo *combined.js* de archivo y lo coloca en el directorio temporal.</span><span class="sxs-lookup"><span data-stu-id="36313-200">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="36313-201">La tarea jshint simplemente se ejecuta y no genera resultados.</span><span class="sxs-lookup"><span data-stu-id="36313-201">The jshint task simply runs and doesn't produce output.</span></span> <span data-ttu-id="36313-202">La tarea uglify crea un nuevo *combined.min.js* de archivo y lo coloca en wwwroot/lib.</span><span class="sxs-lookup"><span data-stu-id="36313-202">The uglify task creates a new *combined.min.js* file and places it into wwwroot/lib.</span></span> <span data-ttu-id="36313-203">Una vez completada, la solución debe ser similar a la captura de pantalla siguiente:</span><span class="sxs-lookup"><span data-stu-id="36313-203">On completion, the solution should look something like the screenshot below:</span></span>

    ![el Explorador de soluciones de tareas después de todo](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > <span data-ttu-id="36313-205">Para obtener más información acerca de las opciones para cada paquete, visite [ https://www.npmjs.com/ ](https://www.npmjs.com/) y el nombre del paquete en el cuadro de búsqueda en la página principal de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="36313-205">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="36313-206">Por ejemplo, puede buscar el paquete grunt-contrib-clean para obtener un vínculo a documentación que explica todos sus parámetros.</span><span class="sxs-lookup"><span data-stu-id="36313-206">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="36313-207">Ahora todos juntos</span><span class="sxs-lookup"><span data-stu-id="36313-207">All together now</span></span>

<span data-ttu-id="36313-208">Utilice el Grunt `registerTask()` método para ejecutar una serie de tareas en una secuencia determinada.</span><span class="sxs-lookup"><span data-stu-id="36313-208">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="36313-209">Por ejemplo, para ejecutar el ejemplo de pasos anteriores en el orden limpio -> concat -> jshint -> a que uglify, agregue el siguiente código al módulo.</span><span class="sxs-lookup"><span data-stu-id="36313-209">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="36313-210">El código debe agregarse en el mismo nivel que las llamadas de loadNpmTasks() fuera initConfig.</span><span class="sxs-lookup"><span data-stu-id="36313-210">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="36313-211">La nueva tarea aparece en el Explorador de ejecutores de tareas en tareas de Alias.</span><span class="sxs-lookup"><span data-stu-id="36313-211">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="36313-212">Puede haga clic en y ejecutarla como lo haría en otras tareas.</span><span class="sxs-lookup"><span data-stu-id="36313-212">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="36313-213">El `all` se ejecutará la tarea `clean`, `concat`, `jshint` y `uglify`, en orden.</span><span class="sxs-lookup"><span data-stu-id="36313-213">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![tareas de grunt de alias](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="36313-215">Observación de cambios</span><span class="sxs-lookup"><span data-stu-id="36313-215">Watching for changes</span></span>

<span data-ttu-id="36313-216">Un `watch` tarea cuida en archivos y directorios.</span><span class="sxs-lookup"><span data-stu-id="36313-216">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="36313-217">La inspección desencadena tareas automáticamente si detecta los cambios.</span><span class="sxs-lookup"><span data-stu-id="36313-217">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="36313-218">Agregue el código siguiente para initConfig para ver los cambios a \*archivos .js en el directorio de TypeScript.</span><span class="sxs-lookup"><span data-stu-id="36313-218">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="36313-219">Si se cambia un archivo JavaScript, `watch` se ejecutará la `all` tarea.</span><span class="sxs-lookup"><span data-stu-id="36313-219">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="36313-220">Agregue una llamada a `loadNpmTasks()` para mostrar la `watch` tarea en Task Runner Explorer.</span><span class="sxs-lookup"><span data-stu-id="36313-220">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="36313-221">Haga clic en la tarea de inspección en Task Runner Explorer y seleccione Ejecutar en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="36313-221">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="36313-222">La ventana de comandos que se muestra la ejecución de tareas de inspección mostrará un bucle "esperando..." Mensaje.</span><span class="sxs-lookup"><span data-stu-id="36313-222">The command window that shows the watch task running will display a "Waiting…" message.</span></span> <span data-ttu-id="36313-223">Abra uno de los archivos de TypeScript, agregue un espacio y, a continuación, guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="36313-223">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="36313-224">Esto desencadenará la tarea de inspección y desencadenar las demás tareas que se ejecutan en orden.</span><span class="sxs-lookup"><span data-stu-id="36313-224">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="36313-225">La captura de pantalla siguiente muestra una ejemplo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="36313-225">The screenshot below shows a sample run.</span></span>

![resultado de las tareas de ejecución](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="36313-227">Enlazar a eventos de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="36313-227">Binding to Visual Studio events</span></span>

<span data-ttu-id="36313-228">A menos que desee iniciar manualmente las tareas cada vez que se trabaja en Visual Studio, puede enlazar tareas a **antes de compilar**, **después de compilar**, **Clean**, y  **Proyecto abierto** eventos.</span><span class="sxs-lookup"><span data-stu-id="36313-228">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="36313-229">Vamos a enlazar `watch` para que se ejecute cada vez que abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36313-229">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="36313-230">En el Explorador de ejecutores de tareas, haga clic en la tarea de inspección y seleccione **enlaces > Abrir proyecto** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="36313-230">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![enlazar una tarea a la apertura del proyecto](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="36313-232">Descargar y recargar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="36313-232">Unload and reload the project.</span></span> <span data-ttu-id="36313-233">Cuando se carga el proyecto de nuevo, iniciará la tarea de inspección se ejecuten automáticamente.</span><span class="sxs-lookup"><span data-stu-id="36313-233">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="36313-234">Resumen</span><span class="sxs-lookup"><span data-stu-id="36313-234">Summary</span></span>

<span data-ttu-id="36313-235">Grunt es un ejecutor de tareas eficaz que puede utilizarse para automatizar la mayoría de las tareas de compilación del cliente.</span><span class="sxs-lookup"><span data-stu-id="36313-235">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="36313-236">Grunt aprovecha NPM para entregar sus paquetes, características y herramientas de integración con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36313-236">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="36313-237">Visual Studio Task Runner Explorer detecta los cambios en los archivos de configuración y proporciona una interfaz adecuada para ejecutar tareas, ver las tareas en ejecución y enlazar tareas a eventos de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36313-237">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>
