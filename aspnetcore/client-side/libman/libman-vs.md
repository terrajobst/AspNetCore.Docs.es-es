---
title: Usar LibMan con ASP.NET Core en Visual Studio
author: scottaddie
description: Obtenga información sobre cómo usar LibMan en un proyecto de ASP.NET Core con Visual Studio.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: b44769f1d0925f38523d6570858de17f37e32c2b
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "41909952"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a>Usar LibMan con ASP.NET Core en Visual Studio

Por [Scott Addie](https://twitter.com/Scott_Addie)

Visual Studio tiene compatibilidad integrada para [LibMan](xref:client-side/libman/index) en proyectos de ASP.NET Core, incluidos:

* Soporte técnico para configurar y ejecutar operaciones de restauración LibMan en la compilación.
* Elementos de menú para desencadenar la restauración LibMan y las operaciones de limpieza.
* Cuadro de diálogo de búsqueda para buscar bibliotecas y agregar los archivos a un proyecto.
* Compatibilidad con para la edición *libman.json*&mdash;el archivo de manifiesto LibMan.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(cómo descargar)](xref:tutorials/index#how-to-download-a-sample)

## <a name="prerequisites"></a>Requisitos previos

* Visual Studio 2017 versión 15,8 o posterior con el **ASP.NET y desarrollo web** carga de trabajo

## <a name="add-library-files"></a>Agregar archivos de biblioteca

Archivos de biblioteca pueden agregarse a un proyecto de ASP.NET Core de dos maneras diferentes:

1. [Utilice el cuadro de diálogo Agregar biblioteca de cliente](#use-the-add-client-side-library-dialog)
1. [Configurar manualmente las entradas del archivo de manifiesto de LibMan](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a>Utilice el cuadro de diálogo Agregar biblioteca de cliente

Siga estos pasos para instalar una biblioteca de cliente:

* En **el Explorador de soluciones**, haga clic en la carpeta del proyecto en el que se deben agregar los archivos. Elija **agregar** > **biblioteca del cliente**. El **Agregar biblioteca de cliente** aparece el cuadro de diálogo:

  ![Agregar cuadro de diálogo Biblioteca de cliente](_static/add-library-dialog.png)

* Seleccione el proveedor de la biblioteca desde el **proveedor** lista desplegable. CDNJS es el proveedor predeterminado.
* Escriba el nombre de la biblioteca para capturar en el **biblioteca** cuadro de texto. IntelliSense proporciona una lista de bibliotecas, empezando por el texto proporcionado.
* Seleccione la biblioteca de la lista de IntelliSense. Tenga en cuenta el nombre de la biblioteca tiene el sufijo del `@` símbolos y la versión estable más reciente que se sabe que el proveedor seleccionado.
* Decida qué archivos se incluyen:
  * Seleccione el **incluir todos los archivos de biblioteca** botón de opción para incluir todos los archivos de la biblioteca.
  * Seleccione el **elegir archivos específicos** botón de opción para incluir un subconjunto de archivos de la biblioteca. Cuando se selecciona el botón de radio, se habilita el árbol del selector de archivos. Active las casillas a la izquierda de los nombres de archivo para descargar.
* Especifique la carpeta del proyecto para almacenar los archivos en el **ubicación de destino** cuadro de texto. Como recomendación, almacenar cada biblioteca en una carpeta independiente.

  El texto sugerido **ubicación de destino** carpeta se basa en la ubicación desde donde se inició el cuadro de diálogo:

  * Si se inicia desde la raíz del proyecto:
    * *Wwwroot/lib* se utiliza si *wwwroot* existe.
    * *lib* se utiliza si *wwwroot* no existe.
  * Si se inicia desde una carpeta de proyecto, se usa el nombre de la carpeta correspondiente.

  La sugerencia de la carpeta tiene el sufijo del nombre de la biblioteca. La siguiente tabla muestra las sugerencias de carpeta al instalar jQuery en un proyecto de páginas de Razor.
  
  |Ubicación de inicio                           |Carpeta sugerido      |
  |------------------------------------------|----------------------|
  |raíz del proyecto (si *wwwroot* existe)        |*Wwwroot/lib/jquery /* |
  |raíz del proyecto (si *wwwroot* no existe) |*jquery/lib /*         |
  |*Páginas* carpeta del proyecto                 |*Páginas/jquery /*       |

* Haga clic en el **instalar** botón para descargar los archivos, por la configuración en *libman.json*.
* Revise el **Administrador de bibliotecas** fuente de la **salida** ventana de detalles de la instalación. Por ejemplo:

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

### <a name="manually-configure-libman-manifest-file-entries"></a>Configurar manualmente las entradas del archivo de manifiesto de LibMan

Todas las operaciones de LibMan en Visual Studio se basan en el contenido del manifiesto de la raíz proyecto LibMan (*libman.json*). Puede editar manualmente *libman.json* para configurar los archivos de biblioteca para el proyecto. Visual Studio restaura todos los archivos de biblioteca una vez *libman.json* se guarda.

Para abrir *libman.json* para su edición, existen las siguientes opciones:

* Haga doble clic en el *libman.json* archivo **el Explorador de soluciones**.
* Haga clic en el proyecto en **el Explorador de soluciones** y seleccione **administrar bibliotecas de cliente**. **&#8224;**
* Seleccione **administrar bibliotecas de cliente** desde Visual Studio **proyecto** menú. **&#8224;**

**&#8224;** Si el *libman.json* archivo ya no existe en la raíz del proyecto, se creará con el contenido predeterminado de la plantilla de elemento.

Visual Studio ofrece JSON enriquecido compatibilidad como la coloración de edición, formato, IntelliSense y validación del esquema. Esquema JSON del manifiesto LibMan se encuentra en [ http://json.schemastore.org/libman ](http://json.schemastore.org/libman).

Con el archivo de manifiesto siguiente, LibMan recupera archivos por la configuración definida en el `libraries` propiedad. Obtener una explicación de los literales de objeto definidos dentro de `libraries` sigue:

* Un subconjunto de [jQuery](https://jquery.com/) versión 3.3.1 se recupera desde el proveedor CDNJS. El subconjunto se define en el `files` propiedad&mdash;*jquery.min.js*, *archivo jquery.js*, y *jquery.min.map*. Los archivos se colocan en el proyecto *wwwroot/lib/jquery* carpeta.
* La totalidad de [Bootstrap](https://getbootstrap.com/) versión 4.1.3 se recuperan y se coloca en un *wwwroot/lib/bootstrap* carpeta. El literal de objeto `provider` reemplazos de propiedad el `defaultProvider` valor de propiedad. LibMan recupera los archivos de arranque del proveedor unpkg.
* Un subconjunto de [Lodash](https://lodash.com/) fue aprobada por un organismo dentro de la organización. El *lodash.js* y *lodash.min.js* se recuperan archivos de sistema de archivos local en *C:\\tmp\\*. Los archivos se copian en el proyecto *wwwroot/lib/lodash* carpeta.

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> LibMan sólo admite una versión de cada biblioteca de cada proveedor. El *libman.json* archivo no supera la validación de esquema si contiene dos bibliotecas con el mismo nombre de biblioteca para un proveedor determinado.

## <a name="restore-library-files"></a>Restaurar archivos de biblioteca

Para restaurar archivos de biblioteca desde Visual Studio, debe haber un válido *libman.json* archivo en la raíz del proyecto. Archivos restaurados se colocan en el proyecto en la ubicación especificada para cada biblioteca.

Archivos de biblioteca se pueden restaurar en un proyecto de ASP.NET Core de dos maneras:

1. [Restaurar archivos durante la compilación](#restore-files-during-build)
1. [Restaurar los archivos manualmente](#restore-files-manually)

### <a name="restore-files-during-build"></a>Restaurar archivos durante la compilación

LibMan puede restaurar los archivos de biblioteca definidos como parte del proceso de compilación. De forma predeterminada, el *restauración en compilación* comportamiento está deshabilitado.

Para habilitar y probar el comportamiento de compilación de restauración:

* Haga clic en *libman.json* en **el Explorador de soluciones** y seleccione **habilitar la restauración de las bibliotecas de cliente en la compilación** en el menú contextual.
* Haga clic en el **Sí** botón cuando se le pida que instale un paquete de NuGet. El [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) paquete NuGet se agrega al proyecto:

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* Compile el proyecto para confirmar que se produce la restauración de archivos LibMan. El `Microsoft.Web.LibraryManager.Build` paquete inyecta un destino de MSBuild que se ejecuta LibMan durante la operación de compilación del proyecto.
* Revise el **compilar** fuente de la **salida** ventana para un registro de actividad LibMan:

  ```console
  1>------ Build started: Project: LibManSample, Configuration: Debug Any CPU ------
  1>
  1>Restore operation started...
  1>Restoring library jquery@3.3.1...
  1>Restoring library bootstrap@4.1.3...
  1>
  1>2 libraries restored in 10.66 seconds
  1>LibManSample -> C:\LibManSample\bin\Debug\netcoreapp2.1\LibManSample.dll
  ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
  ```

Cuando se habilita el comportamiento de compilación de restauración, el *libman.json* menú contextual muestra un **deshabilitar Restaurar las bibliotecas de cliente en la compilación** opción. Al seleccionar esta opción quita el `Microsoft.Web.LibraryManager.Build` referencia del archivo de proyecto del paquete. Por lo tanto, las bibliotecas de cliente ya no se restauran en cada compilación.

Independientemente de la configuración de compilación de restauración, puede restaurar manualmente en cualquier momento desde el *libman.json* menú contextual. Para obtener más información, consulte [restaurar manualmente los archivos](#restore-files-manually).

### <a name="restore-files-manually"></a>Restaurar los archivos manualmente

Para restaurar manualmente los archivos de biblioteca:

* Todos los proyectos de la solución:
  * Haga clic en el nombre de la solución en **el Explorador de soluciones**.
  * Seleccione el **las bibliotecas de cliente de restauración** opción.
* Para un proyecto específico:
  * Haga clic en el *libman.json* archivo **el Explorador de soluciones**.
  * Seleccione el **las bibliotecas de cliente de restauración** opción.

Mientras se está ejecutando la operación de restauración:

* El icono del centro de estado de tarea (TSC) en la barra de estado de Visual Studio se animará y leerá *inició la operación de restauración*. Al hacer clic en el icono se abre una lista de las tareas de fondo conocidos de información sobre herramientas.
* Los mensajes se enviarán a la barra de estado y el **Administrador de bibliotecas** fuente de la **salida** ventana. Por ejemplo:

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

## <a name="delete-library-files"></a>Eliminar archivos de biblioteca

Para realizar la *limpia* operación, lo que elimina los archivos de biblioteca restaurados anteriormente en Visual Studio:

* Haga clic en el *libman.json* archivo **el Explorador de soluciones**.
* Seleccione el **limpia de las bibliotecas de cliente** opción.

Para evitar la eliminación accidental de archivos de la biblioteca no, la operación de limpieza no elimina directorios enteros. Solo quita los archivos que se incluyeron en la restauración anterior.

Mientras se está ejecutando la operación de limpieza:

* En la barra de estado de Visual Studio en el icono TSC se animará y leerá *inició la operación de las bibliotecas de cliente*. Al hacer clic en el icono se abre una lista de las tareas de fondo conocidos de información sobre herramientas.
* Los mensajes se envían a la barra de estado y el **Administrador de bibliotecas** fuente de la **salida** ventana. Por ejemplo:

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

La operación de limpieza elimina solo los archivos del proyecto. Archivos de biblioteca permanecen en la memoria caché para una recuperación más rápida en las operaciones de restauración futuras. Para administrar archivos de biblioteca almacenados en memoria caché del equipo local, use la CLI LibMan.

## <a name="uninstall-library-files"></a>Desinstalar los archivos de biblioteca

Para desinstalar los archivos de biblioteca:

* Abra *libman.json*.
* Colocar el símbolo de intercalación dentro de la correspondiente `libraries` literal de objeto.
* Haga clic en el icono de bombilla que aparece en el margen izquierdo y seleccione **desinstalar \<nombre_de_biblioteca > @\<library_version >**:

  ![Desinstalar la opción de menú contextual de biblioteca](_static/uninstall-menu-option.png)

Como alternativa, puede editar y guardar el manifiesto LibMan manualmente (*libman.json*). El [la operación de restauración](#restore-library-files) se ejecuta cuando se guarda el archivo. Archivos de biblioteca que ya no se definen en *libman.json* se quitó del proyecto.

## <a name="update-library-version"></a>Actualizar la versión de la biblioteca

Para comprobar si una versión actualizada de biblioteca:

* Abra *libman.json*.
* Colocar el símbolo de intercalación dentro de la correspondiente `libraries` literal de objeto.
* Haga clic en el icono de bombilla que aparece en el margen izquierdo. Mantenga el mouse sobre **buscar actualizaciones**.

LibMan comprueba si hay una versión más reciente que la versión instalada de la biblioteca. Pueden producirse los siguientes resultados:

* Un **No se encontraron actualizaciones** se muestra un mensaje si ya está instalada la versión más reciente.
* La versión estable más reciente se muestra si no está instalada.

  ![Busque la opción de menú contextual de las actualizaciones](_static/update-menu-option.png)

* Si está disponible una versión preliminar más reciente que la versión instalada, se muestra la versión preliminar.

Para degradar a una versión anterior de la biblioteca, edite manualmente el *libman.json* archivo. Cuando se guarda el archivo, el LibMan [la operación de restauración](#restore-library-files):

* Quita archivos redundantes de la versión anterior.
* Agrega los archivos nuevos y actualizados de la nueva versión.

## <a name="additional-resources"></a>Recursos adicionales

* [Repositorio de LibMan GitHub](https://github.com/aspnet/LibraryManager)
