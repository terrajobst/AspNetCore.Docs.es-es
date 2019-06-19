---
title: Uso de Grunt en ASP.NET Core
author: rick-anderson
description: Uso de Grunt en ASP.NET Core
ms.author: riande
ms.date: 06/18/2019
uid: client-side/using-grunt
ms.openlocfilehash: 851ce3b50e88fee597518aef23276800f4b50f06
ms.sourcegitcommit: a1283d486ac1dcedfc7ea302e1cc882833e2c515
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67207750"
---
# <a name="use-grunt-in-aspnet-core"></a>Uso de Grunt en ASP.NET Core

Grunt es un ejecutor de tareas de JavaScript que automatiza la minificación de secuencia de comandos, compilación de TypeScript, las herramientas "lint" de calidad de código, preprocesadores CSS y casi cualquier tarea repetitiva que necesita hacer para admitir el desarrollo cliente. Grunt es totalmente compatible en Visual Studio.

Este ejemplo usa un proyecto vacío de ASP.NET Core como punto de partida, para mostrar cómo automatizar el proceso de compilación de cliente desde el principio.

El ejemplo finalizado limpia el directorio de implementación de destino, combina los archivos de JavaScript, comprueba la calidad del código, condensa el contenido del archivo de JavaScript y se implementa en la raíz de la aplicación web. Usamos los siguientes paquetes:

* **grunt**: El paquete de ejecutor de tareas de Grunt.

* **grunt-contrib-clean**: Un complemento que quita los archivos o directorios.

* **grunt-contrib-jshint**: Un complemento que revisa la calidad del código JavaScript.

* **grunt-contrib-concat**: Un complemento que combina los archivos en un único archivo.

* **grunt-contrib-uglify**: Un complemento que minifica el objeto de JavaScript para reducir el tamaño.

* **grunt-contrib-watch**: Un complemento que supervisa la actividad de archivos.

## <a name="preparing-the-application"></a>Preparación de la aplicación

Para empezar, configure una nueva aplicación web vacía y agregar los archivos de ejemplo de TypeScript. Archivos de TypeScript se compilan automáticamente en JavaScript con la configuración de Visual Studio de forma predeterminada y serán la prima para procesar el uso de Grunt.

1. En Visual Studio, cree un nuevo `ASP.NET Web Application`.

2. En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione ASP.NET Core **vacía** plantilla y haga clic en el botón Aceptar.

3. En el Explorador de soluciones, revise la estructura del proyecto. El `\src` carpeta incluye vacía `wwwroot` y `Dependencies` nodos.

    ![solución de web vacía](using-grunt/_static/grunt-solution-explorer.png)

4. Agregue una nueva carpeta denominada `TypeScript` al directorio del proyecto.

5. Antes de agregar los archivos, asegúrese de que Visual Studio tiene la opción ' compilar al guardar ' para TypeScript se protegió ningún archivo. Vaya a **herramientas** > **opciones** > **Editor de texto** > **Typescript**  >  **Proyecto**:

    ![Opciones de configuración de compilación automática de archivos de TypeScript](using-grunt/_static/typescript-options.png)

6. Haga clic en el `TypeScript` directorio y seleccione **Agregar > nuevo elemento** en el menú contextual. Seleccione el **archivo JavaScript** de elemento y asigne el nombre *Tastes.ts* (tenga en cuenta el \*extensión TS). Copie la línea de código TypeScript a continuación en el archivo (cuando se guarda, un nuevo *Tastes.js* archivo aparecerá con el origen de JavaScript).

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. Agregar un segundo archivo en el **TypeScript** directorio y asígnele el nombre `Food.ts`. Copie el código siguiente en el archivo.

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

## <a name="configuring-npm"></a>Configuración de NPM

A continuación, configure Network Performance Monitor para descargar grunt y tareas de grunt.

1. En el Explorador de soluciones, haga clic en el proyecto y seleccione **Agregar > nuevo elemento** en el menú contextual. Seleccione el **archivo de configuración de NPM** de elemento, deje el nombre predeterminado, *package.json*y haga clic en el **agregar** botón.

