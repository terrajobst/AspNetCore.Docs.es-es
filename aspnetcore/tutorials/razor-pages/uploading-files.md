---
title: "Carga de archivos en una página de Razor en ASP.NET Core"
author: guardrex
description: "Aprenda a cargar archivos en una página de Razor."
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 4a2c6da6ed698d1a65ee51bd00a557e607f012da
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="e3b02-103">Carga de archivos en una página de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3b02-103">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="e3b02-104">De [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e3b02-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e3b02-105">En esta sección se muestra cómo cargar archivos con una página de Razor.</span><span class="sxs-lookup"><span data-stu-id="e3b02-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="e3b02-106">La [aplicación de ejemplo Razor Pages Movie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) de este tutorial usa el enlace de modelo simple para cargar archivos, lo que funciona bien para cargar archivos pequeños.</span><span class="sxs-lookup"><span data-stu-id="e3b02-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="e3b02-107">Para más información sobre la transmisión de archivos de gran tamaño, vea [Uploading large files with streaming (Carga de archivos grandes mediante transmisión)](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="e3b02-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="e3b02-108">En los pasos siguientes se agrega a la aplicación de ejemplo una característica de carga de archivo de programación de película.</span><span class="sxs-lookup"><span data-stu-id="e3b02-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="e3b02-109">Una programación de película está representada por una clase `Schedule`.</span><span class="sxs-lookup"><span data-stu-id="e3b02-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="e3b02-110">La clase incluye dos versiones de la programación.</span><span class="sxs-lookup"><span data-stu-id="e3b02-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="e3b02-111">Una versión se proporciona a los clientes, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="e3b02-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="e3b02-112">La otra se usa para los empleados de la empresa, `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="e3b02-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="e3b02-113">Cada versión se carga como un archivo independiente.</span><span class="sxs-lookup"><span data-stu-id="e3b02-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="e3b02-114">El tutorial muestra cómo realizar dos cargas de archivos desde una página con un solo elemento POST en el servidor.</span><span class="sxs-lookup"><span data-stu-id="e3b02-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="e3b02-115">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="e3b02-115">Security considerations</span></span>

<span data-ttu-id="e3b02-116">Debe tener precaución al proporcionar a los usuarios la capacidad de cargar archivos en un servidor,</span><span class="sxs-lookup"><span data-stu-id="e3b02-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="e3b02-117">ya que los atacantes podrían ejecutar un ataque por [denegación de servicio](/windows-hardware/drivers/ifs/denial-of-service) u otros ataques en un sistema.</span><span class="sxs-lookup"><span data-stu-id="e3b02-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="e3b02-118">A continuación se muestran algunos pasos de seguridad con los que se reduce la probabilidad de sufrir ataques:</span><span class="sxs-lookup"><span data-stu-id="e3b02-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="e3b02-119">Cargue archivos en un área de carga de archivos específica del sistema, con lo que resulta más fácil imponer medidas de seguridad en el contenido cargado.</span><span class="sxs-lookup"><span data-stu-id="e3b02-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="e3b02-120">Al permitir las cargas de archivos, asegúrese de que los permisos de ejecución están deshabilitados en la ubicación de carga.</span><span class="sxs-lookup"><span data-stu-id="e3b02-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="e3b02-121">Use un nombre de archivo seguro determinado por la aplicación, y no por la entrada del usuario o el nombre de archivo del archivo cargado.</span><span class="sxs-lookup"><span data-stu-id="e3b02-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="e3b02-122">Permita únicamente un conjunto específico de extensiones de archivo aprobadas.</span><span class="sxs-lookup"><span data-stu-id="e3b02-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="e3b02-123">Compruebe que se llevan a cabo comprobaciones de cliente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="e3b02-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="e3b02-124">Las comprobaciones de cliente son fáciles de sortear.</span><span class="sxs-lookup"><span data-stu-id="e3b02-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="e3b02-125">Compruebe el tamaño de la carga y evite las cargas de mayor tamaño de lo esperado.</span><span class="sxs-lookup"><span data-stu-id="e3b02-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="e3b02-126">Ejecute un escáner de virus/malware en el contenido cargado.</span><span class="sxs-lookup"><span data-stu-id="e3b02-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="e3b02-127">La carga de código malintencionado en un sistema suele ser el primer paso para ejecutar código que puede:</span><span class="sxs-lookup"><span data-stu-id="e3b02-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="e3b02-128">Tomar todo el poder en un sistema.</span><span class="sxs-lookup"><span data-stu-id="e3b02-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="e3b02-129">Sobrecargar un sistema de manera que se producen errores generales del sistema.</span><span class="sxs-lookup"><span data-stu-id="e3b02-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="e3b02-130">Poner en peligro los datos del usuario o del sistema.</span><span class="sxs-lookup"><span data-stu-id="e3b02-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="e3b02-131">Aplicar grafitis a una interfaz pública.</span><span class="sxs-lookup"><span data-stu-id="e3b02-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="e3b02-132">Adición de una clase FileUpload</span><span class="sxs-lookup"><span data-stu-id="e3b02-132">Add a FileUpload class</span></span>

<span data-ttu-id="e3b02-133">Cree una página de Razor para controlar un par de cargas de archivos.</span><span class="sxs-lookup"><span data-stu-id="e3b02-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="e3b02-134">Agregue una clase `FileUpload`, que está enlazada a la página para obtener los datos de programación.</span><span class="sxs-lookup"><span data-stu-id="e3b02-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="e3b02-135">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="e3b02-135">Right click the *Models* folder.</span></span> <span data-ttu-id="e3b02-136">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="e3b02-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="e3b02-137">Asigne a la clase el nombre **FileUpload** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="e3b02-137">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="e3b02-138">La clase tiene una propiedad para el título de la programación y otra para cada una de las dos versiones de la programación.</span><span class="sxs-lookup"><span data-stu-id="e3b02-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="e3b02-139">Las tres propiedades son necesarias y el título debe tener entre 3 y 60 caracteres.</span><span class="sxs-lookup"><span data-stu-id="e3b02-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="e3b02-140">Agregar un método auxiliar para cargar archivos</span><span class="sxs-lookup"><span data-stu-id="e3b02-140">Add a helper method to upload files</span></span>

<span data-ttu-id="e3b02-141">Para evitar la duplicación de código para el procesamiento de archivos de programación cargados, primero agregue un método auxiliar estático.</span><span class="sxs-lookup"><span data-stu-id="e3b02-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="e3b02-142">Cree una carpeta *Utilities* en la aplicación y agregue un archivo *FileHelpers.cs* con el siguiente contenido.</span><span class="sxs-lookup"><span data-stu-id="e3b02-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="e3b02-143">El método auxiliar, `ProcessFormFile`, toma un elemento [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) y [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) y devuelve una cadena con el contenido y el tamaño del archivo.</span><span class="sxs-lookup"><span data-stu-id="e3b02-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="e3b02-144">Se comprueban el tipo de contenido y la longitud.</span><span class="sxs-lookup"><span data-stu-id="e3b02-144">The content type and length are checked.</span></span> <span data-ttu-id="e3b02-145">Si el archivo no pasa una comprobación de validación, se agrega un error a `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="e3b02-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a><span data-ttu-id="e3b02-146">Guardar el archivo en el disco</span><span class="sxs-lookup"><span data-stu-id="e3b02-146">Save the file to disk</span></span>

<span data-ttu-id="e3b02-147">La aplicación de ejemplo guarda el contenido del archivo en un campo de base de datos.</span><span class="sxs-lookup"><span data-stu-id="e3b02-147">The sample app saves the file's content into a database field.</span></span> <span data-ttu-id="e3b02-148">Para guardar el contenido del archivo en el disco, use una clase [FileStream](/dotnet/api/system.io.filestream):</span><span class="sxs-lookup"><span data-stu-id="e3b02-148">To save the file's content to disk, use a [FileStream](/dotnet/api/system.io.filestream):</span></span>

```csharp
using (var fileStream = new FileStream(filePath, FileMode.Create))
{
    await formFile.CopyToAsync(fileStream);
}
```

<span data-ttu-id="e3b02-149">El proceso de trabajo debe tener permisos de escritura en la ubicación especificada por `filePath`.</span><span class="sxs-lookup"><span data-stu-id="e3b02-149">The worker process must have write permissions to the location specified by `filePath`.</span></span>

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="e3b02-150">Guardar el archivo en Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="e3b02-150">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="e3b02-151">Para cargar el contenido del archivo en Azure Blob Storage, vea [Introducción a Azure Blob Storage mediante .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="e3b02-151">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="e3b02-152">En el tema se muestra cómo usar [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) para guardar una clase [FileStream](/dotnet/api/system.io.filestream) en Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="e3b02-152">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="e3b02-153">Adición de la clase Schedule</span><span class="sxs-lookup"><span data-stu-id="e3b02-153">Add the Schedule class</span></span>

<span data-ttu-id="e3b02-154">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="e3b02-154">Right click the *Models* folder.</span></span> <span data-ttu-id="e3b02-155">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="e3b02-155">Select **Add** > **Class**.</span></span> <span data-ttu-id="e3b02-156">Asigne a la clase el nombre **Schedule** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="e3b02-156">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="e3b02-157">La clase usa los atributos `Display` y `DisplayFormat`, que generan títulos descriptivos y formato cuando se representan los datos de programación.</span><span class="sxs-lookup"><span data-stu-id="e3b02-157">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="e3b02-158">Actualización de MovieContext</span><span class="sxs-lookup"><span data-stu-id="e3b02-158">Update the MovieContext</span></span>

<span data-ttu-id="e3b02-159">Especifique `DbSet` en `MovieContext` (*Models/MovieContext.cs*) para las programaciones:</span><span class="sxs-lookup"><span data-stu-id="e3b02-159">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="e3b02-160">Adición de la tabla Schedule a la base de datos</span><span class="sxs-lookup"><span data-stu-id="e3b02-160">Add the Schedule table to the database</span></span>

<span data-ttu-id="e3b02-161">Abra la Consola del Administrador de paquetes (PMC): **Herramientas** > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="e3b02-161">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="e3b02-163">En la PMC, ejecute los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="e3b02-163">In the PMC, execute the following commands.</span></span> <span data-ttu-id="e3b02-164">Estos comandos agregan una tabla `Schedule` a la base de datos:</span><span class="sxs-lookup"><span data-stu-id="e3b02-164">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="e3b02-165">Adición de una página de Razor de carga de archivos</span><span class="sxs-lookup"><span data-stu-id="e3b02-165">Add a file upload Razor Page</span></span>

<span data-ttu-id="e3b02-166">En la carpeta *Pages*, cree una carpeta *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="e3b02-166">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="e3b02-167">En la carpeta *Schedules*, cree una página denominada *Index.cshtml* para cargar una programación con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="e3b02-167">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="e3b02-168">Cada grupo del formulario incluye una **\<etiqueta>** que muestra el nombre de cada propiedad de clase.</span><span class="sxs-lookup"><span data-stu-id="e3b02-168">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="e3b02-169">Los atributos `Display` del modelo `FileUpload` proporcionan los valores de presentación de las etiquetas.</span><span class="sxs-lookup"><span data-stu-id="e3b02-169">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="e3b02-170">Por ejemplo, el nombre para mostrar de la propiedad `UploadPublicSchedule` se establece con `[Display(Name="Public Schedule")]` y, por tanto, muestra "Programación pública" en la etiqueta cuando se presenta el formulario.</span><span class="sxs-lookup"><span data-stu-id="e3b02-170">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="e3b02-171">Cada grupo del formulario incluye un **\<intervalo>** de validación.</span><span class="sxs-lookup"><span data-stu-id="e3b02-171">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="e3b02-172">Si la entrada del usuario no cumple los atributos de propiedad establecidos en la clase `FileUpload` o si se produce un error en alguna de las comprobaciones de validación del archivo del método `ProcessFormFile`, no se valida el modelo.</span><span class="sxs-lookup"><span data-stu-id="e3b02-172">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="e3b02-173">Cuando se produce un error en la validación del modelo, se presenta un útil mensaje de validación al usuario.</span><span class="sxs-lookup"><span data-stu-id="e3b02-173">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="e3b02-174">Por ejemplo, la propiedad `Title` se anota con `[Required]` y `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="e3b02-174">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="e3b02-175">Si el usuario no proporciona un título, recibe un mensaje que indica que se necesita un valor.</span><span class="sxs-lookup"><span data-stu-id="e3b02-175">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="e3b02-176">Si el usuario especifica un valor de menos de tres caracteres o de más de sesenta caracteres, recibe un mensaje que indica que el valor tiene una longitud incorrecta.</span><span class="sxs-lookup"><span data-stu-id="e3b02-176">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="e3b02-177">Si se proporciona un archivo sin contenido, aparece un mensaje que indica que el archivo está vacío.</span><span class="sxs-lookup"><span data-stu-id="e3b02-177">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="e3b02-178">Agregar el modelo de página</span><span class="sxs-lookup"><span data-stu-id="e3b02-178">Add the page model</span></span>

<span data-ttu-id="e3b02-179">Agregue el modelo de página (*Index.cshtml.cs*) a la carpeta *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="e3b02-179">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="e3b02-180">El modelo de página (`IndexModel` en *Index.cshtml.cs*) enlaza la clase `FileUpload`:</span><span class="sxs-lookup"><span data-stu-id="e3b02-180">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="e3b02-181">El modelo además usa una lista de las programaciones (`IList<Schedule>`) para mostrar las programaciones almacenadas en la base de datos en la página:</span><span class="sxs-lookup"><span data-stu-id="e3b02-181">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="e3b02-182">Cuando se carga la página con `OnGetAsync`, `Schedules` se rellena a partir de la base de datos y se usa para generar una tabla HTML de programaciones cargadas:</span><span class="sxs-lookup"><span data-stu-id="e3b02-182">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="e3b02-183">Cuando el formulario se publica en el servidor, se activa `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="e3b02-183">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="e3b02-184">Si no es válido, `Schedule` se vuelve a generar y la página se presenta con uno o más mensajes de validación que indican el motivo del error de validación de la página.</span><span class="sxs-lookup"><span data-stu-id="e3b02-184">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="e3b02-185">Si es válido, las propiedades `FileUpload` se usan en *OnPostAsync* para completar la carga de archivos para las dos versiones de la programación y para crear un nuevo objeto `Schedule` para almacenar los datos.</span><span class="sxs-lookup"><span data-stu-id="e3b02-185">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="e3b02-186">La programación luego se guarda en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="e3b02-186">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="e3b02-187">Vinculación de una página de Razor de carga de archivos</span><span class="sxs-lookup"><span data-stu-id="e3b02-187">Link the file upload Razor Page</span></span>

<span data-ttu-id="e3b02-188">Abra *_Layout.cshtml* y agregue un vínculo a la barra de navegación para llegar a la página de carga de archivos:</span><span class="sxs-lookup"><span data-stu-id="e3b02-188">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="e3b02-189">Adición de una página para confirmar la eliminación de la programación</span><span class="sxs-lookup"><span data-stu-id="e3b02-189">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="e3b02-190">Cuando el usuario hace clic para eliminar una programación, se le da la oportunidad de cancelar la operación.</span><span class="sxs-lookup"><span data-stu-id="e3b02-190">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="e3b02-191">Agregue una página de confirmación de eliminación (*Delete.cshtml*) a la carpeta *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="e3b02-191">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="e3b02-192">El modelo de página (*Delete.cshtml.cs*) carga una sola programación identificada por `id` en los datos de ruta de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="e3b02-192">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="e3b02-193">Agregue el archivo *Delete.cshtml.cs* a la carpeta *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="e3b02-193">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="e3b02-194">El método `OnPostAsync` controla la eliminación de la programación mediante su `id`:</span><span class="sxs-lookup"><span data-stu-id="e3b02-194">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="e3b02-195">Después de eliminar correctamente la programación, `RedirectToPage` vuelve a enviar al usuario a la página de programaciones *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e3b02-195">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="e3b02-196">Página de Razor Schedules activa</span><span class="sxs-lookup"><span data-stu-id="e3b02-196">The working Schedules Razor Page</span></span>

<span data-ttu-id="e3b02-197">Cuando se carga la página, las etiquetas y las entradas del título de la programación, la programación pública y la privada se presentan con un botón de envío:</span><span class="sxs-lookup"><span data-stu-id="e3b02-197">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Página de Razor Schedules tal como se muestra en la carga inicial sin errores de validación ni campos vacíos](uploading-files/_static/browser1.png)

<span data-ttu-id="e3b02-199">La selección del botón **Cargar** sin rellenar ninguno de los campos infringe los atributos `[Required]` en el modelo.</span><span class="sxs-lookup"><span data-stu-id="e3b02-199">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="e3b02-200">`ModelState` no es válido.</span><span class="sxs-lookup"><span data-stu-id="e3b02-200">The `ModelState` is invalid.</span></span> <span data-ttu-id="e3b02-201">Los mensajes de error de validación se muestran al usuario:</span><span class="sxs-lookup"><span data-stu-id="e3b02-201">The validation error messages are displayed to the user:</span></span>

![Los mensajes de error de validación aparecen junto a cada control de entrada](uploading-files/_static/browser2.png)

<span data-ttu-id="e3b02-203">Escriba dos letras en el campo **Título**.</span><span class="sxs-lookup"><span data-stu-id="e3b02-203">Type two letters into the **Title** field.</span></span> <span data-ttu-id="e3b02-204">El mensaje de validación cambia para indicar que el título debe tener entre 3 y 60 caracteres:</span><span class="sxs-lookup"><span data-stu-id="e3b02-204">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Mensaje de validación de título modificado](uploading-files/_static/browser3.png)

