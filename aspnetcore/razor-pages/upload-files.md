---
title: Cargar archivos en una página de Razor en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo cargar archivos en una página de Razor en ASP.NET Core mediante la clase FileUpload.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 11/10/2018
ms.custom: mvc, seodec18
uid: razor-pages/upload-files
ms.openlocfilehash: a12653e5dc99ad731de63e151af6dbdbe38ffefc
ms.sourcegitcommit: 8a84ce880b4c40d6694ba6423038f18fc2eb5746
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60165227"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="48ee4-103">Cargar archivos en una página de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48ee4-103">Upload files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="48ee4-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="48ee4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="48ee4-105">En este tema se basa en el [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) en <xref:tutorials/razor-pages/razor-pages-start>.</span><span class="sxs-lookup"><span data-stu-id="48ee4-105">This topic builds upon the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) in <xref:tutorials/razor-pages/razor-pages-start>.</span></span>

<span data-ttu-id="48ee4-106">En este tema se muestra cómo utilizar el enlace modelo simple para cargar archivos, que funciona bien para cargar archivos pequeños.</span><span class="sxs-lookup"><span data-stu-id="48ee4-106">This topic shows how to use simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="48ee4-107">Para más información sobre la transmisión de archivos de gran tamaño, vea [Uploading large files with streaming (Carga de archivos grandes mediante transmisión)](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="48ee4-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="48ee4-108">En los pasos siguientes se agrega a la aplicación de ejemplo una característica de carga de archivo de programación de película.</span><span class="sxs-lookup"><span data-stu-id="48ee4-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="48ee4-109">Una programación de película está representada por una clase `Schedule`.</span><span class="sxs-lookup"><span data-stu-id="48ee4-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="48ee4-110">La clase incluye dos versiones de la programación.</span><span class="sxs-lookup"><span data-stu-id="48ee4-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="48ee4-111">Una versión se proporciona a los clientes, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="48ee4-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="48ee4-112">La otra se usa para los empleados de la empresa, `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="48ee4-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="48ee4-113">Cada versión se carga como un archivo independiente.</span><span class="sxs-lookup"><span data-stu-id="48ee4-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="48ee4-114">El tutorial muestra cómo realizar dos cargas de archivos desde una página con un solo elemento POST en el servidor.</span><span class="sxs-lookup"><span data-stu-id="48ee4-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

<span data-ttu-id="48ee4-115">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/upload-files/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="48ee4-115">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/upload-files/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="48ee4-116">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="48ee4-116">Security considerations</span></span>

<span data-ttu-id="48ee4-117">Debe tener precaución al proporcionar a los usuarios la capacidad de cargar archivos en un servidor,</span><span class="sxs-lookup"><span data-stu-id="48ee4-117">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="48ee4-118">ya que los atacantes podrían ejecutar un ataque por [denegación de servicio](/windows-hardware/drivers/ifs/denial-of-service) u otros ataques en un sistema.</span><span class="sxs-lookup"><span data-stu-id="48ee4-118">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="48ee4-119">A continuación se muestran algunos pasos de seguridad con los que se reduce la probabilidad de sufrir ataques:</span><span class="sxs-lookup"><span data-stu-id="48ee4-119">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="48ee4-120">Cargue archivos en un área de carga de archivos específica del sistema, con lo que resulta más fácil imponer medidas de seguridad en el contenido cargado.</span><span class="sxs-lookup"><span data-stu-id="48ee4-120">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="48ee4-121">Al permitir las cargas de archivos, asegúrese de que los permisos de ejecución están deshabilitados en la ubicación de carga.</span><span class="sxs-lookup"><span data-stu-id="48ee4-121">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="48ee4-122">Use un nombre de archivo seguro determinado por la aplicación, y no por la entrada del usuario o el nombre de archivo del archivo cargado.</span><span class="sxs-lookup"><span data-stu-id="48ee4-122">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="48ee4-123">Permita únicamente un conjunto específico de extensiones de archivo aprobadas.</span><span class="sxs-lookup"><span data-stu-id="48ee4-123">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="48ee4-124">Compruebe que se llevan a cabo comprobaciones de cliente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="48ee4-124">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="48ee4-125">Las comprobaciones de cliente son fáciles de sortear.</span><span class="sxs-lookup"><span data-stu-id="48ee4-125">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="48ee4-126">Compruebe el tamaño de la carga y evite las cargas de mayor tamaño de lo esperado.</span><span class="sxs-lookup"><span data-stu-id="48ee4-126">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="48ee4-127">Ejecute un escáner de virus/malware en el contenido cargado.</span><span class="sxs-lookup"><span data-stu-id="48ee4-127">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="48ee4-128">La carga de código malintencionado en un sistema suele ser el primer paso para ejecutar código que puede:</span><span class="sxs-lookup"><span data-stu-id="48ee4-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="48ee4-129">Tomar todo el poder en un sistema.</span><span class="sxs-lookup"><span data-stu-id="48ee4-129">Completely takeover a system.</span></span>
> * <span data-ttu-id="48ee4-130">Sobrecargar un sistema de manera que se producen errores generales del sistema.</span><span class="sxs-lookup"><span data-stu-id="48ee4-130">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="48ee4-131">Poner en peligro los datos del usuario o del sistema.</span><span class="sxs-lookup"><span data-stu-id="48ee4-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="48ee4-132">Aplicar grafitis a una interfaz pública.</span><span class="sxs-lookup"><span data-stu-id="48ee4-132">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="48ee4-133">Adición de una clase FileUpload</span><span class="sxs-lookup"><span data-stu-id="48ee4-133">Add a FileUpload class</span></span>

<span data-ttu-id="48ee4-134">Cree una página de Razor para controlar un par de cargas de archivos.</span><span class="sxs-lookup"><span data-stu-id="48ee4-134">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="48ee4-135">Agregue una clase `FileUpload`, que está enlazada a la página para obtener los datos de programación.</span><span class="sxs-lookup"><span data-stu-id="48ee4-135">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="48ee4-136">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="48ee4-136">Right click the *Models* folder.</span></span> <span data-ttu-id="48ee4-137">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="48ee4-137">Select **Add** > **Class**.</span></span> <span data-ttu-id="48ee4-138">Asigne a la clase el nombre **FileUpload** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="48ee4-138">Name the class **FileUpload** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

<span data-ttu-id="48ee4-139">La clase tiene una propiedad para el título de la programación y otra para cada una de las dos versiones de la programación.</span><span class="sxs-lookup"><span data-stu-id="48ee4-139">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="48ee4-140">Las tres propiedades son necesarias y el título debe tener entre 3 y 60 caracteres.</span><span class="sxs-lookup"><span data-stu-id="48ee4-140">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="48ee4-141">Agregar un método del asistente para cargar archivos</span><span class="sxs-lookup"><span data-stu-id="48ee4-141">Add a helper method to upload files</span></span>

<span data-ttu-id="48ee4-142">Para evitar la duplicación de código para el procesamiento de archivos de programación cargados, primero agregue un método del asistente estático.</span><span class="sxs-lookup"><span data-stu-id="48ee4-142">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="48ee4-143">Cree una carpeta *Utilities* en la aplicación y agregue un archivo *FileHelpers.cs* con el siguiente contenido.</span><span class="sxs-lookup"><span data-stu-id="48ee4-143">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="48ee4-144">El método del asistente, `ProcessFormFile`, toma un elemento [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) y [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) y devuelve una cadena con el contenido y el tamaño del archivo.</span><span class="sxs-lookup"><span data-stu-id="48ee4-144">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="48ee4-145">Se comprueban el tipo de contenido y la longitud.</span><span class="sxs-lookup"><span data-stu-id="48ee4-145">The content type and length are checked.</span></span> <span data-ttu-id="48ee4-146">Si el archivo no pasa una comprobación de validación, se agrega un error a `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="48ee4-146">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

### <a name="save-the-file-to-disk"></a><span data-ttu-id="48ee4-147">Guardar el archivo en el disco</span><span class="sxs-lookup"><span data-stu-id="48ee4-147">Save the file to disk</span></span>

<span data-ttu-id="48ee4-148">La aplicación de ejemplo guarda los archivos cargados en campos de base de datos.</span><span class="sxs-lookup"><span data-stu-id="48ee4-148">The sample app saves uploaded files into database fields.</span></span> <span data-ttu-id="48ee4-149">Para guardar un archivo en disco, use una clase [FileStream](/dotnet/api/system.io.filestream).</span><span class="sxs-lookup"><span data-stu-id="48ee4-149">To save a file to disk, use a [FileStream](/dotnet/api/system.io.filestream).</span></span> <span data-ttu-id="48ee4-150">En el siguiente ejemplo, un archivo contenido en `FileUpload.UploadPublicSchedule` se copia en una clase `FileStream` de un método `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="48ee4-150">The following example copies a file held by `FileUpload.UploadPublicSchedule` to a `FileStream` in an `OnPostAsync` method.</span></span> <span data-ttu-id="48ee4-151">`FileStream` escribe el archivo en disco en el `<PATH-AND-FILE-NAME>` proporcionado:</span><span class="sxs-lookup"><span data-stu-id="48ee4-151">The `FileStream` writes the file to disk at the `<PATH-AND-FILE-NAME>` provided:</span></span>

```csharp
public async Task<IActionResult> OnPostAsync()
{
    // Perform an initial check to catch FileUpload class attribute violations.
    if (!ModelState.IsValid)
    {
        return Page();
    }

    var filePath = "<PATH-AND-FILE-NAME>";

    using (var fileStream = new FileStream(filePath, FileMode.Create))
    {
        await FileUpload.UploadPublicSchedule.CopyToAsync(fileStream);
    }

    return RedirectToPage("./Index");
}
```

<span data-ttu-id="48ee4-152">El proceso de trabajo debe tener permisos de escritura en la ubicación especificada por `filePath`.</span><span class="sxs-lookup"><span data-stu-id="48ee4-152">The worker process must have write permissions to the location specified by `filePath`.</span></span>

> [!NOTE]
> <span data-ttu-id="48ee4-153">`filePath` *debe* incluir el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="48ee4-153">The `filePath` *must* include the file name.</span></span> <span data-ttu-id="48ee4-154">Si no se ha proporcionado un nombre de archivo, se produce una excepción [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="48ee4-154">If the file name isn't provided, an [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) is thrown at runtime.</span></span>

> [!WARNING]
> <span data-ttu-id="48ee4-155">Los archivos cargados nunca deben persistir en el mismo árbol de directorio que la aplicación.</span><span class="sxs-lookup"><span data-stu-id="48ee4-155">Never persist uploaded files in the same directory tree as the app.</span></span>
>
> <span data-ttu-id="48ee4-156">En el código de ejemplo no se proporciona ningún tipo de protección de servidor frente a cargas de archivos malintencionados.</span><span class="sxs-lookup"><span data-stu-id="48ee4-156">The code sample provides no server-side protection against malicious file uploads.</span></span> <span data-ttu-id="48ee4-157">Vea los siguientes recursos para más información sobre cómo reducir el área expuesta de ataques al aceptar archivos de los usuarios:</span><span class="sxs-lookup"><span data-stu-id="48ee4-157">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="48ee4-158">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Carga de archivos sin restricciones)</span><span class="sxs-lookup"><span data-stu-id="48ee4-158">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * [<span data-ttu-id="48ee4-159">Seguridad de Azure: Asegúrese de que los controles adecuados estén en vigor al aceptar archivos de usuarios</span><span class="sxs-lookup"><span data-stu-id="48ee4-159">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="48ee4-160">Guardar el archivo en Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="48ee4-160">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="48ee4-161">Para cargar el contenido del archivo en Azure Blob Storage, vea [Introducción a Azure Blob Storage mediante .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="48ee4-161">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="48ee4-162">En el tema se muestra cómo usar [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) para guardar una clase [FileStream](/dotnet/api/system.io.filestream) en Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="48ee4-162">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="48ee4-163">Adición de la clase Schedule</span><span class="sxs-lookup"><span data-stu-id="48ee4-163">Add the Schedule class</span></span>

<span data-ttu-id="48ee4-164">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="48ee4-164">Right click the *Models* folder.</span></span> <span data-ttu-id="48ee4-165">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="48ee4-165">Select **Add** > **Class**.</span></span> <span data-ttu-id="48ee4-166">Asigne a la clase el nombre **Schedule** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="48ee4-166">Name the class **Schedule** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

<span data-ttu-id="48ee4-167">La clase usa los atributos `Display` y `DisplayFormat`, que generan títulos descriptivos y formato cuando se representan los datos de programación.</span><span class="sxs-lookup"><span data-stu-id="48ee4-167">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="update-the-razorpagesmoviecontext"></a><span data-ttu-id="48ee4-168">Actualización de RazorPagesMovieContext</span><span class="sxs-lookup"><span data-stu-id="48ee4-168">Update the RazorPagesMovieContext</span></span>

<span data-ttu-id="48ee4-169">Especifique `DbSet` en `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) para las programaciones:</span><span class="sxs-lookup"><span data-stu-id="48ee4-169">Specify a `DbSet` in the `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Data/RazorPagesMovieContext.cs?highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="update-the-moviecontext"></a><span data-ttu-id="48ee4-170">Actualización de MovieContext</span><span class="sxs-lookup"><span data-stu-id="48ee4-170">Update the MovieContext</span></span>

<span data-ttu-id="48ee4-171">Especifique `DbSet` en `MovieContext` (*Models/MovieContext.cs*) para las programaciones:</span><span class="sxs-lookup"><span data-stu-id="48ee4-171">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

::: moniker-end

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="48ee4-172">Adición de la tabla Schedule a la base de datos</span><span class="sxs-lookup"><span data-stu-id="48ee4-172">Add the Schedule table to the database</span></span>

<span data-ttu-id="48ee4-173">Abra la consola del Administrador de paquetes (PMC): **Herramientas** > **Administrador de paquetes de NuGet** > **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="48ee4-173">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Menú de PMC](upload-files/_static/pmc.png)

<span data-ttu-id="48ee4-175">En la PMC, ejecute los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="48ee4-175">In the PMC, execute the following commands.</span></span> <span data-ttu-id="48ee4-176">Estos comandos agregan una tabla `Schedule` a la base de datos:</span><span class="sxs-lookup"><span data-stu-id="48ee4-176">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="48ee4-177">Adición de una página de Razor de carga de archivos</span><span class="sxs-lookup"><span data-stu-id="48ee4-177">Add a file upload Razor Page</span></span>

<span data-ttu-id="48ee4-178">En la carpeta *Pages*, cree una carpeta *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="48ee4-178">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="48ee4-179">En la carpeta *Schedules*, cree una página denominada *Index.cshtml* para cargar una programación con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="48ee4-179">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

<span data-ttu-id="48ee4-180">Cada grupo del formulario incluye una **\<etiqueta>** que muestra el nombre de cada propiedad de clase.</span><span class="sxs-lookup"><span data-stu-id="48ee4-180">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="48ee4-181">Los atributos `Display` del modelo `FileUpload` proporcionan los valores de presentación de las etiquetas.</span><span class="sxs-lookup"><span data-stu-id="48ee4-181">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="48ee4-182">Por ejemplo, el nombre para mostrar de la propiedad `UploadPublicSchedule` se establece con `[Display(Name="Public Schedule")]` y, por tanto, muestra "Programación pública" en la etiqueta cuando se presenta el formulario.</span><span class="sxs-lookup"><span data-stu-id="48ee4-182">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="48ee4-183">Cada grupo del formulario incluye un **\<intervalo>** de validación.</span><span class="sxs-lookup"><span data-stu-id="48ee4-183">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="48ee4-184">Si la entrada del usuario no cumple los atributos de propiedad establecidos en la clase `FileUpload` o si se produce un error en alguna de las comprobaciones de validación del archivo del método `ProcessFormFile`, no se valida el modelo.</span><span class="sxs-lookup"><span data-stu-id="48ee4-184">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="48ee4-185">Cuando se produce un error en la validación del modelo, se presenta un útil mensaje de validación al usuario.</span><span class="sxs-lookup"><span data-stu-id="48ee4-185">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="48ee4-186">Por ejemplo, la propiedad `Title` se anota con `[Required]` y `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="48ee4-186">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="48ee4-187">Si el usuario no proporciona un título, recibe un mensaje que indica que se necesita un valor.</span><span class="sxs-lookup"><span data-stu-id="48ee4-187">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="48ee4-188">Si el usuario especifica un valor de menos de tres caracteres o de más de sesenta caracteres, recibe un mensaje que indica que el valor tiene una longitud incorrecta.</span><span class="sxs-lookup"><span data-stu-id="48ee4-188">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="48ee4-189">Si se proporciona un archivo sin contenido, aparece un mensaje que indica que el archivo está vacío.</span><span class="sxs-lookup"><span data-stu-id="48ee4-189">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="48ee4-190">Agregar el modelo de página</span><span class="sxs-lookup"><span data-stu-id="48ee4-190">Add the page model</span></span>

<span data-ttu-id="48ee4-191">Agregue el modelo de página (*Index.cshtml.cs*) a la carpeta *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="48ee4-191">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

<span data-ttu-id="48ee4-192">El modelo de página (`IndexModel` en *Index.cshtml.cs*) enlaza la clase `FileUpload`:</span><span class="sxs-lookup"><span data-stu-id="48ee4-192">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="48ee4-193">El modelo además usa una lista de las programaciones (`IList<Schedule>`) para mostrar las programaciones almacenadas en la base de datos en la página:</span><span class="sxs-lookup"><span data-stu-id="48ee4-193">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="48ee4-194">Cuando se carga la página con `OnGetAsync`, `Schedules` se rellena a partir de la base de datos y se usa para generar una tabla HTML de programaciones cargadas:</span><span class="sxs-lookup"><span data-stu-id="48ee4-194">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="48ee4-195">Cuando el formulario se publica en el servidor, se activa `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="48ee4-195">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="48ee4-196">Si no es válido, `Schedule` se vuelve a generar y la página se presenta con uno o más mensajes de validación que indican el motivo del error de validación de la página.</span><span class="sxs-lookup"><span data-stu-id="48ee4-196">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="48ee4-197">Si es válido, las propiedades `FileUpload` se usan en *OnPostAsync* para completar la carga de archivos para las dos versiones de la programación y para crear un nuevo objeto `Schedule` para almacenar los datos.</span><span class="sxs-lookup"><span data-stu-id="48ee4-197">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="48ee4-198">La programación luego se guarda en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="48ee4-198">The schedule is then saved to the database:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="48ee4-199">Vinculación de una página de Razor de carga de archivos</span><span class="sxs-lookup"><span data-stu-id="48ee4-199">Link the file upload Razor Page</span></span>

<span data-ttu-id="48ee4-200">Abra *Pages/Shared/_Layout.cshtml* y agregue un vínculo a la barra de navegación para llegar a la página de programaciones:</span><span class="sxs-lookup"><span data-stu-id="48ee4-200">Open *Pages/Shared/_Layout.cshtml* and add a link to the navigation bar to reach the Schedules page:</span></span>

```cshtml
<div class="navbar-collapse collapse">
    <ul class="nav navbar-nav">
        <li><a asp-page="/Index">Home</a></li>
        <li><a asp-page="/Schedules/Index">Schedules</a></li>
        <li><a asp-page="/About">About</a></li>
        <li><a asp-page="/Contact">Contact</a></li>
    </ul>
</div>
```

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="48ee4-201">Adición de una página para confirmar la eliminación de la programación</span><span class="sxs-lookup"><span data-stu-id="48ee4-201">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="48ee4-202">Cuando el usuario hace clic para eliminar una programación, se le da la oportunidad de cancelar la operación.</span><span class="sxs-lookup"><span data-stu-id="48ee4-202">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="48ee4-203">Agregue una página de confirmación de eliminación (*Delete.cshtml*) a la carpeta *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="48ee4-203">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

<span data-ttu-id="48ee4-204">El modelo de página (*Delete.cshtml.cs*) carga una sola programación identificada por `id` en los datos de ruta de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="48ee4-204">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="48ee4-205">Agregue el archivo *Delete.cshtml.cs* a la carpeta *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="48ee4-205">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

<span data-ttu-id="48ee4-206">El método `OnPostAsync` controla la eliminación de la programación mediante su `id`:</span><span class="sxs-lookup"><span data-stu-id="48ee4-206">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

<span data-ttu-id="48ee4-207">Después de eliminar correctamente la programación, `RedirectToPage` vuelve a enviar al usuario a la página de programaciones *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="48ee4-207">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="48ee4-208">Página de Razor Schedules activa</span><span class="sxs-lookup"><span data-stu-id="48ee4-208">The working Schedules Razor Page</span></span>

<span data-ttu-id="48ee4-209">Cuando se carga la página, las etiquetas y las entradas del título de la programación, la programación pública y la privada se presentan con un botón de envío:</span><span class="sxs-lookup"><span data-stu-id="48ee4-209">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Página de Razor Schedules tal como se muestra en la carga inicial sin errores de validación ni campos vacíos](upload-files/_static/browser1.png)

<span data-ttu-id="48ee4-211">La selección del botón **Cargar** sin rellenar ninguno de los campos infringe los atributos `[Required]` en el modelo.</span><span class="sxs-lookup"><span data-stu-id="48ee4-211">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="48ee4-212">`ModelState` no es válido.</span><span class="sxs-lookup"><span data-stu-id="48ee4-212">The `ModelState` is invalid.</span></span> <span data-ttu-id="48ee4-213">Los mensajes de error de validación se muestran al usuario:</span><span class="sxs-lookup"><span data-stu-id="48ee4-213">The validation error messages are displayed to the user:</span></span>

![Los mensajes de error de validación aparecen junto a cada control de entrada](upload-files/_static/browser2.png)

<span data-ttu-id="48ee4-215">Escriba dos letras en el campo **Título**.</span><span class="sxs-lookup"><span data-stu-id="48ee4-215">Type two letters into the **Title** field.</span></span> <span data-ttu-id="48ee4-216">El mensaje de validación cambia para indicar que el título debe tener entre 3 y 60 caracteres:</span><span class="sxs-lookup"><span data-stu-id="48ee4-216">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Mensaje de validación de título modificado](upload-files/_static/browser3.png)

<span data-ttu-id="48ee4-218">Cuando se cargan una o más programaciones, la sección **Programaciones cargadas** presenta las programaciones cargadas:</span><span class="sxs-lookup"><span data-stu-id="48ee4-218">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabla de programaciones cargadas que muestra el título de cada programación, la fecha de carga en UTC, el tamaño de archivo de versión pública y privada](upload-files/_static/browser4.png)

<span data-ttu-id="48ee4-220">El usuario puede hacer clic en el vínculo **Eliminar** desde allí para llegar a la vista de confirmación de eliminación, donde tiene una oportunidad de confirmar o cancelar la operación de eliminación.</span><span class="sxs-lookup"><span data-stu-id="48ee4-220">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="48ee4-221">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="48ee4-221">Troubleshooting</span></span>

<span data-ttu-id="48ee4-222">Para obtener más información con `IFormFile` cargar, consulte [cargas de archivos en ASP.NET Core: Solución de problemas](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="48ee4-222">For troubleshooting information with `IFormFile` uploading, see [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>