2. En el *package.json* de archivos, en el `devDependencies` objetos entre llaves, escriba "grunt". Seleccione `grunt` en Intellisense lista y presione la tecla ENTRAR. Visual Studio entrecomillar el nombre del paquete de grunt y agregue dos puntos. A la derecha de los dos puntos, seleccione la última versión estable del paquete en la parte superior de la lista de Intellisense (presione `Ctrl-Space` si Intellisense no aparece).

    ![grunt Intellisense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > NPM usa [versionamiento semántico](http://semver.org/) para organizar las dependencias. Control de versiones semántico, también conocido como SemVer, identifica los paquetes con el esquema de numeración \<principal >.\< secundaria >. \<revisión >. IntelliSense simplifica el control de versiones semántico mostrando solo algunas opciones comunes. El elemento superior de la lista de Intellisense (0.4.5 en el ejemplo anterior) se considera la última versión estable del paquete. El símbolo de intercalación (^) coincide con la versión principal más reciente y la tilde (~) con la versión secundaria más reciente. Consulte la [referencia del analizador de versión NPM semver](https://www.npmjs.com/package/semver) como guía para la expresividad completa que proporciona SemVer.

3. Agregar más dependencias para cargar grunt-contrib -\* empaqueta para su *limpia*, *jshint*, *concat*, *a que uglify*y *inspección* tal como se muestra en el ejemplo siguiente. Las versiones no es necesario para que coincida con el ejemplo.

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

4. Guardar el *package.json* archivo.

Los paquetes para cada `devDependencies` elemento descargará, junto con los archivos que requiere cada paquete. Puede encontrar los archivos del paquete en el *node_modules* directorio habilitando la **mostrar todos los archivos** botón **el Explorador de soluciones**.

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> Si necesita, puede restaurar manualmente las dependencias en **el Explorador de soluciones** con el botón secundario en `Dependencies\NPM` y seleccionando el **restaurar paquetes** opción de menú.

![restaurar paquetes](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Configuración de Grunt

Grunt se configura mediante un manifiesto llamado *Gruntfile.js* que define, carga y registra las tareas que se pueden ejecutar manualmente o configuradas para ejecutarse automáticamente basándose en eventos en Visual Studio.

1. Haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**. Seleccione el **archivo JavaScript** plantilla de elemento, cambie el nombre a *Gruntfile.js*y haga clic en el **agregar** botón.

1. Agregue el código siguiente al *Gruntfile.js*. El `initConfig` función establece opciones para cada paquete, y el resto del módulo de carga y registrar las tareas.

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

1. Dentro de la `initConfig` funcione, agregue las opciones para la `clean` tareas tal como se muestra en el ejemplo *Gruntfile.js* a continuación. El `clean` tarea acepta una matriz de cadenas de directorio. Esta tarea elimina los archivos de *wwwroot/lib* y quita toda la */temp* directory.

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

1. A continuación el `initConfig` de función, agregue una llamada a `grunt.loadNpmTasks`. Esto hará que la tarea se puede ejecutar desde Visual Studio.

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

1. Guardar *Gruntfile.js*. El archivo debe ser similar a la captura de pantalla siguiente.

    ![gruntfile inicial](using-grunt/_static/gruntfile-js-initial.png)

1. Haga clic en *Gruntfile.js* y seleccione **Task Runner Explorer** en el menú contextual. El **Task Runner Explorer** se abrirá la ventana.

    ![menú del explorador de ejecutor de tareas](using-grunt/_static/task-runner-explorer-menu.png)

1. Compruebe que `clean` se muestra bajo **tareas** en el **Task Runner Explorer**.

    ![lista de tareas del explorador de ejecutor de tarea](using-grunt/_static/task-runner-explorer-tasks.png)

1. Haga clic en la tarea clean y seleccione **ejecutar** en el menú contextual. Una ventana de comandos muestra el progreso de la tarea.

    ![tarea de limpieza de ejecutar el Explorador de ejecutor tarea](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > No hay ningún archivos o directorios para limpiar todavía. Si lo desea, puede crearlas manualmente en el Explorador de soluciones y, a continuación, ejecutar la tarea clean como prueba.

1. En el `initConfig` de función, agregue una entrada para `concat` con el código siguiente.

    El `src` matriz de propiedades incluyen archivos para combinar en el orden en que deben combinarse. El `dest` propiedad asigna la ruta de acceso al archivo combinado que se genera.

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > El `all` propiedad en el código anterior es el nombre de un destino. Los destinos se usan en algunas tareas de Grunt para permitir que varios entornos de compilación. Puede ver los destinos integrados mediante IntelliSense o asignar su propio.

1. Agregue el `jshint` de tareas con el código siguiente.

    El jshint `code-quality` utilidad se ejecuta en cada archivo de JavaScript que se encuentra en la *temp* directory.

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > La opción "-W069" es un error generado por jshint cuando JavaScript usa la sintaxis para asignar una propiedad en lugar de la notación de puntos, es decir, los corchetes `Tastes["Sweet"]` en lugar de `Tastes.Sweet`. La opción desactiva la advertencia para permitir que el resto del proceso para continuar.

1. Agregue el `uglify` de tareas con el código siguiente.

    La tarea minifica objeto el *combined.js* archivo se encuentra en el directorio temporal y crea el archivo de resultados en wwwroot/lib siguiendo la convención de nomenclatura estándar  *\<nombre de archivo\>. min.js*.

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

1. En la llamada a `grunt.loadNpmTasks` que carga `grunt-contrib-clean`, incluir la misma llamada para jshint, concat y a que uglify con el código siguiente.

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

1. Guardar *Gruntfile.js*. El archivo debe tener un aspecto similar al ejemplo siguiente.

    ![ejemplo de archivo completa de grunt](using-grunt/_static/gruntfile-js-complete.png)

1. Tenga en cuenta que el **Task Runner Explorer** lista de tareas incluye `clean`, `concat`, `jshint` y `uglify` tareas. Cada tarea se ejecutan en orden y observe los resultados en **el Explorador de soluciones**. Cada tarea debe ejecutarse sin errores.

    ![Explorador de ejecutores de tareas ejecutar cada tarea.](using-grunt/_static/task-runner-explorer-run-each-task.png)

    La tarea de concat crea un nuevo *combined.js* de archivo y lo coloca en el directorio temporal. El `jshint` tarea simplemente se ejecuta y no genera resultados. El `uglify` crea una nueva tarea *combined.min.js* de archivo y lo coloca en *wwwroot/lib*. Una vez completada, la solución debe ser similar a la captura de pantalla siguiente:

    ![el Explorador de soluciones de tareas después de todo](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > Para obtener más información acerca de las opciones para cada paquete, visite [ https://www.npmjs.com/ ](https://www.npmjs.com/) y el nombre del paquete en el cuadro de búsqueda en la página principal de búsqueda. Por ejemplo, puede buscar el paquete grunt-contrib-clean para obtener un vínculo a documentación que explica todos sus parámetros.

### <a name="all-together-now"></a>Ahora todos juntos

Utilice el Grunt `registerTask()` método para ejecutar una serie de tareas en una secuencia determinada. Por ejemplo, para ejecutar el ejemplo de pasos anteriores en el orden limpio -> concat -> jshint -> a que uglify, agregue el siguiente código al módulo. El código debe agregarse en el mismo nivel que las llamadas de loadNpmTasks() fuera initConfig.

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

La nueva tarea aparece en el Explorador de ejecutores de tareas en tareas de Alias. Puede haga clic en y ejecutarla como lo haría en otras tareas. El `all` se ejecutará la tarea `clean`, `concat`, `jshint` y `uglify`, en orden.

![tareas de grunt de alias](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>Observación de cambios

Un `watch` tarea cuida en archivos y directorios. La inspección desencadena tareas automáticamente si detecta los cambios. Agregue el código siguiente para initConfig para ver los cambios a \*archivos .js en el directorio de TypeScript. Si se cambia un archivo JavaScript, `watch` se ejecutará la `all` tarea.

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

Agregue una llamada a `loadNpmTasks()` para mostrar la `watch` tarea en Task Runner Explorer.

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

Haga clic en la tarea de inspección en Task Runner Explorer y seleccione Ejecutar en el menú contextual. La ventana de comandos que se muestra la ejecución de tareas de inspección mostrará un bucle "esperando..." Mensaje. Abra uno de los archivos de TypeScript, agregue un espacio y, a continuación, guarde el archivo. Esto desencadenará la tarea de inspección y desencadenar las demás tareas que se ejecutan en orden. La captura de pantalla siguiente muestra una ejemplo de ejecución.

![resultado de las tareas de ejecución](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Enlazar a eventos de Visual Studio

A menos que desee iniciar manualmente las tareas cada vez que se trabaja en Visual Studio, puede enlazar tareas a **antes de compilar**, **después de compilar**, **Clean**, y  **Proyecto abierto** eventos.

Vamos a enlazar `watch` para que se ejecute cada vez que abra Visual Studio. En el Explorador de ejecutores de tareas, haga clic en la tarea de inspección y seleccione **enlaces > Abrir proyecto** en el menú contextual.

![enlazar una tarea a la apertura del proyecto](using-grunt/_static/bindings-project-open.png)

Descargar y recargar el proyecto. Cuando se carga el proyecto de nuevo, iniciará la tarea de inspección se ejecuten automáticamente.

## <a name="summary"></a>Resumen

Grunt es un ejecutor de tareas eficaz que puede utilizarse para automatizar la mayoría de las tareas de compilación del cliente. Grunt aprovecha NPM para entregar sus paquetes, características y herramientas de integración con Visual Studio. Visual Studio Task Runner Explorer detecta los cambios en los archivos de configuración y proporciona una interfaz adecuada para ejecutar tareas, ver las tareas en ejecución y enlazar tareas a eventos de Visual Studio.
