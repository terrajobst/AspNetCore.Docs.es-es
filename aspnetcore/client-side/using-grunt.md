---
title: Use dispesados en ASP.NET Core
author: rick-anderson
description: Use dispesados en ASP.NET Core
ms.author: riande
ms.date: 12/05/2019
uid: client-side/using-grunt
ms.openlocfilehash: e516b85da7e94d0c93be642086fede0a11fea3c2
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74879789"
---
# <a name="use-grunt-in-aspnet-core"></a>Use dispesados en ASP.NET Core

Se trata de un ejecutor de tareas de JavaScript que automatiza el script minificación, la compilación de TypeScript, las herramientas de "pelusa" de calidad del código, los preprocesadores de CSS y las tareas repetitivas que se necesitan para admitir el desarrollo de clientes. El impesado es totalmente compatible con Visual Studio.

En este ejemplo se usa un proyecto de ASP.NET Core vacío como punto de partida para mostrar cómo automatizar el proceso de compilación del cliente desde cero.

El ejemplo terminado limpia el directorio de implementación de destino, combina los archivos JavaScript, comprueba la calidad del código, condensa el contenido del archivo JavaScript e implementa en la raíz de la aplicación Web. Usaremos los siguientes paquetes:

* **impesados**: el pesado paquete del Ejecutor de tareas.

* **pesado-entrib-Clean**: un complemento que quita archivos o directorios.

* **impesado-JSHint**: un complemento que revisa la calidad del código de JavaScript.

* **immisión-contrib-concat**: un complemento que une archivos en un único archivo.

* **distrib-uglify**: un complemento que minifica JavaScript para reducir el tamaño.

* **distrib-enmisión-Watch**: un complemento que inspecciona la actividad de archivo.

## <a name="preparing-the-application"></a>Preparar la aplicación

Para empezar, configure una nueva aplicación Web vacía y agregue los archivos de ejemplo de TypeScript. Los archivos TypeScript se compilan automáticamente en JavaScript con la configuración predeterminada de Visual Studio y será nuestro material sin procesar para procesar con dispesados.

1. En Visual Studio, cree una nueva `ASP.NET Web Application`.

2. En el cuadro de diálogo **nuevo proyecto ASP.net** , seleccione el ASP.net Core plantilla **vacía** y haga clic en el botón Aceptar.

3. En el Explorador de soluciones, revise la estructura del proyecto. La carpeta `\src` incluye nodos vacíos `wwwroot` y `Dependencies`.

    ![solución web vacía](using-grunt/_static/grunt-solution-explorer.png)

4. Agregue una nueva carpeta denominada `TypeScript` al directorio del proyecto.

5. Antes de agregar archivos, asegúrese de que Visual Studio tiene activada la opción ' compilar al guardar ' para los archivos TypeScript. Vaya a **herramientas** > **Opciones** > **Editor de texto** > **Typescript** > **proyecto**:

    ![Opciones de configuración de la compilación automática de archivos TypeScript](using-grunt/_static/typescript-options.png)

6. Haga clic con el botón secundario en el directorio `TypeScript` y seleccione **agregar > nuevo elemento** en el menú contextual. Seleccione el elemento **archivo JavaScript** y asigne al archivo el nombre *gusto. ts* (tenga en cuenta la extensión \*. TS). Copie la línea del código TypeScript siguiente en el archivo (cuando guarde, aparecerá un nuevo archivo de *gusto. js* con el origen de JavaScript).

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. Agregue un segundo archivo al directorio **TypeScript** y asígnele el nombre `Food.ts`. Copie el código siguiente en el archivo.

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

A continuación, configure NPM para descargar tareas disgustas y dispesados.

1. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **agregar > nuevo elemento** en el menú contextual. Seleccione el elemento de **archivo de configuración de NPM** , deje el nombre predeterminado, *Package. JSON*, y haga clic en el botón **Agregar** .

