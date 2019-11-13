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
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a><span data-ttu-id="21635-103">Use la interfaz de la línea de comandos (CLI) de LibMan con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21635-103">Use the LibMan command-line interface (CLI) with ASP.NET Core</span></span>

<span data-ttu-id="21635-104">Por [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="21635-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="21635-105">La CLI de [LibMan](xref:client-side/libman/index) es una herramienta multiplataforma compatible con .net Core.</span><span class="sxs-lookup"><span data-stu-id="21635-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21635-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="21635-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="21635-107">Instalación</span><span class="sxs-lookup"><span data-stu-id="21635-107">Installation</span></span>

<span data-ttu-id="21635-108">Para instalar la CLI de LibMan:</span><span class="sxs-lookup"><span data-stu-id="21635-108">To install the LibMan CLI:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="21635-109">Se instala una [herramienta global de .net Core](/dotnet/core/tools/global-tools#install-a-global-tool) desde el paquete NuGet [Microsoft. Web. LibraryManager. CLI](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) .</span><span class="sxs-lookup"><span data-stu-id="21635-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="21635-110">Para instalar la CLI de LibMan desde un origen de paquete de NuGet específico:</span><span class="sxs-lookup"><span data-stu-id="21635-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="21635-111">En el ejemplo anterior, se instala una herramienta global de .NET Core desde el archivo *C:\Temp\Microsoft.Web.LibraryManager.CLI.1.0.94-g606058a278.nupkg* de la máquina local de Windows.</span><span class="sxs-lookup"><span data-stu-id="21635-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="21635-112">Uso</span><span class="sxs-lookup"><span data-stu-id="21635-112">Usage</span></span>

<span data-ttu-id="21635-113">Después de la instalación correcta de la CLI, se puede usar el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="21635-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="21635-114">Para ver la versión instalada de la CLI:</span><span class="sxs-lookup"><span data-stu-id="21635-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="21635-115">Para ver los comandos de la CLI disponibles:</span><span class="sxs-lookup"><span data-stu-id="21635-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="21635-116">El comando anterior muestra una salida similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="21635-116">The preceding command displays output similar to the following:</span></span>

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

<span data-ttu-id="21635-117">En las secciones siguientes se describen los comandos disponibles de la CLI.</span><span class="sxs-lookup"><span data-stu-id="21635-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="21635-118">Inicializar LibMan en el proyecto</span><span class="sxs-lookup"><span data-stu-id="21635-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="21635-119">El comando `libman init` crea un archivo *Libman. JSON* si no existe ninguno.</span><span class="sxs-lookup"><span data-stu-id="21635-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="21635-120">El archivo se crea con el contenido de la plantilla de elemento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="21635-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="21635-121">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="21635-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="21635-122">Opciones</span><span class="sxs-lookup"><span data-stu-id="21635-122">Options</span></span>

<span data-ttu-id="21635-123">Las siguientes opciones están disponibles para el comando `libman init`:</span><span class="sxs-lookup"><span data-stu-id="21635-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="21635-124">Ruta de acceso relativa a la carpeta actual.</span><span class="sxs-lookup"><span data-stu-id="21635-124">A path relative to the current folder.</span></span> <span data-ttu-id="21635-125">Los archivos de biblioteca se instalan en esta ubicación si no se ha definido ninguna propiedad `destination` para una biblioteca en *Libman. JSON*.</span><span class="sxs-lookup"><span data-stu-id="21635-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="21635-126">El valor `<PATH>` se escribe en la propiedad `defaultDestination` de *Libman. JSON*.</span><span class="sxs-lookup"><span data-stu-id="21635-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="21635-127">Proveedor que se va a usar si no se ha definido ningún proveedor para una biblioteca determinada.</span><span class="sxs-lookup"><span data-stu-id="21635-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="21635-128">El valor `<PROVIDER>` se escribe en la propiedad `defaultProvider` de *Libman. JSON*.</span><span class="sxs-lookup"><span data-stu-id="21635-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="21635-129">Reemplace `<PROVIDER>` por uno de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="21635-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="21635-130">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="21635-130">Examples</span></span>

<span data-ttu-id="21635-131">Para crear un archivo *Libman. JSON* en un proyecto ASP.net Core:</span><span class="sxs-lookup"><span data-stu-id="21635-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="21635-132">Navegue hasta la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="21635-132">Navigate to the project root.</span></span>
* <span data-ttu-id="21635-133">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="21635-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="21635-134">Escriba el nombre del proveedor predeterminado o presione `Enter` para usar el proveedor CDNJS predeterminado.</span><span class="sxs-lookup"><span data-stu-id="21635-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="21635-135">Los valores válidos son:</span><span class="sxs-lookup"><span data-stu-id="21635-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![comando init de Libman: proveedor predeterminado](_static/libman-init-provider.png)

<span data-ttu-id="21635-137">Se agrega un archivo *Libman. JSON* a la raíz del proyecto con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="21635-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="21635-138">Agregar archivos de biblioteca</span><span class="sxs-lookup"><span data-stu-id="21635-138">Add library files</span></span>

<span data-ttu-id="21635-139">El comando `libman install` descarga e instala los archivos de biblioteca en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="21635-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="21635-140">Si no existe una, se agrega un archivo *Libman. JSON* .</span><span class="sxs-lookup"><span data-stu-id="21635-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="21635-141">El archivo *Libman. JSON* se modifica para almacenar los detalles de configuración de los archivos de biblioteca.</span><span class="sxs-lookup"><span data-stu-id="21635-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="21635-142">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="21635-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="21635-143">Argumentos</span><span class="sxs-lookup"><span data-stu-id="21635-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="21635-144">Nombre de la biblioteca que se va a instalar.</span><span class="sxs-lookup"><span data-stu-id="21635-144">The name of the library to install.</span></span> <span data-ttu-id="21635-145">Este nombre puede incluir la notación del número de versión (por ejemplo, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="21635-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="21635-146">Opciones</span><span class="sxs-lookup"><span data-stu-id="21635-146">Options</span></span>

<span data-ttu-id="21635-147">Las siguientes opciones están disponibles para el comando `libman install`:</span><span class="sxs-lookup"><span data-stu-id="21635-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="21635-148">Ubicación en la que se va a instalar la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="21635-148">The location to install the library.</span></span> <span data-ttu-id="21635-149">Si no se especifica, se utiliza la ubicación predeterminada.</span><span class="sxs-lookup"><span data-stu-id="21635-149">If not specified, the default location is used.</span></span> <span data-ttu-id="21635-150">Si no se especifica ninguna propiedad `defaultDestination` en *Libman. JSON*, esta opción es obligatoria.</span><span class="sxs-lookup"><span data-stu-id="21635-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="21635-151">Especifique el nombre del archivo que se va a instalar de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="21635-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="21635-152">Si no se especifica, se instalan todos los archivos de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="21635-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="21635-153">Proporcione una `--files` opción por cada archivo que se va a instalar.</span><span class="sxs-lookup"><span data-stu-id="21635-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="21635-154">También se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="21635-154">Relative paths are supported too.</span></span> <span data-ttu-id="21635-155">Por ejemplo: `--files dist/browser/signalr.js`.</span><span class="sxs-lookup"><span data-stu-id="21635-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="21635-156">Nombre del proveedor que se va a utilizar para la adquisición de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="21635-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="21635-157">Reemplace `<PROVIDER>` por uno de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="21635-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="21635-158">Si no se especifica, se usa la propiedad `defaultProvider` en *Libman. JSON* .</span><span class="sxs-lookup"><span data-stu-id="21635-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="21635-159">Si no se especifica ninguna propiedad `defaultProvider` en *Libman. JSON*, esta opción es obligatoria.</span><span class="sxs-lookup"><span data-stu-id="21635-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="21635-160">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="21635-160">Examples</span></span>

<span data-ttu-id="21635-161">Considere el siguiente archivo *Libman. JSON* :</span><span class="sxs-lookup"><span data-stu-id="21635-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="21635-162">Para instalar el archivo jQuery *. min. js* de jQuery versión 3.2.1 en la carpeta *wwwroot/scripts/jQuery* mediante el proveedor CDNJS:</span><span class="sxs-lookup"><span data-stu-id="21635-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot/scripts/jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

<span data-ttu-id="21635-163">El archivo *Libman. JSON* es similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="21635-163">The *libman.json* file resembles the following:</span></span>

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

<span data-ttu-id="21635-164">Para instalar los archivos *Calendar. js* y *Calendar. CSS* desde *C:\\Temp\\contosoCalendar\\* con el proveedor del sistema de archivos:</span><span class="sxs-lookup"><span data-stu-id="21635-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="21635-165">El siguiente mensaje aparece por dos motivos:</span><span class="sxs-lookup"><span data-stu-id="21635-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="21635-166">El archivo *Libman. JSON* no contiene una propiedad `defaultDestination`.</span><span class="sxs-lookup"><span data-stu-id="21635-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="21635-167">El comando `libman install` no contiene la opción `-d|--destination`.</span><span class="sxs-lookup"><span data-stu-id="21635-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![comando de instalación de Libman: destino](_static/libman-install-destination.png)

<span data-ttu-id="21635-169">Después de aceptar el destino predeterminado, el archivo *Libman. JSON* es similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="21635-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

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

## <a name="restore-library-files"></a><span data-ttu-id="21635-170">Restaurar archivos de biblioteca</span><span class="sxs-lookup"><span data-stu-id="21635-170">Restore library files</span></span>

<span data-ttu-id="21635-171">El comando `libman restore` instala los archivos de biblioteca definidos en *Libman. JSON*.</span><span class="sxs-lookup"><span data-stu-id="21635-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="21635-172">Se aplican las siguientes reglas:</span><span class="sxs-lookup"><span data-stu-id="21635-172">The following rules apply:</span></span>

* <span data-ttu-id="21635-173">Si no existe ningún archivo *Libman. JSON* en la raíz del proyecto, se devuelve un error.</span><span class="sxs-lookup"><span data-stu-id="21635-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="21635-174">Si una biblioteca especifica un proveedor, se omite la propiedad `defaultProvider` de *Libman. JSON* .</span><span class="sxs-lookup"><span data-stu-id="21635-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="21635-175">Si una biblioteca especifica un destino, se omite la propiedad `defaultDestination` de *Libman. JSON* .</span><span class="sxs-lookup"><span data-stu-id="21635-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="21635-176">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="21635-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="21635-177">Opciones</span><span class="sxs-lookup"><span data-stu-id="21635-177">Options</span></span>

<span data-ttu-id="21635-178">Las siguientes opciones están disponibles para el comando `libman restore`:</span><span class="sxs-lookup"><span data-stu-id="21635-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="21635-179">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="21635-179">Examples</span></span>

<span data-ttu-id="21635-180">Para restaurar los archivos de biblioteca definidos en *Libman. JSON*:</span><span class="sxs-lookup"><span data-stu-id="21635-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="21635-181">Eliminar archivos de biblioteca</span><span class="sxs-lookup"><span data-stu-id="21635-181">Delete library files</span></span>

<span data-ttu-id="21635-182">El comando `libman clean` elimina los archivos de biblioteca restaurados previamente a través de LibMan.</span><span class="sxs-lookup"><span data-stu-id="21635-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="21635-183">Las carpetas que se vacían después de esta operación se eliminan.</span><span class="sxs-lookup"><span data-stu-id="21635-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="21635-184">No se quitan las configuraciones asociadas de los archivos de biblioteca de la propiedad `libraries` de *Libman. JSON* .</span><span class="sxs-lookup"><span data-stu-id="21635-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="21635-185">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="21635-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="21635-186">Opciones</span><span class="sxs-lookup"><span data-stu-id="21635-186">Options</span></span>

<span data-ttu-id="21635-187">Las siguientes opciones están disponibles para el comando `libman clean`:</span><span class="sxs-lookup"><span data-stu-id="21635-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="21635-188">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="21635-188">Examples</span></span>

<span data-ttu-id="21635-189">Para eliminar archivos de biblioteca instalados a través de LibMan:</span><span class="sxs-lookup"><span data-stu-id="21635-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="21635-190">Desinstalar archivos de biblioteca</span><span class="sxs-lookup"><span data-stu-id="21635-190">Uninstall library files</span></span>

<span data-ttu-id="21635-191">El comando `libman uninstall`:</span><span class="sxs-lookup"><span data-stu-id="21635-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="21635-192">Elimina todos los archivos asociados a la biblioteca especificada del destino en *Libman. JSON*.</span><span class="sxs-lookup"><span data-stu-id="21635-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="21635-193">Quita la configuración de biblioteca asociada de *Libman. JSON*.</span><span class="sxs-lookup"><span data-stu-id="21635-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="21635-194">Se produce un error cuando:</span><span class="sxs-lookup"><span data-stu-id="21635-194">An error occurs when:</span></span>

* <span data-ttu-id="21635-195">No existe ningún archivo *Libman. JSON* en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="21635-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="21635-196">La biblioteca especificada no existe.</span><span class="sxs-lookup"><span data-stu-id="21635-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="21635-197">Si hay más de una biblioteca con el mismo nombre instalada, se le pedirá que elija una.</span><span class="sxs-lookup"><span data-stu-id="21635-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="21635-198">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="21635-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="21635-199">Argumentos</span><span class="sxs-lookup"><span data-stu-id="21635-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="21635-200">Nombre de la biblioteca que se va a desinstalar.</span><span class="sxs-lookup"><span data-stu-id="21635-200">The name of the library to uninstall.</span></span> <span data-ttu-id="21635-201">Este nombre puede incluir la notación del número de versión (por ejemplo, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="21635-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="21635-202">Opciones</span><span class="sxs-lookup"><span data-stu-id="21635-202">Options</span></span>

<span data-ttu-id="21635-203">Las siguientes opciones están disponibles para el comando `libman uninstall`:</span><span class="sxs-lookup"><span data-stu-id="21635-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="21635-204">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="21635-204">Examples</span></span>

<span data-ttu-id="21635-205">Considere el siguiente archivo *Libman. JSON* :</span><span class="sxs-lookup"><span data-stu-id="21635-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="21635-206">Para desinstalar jQuery, cualquiera de los siguientes comandos se realiza correctamente:</span><span class="sxs-lookup"><span data-stu-id="21635-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="21635-207">Para desinstalar los archivos Lodash instalados a través del proveedor de `filesystem`:</span><span class="sxs-lookup"><span data-stu-id="21635-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="21635-208">Actualizar la versión de la biblioteca</span><span class="sxs-lookup"><span data-stu-id="21635-208">Update library version</span></span>

<span data-ttu-id="21635-209">El comando `libman update` actualiza una biblioteca instalada a través de LibMan a la versión especificada.</span><span class="sxs-lookup"><span data-stu-id="21635-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="21635-210">Se produce un error cuando:</span><span class="sxs-lookup"><span data-stu-id="21635-210">An error occurs when:</span></span>

* <span data-ttu-id="21635-211">No existe ningún archivo *Libman. JSON* en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="21635-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="21635-212">La biblioteca especificada no existe.</span><span class="sxs-lookup"><span data-stu-id="21635-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="21635-213">Si hay más de una biblioteca con el mismo nombre instalada, se le pedirá que elija una.</span><span class="sxs-lookup"><span data-stu-id="21635-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="21635-214">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="21635-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="21635-215">Argumentos</span><span class="sxs-lookup"><span data-stu-id="21635-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="21635-216">Nombre de la biblioteca que se va a actualizar.</span><span class="sxs-lookup"><span data-stu-id="21635-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="21635-217">Opciones</span><span class="sxs-lookup"><span data-stu-id="21635-217">Options</span></span>

<span data-ttu-id="21635-218">Las siguientes opciones están disponibles para el comando `libman update`:</span><span class="sxs-lookup"><span data-stu-id="21635-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="21635-219">Obtenga la versión preliminar más reciente de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="21635-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="21635-220">Obtener una versión específica de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="21635-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="21635-221">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="21635-221">Examples</span></span>

* <span data-ttu-id="21635-222">Para actualizar jQuery a la versión más reciente:</span><span class="sxs-lookup"><span data-stu-id="21635-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="21635-223">Para actualizar jQuery a la versión 3.3.1:</span><span class="sxs-lookup"><span data-stu-id="21635-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="21635-224">Para actualizar jQuery a la versión preliminar más reciente:</span><span class="sxs-lookup"><span data-stu-id="21635-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="21635-225">Administrar caché de biblioteca</span><span class="sxs-lookup"><span data-stu-id="21635-225">Manage library cache</span></span>

<span data-ttu-id="21635-226">El comando `libman cache` administra la memoria caché de la biblioteca LibMan.</span><span class="sxs-lookup"><span data-stu-id="21635-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="21635-227">El proveedor de `filesystem` no utiliza la memoria caché de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="21635-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="21635-228">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="21635-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="21635-229">Argumentos</span><span class="sxs-lookup"><span data-stu-id="21635-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="21635-230">Solo se usa con el comando `clean`.</span><span class="sxs-lookup"><span data-stu-id="21635-230">Only used with the `clean` command.</span></span> <span data-ttu-id="21635-231">Especifica la caché del proveedor que se va a limpiar.</span><span class="sxs-lookup"><span data-stu-id="21635-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="21635-232">Los valores válidos son:</span><span class="sxs-lookup"><span data-stu-id="21635-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="21635-233">Opciones</span><span class="sxs-lookup"><span data-stu-id="21635-233">Options</span></span>

<span data-ttu-id="21635-234">Las siguientes opciones están disponibles para el comando `libman cache`:</span><span class="sxs-lookup"><span data-stu-id="21635-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="21635-235">Enumera los nombres de los archivos que se almacenan en caché.</span><span class="sxs-lookup"><span data-stu-id="21635-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="21635-236">Enumera los nombres de las bibliotecas que se almacenan en caché.</span><span class="sxs-lookup"><span data-stu-id="21635-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="21635-237">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="21635-237">Examples</span></span>

* <span data-ttu-id="21635-238">Para ver los nombres de las bibliotecas almacenadas en caché por proveedor, use uno de los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="21635-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="21635-239">Esto genera una salida similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="21635-239">Output similar to the following is displayed:</span></span>

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

* <span data-ttu-id="21635-240">Para ver los nombres de los archivos de biblioteca almacenados en caché por proveedor:</span><span class="sxs-lookup"><span data-stu-id="21635-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="21635-241">Esto genera una salida similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="21635-241">Output similar to the following is displayed:</span></span>

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

  <span data-ttu-id="21635-242">Observe que la salida anterior muestra que las versiones 3.2.1 y 3.3.1 de jQuery están almacenadas en caché en el proveedor CDNJS.</span><span class="sxs-lookup"><span data-stu-id="21635-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="21635-243">Para vaciar la memoria caché de la biblioteca del proveedor CDNJS:</span><span class="sxs-lookup"><span data-stu-id="21635-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="21635-244">Después de vaciar la memoria caché del proveedor de CDNJS, el comando `libman cache list` muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="21635-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

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

* <span data-ttu-id="21635-245">Para vaciar la memoria caché de todos los proveedores admitidos:</span><span class="sxs-lookup"><span data-stu-id="21635-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="21635-246">Después de vaciar todas las memorias caché del proveedor, el comando `libman cache list` muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="21635-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="21635-247">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="21635-247">Additional resources</span></span>

* [<span data-ttu-id="21635-248">Instalar una herramienta global</span><span class="sxs-lookup"><span data-stu-id="21635-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="21635-249">Repositorio de LibMan en GitHub</span><span class="sxs-lookup"><span data-stu-id="21635-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
