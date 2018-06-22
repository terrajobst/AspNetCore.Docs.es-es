---
title: Usar Grunt en ASP.NET Core
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2016
uid: client-side/using-grunt
ms.openlocfilehash: 21fa565c930563bbc819c2a02ea71655193513d0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272979"
---
# <a name="use-grunt-in-aspnet-core"></a>Usar Grunt en ASP.NET Core

Por [Noel arroz](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)

Grunt es un ejecutor de tareas de JavaScript que automatiza la reducción de la secuencia de comandos, compilación de TypeScript, herramientas de "quitar" de calidad de código, preprocesadores CSS y casi cualquier tareas repetitivas que debe realizar para admitir el desarrollo de cliente. Grunt es totalmente compatible en Visual Studio, aunque las plantillas de proyecto ASP.NET utilizan Gulp de forma predeterminada (vea [usar Gulp](using-gulp.md)).

Este ejemplo utiliza un proyecto de ASP.NET Core vacío como punto de partida, para mostrar cómo automatizar el proceso de compilación de cliente desde el principio.

El ejemplo terminado limpia el directorio de implementación de destino, combina los archivos de JavaScript, comprueba la calidad del código, condensa el contenido del archivo de JavaScript y distribuye a la raíz de la aplicación web. Usamos los siguientes paquetes:

* **grunt**: paquete de ejecutor de tareas de la Grunt.

* **limpieza del hogar en grunt**: un complemento que quita los archivos o directorios.

* **grunt-hogar-jshint**: un complemento que se revisa la calidad del código JavaScript.

* **grunt-hogar-concat**: un complemento que combina los archivos en un único archivo.

* **hogar de grunt uglify**: un complemento que minifica el objeto de JavaScript para reducir el tamaño.

* **grunt-hogar-inspección**: un complemento que supervisa la actividad de archivo.

## <a name="preparing-the-application"></a>Preparación de la aplicación

Para empezar, configure una nueva aplicación web vacía y agregar archivos de ejemplo de TypeScript. Archivos de typeScript se compilan automáticamente en JavaScript con la configuración de Visual Studio de forma predeterminada y serán nuestro materias primas para que procese utilizando Grunt.

1.  En Visual Studio, cree un nuevo `ASP.NET Web Application`.

2.  En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el núcleo de ASP.NET **vacía** plantilla y haga clic en el botón Aceptar.

3.  En el Explorador de soluciones, revise la estructura del proyecto. El `\src` carpeta incluye vacía `wwwroot` y `Dependencies` nodos.

    ![solución web vacía](using-grunt/_static/grunt-solution-explorer.png)

4.  Agregar una nueva carpeta denominada `TypeScript` al directorio del proyecto.

5.  Antes de agregar los archivos, asegúrese de que Visual Studio tiene la opción ' compilar al guardar ' para comprobar los archivos TypeScript. Vaya a **herramientas** > **opciones** > **Editor de texto** > **Typescript**  >  **Proyecto**:

    ![Opciones de configuración compliation automática de archivos de TypeScript](using-grunt/_static/typescript-options.png)

6.  Haga clic en el `TypeScript` directorio y seleccione **Agregar > nuevo elemento** en el menú contextual. Seleccione el **archivo JavaScript** de elemento y un nombre al archivo *Tastes.ts* (tenga en cuenta el \*.ts extensión). Copie la línea de código TypeScript a continuación en el archivo (cuando se guarda, un nuevo *Tastes.js* archivo aparecerá con el origen de JavaScript).
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  Agregar un segundo archivo en el **TypeScript** directorio y asígnele el nombre `Food.ts`. Copie el código siguiente en el archivo.

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

A continuación, configure NPM para descargar grunt y tareas grunt.

1. En el Explorador de soluciones, haga clic en el proyecto y seleccione **Agregar > nuevo elemento** en el menú contextual. Seleccione el **archivo de configuración de NPM** item, deje el nombre predeterminado, *package.json*y haga clic en el **agregar** botón.

