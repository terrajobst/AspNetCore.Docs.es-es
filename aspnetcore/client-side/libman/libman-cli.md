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
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a><span data-ttu-id="bc02d-103">Usar la interfaz de línea de comandos (CLI) de LibMan con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc02d-103">Use the LibMan command-line interface (CLI) with ASP.NET Core</span></span>

<span data-ttu-id="bc02d-104">Por [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="bc02d-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="bc02d-105">El [LibMan](xref:client-side/libman/index) CLI es una herramienta multiplataforma que se admite en todas partes .NET Core es compatible.</span><span class="sxs-lookup"><span data-stu-id="bc02d-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc02d-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="bc02d-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="bc02d-107">Instalación</span><span class="sxs-lookup"><span data-stu-id="bc02d-107">Installation</span></span>

<span data-ttu-id="bc02d-108">Para instalar la CLI LibMan:</span><span class="sxs-lookup"><span data-stu-id="bc02d-108">To install the LibMan CLI:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="bc02d-109">Un [herramienta Global de .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) se instala desde el [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="bc02d-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="bc02d-110">Para instalar la CLI LibMan desde un origen de paquete de NuGet específico:</span><span class="sxs-lookup"><span data-stu-id="bc02d-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="bc02d-111">En el ejemplo anterior, se instala una herramienta Global de .NET Core desde la máquina Windows local *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* archivo.</span><span class="sxs-lookup"><span data-stu-id="bc02d-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="bc02d-112">Uso</span><span class="sxs-lookup"><span data-stu-id="bc02d-112">Usage</span></span>

<span data-ttu-id="bc02d-113">Tras una instalación correcta de la CLI, se puede usar el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc02d-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="bc02d-114">Para ver la versión instalada de CLI:</span><span class="sxs-lookup"><span data-stu-id="bc02d-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="bc02d-115">Para ver los comandos CLI disponibles:</span><span class="sxs-lookup"><span data-stu-id="bc02d-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="bc02d-116">El comando anterior muestra un resultado similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc02d-116">The preceding command displays output similar to the following:</span></span>

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

<span data-ttu-id="bc02d-117">Las siguientes secciones describen los comandos CLI disponibles.</span><span class="sxs-lookup"><span data-stu-id="bc02d-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="bc02d-118">Inicializar LibMan en el proyecto</span><span class="sxs-lookup"><span data-stu-id="bc02d-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="bc02d-119">El `libman init` comando crea un *libman.json* archivo si no existe ninguno.</span><span class="sxs-lookup"><span data-stu-id="bc02d-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="bc02d-120">El archivo se crea con el contenido predeterminado de la plantilla de elemento.</span><span class="sxs-lookup"><span data-stu-id="bc02d-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="bc02d-121">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="bc02d-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="bc02d-122">Opciones</span><span class="sxs-lookup"><span data-stu-id="bc02d-122">Options</span></span>

<span data-ttu-id="bc02d-123">Las siguientes opciones están disponibles para el `libman init` comando:</span><span class="sxs-lookup"><span data-stu-id="bc02d-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="bc02d-124">Una ruta de acceso relativa a la carpeta actual.</span><span class="sxs-lookup"><span data-stu-id="bc02d-124">A path relative to the current folder.</span></span> <span data-ttu-id="bc02d-125">Archivos de biblioteca se instalan en esta ubicación si no hay ningún `destination` propiedad está definida para una biblioteca en *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="bc02d-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="bc02d-126">El `<PATH>` valor se escribe en el `defaultDestination` propiedad de *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="bc02d-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="bc02d-127">El proveedor debe usar si no se define ningún proveedor para una determinada biblioteca.</span><span class="sxs-lookup"><span data-stu-id="bc02d-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="bc02d-128">El `<PROVIDER>` valor se escribe en el `defaultProvider` propiedad de *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="bc02d-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="bc02d-129">Reemplace `<PROVIDER>` con uno de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="bc02d-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="bc02d-130">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="bc02d-130">Examples</span></span>

<span data-ttu-id="bc02d-131">Para crear un *libman.json* archivo en un proyecto de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="bc02d-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="bc02d-132">Vaya a la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="bc02d-132">Navigate to the project root.</span></span>
* <span data-ttu-id="bc02d-133">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="bc02d-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="bc02d-134">Escriba el nombre del proveedor predeterminado, o presione `Enter` para utilizar el proveedor CDNJS predeterminado.</span><span class="sxs-lookup"><span data-stu-id="bc02d-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="bc02d-135">Los valores válidos son:</span><span class="sxs-lookup"><span data-stu-id="bc02d-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![comando de init libman: proveedor predeterminado](_static/libman-init-provider.png)

<span data-ttu-id="bc02d-137">Un *libman.json* archivo se agrega a la raíz del proyecto con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="bc02d-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="bc02d-138">Agregar archivos de biblioteca</span><span class="sxs-lookup"><span data-stu-id="bc02d-138">Add library files</span></span>

<span data-ttu-id="bc02d-139">El `libman install` comando descarga e instala los archivos de biblioteca en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="bc02d-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="bc02d-140">Un *libman.json* se agrega un archivo si no existe.</span><span class="sxs-lookup"><span data-stu-id="bc02d-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="bc02d-141">El *libman.json* se ha modificado para almacenar los detalles de configuración para los archivos de biblioteca.</span><span class="sxs-lookup"><span data-stu-id="bc02d-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="bc02d-142">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="bc02d-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="bc02d-143">Argumentos</span><span class="sxs-lookup"><span data-stu-id="bc02d-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="bc02d-144">El nombre de la biblioteca para instalar.</span><span class="sxs-lookup"><span data-stu-id="bc02d-144">The name of the library to install.</span></span> <span data-ttu-id="bc02d-145">Este nombre puede incluir una notación de número de versión (por ejemplo, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="bc02d-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="bc02d-146">Opciones</span><span class="sxs-lookup"><span data-stu-id="bc02d-146">Options</span></span>

<span data-ttu-id="bc02d-147">Las siguientes opciones están disponibles para el `libman install` comando:</span><span class="sxs-lookup"><span data-stu-id="bc02d-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="bc02d-148">La ubicación para instalar la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="bc02d-148">The location to install the library.</span></span> <span data-ttu-id="bc02d-149">Si no se especifica, se usa la ubicación predeterminada.</span><span class="sxs-lookup"><span data-stu-id="bc02d-149">If not specified, the default location is used.</span></span> <span data-ttu-id="bc02d-150">Si no hay ningún `defaultDestination` propiedad se especifica en *libman.json*, esta opción es necesaria.</span><span class="sxs-lookup"><span data-stu-id="bc02d-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="bc02d-151">Especifique el nombre del archivo que se instale desde la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="bc02d-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="bc02d-152">Si no se especifica, se instalan todos los archivos de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="bc02d-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="bc02d-153">Proporcione uno `--files` opción por archivo para instalarse.</span><span class="sxs-lookup"><span data-stu-id="bc02d-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="bc02d-154">También se admiten rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="bc02d-154">Relative paths are supported too.</span></span> <span data-ttu-id="bc02d-155">Por ejemplo: `--files dist/browser/signalr.js`.</span><span class="sxs-lookup"><span data-stu-id="bc02d-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="bc02d-156">El nombre del proveedor que se usará para la adquisición de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="bc02d-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="bc02d-157">Reemplace `<PROVIDER>` con uno de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="bc02d-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="bc02d-158">Si no se especifica, el `defaultProvider` propiedad *libman.json* se utiliza.</span><span class="sxs-lookup"><span data-stu-id="bc02d-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="bc02d-159">Si no hay ningún `defaultProvider` propiedad se especifica en *libman.json*, esta opción es necesaria.</span><span class="sxs-lookup"><span data-stu-id="bc02d-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="bc02d-160">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="bc02d-160">Examples</span></span>

<span data-ttu-id="bc02d-161">Tenga en cuenta la siguiente *libman.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="bc02d-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="bc02d-162">Para instalar la versión de jQuery 3.2.1 *jquery.min.js* del archivo a la *wwwroot/scripts/jquery* carpeta mediante el proveedor CDNJS:</span><span class="sxs-lookup"><span data-stu-id="bc02d-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot/scripts/jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

<span data-ttu-id="bc02d-163">El *libman.json* archivo es similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc02d-163">The *libman.json* file resembles the following:</span></span>

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

<span data-ttu-id="bc02d-164">Para instalar el *calendar.js* y *calendar.css* archivos desde *C:\\temp\\contosoCalendar\\*  con el sistema de archivos proveedor:</span><span class="sxs-lookup"><span data-stu-id="bc02d-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="bc02d-165">Aparece el mensaje siguiente por dos motivos:</span><span class="sxs-lookup"><span data-stu-id="bc02d-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="bc02d-166">El *libman.json* archivo no contiene un `defaultDestination` propiedad.</span><span class="sxs-lookup"><span data-stu-id="bc02d-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="bc02d-167">El `libman install` comando no debe contener el `-d|--destination` opción.</span><span class="sxs-lookup"><span data-stu-id="bc02d-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![libman instalar command - destino](_static/libman-install-destination.png)

<span data-ttu-id="bc02d-169">Después de aceptar el destino predeterminado, el *libman.json* archivo es similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc02d-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

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

## <a name="restore-library-files"></a><span data-ttu-id="bc02d-170">Restaurar archivos de biblioteca</span><span class="sxs-lookup"><span data-stu-id="bc02d-170">Restore library files</span></span>

<span data-ttu-id="bc02d-171">El `libman restore` comando instala los archivos de biblioteca definidos en *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="bc02d-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="bc02d-172">Se aplican las siguientes reglas:</span><span class="sxs-lookup"><span data-stu-id="bc02d-172">The following rules apply:</span></span>

* <span data-ttu-id="bc02d-173">Si no hay ningún *libman.json* archivo existe en la raíz del proyecto, se devuelve un error.</span><span class="sxs-lookup"><span data-stu-id="bc02d-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="bc02d-174">Si una biblioteca especifica un proveedor, el `defaultProvider` propiedad *libman.json* se omite.</span><span class="sxs-lookup"><span data-stu-id="bc02d-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="bc02d-175">Si una biblioteca especifica un destino, el `defaultDestination` propiedad *libman.json* se omite.</span><span class="sxs-lookup"><span data-stu-id="bc02d-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="bc02d-176">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="bc02d-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="bc02d-177">Opciones</span><span class="sxs-lookup"><span data-stu-id="bc02d-177">Options</span></span>

<span data-ttu-id="bc02d-178">Las siguientes opciones están disponibles para el `libman restore` comando:</span><span class="sxs-lookup"><span data-stu-id="bc02d-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="bc02d-179">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="bc02d-179">Examples</span></span>

<span data-ttu-id="bc02d-180">Para restaurar los archivos de biblioteca definidos en *libman.json*:</span><span class="sxs-lookup"><span data-stu-id="bc02d-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="bc02d-181">Eliminar archivos de biblioteca</span><span class="sxs-lookup"><span data-stu-id="bc02d-181">Delete library files</span></span>

<span data-ttu-id="bc02d-182">El `libman clean` comando elimina los archivos de biblioteca previamente se restaurará a través de LibMan.</span><span class="sxs-lookup"><span data-stu-id="bc02d-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="bc02d-183">Se eliminan las carpetas que se convierten en vacías después de esta operación.</span><span class="sxs-lookup"><span data-stu-id="bc02d-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="bc02d-184">Los archivos de biblioteca asociados a las configuraciones en el `libraries` propiedad de *libman.json* no se quitan.</span><span class="sxs-lookup"><span data-stu-id="bc02d-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="bc02d-185">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="bc02d-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="bc02d-186">Opciones</span><span class="sxs-lookup"><span data-stu-id="bc02d-186">Options</span></span>

<span data-ttu-id="bc02d-187">Las siguientes opciones están disponibles para el `libman clean` comando:</span><span class="sxs-lookup"><span data-stu-id="bc02d-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="bc02d-188">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="bc02d-188">Examples</span></span>

<span data-ttu-id="bc02d-189">Para eliminar los archivos de biblioteca instalados a través de LibMan:</span><span class="sxs-lookup"><span data-stu-id="bc02d-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="bc02d-190">Desinstalar los archivos de biblioteca</span><span class="sxs-lookup"><span data-stu-id="bc02d-190">Uninstall library files</span></span>

<span data-ttu-id="bc02d-191">El `libman uninstall` comando:</span><span class="sxs-lookup"><span data-stu-id="bc02d-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="bc02d-192">Elimina todos los archivos asociados con la biblioteca especificada desde el destino en *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="bc02d-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="bc02d-193">Quita la configuración de biblioteca asociado desde *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="bc02d-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="bc02d-194">Se produce un error cuando:</span><span class="sxs-lookup"><span data-stu-id="bc02d-194">An error occurs when:</span></span>

* <span data-ttu-id="bc02d-195">No *libman.json* archivo existe en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="bc02d-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="bc02d-196">La biblioteca especificada no existe.</span><span class="sxs-lookup"><span data-stu-id="bc02d-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="bc02d-197">Si se instala más de una biblioteca con el mismo nombre, se le pedirá que elija uno.</span><span class="sxs-lookup"><span data-stu-id="bc02d-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="bc02d-198">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="bc02d-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="bc02d-199">Argumentos</span><span class="sxs-lookup"><span data-stu-id="bc02d-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="bc02d-200">El nombre de la biblioteca que se va a desinstalar.</span><span class="sxs-lookup"><span data-stu-id="bc02d-200">The name of the library to uninstall.</span></span> <span data-ttu-id="bc02d-201">Este nombre puede incluir una notación de número de versión (por ejemplo, `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="bc02d-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="bc02d-202">Opciones</span><span class="sxs-lookup"><span data-stu-id="bc02d-202">Options</span></span>

<span data-ttu-id="bc02d-203">Las siguientes opciones están disponibles para el `libman uninstall` comando:</span><span class="sxs-lookup"><span data-stu-id="bc02d-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="bc02d-204">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="bc02d-204">Examples</span></span>

<span data-ttu-id="bc02d-205">Tenga en cuenta la siguiente *libman.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="bc02d-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="bc02d-206">Para desinstalar jQuery, de los siguientes comandos se completan correctamente:</span><span class="sxs-lookup"><span data-stu-id="bc02d-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="bc02d-207">Para desinstalar los archivos de Lodash instalados a través de la `filesystem` proveedor:</span><span class="sxs-lookup"><span data-stu-id="bc02d-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="bc02d-208">Actualizar la versión de la biblioteca</span><span class="sxs-lookup"><span data-stu-id="bc02d-208">Update library version</span></span>

<span data-ttu-id="bc02d-209">El `libman update` comando actualiza una biblioteca se instala a través de LibMan a la versión especificada.</span><span class="sxs-lookup"><span data-stu-id="bc02d-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="bc02d-210">Se produce un error cuando:</span><span class="sxs-lookup"><span data-stu-id="bc02d-210">An error occurs when:</span></span>

* <span data-ttu-id="bc02d-211">No *libman.json* archivo existe en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="bc02d-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="bc02d-212">La biblioteca especificada no existe.</span><span class="sxs-lookup"><span data-stu-id="bc02d-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="bc02d-213">Si se instala más de una biblioteca con el mismo nombre, se le pedirá que elija uno.</span><span class="sxs-lookup"><span data-stu-id="bc02d-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="bc02d-214">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="bc02d-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="bc02d-215">Argumentos</span><span class="sxs-lookup"><span data-stu-id="bc02d-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="bc02d-216">El nombre de la biblioteca para actualizar.</span><span class="sxs-lookup"><span data-stu-id="bc02d-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="bc02d-217">Opciones</span><span class="sxs-lookup"><span data-stu-id="bc02d-217">Options</span></span>

<span data-ttu-id="bc02d-218">Las siguientes opciones están disponibles para el `libman update` comando:</span><span class="sxs-lookup"><span data-stu-id="bc02d-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="bc02d-219">Obtener la última versión preliminar de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="bc02d-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="bc02d-220">Obtener una versión específica de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="bc02d-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="bc02d-221">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="bc02d-221">Examples</span></span>

* <span data-ttu-id="bc02d-222">Para actualizar jQuery para la versión más reciente:</span><span class="sxs-lookup"><span data-stu-id="bc02d-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="bc02d-223">Para actualizar jQuery para la versión 3.3.1:</span><span class="sxs-lookup"><span data-stu-id="bc02d-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="bc02d-224">Para actualizar jQuery para la versión preliminar más reciente:</span><span class="sxs-lookup"><span data-stu-id="bc02d-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="bc02d-225">Administrar la caché de la biblioteca</span><span class="sxs-lookup"><span data-stu-id="bc02d-225">Manage library cache</span></span>

<span data-ttu-id="bc02d-226">El `libman cache` comando administra la caché de la biblioteca LibMan.</span><span class="sxs-lookup"><span data-stu-id="bc02d-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="bc02d-227">El `filesystem` proveedor no usa la caché de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="bc02d-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="bc02d-228">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="bc02d-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="bc02d-229">Argumentos</span><span class="sxs-lookup"><span data-stu-id="bc02d-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="bc02d-230">Solo se usa con el `clean` comando.</span><span class="sxs-lookup"><span data-stu-id="bc02d-230">Only used with the `clean` command.</span></span> <span data-ttu-id="bc02d-231">Especifica la caché del proveedor para limpiar.</span><span class="sxs-lookup"><span data-stu-id="bc02d-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="bc02d-232">Los valores válidos son:</span><span class="sxs-lookup"><span data-stu-id="bc02d-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="bc02d-233">Opciones</span><span class="sxs-lookup"><span data-stu-id="bc02d-233">Options</span></span>

<span data-ttu-id="bc02d-234">Las siguientes opciones están disponibles para el `libman cache` comando:</span><span class="sxs-lookup"><span data-stu-id="bc02d-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="bc02d-235">Enumerar los nombres de los archivos que se almacenan en caché.</span><span class="sxs-lookup"><span data-stu-id="bc02d-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="bc02d-236">Enumerar los nombres de las bibliotecas que se almacenan en caché.</span><span class="sxs-lookup"><span data-stu-id="bc02d-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="bc02d-237">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="bc02d-237">Examples</span></span>

* <span data-ttu-id="bc02d-238">Para ver los nombres de las bibliotecas en caché por el proveedor, use uno de los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="bc02d-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="bc02d-239">Esto genera una salida similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc02d-239">Output similar to the following is displayed:</span></span>

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

* <span data-ttu-id="bc02d-240">Para ver los nombres de archivos de biblioteca almacenados en caché por el proveedor:</span><span class="sxs-lookup"><span data-stu-id="bc02d-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="bc02d-241">Esto genera una salida similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc02d-241">Output similar to the following is displayed:</span></span>

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

  <span data-ttu-id="bc02d-242">Observe que la salida anterior muestra que jQuery se almacenan en caché las versiones 3.2.1 y 3.3.1 con el proveedor de CDNJS.</span><span class="sxs-lookup"><span data-stu-id="bc02d-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="bc02d-243">Para vaciar la caché de biblioteca para el proveedor CDNJS:</span><span class="sxs-lookup"><span data-stu-id="bc02d-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="bc02d-244">Después de vaciar la caché del proveedor CDNJS, el `libman cache list` comando muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc02d-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

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

* <span data-ttu-id="bc02d-245">Para vaciar la memoria caché para todos los proveedores admitidos:</span><span class="sxs-lookup"><span data-stu-id="bc02d-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="bc02d-246">Después de vaciar todas las cachés de proveedor, el `libman cache list` comando muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc02d-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="bc02d-247">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="bc02d-247">Additional resources</span></span>

* [<span data-ttu-id="bc02d-248">Instale una herramienta Global</span><span class="sxs-lookup"><span data-stu-id="bc02d-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="bc02d-249">Repositorio de LibMan en GitHub</span><span class="sxs-lookup"><span data-stu-id="bc02d-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