2. En el archivo *Package. JSON* , dentro de las llaves del objeto `devDependencies`, escriba "pesado". Seleccione `grunt` de la lista de IntelliSense y presione la tecla entrar. Visual Studio cotizará el nombre del paquete pesado y agregará un signo de dos puntos. A la derecha de los dos puntos, seleccione la versión estable más reciente del paquete en la parte superior de la lista de IntelliSense (Presione `Ctrl-Space` si IntelliSense no aparece).

    ![impesado IntelliSense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > NPM usa el [control de versiones semántico](https://semver.org/) para organizar las dependencias. El control de versiones semántico, también conocido como SemVer, identifica los paquetes con el esquema de numeración \<> principales.\<> secundaria.\<revisión >. IntelliSense simplifica el control de versiones semántico al mostrar solo algunas opciones comunes. El elemento superior de la lista de IntelliSense (0.4.5 en el ejemplo anterior) se considera la versión estable más reciente del paquete. El símbolo de intercalación (^) coincide con la versión principal más reciente y la tilde (~) coincide con la versión secundaria más reciente. Vea la [Referencia del analizador de versiones de NPM SemVer](https://www.npmjs.com/package/semver) como guía para el expresividad completo que SemVer proporciona.

3. Agregue más dependencias para cargar paquetes de distrib-distrib-\* para *Clean*, *JSHint*, *concat*, *uglify*y *Watch* , tal como se muestra en el ejemplo siguiente. No es necesario que las versiones coincidan con el ejemplo.

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

4. Guarde el archivo *Package. JSON* .

Se descargarán los paquetes de cada elemento de `devDependencies`, junto con los archivos que requiera cada paquete. Puede buscar los archivos de paquete en el directorio de *node_modules* habilitando el botón **Mostrar todos los archivos** en **Explorador de soluciones**.

![node_modules impesados](using-grunt/_static/node-modules.png)

> [!NOTE]
> Si lo necesita, puede restaurar manualmente las dependencias en **Explorador de soluciones** haciendo clic con el botón secundario en `Dependencies\NPM` y seleccionando la opción de menú **restaurar paquetes** .

![restaurar paquetes](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Configuración de pesado

El uso de un manifiesto con el nombre *Gruntfile. js* que define, carga y registra las tareas que se pueden ejecutar manualmente o configurar para que se ejecuten automáticamente en función de los eventos de Visual Studio.

1. Haga clic con el botón derecho en el proyecto y seleccione **agregar** > **nuevo elemento**. Seleccione la plantilla de elemento de **archivo de JavaScript** , cambie el nombre a *Gruntfile. js*y haga clic en el botón **Agregar** .

1. Agregue el código siguiente a *Gruntfile. js*. La función `initConfig` establece las opciones para cada paquete y el resto del módulo carga y registra las tareas.

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

1. Dentro de la función `initConfig`, agregue opciones para la tarea `clean` como se muestra en el ejemplo *Gruntfile. js* siguiente. La tarea `clean` acepta una matriz de cadenas de directorio. Esta tarea quita archivos de *wwwroot/lib* y quita el directorio */temp* completo.

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

1. Debajo de la función `initConfig`, agregue una llamada a `grunt.loadNpmTasks`. Esto hará que la tarea se pueda ejecutar desde Visual Studio.

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

1. Guarde *Gruntfile. js*. El archivo debe tener un aspecto similar al de la captura de pantalla siguiente.

    ![gruntfile inicial](using-grunt/_static/gruntfile-js-initial.png)

1. Haga clic con el botón secundario en *Gruntfile. js* y seleccione **Explorador de Ejecutor de tareas** en el menú contextual. Se abrirá la ventana **Explorador del Ejecutor de tareas** .

    ![menú del explorador del Ejecutor de tareas](using-grunt/_static/task-runner-explorer-menu.png)

1. Compruebe que `clean` muestra en **tareas** en el **Explorador del Ejecutor de tareas**.

    ![lista de tareas del explorador del Ejecutor de tareas](using-grunt/_static/task-runner-explorer-tasks.png)

1. Haga clic con el botón secundario en la tarea limpiar y seleccione **Ejecutar** en el menú contextual. Una ventana de comandos muestra el progreso de la tarea.

    ![Ejecutar tarea limpia del explorador del Ejecutor de tareas](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > No hay archivos o directorios para limpiar todavía. Si lo desea, puede crearlos manualmente en el Explorador de soluciones y, a continuación, ejecutar la tarea limpiar como prueba.

1. En la función `initConfig`, agregue una entrada para `concat` mediante el código siguiente.

    La `src` matriz de propiedades enumera los archivos que se van a combinar, en el orden en que deben combinarse. La propiedad `dest` asigna la ruta de acceso al archivo combinado que se genera.

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > La propiedad `all` en el código anterior es el nombre de un destino. Los destinos se usan en algunas tareas disgustas para permitir varios entornos de compilación. Puede ver los destinos integrados mediante IntelliSense o asignar el suyo propio.

1. Agregue la tarea `jshint` mediante el código siguiente.

    La utilidad JSHint `code-quality` se ejecuta en cada archivo JavaScript que se encuentra en el directorio *temp* .

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > La opción "-W069" es un error producido por JSHint cuando JavaScript usa la sintaxis de corchetes para asignar una propiedad en lugar de la notación de puntos, es decir, `Tastes["Sweet"]` en lugar de `Tastes.Sweet`. La opción desactiva la advertencia para permitir que continúe el resto del proceso.

1. Agregue la tarea `uglify` mediante el código siguiente.

    La tarea minifica el archivo *combinado. js* que se encuentra en el directorio Temp y crea el archivo de resultados en wwwroot/lib siguiendo la Convención de nomenclatura estándar *\<nombre de archivo\>. min. js*.

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

1. En la llamada a `grunt.loadNpmTasks` que carga `grunt-contrib-clean`, incluya la misma llamada para JSHint, Concat y uglify con el código siguiente.

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

1. Guarde *Gruntfile. js*. El archivo debe tener un aspecto similar al del ejemplo siguiente.

    ![ejemplo de archivo pesado completo](using-grunt/_static/gruntfile-js-complete.png)

1. Tenga en cuenta que la lista de tareas del **Explorador del Ejecutor de tareas** incluye tareas de `clean`, `concat`, `jshint` y `uglify`. Ejecute cada tarea en orden y observe los resultados en **Explorador de soluciones**. Cada tarea se debe ejecutar sin errores.

    ![explorador del Ejecutor de tareas ejecutar cada tarea](using-grunt/_static/task-runner-explorer-run-each-task.png)

    La tarea concat crea un nuevo archivo *combinado. js* y lo coloca en el directorio temporal. La tarea `jshint` simplemente se ejecuta y no genera resultados. La tarea `uglify` crea un nuevo archivo *. min. js combinado* y lo coloca en *wwwroot/lib*. Al finalizar, la solución debe tener un aspecto similar al de la siguiente captura de pantalla:

    ![Explorador de soluciones después de todas las tareas](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > Para obtener más información sobre las opciones de cada paquete, visite [https://www.npmjs.com/](https://www.npmjs.com/) y busque el nombre del paquete en el cuadro de búsqueda de la Página principal. Por ejemplo, puede buscar el paquete distrib-Clean-Clean para obtener un vínculo de documentación que explique todos sus parámetros.

### <a name="all-together-now"></a>Ahora, todo junto

Utilice el método de `registerTask()` pesado para ejecutar una serie de tareas en una secuencia determinada. Por ejemplo, para ejecutar los pasos de ejemplo anteriores en el orden Clean-> concat-> JSHint-> uglify, agregue el código siguiente al módulo. El código se debe agregar al mismo nivel que las llamadas a loadNpmTasks (), fuera de initConfig.

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

La nueva tarea aparece en el explorador del Ejecutor de tareas en tareas de alias. Puede hacer clic con el botón secundario y ejecutarlo igual que haría con otras tareas. La tarea `all` se ejecutará `clean`, `concat`, `jshint` y `uglify`, en orden.

![tareas de alias](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>Observación de cambios

Una tarea `watch` está atento a los archivos y directorios. El reloj desencadena las tareas automáticamente si detecta cambios. Agregue el código siguiente a initConfig para ver los cambios en los archivos \*. js en el directorio TypeScript. Si se cambia un archivo JavaScript, `watch` ejecutará la tarea `all`.

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

Agregue una llamada a `loadNpmTasks()` para mostrar la tarea de `watch` en el explorador del Ejecutor de tareas.

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

Haga clic con el botón derecho en la tarea inspección del explorador del Ejecutor de tareas y seleccione Ejecutar en el menú contextual. La ventana de comandos que muestra la tarea de inspección en ejecución mostrará una "esperando..." Mensaje. Abra uno de los archivos TypeScript, agregue un espacio y, a continuación, guarde el archivo. Esto desencadenará la tarea inspección y desencadenará las demás tareas para que se ejecuten en orden. En la captura de pantalla siguiente se muestra una ejecución de ejemplo.

![resultados de tareas en ejecución](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Enlazar a eventos de Visual Studio

A menos que desee iniciar manualmente las tareas cada vez que trabaje en Visual Studio, enlace tareas a **antes**de la compilación, **después de compilar**, **limpiar**y eventos **abiertos del proyecto** .

Enlace `watch` para que se ejecute cada vez que se abra Visual Studio. En el explorador del Ejecutor de tareas, haga clic con el botón derecho en la tarea inspección y seleccione **enlaces** > **proyecto abrir** en el menú contextual.

![enlazar una tarea a la apertura del proyecto](using-grunt/_static/bindings-project-open.png)

Descargue y vuelva a cargar el proyecto. Cuando el proyecto se vuelve a cargar, la tarea de inspección comienza a ejecutarse automáticamente.

## <a name="summary"></a>Resumen

El pesado es un potente ejecutor de tareas que se puede usar para automatizar la mayoría de las tareas de compilación de cliente. El pesado aprovecha NPM para ofrecer sus paquetes y ofrece integración de herramientas con Visual Studio. El explorador del Ejecutor de tareas de Visual Studio detecta cambios en los archivos de configuración y proporciona una interfaz adecuada para ejecutar tareas, ver tareas en ejecución y enlazar tareas a eventos de Visual Studio.
