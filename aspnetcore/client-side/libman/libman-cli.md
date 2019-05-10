---
title: Usar la interfaz de línea de comandos (CLI) de LibMan con ASP.NET Core
author: scottaddie
description: Obtenga información sobre cómo usar la interfaz de línea de comandos (CLI) de LibMan en un proyecto de ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: 5667f79648a60b8fd9496f8041ef08891ab766af
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894762"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a>Usar la interfaz de línea de comandos (CLI) de LibMan con ASP.NET Core

Por [Scott Addie](https://twitter.com/Scott_Addie)

El [LibMan](xref:client-side/libman/index) CLI es una herramienta multiplataforma que se admite en todas partes .NET Core es compatible.

## <a name="prerequisites"></a>Requisitos previos

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a>Instalación

Para instalar la CLI LibMan:

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

Un [herramienta Global de .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) se instala desde el [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) paquete NuGet.

Para instalar la CLI LibMan desde un origen de paquete de NuGet específico:

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

En el ejemplo anterior, se instala una herramienta Global de .NET Core desde la máquina Windows local *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* archivo.

## <a name="usage"></a>Uso

Tras una instalación correcta de la CLI, se puede usar el comando siguiente:

```console
libman
```

Para ver la versión instalada de CLI:

```console
libman --version
```

Para ver los comandos CLI disponibles:

```console
libman --help
```

El comando anterior muestra un resultado similar al siguiente:

```console
 1.0.163+g45474d37ed

Usage: libman [options] [command]

Options:
  --help|-h  Show help information
  --version  Show version information

Commands:
  cache      List or clean libman cache contents
  clean      Deletes all library files defined in libman.json from the project
  init       Create a new libman.json
  install    Add a library definition to the libman.json file, and download the 
             library to the specified location
  restore    Downloads all files from provider and saves them to specified 
             destination
  uninstall  Deletes all files for the specified library from their specified 
             destination, then removes the specified library definition from 
             libman.json
  update     Updates the specified library

Use "libman [command] --help" for more information about a command.
```

Las siguientes secciones describen los comandos CLI disponibles.

## <a name="initialize-libman-in-the-project"></a>Inicializar LibMan en el proyecto

El `libman init` comando crea un *libman.json* archivo si no existe ninguno. El archivo se crea con el contenido predeterminado de la plantilla de elemento.

### <a name="synopsis"></a>Sinopsis

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a>Opciones

Las siguientes opciones están disponibles para el `libman init` comando:

* `-d|--default-destination <PATH>`

  Una ruta de acceso relativa a la carpeta actual. Archivos de biblioteca se instalan en esta ubicación si no hay ningún `destination` propiedad está definida para una biblioteca en *libman.json*. El `<PATH>` valor se escribe en el `defaultDestination` propiedad de *libman.json*.

* `-p|--default-provider <PROVIDER>`

  El proveedor debe usar si no se define ningún proveedor para una determinada biblioteca. El `<PROVIDER>` valor se escribe en el `defaultProvider` propiedad de *libman.json*. Reemplace `<PROVIDER>` con uno de los siguientes valores:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Ejemplos

Para crear un *libman.json* archivo en un proyecto de ASP.NET Core:

* Vaya a la raíz del proyecto.
* Ejecute el siguiente comando:

  ```console
  libman init
  ```

* Escriba el nombre del proveedor predeterminado, o presione `Enter` para utilizar el proveedor CDNJS predeterminado. Los valores válidos son:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![comando de init libman: proveedor predeterminado](_static/libman-init-provider.png)

Un *libman.json* archivo se agrega a la raíz del proyecto con el siguiente contenido:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a>Agregar archivos de biblioteca

El `libman install` comando descarga e instala los archivos de biblioteca en el proyecto. Un *libman.json* se agrega un archivo si no existe. El *libman.json* se ha modificado para almacenar los detalles de configuración para los archivos de biblioteca.

### <a name="synopsis"></a>Sinopsis

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a>Argumentos

`LIBRARY`

El nombre de la biblioteca para instalar. Este nombre puede incluir una notación de número de versión (por ejemplo, `@1.2.0`).

### <a name="options"></a>Opciones

Las siguientes opciones están disponibles para el `libman install` comando:

* `-d|--destination <PATH>`

  La ubicación para instalar la biblioteca. Si no se especifica, se usa la ubicación predeterminada. Si no hay ningún `defaultDestination` propiedad se especifica en *libman.json*, esta opción es necesaria.

* `--files <FILE>`

  Especifique el nombre del archivo que se instale desde la biblioteca. Si no se especifica, se instalan todos los archivos de la biblioteca. Proporcione uno `--files` opción por archivo para instalarse. También se admiten rutas de acceso relativas. Por ejemplo: `--files dist/browser/signalr.js`.

* `-p|--provider <PROVIDER>`

  El nombre del proveedor que se usará para la adquisición de la biblioteca. Reemplace `<PROVIDER>` con uno de los siguientes valores:
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  Si no se especifica, el `defaultProvider` propiedad *libman.json* se utiliza. Si no hay ningún `defaultProvider` propiedad se especifica en *libman.json*, esta opción es necesaria.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Ejemplos

Tenga en cuenta la siguiente *libman.json* archivo:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

Para instalar la versión de jQuery 3.2.1 *jquery.min.js* del archivo a la *wwwroot/scripts/jquery* carpeta mediante el proveedor CDNJS:

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

El *libman.json* archivo es similar al siguiente:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

Para instalar el *calendar.js* y *calendar.css* archivos desde *C:\\temp\\contosoCalendar\\*  con el sistema de archivos proveedor:

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

Aparece el mensaje siguiente por dos motivos:

* El *libman.json* archivo no contiene un `defaultDestination` propiedad.
* El `libman install` comando no debe contener el `-d|--destination` opción.

![libman instalar command - destino](_static/libman-install-destination.png)

Después de aceptar el destino predeterminado, el *libman.json* archivo es similar al siguiente:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot/lib/contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a>Restaurar archivos de biblioteca

El `libman restore` comando instala los archivos de biblioteca definidos en *libman.json*. Se aplican las siguientes reglas:

* Si no hay ningún *libman.json* archivo existe en la raíz del proyecto, se devuelve un error.
* Si una biblioteca especifica un proveedor, el `defaultProvider` propiedad *libman.json* se omite.
* Si una biblioteca especifica un destino, el `defaultDestination` propiedad *libman.json* se omite.

### <a name="synopsis"></a>Sinopsis

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a>Opciones

Las siguientes opciones están disponibles para el `libman restore` comando:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Ejemplos

Para restaurar los archivos de biblioteca definidos en *libman.json*:

```console
libman restore
```

## <a name="delete-library-files"></a>Eliminar archivos de biblioteca

El `libman clean` comando elimina los archivos de biblioteca previamente se restaurará a través de LibMan. Se eliminan las carpetas que se convierten en vacías después de esta operación. Los archivos de biblioteca asociados a las configuraciones en el `libraries` propiedad de *libman.json* no se quitan.

### <a name="synopsis"></a>Sinopsis

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a>Opciones

Las siguientes opciones están disponibles para el `libman clean` comando:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Ejemplos

Para eliminar los archivos de biblioteca instalados a través de LibMan:

```console
libman clean
```

## <a name="uninstall-library-files"></a>Desinstalar los archivos de biblioteca

El `libman uninstall` comando:

* Elimina todos los archivos asociados con la biblioteca especificada desde el destino en *libman.json*.
* Quita la configuración de biblioteca asociado desde *libman.json*.

Se produce un error cuando:

* No *libman.json* archivo existe en la raíz del proyecto.
* La biblioteca especificada no existe.

Si se instala más de una biblioteca con el mismo nombre, se le pedirá que elija uno.

### <a name="synopsis"></a>Sinopsis

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a>Argumentos

`LIBRARY`

El nombre de la biblioteca que se va a desinstalar. Este nombre puede incluir una notación de número de versión (por ejemplo, `@1.2.0`).

### <a name="options"></a>Opciones

Las siguientes opciones están disponibles para el `libman uninstall` comando:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Ejemplos

Tenga en cuenta la siguiente *libman.json* archivo:

[!code-json[](samples/LibManSample/libman.json)]

* Para desinstalar jQuery, de los siguientes comandos se completan correctamente:

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* Para desinstalar los archivos de Lodash instalados a través de la `filesystem` proveedor:

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a>Actualizar la versión de la biblioteca

El `libman update` comando actualiza una biblioteca se instala a través de LibMan a la versión especificada.

Se produce un error cuando:

* No *libman.json* archivo existe en la raíz del proyecto.
* La biblioteca especificada no existe.

Si se instala más de una biblioteca con el mismo nombre, se le pedirá que elija uno.

### <a name="synopsis"></a>Sinopsis

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a>Argumentos

`LIBRARY`

El nombre de la biblioteca para actualizar.

### <a name="options"></a>Opciones

Las siguientes opciones están disponibles para el `libman update` comando:

* `-pre`

  Obtener la última versión preliminar de la biblioteca.

* `--to <VERSION>`

  Obtener una versión específica de la biblioteca.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Ejemplos

* Para actualizar jQuery para la versión más reciente:

  ```console
  libman update jquery
  ```

* Para actualizar jQuery para la versión 3.3.1:

  ```console
  libman update jquery --to 3.3.1
  ```

* Para actualizar jQuery para la versión preliminar más reciente:

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a>Administrar la caché de la biblioteca

El `libman cache` comando administra la caché de la biblioteca LibMan. El `filesystem` proveedor no usa la caché de la biblioteca.

### <a name="synopsis"></a>Sinopsis

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a>Argumentos

`PROVIDER`

Solo se usa con el `clean` comando. Especifica la caché del proveedor para limpiar. Los valores válidos son:

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a>Opciones

Las siguientes opciones están disponibles para el `libman cache` comando:

* `--files`

  Enumerar los nombres de los archivos que se almacenan en caché.

* `--libraries`

  Enumerar los nombres de las bibliotecas que se almacenan en caché.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Ejemplos

* Para ver los nombres de las bibliotecas en caché por el proveedor, use uno de los siguientes comandos:

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  Esto genera una salida similar a la siguiente:

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      font-awesome
      jquery
      knockout
      lodash.js
      react
  ```

* Para ver los nombres de archivos de biblioteca almacenados en caché por el proveedor:

  ```console
  libman cache list --files
  ```

  Esto genera una salida similar a la siguiente:

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout:
          <list omitted for brevity>
      react:
          <list omitted for brevity>
      vue:
          <list omitted for brevity>
  cdnjs:
      font-awesome
          metadata.json
      jquery
          metadata.json
          3.2.1\core.js
          3.2.1\jquery.js
          3.2.1\jquery.min.js
          3.2.1\jquery.min.map
          3.2.1\jquery.slim.js
          3.2.1\jquery.slim.min.js
          3.2.1\jquery.slim.min.map
          3.3.1\core.js
          3.3.1\jquery.js
          3.3.1\jquery.min.js
          3.3.1\jquery.min.map
          3.3.1\jquery.slim.js
          3.3.1\jquery.slim.min.js
          3.3.1\jquery.slim.min.map
      knockout
          metadata.json
          3.4.2\knockout-debug.js
          3.4.2\knockout-min.js
      lodash.js
          metadata.json
          4.17.10\lodash.js
          4.17.10\lodash.min.js
      react
          metadata.json
  ```

  Observe que la salida anterior muestra que jQuery se almacenan en caché las versiones 3.2.1 y 3.3.1 con el proveedor de CDNJS.

* Para vaciar la caché de biblioteca para el proveedor CDNJS:

  ```console
  libman cache clean cdnjs
  ```

  Después de vaciar la caché del proveedor CDNJS, el `libman cache list` comando muestra lo siguiente:

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      (empty)
  ```

* Para vaciar la memoria caché para todos los proveedores admitidos:

  ```console
  libman cache clean
  ```

  Después de vaciar todas las cachés de proveedor, el `libman cache list` comando muestra lo siguiente:

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a>Recursos adicionales

* [Instale una herramienta Global](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [Repositorio de LibMan en GitHub](https://github.com/aspnet/LibraryManager)
