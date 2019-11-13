---
title: Use la interfaz de la línea de comandos (CLI) de LibMan con ASP.NET Core
author: scottaddie
description: Aprenda a usar la interfaz de la línea de comandos (CLI) de LibMan en un proyecto de ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/libman/libman-cli
ms.openlocfilehash: 8b2b1e45ab4685482554ac439b0276e0cf381609
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962801"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a>Use la interfaz de la línea de comandos (CLI) de LibMan con ASP.NET Core

Por [Scott Addie](https://twitter.com/Scott_Addie)

La CLI de [LibMan](xref:client-side/libman/index) es una herramienta multiplataforma compatible con .net Core.

## <a name="prerequisites"></a>Requisitos previos

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a>Instalación

Para instalar la CLI de LibMan:

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

Se instala una [herramienta global de .net Core](/dotnet/core/tools/global-tools#install-a-global-tool) desde el paquete NuGet [Microsoft. Web. LibraryManager. CLI](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) .

Para instalar la CLI de LibMan desde un origen de paquete de NuGet específico:

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

En el ejemplo anterior, se instala una herramienta global de .NET Core desde el archivo *C:\Temp\Microsoft.Web.LibraryManager.CLI.1.0.94-g606058a278.nupkg* de la máquina local de Windows.

## <a name="usage"></a>Uso

Después de la instalación correcta de la CLI, se puede usar el siguiente comando:

```console
libman
```

Para ver la versión instalada de la CLI:

```console
libman --version
```

Para ver los comandos de la CLI disponibles:

```console
libman --help
```

El comando anterior muestra una salida similar a la siguiente:

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

En las secciones siguientes se describen los comandos disponibles de la CLI.

## <a name="initialize-libman-in-the-project"></a>Inicializar LibMan en el proyecto

El comando `libman init` crea un archivo *Libman. JSON* si no existe ninguno. El archivo se crea con el contenido de la plantilla de elemento predeterminado.

### <a name="synopsis"></a>Sinopsis

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a>Opciones

Las siguientes opciones están disponibles para el comando `libman init`:

* `-d|--default-destination <PATH>`

  Ruta de acceso relativa a la carpeta actual. Los archivos de biblioteca se instalan en esta ubicación si no se ha definido ninguna propiedad `destination` para una biblioteca en *Libman. JSON*. El valor `<PATH>` se escribe en la propiedad `defaultDestination` de *Libman. JSON*.

* `-p|--default-provider <PROVIDER>`

  Proveedor que se va a usar si no se ha definido ningún proveedor para una biblioteca determinada. El valor `<PROVIDER>` se escribe en la propiedad `defaultProvider` de *Libman. JSON*. Reemplace `<PROVIDER>` por uno de los siguientes valores:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Ejemplos

Para crear un archivo *Libman. JSON* en un proyecto ASP.net Core:

* Navegue hasta la raíz del proyecto.
* Ejecute el siguiente comando:

  ```console
  libman init
  ```

* Escriba el nombre del proveedor predeterminado o presione `Enter` para usar el proveedor CDNJS predeterminado. Los valores válidos son:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![comando init de Libman: proveedor predeterminado](_static/libman-init-provider.png)

Se agrega un archivo *Libman. JSON* a la raíz del proyecto con el siguiente contenido:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a>Agregar archivos de biblioteca

El comando `libman install` descarga e instala los archivos de biblioteca en el proyecto. Si no existe una, se agrega un archivo *Libman. JSON* . El archivo *Libman. JSON* se modifica para almacenar los detalles de configuración de los archivos de biblioteca.

### <a name="synopsis"></a>Sinopsis

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a>Argumentos

`LIBRARY`

Nombre de la biblioteca que se va a instalar. Este nombre puede incluir la notación del número de versión (por ejemplo, `@1.2.0`).

### <a name="options"></a>Opciones

Las siguientes opciones están disponibles para el comando `libman install`:

* `-d|--destination <PATH>`

  Ubicación en la que se va a instalar la biblioteca. Si no se especifica, se utiliza la ubicación predeterminada. Si no se especifica ninguna propiedad `defaultDestination` en *Libman. JSON*, esta opción es obligatoria.

* `--files <FILE>`

  Especifique el nombre del archivo que se va a instalar de la biblioteca. Si no se especifica, se instalan todos los archivos de la biblioteca. Proporcione una `--files` opción por cada archivo que se va a instalar. También se admiten rutas de acceso relativas. Por ejemplo: `--files dist/browser/signalr.js`.

* `-p|--provider <PROVIDER>`

  Nombre del proveedor que se va a utilizar para la adquisición de la biblioteca. Reemplace `<PROVIDER>` por uno de los siguientes valores:
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  Si no se especifica, se usa la propiedad `defaultProvider` en *Libman. JSON* . Si no se especifica ninguna propiedad `defaultProvider` en *Libman. JSON*, esta opción es obligatoria.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Ejemplos

Considere el siguiente archivo *Libman. JSON* :

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

Para instalar el archivo jQuery *. min. js* de jQuery versión 3.2.1 en la carpeta *wwwroot/scripts/jQuery* mediante el proveedor CDNJS:

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

El archivo *Libman. JSON* es similar al siguiente:

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

Para instalar los archivos *Calendar. js* y *Calendar. CSS* desde *C:\\Temp\\contosoCalendar\\* con el proveedor del sistema de archivos:

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

El siguiente mensaje aparece por dos motivos:

* El archivo *Libman. JSON* no contiene una propiedad `defaultDestination`.
* El comando `libman install` no contiene la opción `-d|--destination`.

![comando de instalación de Libman: destino](_static/libman-install-destination.png)

Después de aceptar el destino predeterminado, el archivo *Libman. JSON* es similar al siguiente:

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

El comando `libman restore` instala los archivos de biblioteca definidos en *Libman. JSON*. Se aplican las siguientes reglas:

* Si no existe ningún archivo *Libman. JSON* en la raíz del proyecto, se devuelve un error.
* Si una biblioteca especifica un proveedor, se omite la propiedad `defaultProvider` de *Libman. JSON* .
* Si una biblioteca especifica un destino, se omite la propiedad `defaultDestination` de *Libman. JSON* .

### <a name="synopsis"></a>Sinopsis

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a>Opciones

Las siguientes opciones están disponibles para el comando `libman restore`:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Ejemplos

Para restaurar los archivos de biblioteca definidos en *Libman. JSON*:

```console
libman restore
```

## <a name="delete-library-files"></a>Eliminar archivos de biblioteca

El comando `libman clean` elimina los archivos de biblioteca restaurados previamente a través de LibMan. Las carpetas que se vacían después de esta operación se eliminan. No se quitan las configuraciones asociadas de los archivos de biblioteca de la propiedad `libraries` de *Libman. JSON* .

### <a name="synopsis"></a>Sinopsis

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a>Opciones

Las siguientes opciones están disponibles para el comando `libman clean`:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Ejemplos

Para eliminar archivos de biblioteca instalados a través de LibMan:

```console
libman clean
```

## <a name="uninstall-library-files"></a>Desinstalar archivos de biblioteca

El comando `libman uninstall`:

* Elimina todos los archivos asociados a la biblioteca especificada del destino en *Libman. JSON*.
* Quita la configuración de biblioteca asociada de *Libman. JSON*.

Se produce un error cuando:

* No existe ningún archivo *Libman. JSON* en la raíz del proyecto.
* La biblioteca especificada no existe.

Si hay más de una biblioteca con el mismo nombre instalada, se le pedirá que elija una.

### <a name="synopsis"></a>Sinopsis

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a>Argumentos

`LIBRARY`

Nombre de la biblioteca que se va a desinstalar. Este nombre puede incluir la notación del número de versión (por ejemplo, `@1.2.0`).

### <a name="options"></a>Opciones

Las siguientes opciones están disponibles para el comando `libman uninstall`:

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Ejemplos

Considere el siguiente archivo *Libman. JSON* :

[!code-json[](samples/LibManSample/libman.json)]

* Para desinstalar jQuery, cualquiera de los siguientes comandos se realiza correctamente:

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* Para desinstalar los archivos Lodash instalados a través del proveedor de `filesystem`:

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a>Actualizar la versión de la biblioteca

El comando `libman update` actualiza una biblioteca instalada a través de LibMan a la versión especificada.

Se produce un error cuando:

* No existe ningún archivo *Libman. JSON* en la raíz del proyecto.
* La biblioteca especificada no existe.

Si hay más de una biblioteca con el mismo nombre instalada, se le pedirá que elija una.

### <a name="synopsis"></a>Sinopsis

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a>Argumentos

`LIBRARY`

Nombre de la biblioteca que se va a actualizar.

### <a name="options"></a>Opciones

Las siguientes opciones están disponibles para el comando `libman update`:

* `-pre`

  Obtenga la versión preliminar más reciente de la biblioteca.

* `--to <VERSION>`

  Obtener una versión específica de la biblioteca.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Ejemplos

* Para actualizar jQuery a la versión más reciente:

  ```console
  libman update jquery
  ```

* Para actualizar jQuery a la versión 3.3.1:

  ```console
  libman update jquery --to 3.3.1
  ```

* Para actualizar jQuery a la versión preliminar más reciente:

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a>Administrar caché de biblioteca

El comando `libman cache` administra la memoria caché de la biblioteca LibMan. El proveedor de `filesystem` no utiliza la memoria caché de la biblioteca.

### <a name="synopsis"></a>Sinopsis

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a>Argumentos

`PROVIDER`

Solo se usa con el comando `clean`. Especifica la caché del proveedor que se va a limpiar. Los valores válidos son:

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a>Opciones

Las siguientes opciones están disponibles para el comando `libman cache`:

* `--files`

  Enumera los nombres de los archivos que se almacenan en caché.

* `--libraries`

  Enumera los nombres de las bibliotecas que se almacenan en caché.

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>Ejemplos

* Para ver los nombres de las bibliotecas almacenadas en caché por proveedor, use uno de los siguientes comandos:

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

* Para ver los nombres de los archivos de biblioteca almacenados en caché por proveedor:

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

  Observe que la salida anterior muestra que las versiones 3.2.1 y 3.3.1 de jQuery están almacenadas en caché en el proveedor CDNJS.

* Para vaciar la memoria caché de la biblioteca del proveedor CDNJS:

  ```console
  libman cache clean cdnjs
  ```

  Después de vaciar la memoria caché del proveedor de CDNJS, el comando `libman cache list` muestra lo siguiente:

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

* Para vaciar la memoria caché de todos los proveedores admitidos:

  ```console
  libman cache clean
  ```

  Después de vaciar todas las memorias caché del proveedor, el comando `libman cache list` muestra lo siguiente:

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a>Recursos adicionales

* [Instalar una herramienta global](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [Repositorio de LibMan en GitHub](https://github.com/aspnet/LibraryManager)
