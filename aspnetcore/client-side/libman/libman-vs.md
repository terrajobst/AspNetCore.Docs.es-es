---
title: Usar LibMan con ASP.NET Core en Visual Studio
author: scottaddie
description: Obtenga información sobre cómo usar LibMan en un proyecto de ASP.NET Core con Visual Studio.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: 727bd80b7f37f6ebd9d37b7aab1aa6c33b85678c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206732"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a><span data-ttu-id="a78a0-103">Usar LibMan con ASP.NET Core en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a78a0-103">Use LibMan with ASP.NET Core in Visual Studio</span></span>

<span data-ttu-id="a78a0-104">Por [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="a78a0-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="a78a0-105">Visual Studio tiene compatibilidad integrada para [LibMan](xref:client-side/libman/index) en proyectos de ASP.NET Core, incluidos:</span><span class="sxs-lookup"><span data-stu-id="a78a0-105">Visual Studio has built-in support for [LibMan](xref:client-side/libman/index) in ASP.NET Core projects, including:</span></span>

* <span data-ttu-id="a78a0-106">Soporte técnico para configurar y ejecutar operaciones de restauración LibMan en la compilación.</span><span class="sxs-lookup"><span data-stu-id="a78a0-106">Support for configuring and running LibMan restore operations on build.</span></span>
* <span data-ttu-id="a78a0-107">Elementos de menú para desencadenar la restauración LibMan y las operaciones de limpieza.</span><span class="sxs-lookup"><span data-stu-id="a78a0-107">Menu items for triggering LibMan restore and clean operations.</span></span>
* <span data-ttu-id="a78a0-108">Cuadro de diálogo de búsqueda para buscar bibliotecas y agregar los archivos a un proyecto.</span><span class="sxs-lookup"><span data-stu-id="a78a0-108">Search dialog for finding libraries and adding the files to a project.</span></span>
* <span data-ttu-id="a78a0-109">Compatibilidad con para la edición *libman.json*&mdash;el archivo de manifiesto LibMan.</span><span class="sxs-lookup"><span data-stu-id="a78a0-109">Editing support for *libman.json*&mdash;the LibMan manifest file.</span></span>

<span data-ttu-id="a78a0-110">[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(cómo descargar)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="a78a0-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a78a0-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="a78a0-111">Prerequisites</span></span>

* <span data-ttu-id="a78a0-112">Visual Studio 2017 versión 15,8 o posterior con el **ASP.NET y desarrollo web** carga de trabajo</span><span class="sxs-lookup"><span data-stu-id="a78a0-112">Visual Studio 2017 version 15.8 or later with the **ASP.NET and web development** workload</span></span>

## <a name="add-library-files"></a><span data-ttu-id="a78a0-113">Agregar archivos de biblioteca</span><span class="sxs-lookup"><span data-stu-id="a78a0-113">Add library files</span></span>

<span data-ttu-id="a78a0-114">Archivos de biblioteca pueden agregarse a un proyecto de ASP.NET Core de dos maneras diferentes:</span><span class="sxs-lookup"><span data-stu-id="a78a0-114">Library files can be added to an ASP.NET Core project in two different ways:</span></span>

1. [<span data-ttu-id="a78a0-115">Utilice el cuadro de diálogo Agregar biblioteca de cliente</span><span class="sxs-lookup"><span data-stu-id="a78a0-115">Use the Add Client-Side Library dialog</span></span>](#use-the-add-client-side-library-dialog)
1. [<span data-ttu-id="a78a0-116">Configurar manualmente las entradas del archivo de manifiesto de LibMan</span><span class="sxs-lookup"><span data-stu-id="a78a0-116">Manually configure LibMan manifest file entries</span></span>](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a><span data-ttu-id="a78a0-117">Utilice el cuadro de diálogo Agregar biblioteca de cliente</span><span class="sxs-lookup"><span data-stu-id="a78a0-117">Use the Add Client-Side Library dialog</span></span>

<span data-ttu-id="a78a0-118">Siga estos pasos para instalar una biblioteca de cliente:</span><span class="sxs-lookup"><span data-stu-id="a78a0-118">Follow these steps to install a client-side library:</span></span>

* <span data-ttu-id="a78a0-119">En **el Explorador de soluciones**, haga clic en la carpeta del proyecto en el que se deben agregar los archivos.</span><span class="sxs-lookup"><span data-stu-id="a78a0-119">In **Solution Explorer**, right-click the project folder in which the files should be added.</span></span> <span data-ttu-id="a78a0-120">Elija **agregar** > **biblioteca del cliente**.</span><span class="sxs-lookup"><span data-stu-id="a78a0-120">Choose **Add** > **Client-Side Library**.</span></span> <span data-ttu-id="a78a0-121">El **Agregar biblioteca de cliente** aparece el cuadro de diálogo:</span><span class="sxs-lookup"><span data-stu-id="a78a0-121">The **Add Client-Side Library** dialog appears:</span></span>

  ![Agregar cuadro de diálogo Biblioteca de cliente](_static/add-library-dialog.png)

* <span data-ttu-id="a78a0-123">Seleccione el proveedor de la biblioteca desde el **proveedor** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="a78a0-123">Select the library provider from the **Provider** drop down.</span></span> <span data-ttu-id="a78a0-124">CDNJS es el proveedor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="a78a0-124">CDNJS is the default provider.</span></span>
* <span data-ttu-id="a78a0-125">Escriba el nombre de la biblioteca para capturar en el **biblioteca** cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="a78a0-125">Type the library name to fetch in the **Library** text box.</span></span> <span data-ttu-id="a78a0-126">IntelliSense proporciona una lista de bibliotecas, empezando por el texto proporcionado.</span><span class="sxs-lookup"><span data-stu-id="a78a0-126">IntelliSense provides a list of libraries beginning with the provided text.</span></span>
* <span data-ttu-id="a78a0-127">Seleccione la biblioteca de la lista de IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="a78a0-127">Select the library from the IntelliSense list.</span></span> <span data-ttu-id="a78a0-128">Tenga en cuenta el nombre de la biblioteca tiene el sufijo del `@` símbolos y la versión estable más reciente que se sabe que el proveedor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="a78a0-128">Notice the library name is suffixed with the `@` symbol and the latest stable version known to the selected provider.</span></span>
* <span data-ttu-id="a78a0-129">Decida qué archivos se incluyen:</span><span class="sxs-lookup"><span data-stu-id="a78a0-129">Decide which files to include:</span></span>
  * <span data-ttu-id="a78a0-130">Seleccione el **incluir todos los archivos de biblioteca** botón de opción para incluir todos los archivos de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="a78a0-130">Select the **Include all library files** radio button to include all of the library's files.</span></span>
  * <span data-ttu-id="a78a0-131">Seleccione el **elegir archivos específicos** botón de opción para incluir un subconjunto de archivos de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="a78a0-131">Select the **Choose specific files** radio button to include a subset of the library's files.</span></span> <span data-ttu-id="a78a0-132">Cuando se selecciona el botón de radio, se habilita el árbol del selector de archivos.</span><span class="sxs-lookup"><span data-stu-id="a78a0-132">When the radio button is selected, the file selector tree is enabled.</span></span> <span data-ttu-id="a78a0-133">Active las casillas a la izquierda de los nombres de archivo para descargar.</span><span class="sxs-lookup"><span data-stu-id="a78a0-133">Check the boxes to the left of the file names to download.</span></span>
* <span data-ttu-id="a78a0-134">Especifique la carpeta del proyecto para almacenar los archivos en el **ubicación de destino** cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="a78a0-134">Specify the project folder for storing the files in the **Target Location** text box.</span></span> <span data-ttu-id="a78a0-135">Como recomendación, almacenar cada biblioteca en una carpeta independiente.</span><span class="sxs-lookup"><span data-stu-id="a78a0-135">As a recommendation, store each library in a separate folder.</span></span>

  <span data-ttu-id="a78a0-136">El texto sugerido **ubicación de destino** carpeta se basa en la ubicación desde donde se inició el cuadro de diálogo:</span><span class="sxs-lookup"><span data-stu-id="a78a0-136">The suggested **Target Location** folder is based on the location from which the dialog launched:</span></span>

  * <span data-ttu-id="a78a0-137">Si se inicia desde la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="a78a0-137">If launched from the project root:</span></span>
    * <span data-ttu-id="a78a0-138">*Wwwroot/lib* se utiliza si *wwwroot* existe.</span><span class="sxs-lookup"><span data-stu-id="a78a0-138">*wwwroot/lib* is used if *wwwroot* exists.</span></span>
    * <span data-ttu-id="a78a0-139">*lib* se utiliza si *wwwroot* no existe.</span><span class="sxs-lookup"><span data-stu-id="a78a0-139">*lib* is used if *wwwroot* doesn't exist.</span></span>
  * <span data-ttu-id="a78a0-140">Si se inicia desde una carpeta de proyecto, se usa el nombre de la carpeta correspondiente.</span><span class="sxs-lookup"><span data-stu-id="a78a0-140">If launched from a project folder, the corresponding folder name is used.</span></span>

  <span data-ttu-id="a78a0-141">La sugerencia de la carpeta tiene el sufijo del nombre de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="a78a0-141">The folder suggestion is suffixed with the library name.</span></span> <span data-ttu-id="a78a0-142">La siguiente tabla muestra las sugerencias de carpeta al instalar jQuery en un proyecto de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="a78a0-142">The following table illustrates folder suggestions when installing jQuery in a Razor Pages project.</span></span>
  
  |<span data-ttu-id="a78a0-143">Ubicación de inicio</span><span class="sxs-lookup"><span data-stu-id="a78a0-143">Launch location</span></span>                           |<span data-ttu-id="a78a0-144">Carpeta sugerido</span><span class="sxs-lookup"><span data-stu-id="a78a0-144">Suggested folder</span></span>      |
  |------------------------------------------|----------------------|
  |<span data-ttu-id="a78a0-145">raíz del proyecto (si *wwwroot* existe)</span><span class="sxs-lookup"><span data-stu-id="a78a0-145">project root (if *wwwroot* exists)</span></span>        |<span data-ttu-id="a78a0-146">*Wwwroot/lib/jquery /*</span><span class="sxs-lookup"><span data-stu-id="a78a0-146">*wwwroot/lib/jquery/*</span></span> |
  |<span data-ttu-id="a78a0-147">raíz del proyecto (si *wwwroot* no existe)</span><span class="sxs-lookup"><span data-stu-id="a78a0-147">project root (if *wwwroot* doesn't exist)</span></span> |<span data-ttu-id="a78a0-148">*jquery/lib /*</span><span class="sxs-lookup"><span data-stu-id="a78a0-148">*lib/jquery/*</span></span>         |
  |<span data-ttu-id="a78a0-149">*Páginas* carpeta del proyecto</span><span class="sxs-lookup"><span data-stu-id="a78a0-149">*Pages* folder in project</span></span>                 |<span data-ttu-id="a78a0-150">*Páginas/jquery /*</span><span class="sxs-lookup"><span data-stu-id="a78a0-150">*Pages/jquery/*</span></span>       |

* <span data-ttu-id="a78a0-151">Haga clic en el **instalar** botón para descargar los archivos, por la configuración en *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="a78a0-151">Click the **Install** button to download the files, per the configuration in *libman.json*.</span></span>
* <span data-ttu-id="a78a0-152">Revise el **Administrador de bibliotecas** fuente de la **salida** ventana de detalles de la instalación.</span><span class="sxs-lookup"><span data-stu-id="a78a0-152">Review the **Library Manager** feed of the **Output** window for installation details.</span></span> <span data-ttu-id="a78a0-153">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a78a0-153">For example:</span></span>

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

### <a name="manually-configure-libman-manifest-file-entries"></a><span data-ttu-id="a78a0-154">Configurar manualmente las entradas del archivo de manifiesto de LibMan</span><span class="sxs-lookup"><span data-stu-id="a78a0-154">Manually configure LibMan manifest file entries</span></span>

<span data-ttu-id="a78a0-155">Todas las operaciones de LibMan en Visual Studio se basan en el contenido del manifiesto de la raíz proyecto LibMan (*libman.json*).</span><span class="sxs-lookup"><span data-stu-id="a78a0-155">All LibMan operations in Visual Studio are based on the content of the project root's LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="a78a0-156">Puede editar manualmente *libman.json* para configurar los archivos de biblioteca para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a78a0-156">You can manually edit *libman.json* to configure library files for the project.</span></span> <span data-ttu-id="a78a0-157">Visual Studio restaura todos los archivos de biblioteca una vez *libman.json* se guarda.</span><span class="sxs-lookup"><span data-stu-id="a78a0-157">Visual Studio restores all library files once *libman.json* is saved.</span></span>

<span data-ttu-id="a78a0-158">Para abrir *libman.json* para su edición, existen las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="a78a0-158">To open *libman.json* for editing, the following options exist:</span></span>

* <span data-ttu-id="a78a0-159">Haga doble clic en el *libman.json* archivo **el Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="a78a0-159">Double-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="a78a0-160">Haga clic en el proyecto en **el Explorador de soluciones** y seleccione **administrar bibliotecas de cliente**.</span><span class="sxs-lookup"><span data-stu-id="a78a0-160">Right-click the project in **Solution Explorer** and select **Manage Client-Side Libraries**.</span></span> <span data-ttu-id="a78a0-161">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="a78a0-161">**&#8224;**</span></span>
* <span data-ttu-id="a78a0-162">Seleccione **administrar bibliotecas de cliente** desde Visual Studio **proyecto** menú.</span><span class="sxs-lookup"><span data-stu-id="a78a0-162">Select **Manage Client-Side Libraries** from the Visual Studio **Project** menu.</span></span> <span data-ttu-id="a78a0-163">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="a78a0-163">**&#8224;**</span></span>

<span data-ttu-id="a78a0-164">**&#8224;** Si el *libman.json* archivo ya no existe en la raíz del proyecto, se creará con el contenido predeterminado de la plantilla de elemento.</span><span class="sxs-lookup"><span data-stu-id="a78a0-164">**&#8224;** If the *libman.json* file doesn't already exist in the project root, it will be created with the default item template content.</span></span>

<span data-ttu-id="a78a0-165">Visual Studio ofrece JSON enriquecido compatibilidad como la coloración de edición, formato, IntelliSense y validación del esquema.</span><span class="sxs-lookup"><span data-stu-id="a78a0-165">Visual Studio offers rich JSON editing support such as colorization, formatting, IntelliSense, and schema validation.</span></span> <span data-ttu-id="a78a0-166">Esquema JSON del manifiesto LibMan se encuentra en [ http://json.schemastore.org/libman ](http://json.schemastore.org/libman).</span><span class="sxs-lookup"><span data-stu-id="a78a0-166">The LibMan manifest's JSON schema is found at [http://json.schemastore.org/libman](http://json.schemastore.org/libman).</span></span>

<span data-ttu-id="a78a0-167">Con el archivo de manifiesto siguiente, LibMan recupera archivos por la configuración definida en el `libraries` propiedad.</span><span class="sxs-lookup"><span data-stu-id="a78a0-167">With the following manifest file, LibMan retrieves files per the configuration defined in the `libraries` property.</span></span> <span data-ttu-id="a78a0-168">Obtener una explicación de los literales de objeto definidos dentro de `libraries` sigue:</span><span class="sxs-lookup"><span data-stu-id="a78a0-168">An explanation of the object literals defined within `libraries` follows:</span></span>

* <span data-ttu-id="a78a0-169">Un subconjunto de [jQuery](https://jquery.com/) versión 3.3.1 se recupera desde el proveedor CDNJS.</span><span class="sxs-lookup"><span data-stu-id="a78a0-169">A subset of [jQuery](https://jquery.com/) version 3.3.1 is retrieved from the CDNJS provider.</span></span> <span data-ttu-id="a78a0-170">El subconjunto se define en el `files` propiedad&mdash;*jquery.min.js*, *archivo jquery.js*, y *jquery.min.map*.</span><span class="sxs-lookup"><span data-stu-id="a78a0-170">The subset is defined in the `files` property&mdash;*jquery.min.js*, *jquery.js*, and *jquery.min.map*.</span></span> <span data-ttu-id="a78a0-171">Los archivos se colocan en el proyecto *wwwroot/lib/jquery* carpeta.</span><span class="sxs-lookup"><span data-stu-id="a78a0-171">The files are placed in the project's *wwwroot/lib/jquery* folder.</span></span>
* <span data-ttu-id="a78a0-172">La totalidad de [Bootstrap](https://getbootstrap.com/) versión 4.1.3 se recuperan y se coloca en un *wwwroot/lib/bootstrap* carpeta.</span><span class="sxs-lookup"><span data-stu-id="a78a0-172">The entirety of [Bootstrap](https://getbootstrap.com/) version 4.1.3 is retrieved and placed in a *wwwroot/lib/bootstrap* folder.</span></span> <span data-ttu-id="a78a0-173">El literal de objeto `provider` reemplazos de propiedad el `defaultProvider` valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="a78a0-173">The object literal's `provider` property overrides the `defaultProvider` property value.</span></span> <span data-ttu-id="a78a0-174">LibMan recupera los archivos de arranque del proveedor unpkg.</span><span class="sxs-lookup"><span data-stu-id="a78a0-174">LibMan retrieves the Bootstrap files from the unpkg provider.</span></span>
* <span data-ttu-id="a78a0-175">Un subconjunto de [Lodash](https://lodash.com/) fue aprobada por un organismo dentro de la organización.</span><span class="sxs-lookup"><span data-stu-id="a78a0-175">A subset of [Lodash](https://lodash.com/) was approved by a governing body within the organization.</span></span> <span data-ttu-id="a78a0-176">El *lodash.js* y *lodash.min.js* se recuperan archivos de sistema de archivos local en *C:\\temp\\lodash\\*.</span><span class="sxs-lookup"><span data-stu-id="a78a0-176">The *lodash.js* and *lodash.min.js* files are retrieved from the local file system at *C:\\temp\\lodash\\*.</span></span> <span data-ttu-id="a78a0-177">Los archivos se copian en el proyecto *wwwroot/lib/lodash* carpeta.</span><span class="sxs-lookup"><span data-stu-id="a78a0-177">The files are copied to the project's *wwwroot/lib/lodash* folder.</span></span>

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> <span data-ttu-id="a78a0-178">LibMan sólo admite una versión de cada biblioteca de cada proveedor.</span><span class="sxs-lookup"><span data-stu-id="a78a0-178">LibMan only supports one version of each library from each provider.</span></span> <span data-ttu-id="a78a0-179">El *libman.json* archivo no supera la validación de esquema si contiene dos bibliotecas con el mismo nombre de biblioteca para un proveedor determinado.</span><span class="sxs-lookup"><span data-stu-id="a78a0-179">The *libman.json* file fails schema validation if it contains two libraries with the same library name for a given provider.</span></span>

## <a name="restore-library-files"></a><span data-ttu-id="a78a0-180">Restaurar archivos de biblioteca</span><span class="sxs-lookup"><span data-stu-id="a78a0-180">Restore library files</span></span>

<span data-ttu-id="a78a0-181">Para restaurar archivos de biblioteca desde Visual Studio, debe haber un válido *libman.json* archivo en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a78a0-181">To restore library files from within Visual Studio, there must be a valid *libman.json* file in the project root.</span></span> <span data-ttu-id="a78a0-182">Archivos restaurados se colocan en el proyecto en la ubicación especificada para cada biblioteca.</span><span class="sxs-lookup"><span data-stu-id="a78a0-182">Restored files are placed in the project at the location specified for each library.</span></span>

<span data-ttu-id="a78a0-183">Archivos de biblioteca se pueden restaurar en un proyecto de ASP.NET Core de dos maneras:</span><span class="sxs-lookup"><span data-stu-id="a78a0-183">Library files can be restored in an ASP.NET Core project in two ways:</span></span>

1. [<span data-ttu-id="a78a0-184">Restaurar archivos durante la compilación</span><span class="sxs-lookup"><span data-stu-id="a78a0-184">Restore files during build</span></span>](#restore-files-during-build)
1. [<span data-ttu-id="a78a0-185">Restaurar los archivos manualmente</span><span class="sxs-lookup"><span data-stu-id="a78a0-185">Restore files manually</span></span>](#restore-files-manually)

### <a name="restore-files-during-build"></a><span data-ttu-id="a78a0-186">Restaurar archivos durante la compilación</span><span class="sxs-lookup"><span data-stu-id="a78a0-186">Restore files during build</span></span>

<span data-ttu-id="a78a0-187">LibMan puede restaurar los archivos de biblioteca definidos como parte del proceso de compilación.</span><span class="sxs-lookup"><span data-stu-id="a78a0-187">LibMan can restore the defined library files as part of the build process.</span></span> <span data-ttu-id="a78a0-188">De forma predeterminada, el *restauración en compilación* comportamiento está deshabilitado.</span><span class="sxs-lookup"><span data-stu-id="a78a0-188">By default, the *restore-on-build* behavior is disabled.</span></span>

<span data-ttu-id="a78a0-189">Para habilitar y probar el comportamiento de compilación de restauración:</span><span class="sxs-lookup"><span data-stu-id="a78a0-189">To enable and test the restore-on-build behavior:</span></span>

* <span data-ttu-id="a78a0-190">Haga clic en *libman.json* en **el Explorador de soluciones** y seleccione **habilitar la restauración de las bibliotecas de cliente en la compilación** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="a78a0-190">Right-click *libman.json* in **Solution Explorer** and select **Enable Restore Client-Side Libraries on Build** from the context menu.</span></span>
* <span data-ttu-id="a78a0-191">Haga clic en el **Sí** botón cuando se le pida que instale un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="a78a0-191">Click the **Yes** button when prompted to install a NuGet package.</span></span> <span data-ttu-id="a78a0-192">El [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) paquete NuGet se agrega al proyecto:</span><span class="sxs-lookup"><span data-stu-id="a78a0-192">The [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet package is added to the project:</span></span>

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* <span data-ttu-id="a78a0-193">Compile el proyecto para confirmar que se produce la restauración de archivos LibMan.</span><span class="sxs-lookup"><span data-stu-id="a78a0-193">Build the project to confirm LibMan file restoration occurs.</span></span> <span data-ttu-id="a78a0-194">El `Microsoft.Web.LibraryManager.Build` paquete inyecta un destino de MSBuild que se ejecuta LibMan durante la operación de compilación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a78a0-194">The `Microsoft.Web.LibraryManager.Build` package injects an MSBuild target that runs LibMan during the project's build operation.</span></span>
* <span data-ttu-id="a78a0-195">Revise el **compilar** fuente de la **salida** ventana para un registro de actividad LibMan:</span><span class="sxs-lookup"><span data-stu-id="a78a0-195">Review the **Build** feed of the **Output** window for a LibMan activity log:</span></span>

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

<span data-ttu-id="a78a0-196">Cuando se habilita el comportamiento de compilación de restauración, el *libman.json* menú contextual muestra un **deshabilitar Restaurar las bibliotecas de cliente en la compilación** opción.</span><span class="sxs-lookup"><span data-stu-id="a78a0-196">When the restore-on-build behavior is enabled, the *libman.json* context menu displays a **Disable Restore Client-Side Libraries on Build** option.</span></span> <span data-ttu-id="a78a0-197">Al seleccionar esta opción quita el `Microsoft.Web.LibraryManager.Build` referencia del archivo de proyecto del paquete.</span><span class="sxs-lookup"><span data-stu-id="a78a0-197">Selecting this option removes the `Microsoft.Web.LibraryManager.Build` package reference from the project file.</span></span> <span data-ttu-id="a78a0-198">Por lo tanto, las bibliotecas de cliente ya no se restauran en cada compilación.</span><span class="sxs-lookup"><span data-stu-id="a78a0-198">Consequently, the client-side libraries are no longer restored on each build.</span></span>

<span data-ttu-id="a78a0-199">Independientemente de la configuración de compilación de restauración, puede restaurar manualmente en cualquier momento desde el *libman.json* menú contextual.</span><span class="sxs-lookup"><span data-stu-id="a78a0-199">Regardless of the restore-on-build setting, you can manually restore at any time from the *libman.json* context menu.</span></span> <span data-ttu-id="a78a0-200">Para obtener más información, consulte [restaurar manualmente los archivos](#restore-files-manually).</span><span class="sxs-lookup"><span data-stu-id="a78a0-200">For more information, see [Restore files manually](#restore-files-manually).</span></span>

### <a name="restore-files-manually"></a><span data-ttu-id="a78a0-201">Restaurar los archivos manualmente</span><span class="sxs-lookup"><span data-stu-id="a78a0-201">Restore files manually</span></span>

<span data-ttu-id="a78a0-202">Para restaurar manualmente los archivos de biblioteca:</span><span class="sxs-lookup"><span data-stu-id="a78a0-202">To manually restore library files:</span></span>

* <span data-ttu-id="a78a0-203">Todos los proyectos de la solución:</span><span class="sxs-lookup"><span data-stu-id="a78a0-203">For all projects in the solution:</span></span>
  * <span data-ttu-id="a78a0-204">Haga clic en el nombre de la solución en **el Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="a78a0-204">Right-click the solution name in **Solution Explorer**.</span></span>
  * <span data-ttu-id="a78a0-205">Seleccione el **las bibliotecas de cliente de restauración** opción.</span><span class="sxs-lookup"><span data-stu-id="a78a0-205">Select the **Restore Client-Side Libraries** option.</span></span>
* <span data-ttu-id="a78a0-206">Para un proyecto específico:</span><span class="sxs-lookup"><span data-stu-id="a78a0-206">For a specific project:</span></span>
  * <span data-ttu-id="a78a0-207">Haga clic en el *libman.json* archivo **el Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="a78a0-207">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
  * <span data-ttu-id="a78a0-208">Seleccione el **las bibliotecas de cliente de restauración** opción.</span><span class="sxs-lookup"><span data-stu-id="a78a0-208">Select the **Restore Client-Side Libraries** option.</span></span>

<span data-ttu-id="a78a0-209">Mientras se está ejecutando la operación de restauración:</span><span class="sxs-lookup"><span data-stu-id="a78a0-209">While the restore operation is running:</span></span>

* <span data-ttu-id="a78a0-210">El icono del centro de estado de tarea (TSC) en la barra de estado de Visual Studio se animará y leerá *inició la operación de restauración*.</span><span class="sxs-lookup"><span data-stu-id="a78a0-210">The Task Status Center (TSC) icon on the Visual Studio status bar will be animated and will read *Restore operation started*.</span></span> <span data-ttu-id="a78a0-211">Al hacer clic en el icono se abre una lista de las tareas de fondo conocidos de información sobre herramientas.</span><span class="sxs-lookup"><span data-stu-id="a78a0-211">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="a78a0-212">Los mensajes se enviarán a la barra de estado y el **Administrador de bibliotecas** fuente de la **salida** ventana.</span><span class="sxs-lookup"><span data-stu-id="a78a0-212">Messages will be sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="a78a0-213">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a78a0-213">For example:</span></span>

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

## <a name="delete-library-files"></a><span data-ttu-id="a78a0-214">Eliminar archivos de biblioteca</span><span class="sxs-lookup"><span data-stu-id="a78a0-214">Delete library files</span></span>

<span data-ttu-id="a78a0-215">Para realizar la *limpia* operación, lo que elimina los archivos de biblioteca restaurados anteriormente en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a78a0-215">To perform the *clean* operation, which deletes library files previously restored in Visual Studio:</span></span>

* <span data-ttu-id="a78a0-216">Haga clic en el *libman.json* archivo **el Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="a78a0-216">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="a78a0-217">Seleccione el **limpia de las bibliotecas de cliente** opción.</span><span class="sxs-lookup"><span data-stu-id="a78a0-217">Select the **Clean Client-Side Libraries** option.</span></span>

<span data-ttu-id="a78a0-218">Para evitar la eliminación accidental de archivos de la biblioteca no, la operación de limpieza no elimina directorios enteros.</span><span class="sxs-lookup"><span data-stu-id="a78a0-218">To prevent unintentional removal of non-library files, the clean operation doesn't delete whole directories.</span></span> <span data-ttu-id="a78a0-219">Solo quita los archivos que se incluyeron en la restauración anterior.</span><span class="sxs-lookup"><span data-stu-id="a78a0-219">It only removes files that were included in the previous restore.</span></span>

<span data-ttu-id="a78a0-220">Mientras se está ejecutando la operación de limpieza:</span><span class="sxs-lookup"><span data-stu-id="a78a0-220">While the clean operation is running:</span></span>

* <span data-ttu-id="a78a0-221">En la barra de estado de Visual Studio en el icono TSC se animará y leerá *inició la operación de las bibliotecas de cliente*.</span><span class="sxs-lookup"><span data-stu-id="a78a0-221">The TSC icon on the Visual Studio status bar will be animated and will read *Client libraries operation started*.</span></span> <span data-ttu-id="a78a0-222">Al hacer clic en el icono se abre una lista de las tareas de fondo conocidos de información sobre herramientas.</span><span class="sxs-lookup"><span data-stu-id="a78a0-222">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="a78a0-223">Los mensajes se envían a la barra de estado y el **Administrador de bibliotecas** fuente de la **salida** ventana.</span><span class="sxs-lookup"><span data-stu-id="a78a0-223">Messages are sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="a78a0-224">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a78a0-224">For example:</span></span>

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

<span data-ttu-id="a78a0-225">La operación de limpieza elimina solo los archivos del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a78a0-225">The clean operation only deletes files from the project.</span></span> <span data-ttu-id="a78a0-226">Archivos de biblioteca permanecen en la memoria caché para una recuperación más rápida en las operaciones de restauración futuras.</span><span class="sxs-lookup"><span data-stu-id="a78a0-226">Library files stay in the cache for faster retrieval on future restore operations.</span></span> <span data-ttu-id="a78a0-227">Para administrar archivos de biblioteca almacenados en memoria caché del equipo local, use el [LibMan CLI](xref:client-side/libman/libman-cli).</span><span class="sxs-lookup"><span data-stu-id="a78a0-227">To manage library files stored in the local machine's cache, use the [LibMan CLI](xref:client-side/libman/libman-cli).</span></span>

## <a name="uninstall-library-files"></a><span data-ttu-id="a78a0-228">Desinstalar los archivos de biblioteca</span><span class="sxs-lookup"><span data-stu-id="a78a0-228">Uninstall library files</span></span>

<span data-ttu-id="a78a0-229">Para desinstalar los archivos de biblioteca:</span><span class="sxs-lookup"><span data-stu-id="a78a0-229">To uninstall library files:</span></span>

* <span data-ttu-id="a78a0-230">Abra *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="a78a0-230">Open *libman.json*.</span></span>
* <span data-ttu-id="a78a0-231">Colocar el símbolo de intercalación dentro de la correspondiente `libraries` literal de objeto.</span><span class="sxs-lookup"><span data-stu-id="a78a0-231">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="a78a0-232">Haga clic en el icono de bombilla que aparece en el margen izquierdo y seleccione **desinstalar \<nombre_de_biblioteca > @\<library_version >**:</span><span class="sxs-lookup"><span data-stu-id="a78a0-232">Click the light bulb icon that appears in the left margin, and select **Uninstall \<library_name>@\<library_version>**:</span></span>

  ![Desinstalar la opción de menú contextual de biblioteca](_static/uninstall-menu-option.png)

<span data-ttu-id="a78a0-234">Como alternativa, puede editar y guardar el manifiesto LibMan manualmente (*libman.json*).</span><span class="sxs-lookup"><span data-stu-id="a78a0-234">Alternatively, you can manually edit and save the LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="a78a0-235">El [la operación de restauración](#restore-library-files) se ejecuta cuando se guarda el archivo.</span><span class="sxs-lookup"><span data-stu-id="a78a0-235">The [restore operation](#restore-library-files) runs when the file is saved.</span></span> <span data-ttu-id="a78a0-236">Archivos de biblioteca que ya no se definen en *libman.json* se quitó del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a78a0-236">Library files that are no longer defined in *libman.json* are removed from the project.</span></span>

## <a name="update-library-version"></a><span data-ttu-id="a78a0-237">Actualizar la versión de la biblioteca</span><span class="sxs-lookup"><span data-stu-id="a78a0-237">Update library version</span></span>

<span data-ttu-id="a78a0-238">Para comprobar si una versión actualizada de biblioteca:</span><span class="sxs-lookup"><span data-stu-id="a78a0-238">To check for an updated library version:</span></span>

* <span data-ttu-id="a78a0-239">Abra *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="a78a0-239">Open *libman.json*.</span></span>
* <span data-ttu-id="a78a0-240">Colocar el símbolo de intercalación dentro de la correspondiente `libraries` literal de objeto.</span><span class="sxs-lookup"><span data-stu-id="a78a0-240">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="a78a0-241">Haga clic en el icono de bombilla que aparece en el margen izquierdo.</span><span class="sxs-lookup"><span data-stu-id="a78a0-241">Click the light bulb icon that appears in the left margin.</span></span> <span data-ttu-id="a78a0-242">Mantenga el mouse sobre **buscar actualizaciones**.</span><span class="sxs-lookup"><span data-stu-id="a78a0-242">Hover over **Check for updates**.</span></span>

<span data-ttu-id="a78a0-243">LibMan comprueba si hay una versión más reciente que la versión instalada de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="a78a0-243">LibMan checks for a library version newer than the version installed.</span></span> <span data-ttu-id="a78a0-244">Pueden producirse los siguientes resultados:</span><span class="sxs-lookup"><span data-stu-id="a78a0-244">The following outcomes can occur:</span></span>

* <span data-ttu-id="a78a0-245">Un **No se encontraron actualizaciones** se muestra un mensaje si ya está instalada la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="a78a0-245">A **No updates found** message is displayed if the latest version is already installed.</span></span>
* <span data-ttu-id="a78a0-246">La versión estable más reciente se muestra si no está instalada.</span><span class="sxs-lookup"><span data-stu-id="a78a0-246">The latest stable version is displayed if not already installed.</span></span>

  ![Busque la opción de menú contextual de las actualizaciones](_static/update-menu-option.png)

* <span data-ttu-id="a78a0-248">Si está disponible una versión preliminar más reciente que la versión instalada, se muestra la versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="a78a0-248">If a pre-release newer than the installed version is available, the pre-release is displayed.</span></span>

<span data-ttu-id="a78a0-249">Para degradar a una versión anterior de la biblioteca, edite manualmente el *libman.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="a78a0-249">To downgrade to an older library version, manually edit the *libman.json* file.</span></span> <span data-ttu-id="a78a0-250">Cuando se guarda el archivo, el LibMan [la operación de restauración](#restore-library-files):</span><span class="sxs-lookup"><span data-stu-id="a78a0-250">When the file is saved, the LibMan [restore operation](#restore-library-files):</span></span>

* <span data-ttu-id="a78a0-251">Quita archivos redundantes de la versión anterior.</span><span class="sxs-lookup"><span data-stu-id="a78a0-251">Removes redundant files from the previous version.</span></span>
* <span data-ttu-id="a78a0-252">Agrega los archivos nuevos y actualizados de la nueva versión.</span><span class="sxs-lookup"><span data-stu-id="a78a0-252">Adds new and updated files from the new version.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a78a0-253">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a78a0-253">Additional resources</span></span>

* <xref:client-side/libman/libman-cli>
* [<span data-ttu-id="a78a0-254">Repositorio de LibMan en GitHub</span><span class="sxs-lookup"><span data-stu-id="a78a0-254">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