<span data-ttu-id="e3b02-206">Cuando se cargan una o más programaciones, la sección **Programaciones cargadas** presenta las programaciones cargadas:</span><span class="sxs-lookup"><span data-stu-id="e3b02-206">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabla de programaciones cargadas que muestra el título de cada programación, la fecha de carga en UTC, el tamaño de archivo de versión pública y privada](uploading-files/_static/browser4.png)

<span data-ttu-id="e3b02-208">El usuario puede hacer clic en el vínculo **Eliminar** desde allí para llegar a la vista de confirmación de eliminación, donde tiene una oportunidad de confirmar o cancelar la operación de eliminación.</span><span class="sxs-lookup"><span data-stu-id="e3b02-208">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="e3b02-209">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="e3b02-209">Troubleshooting</span></span>

<span data-ttu-id="e3b02-210">Para más información de solución de problemas de carga de `IFormFile`, vea [Cargas de archivos en ASP.NET Core: Solución de problemas](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="e3b02-210">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="e3b02-211">Gracias por seguir esta introducción a las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="e3b02-211">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="e3b02-212">Le agradeceremos cualquier comentario.</span><span class="sxs-lookup"><span data-stu-id="e3b02-212">We appreciate feedback.</span></span> <span data-ttu-id="e3b02-213">[Introducción a MVC y EF Core](xref:data/ef-mvc/intro) es un excelente seguimiento de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="e3b02-213">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e3b02-214">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e3b02-214">Additional resources</span></span>

* [<span data-ttu-id="e3b02-215">Cargas de archivos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3b02-215">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="e3b02-216">IFormFile</span><span class="sxs-lookup"><span data-stu-id="e3b02-216">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="e3b02-217">Anterior: Validación</span><span class="sxs-lookup"><span data-stu-id="e3b02-217">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