2. En el *package.json* de archivos, en la `devDependencies` objeto llaves, escriba "grunt". Seleccione `grunt` de Intellisense, la lista y presione la tecla ENTRAR. Visual Studio entrecomillar el nombre del paquete grunt y agregar un signo de dos puntos. A la derecha de los dos puntos, seleccione la versión estable más reciente del paquete de la parte superior de la lista de Intellisense (presione `Ctrl-Space` si no aparece Intellisense).

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > Usa NPM [control de versiones semántico](http://semver.org/) para organizar las dependencias. Control de versiones semántico, también conocido como SemVer, identifica los paquetes con el esquema de numeración <major>.<minor>. <patch>. IntelliSense simplifica el control de versiones semántico presentando unas cuantas opciones comunes. El elemento superior en la lista de Intellisense (0.4.5 en el ejemplo anterior) se considera la versión estable más reciente del paquete. El símbolo de intercalación (^) coincide con la versión principal más reciente y la tilde (~) coincide con la versión secundaria más reciente. Consulte la [referencia del analizador de versión NPM semver](https://www.npmjs.com/package/semver) como guía para la expresividad completa que proporciona SemVer.

3. Agregar más dependencias de carga grunt-hogar -\* empaqueta para su *limpia*, *jshint*, *concat*, *uglify*y *inspección* tal como se muestra en el ejemplo siguiente. Las versiones no es necesario para que coincida con el ejemplo.

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

Los paquetes para cada elemento devDependencies van a descargar, junto con los archivos que necesita cada paquete. Puede encontrar los archivos de paquete en el `node_modules` directorio habilitando la **mostrar todos los archivos** botón en el Explorador de soluciones.

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> Si necesita, puede restaurar manualmente las dependencias en el Explorador de soluciones con el botón secundario en `Dependencies\NPM` y seleccionando el **restaurar paquetes** opción de menú.

![restaurar los paquetes](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Configurar Grunt

Grunt se configura mediante un manifiesto llamado *Gruntfile.js* que define, carga y registra las tareas que se pueden ejecutar manualmente o configuradas para ejecutarse automáticamente basándose en eventos en Visual Studio.

1. Haga clic en el proyecto y seleccione **Agregar > nuevo elemento**. Seleccione el **archivo de configuración de Grunt** opción, deje el nombre predeterminado, *Gruntfile.js*y haga clic en el **agregar** botón.

   El código inicial incluye una definición de módulo y el `grunt.initConfig()` método. El `initConfig()` se usa para establecer las opciones para cada paquete, y el resto del módulo se cargarán y registrar las tareas.
    
   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

2. Dentro de la `initConfig()` método, agregue las opciones para la `clean` tareas tal como se muestra en el ejemplo *Gruntfile.js* a continuación. La tarea clean acepta una matriz de cadenas de directorio. Esta tarea quita archivos wwwroot/lib y quita el directorio temp/todo.

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. Debajo del método initConfig(), agregue una llamada a `grunt.loadNpmTasks()`. Esto hará que la tarea se puede ejecutar desde Visual Studio.

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. Guardar *Gruntfile.js*. El archivo debe ser similar a la captura de pantalla siguiente.

    ![gruntfile inicial](using-grunt/_static/gruntfile-js-initial.png)

5. Haga clic en *Gruntfile.js* y seleccione **explorador del ejecutor de tareas** en el menú contextual. Se abrirá la ventana del explorador del ejecutor de tareas.

    ![menú del explorador de ejecutor de tareas](using-grunt/_static/task-runner-explorer-menu.png)

6. Compruebe que `clean` muestra en **tareas** en el explorador del ejecutor de tareas.

    ![lista de tareas del explorador de ejecutor de tareas](using-grunt/_static/task-runner-explorer-tasks.png)

7. Haga clic en la tarea clean y seleccione **ejecutar** en el menú contextual. Una ventana de comandos muestra el progreso de la tarea.

    ![tarea de limpieza de ejecutar el Explorador de ejecutor de tareas](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > No hay ningún archivos o directorios para limpiar todavía. Si lo desea, puede crearlos manualmente en el Explorador de soluciones y, a continuación, ejecutar la tarea clean como prueba.
    
8. En el método initConfig(), agregue una entrada para `concat` con el código siguiente.

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
    > El `all` propiedad en el código anterior es el nombre de un destino. Los destinos se usan en algunas tareas Grunt para admitir varios entornos de compilación. Puede ver los destinos integrados mediante Intellisense o asignar la suya propia.
    
9. Agregar el `jshint` tareas mediante el código siguiente.

    La utilidad de la calidad del código jshint se ejecuta en cada archivo de JavaScript que se encuentra en el directorio temporal.
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > La opción "-W069" es un error generado por jshint al corchete de cierre de JavaScript usa la sintaxis para asignar una propiedad en lugar de la notación de puntos, es decir, `Tastes["Sweet"]` en lugar de `Tastes.Sweet`. La opción desactiva la advertencia para permitir que el resto del proceso para continuar.

10. Agregar el `uglify` tareas mediante el código siguiente.

    La tarea minifica objeto el *combined.js* archivo se encuentra en el directorio temp y crea el archivo de resultados en wwwroot/lib sigue la convención de nomenclatura estándar  *\<nombre de archivo\>. min.js*.
    
    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

11. En grunt.loadNpmTasks() de llamada que carga grunt de limpieza del hogar, incluir la misma llamada para jshint, concat y uglify con el código siguiente.
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. Guardar *Gruntfile.js*. El archivo debe tener un aspecto similar al ejemplo siguiente.

    ![ejemplo de archivo de grunt completa](using-grunt/_static/gruntfile-js-complete.png)

13. Tenga en cuenta que la lista de tareas del explorador de ejecutor de tareas incluye `clean`, `concat`, `jshint` y `uglify` tareas. Ejecutar cada tarea en orden y observe los resultados en el Explorador de soluciones. Cada tarea se debe ejecutar sin errores.
    
    ![explorador del ejecutor de tareas ejecute cada tarea](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    La tarea de concat crea un nuevo *combined.js* de archivo y lo coloca en el directorio temporal. La tarea de jshint simplemente se ejecuta y no genera resultados. La tarea de uglify crea un nuevo *combined.min.js* de archivo y lo coloca en wwwroot/lib. Al finalizar, la solución debe ser similar a la captura de pantalla siguiente:
    
    ![el Explorador de soluciones después de que todos las tareas](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > Para obtener más información acerca de las opciones para cada paquete, visite [ https://www.npmjs.com/ ](https://www.npmjs.com/) y el nombre del paquete en el cuadro de búsqueda en la página principal de búsqueda. Por ejemplo, puede buscar el paquete grunt de limpieza del hogar para obtener un vínculo de la documentación que explica todos sus parámetros.

### <a name="all-together-now"></a>Ahora todos junto

Utilice la Grunt `registerTask()` método para ejecutar una serie de tareas en un orden determinado. Por ejemplo, para ejecutar el ejemplo pasos anteriores en el orden correcto -> concat -> jshint -> uglify, agregue el siguiente código al módulo. El código debe agregarse en el mismo nivel que las llamadas loadNpmTasks(), fuera de initConfig.

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

La nueva tarea aparece en el explorador del ejecutor de tareas en tareas de Alias. Puede haga y ejecutarla como lo haría en otras tareas. El `all` tarea ejecutará `clean`, `concat`, `jshint` y `uglify`, en orden.

![tareas de grunt de alias](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>Observación de cambios

Un `watch` tarea vigila en archivos y directorios. La inspección desencadena tareas automáticamente si detecta los cambios. Agregue el siguiente código para initConfig para inspeccionar los cambios realizados en \*archivos .js en el directorio TypeScript. Si se modifica un archivo de JavaScript, `watch` se ejecutará la `all` tarea.

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

Agregue una llamada a `loadNpmTasks()` para mostrar la `watch` tarea en el explorador del ejecutor de tareas.

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

Haga clic en la tarea de inspección de explorador del ejecutor de tareas y seleccione Ejecutar en el menú contextual. Mostrará la ventana de comandos que muestra la tarea de inspección que se ejecuta un bucle "esperando..." . Abra uno de los archivos TypeScript, agregue un espacio y, a continuación, guarde el archivo. Esto desencadena la tarea de inspección y desencadenar las demás tareas que se ejecutan en orden. La captura de pantalla siguiente muestra una ejemplo de ejecución.

![resultado de las tareas de ejecución](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Enlazar a eventos de Visual Studio

A menos que desee iniciar manualmente las tareas de cada vez que funcionan en Visual Studio, puede enlazar tareas a **antes de compilar**, **después de compilar**, **limpiar**, y  **Proyecto abierto** eventos.

Vamos a enlazar `watch` para que se ejecute cada vez que abre Visual Studio. En el explorador del ejecutor de tareas, haga clic en la tarea de inspección y seleccione **enlaces > abrir el proyecto** en el menú contextual.

![vincular una tarea a la apertura del proyecto](using-grunt/_static/bindings-project-open.png)

Descargar y recargar el proyecto. Cuando se carga el proyecto de nuevo, iniciará la tarea de inspección ejecuta automáticamente.

## <a name="summary"></a>Resumen

Grunt es un ejecutor de tareas eficaz que puede utilizarse para automatizar la mayoría de las tareas de compilación del cliente. Grunt aprovecha NPM para entregar sus paquetes, características y herramientas de integración con Visual Studio. Explorador del ejecutor de tareas de Visual Studio detecta los cambios en archivos de configuración y proporciona una interfaz adecuada para ejecutar tareas, ver las tareas en ejecución y enlazar tareas a eventos de Visual Studio.

## <a name="additional-resources"></a>Recursos adicionales

   * [Uso de Gulp](using-gulp.md)
