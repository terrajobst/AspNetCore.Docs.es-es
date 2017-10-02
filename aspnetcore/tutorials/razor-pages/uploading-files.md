---
title: "Carga de archivos en una página de Razor en ASP.NET Core"
author: guardrex
description: "Aprenda a cargar archivos en una página de Razor."
keywords: "ASP.NET Core,Razor,páginas de Razor,IFormFile,carga de archivos,fileupload"
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 5a3dc302186c7fd0a5730bc2c7599676fb543ba7
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="a2292-104">Carga de archivos en una página de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2292-104">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="a2292-105">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a2292-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a2292-106">En esta sección se muestra cómo cargar archivos con una página de Razor.</span><span class="sxs-lookup"><span data-stu-id="a2292-106">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="a2292-107">La [aplicación de ejemplo Razor Pages Movie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) de este tutorial usa el enlace de modelo simple para cargar archivos, lo que funciona bien para cargar archivos pequeños.</span><span class="sxs-lookup"><span data-stu-id="a2292-107">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="a2292-108">Para más información sobre la transmisión de archivos de gran tamaño, vea [Uploading large files with streaming (Carga de archivos grandes mediante transmisión)](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="a2292-108">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="a2292-109">En los pasos siguientes se agrega una característica de carga de archivo de programación de película a la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a2292-109">In the steps below, you add a movie schedule file upload feature to the sample app.</span></span> <span data-ttu-id="a2292-110">Una programación de película está representada por una clase `Schedule`.</span><span class="sxs-lookup"><span data-stu-id="a2292-110">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="a2292-111">La clase incluye dos versiones de la programación.</span><span class="sxs-lookup"><span data-stu-id="a2292-111">The class includes two versions of the schedule.</span></span> <span data-ttu-id="a2292-112">Una versión se proporciona a los clientes, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="a2292-112">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="a2292-113">La otra se usa para los empleados de la empresa, `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="a2292-113">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="a2292-114">Cada versión se carga como un archivo independiente.</span><span class="sxs-lookup"><span data-stu-id="a2292-114">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="a2292-115">El tutorial muestra cómo realizar dos cargas de archivos desde una página con un solo elemento POST en el servidor.</span><span class="sxs-lookup"><span data-stu-id="a2292-115">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="a2292-116">Agregar un método auxiliar para cargar archivos</span><span class="sxs-lookup"><span data-stu-id="a2292-116">Add a helper method to upload files</span></span>

<span data-ttu-id="a2292-117">Para evitar la duplicación de código para el procesamiento de archivos de programación cargados, primero agregue un método auxiliar estático.</span><span class="sxs-lookup"><span data-stu-id="a2292-117">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="a2292-118">Cree una carpeta *Utilities* en la aplicación y agregue un archivo *FileHelpers.cs* con el siguiente contenido.</span><span class="sxs-lookup"><span data-stu-id="a2292-118">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="a2292-119">El método auxiliar, `ProcessFormFile`, toma un elemento [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) y [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) y devuelve una cadena con el contenido y el tamaño del archivo.</span><span class="sxs-lookup"><span data-stu-id="a2292-119">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="a2292-120">Se comprueban el tipo de contenido y la longitud.</span><span class="sxs-lookup"><span data-stu-id="a2292-120">The content type and length are checked.</span></span> <span data-ttu-id="a2292-121">Si el archivo no pasa una comprobación de validación, se agrega un error a `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="a2292-121">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a><span data-ttu-id="a2292-122">Adición de la clase Schedule</span><span class="sxs-lookup"><span data-stu-id="a2292-122">Add the Schedule class</span></span>

<span data-ttu-id="a2292-123">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="a2292-123">Right click the *Models* folder.</span></span> <span data-ttu-id="a2292-124">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="a2292-124">Select **Add** > **Class**.</span></span> <span data-ttu-id="a2292-125">Asigne a la clase el nombre **Schedule** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="a2292-125">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="a2292-126">La clase usa los atributos `Display` y `DisplayFormat`, que generan títulos descriptivos y formato cuando se representan los datos de programación.</span><span class="sxs-lookup"><span data-stu-id="a2292-126">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="a2292-127">Actualización de MovieContext</span><span class="sxs-lookup"><span data-stu-id="a2292-127">Update the MovieContext</span></span>

<span data-ttu-id="a2292-128">Especifique un elemento `DbSet` en `MovieContext` (*Models/MovieContext.cs*) para las programaciones y agregue una línea al método `OnModelCreating` que establezca un nombre de tabla de base de datos singular (`Schedule`) para la propiedad `DbSet`:</span><span class="sxs-lookup"><span data-stu-id="a2292-128">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules and add a line to the `OnModelCreating` method that sets a singular database table name (`Schedule`) for the `DbSet` property:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13,18)]

<span data-ttu-id="a2292-129">Nota: Si no invalida `OnModelCreating` para usar nombres de tabla singulares, Entity Framework da por hecho que está usando nombres de tabla de base de datos plurales (por ejemplo, `Movies` y `Schedules`).</span><span class="sxs-lookup"><span data-stu-id="a2292-129">Note: If you don't override `OnModelCreating` to use singular table names, Entity Framework assumes that you're using plural database table names (for example, `Movies` and `Schedules`).</span></span> <span data-ttu-id="a2292-130">Los desarrolladores están en desacuerdo sobre si los nombres de tabla deben ser plurales o no.</span><span class="sxs-lookup"><span data-stu-id="a2292-130">Developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="a2292-131">Configure `MovieContext` y la base de datos del mismo modo.</span><span class="sxs-lookup"><span data-stu-id="a2292-131">Configure the `MovieContext` and the database the same way.</span></span> <span data-ttu-id="a2292-132">Use nombres de tabla de base de datos singulares o plurales en ambos lugares.</span><span class="sxs-lookup"><span data-stu-id="a2292-132">Either use singular or pluralized database table names in both places.</span></span>

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="a2292-133">Adición de la tabla Schedule a la base de datos</span><span class="sxs-lookup"><span data-stu-id="a2292-133">Add the Schedule table to the database</span></span>

<span data-ttu-id="a2292-134">Abra la Consola del Administrador de paquetes (PMC): **Herramientas** > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="a2292-134">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="a2292-136">En la PMC, ejecute los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="a2292-136">In the PMC, execute the following commands.</span></span> <span data-ttu-id="a2292-137">Estos comandos agregan una tabla `Schedule` a la base de datos:</span><span class="sxs-lookup"><span data-stu-id="a2292-137">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-fileupload-class"></a><span data-ttu-id="a2292-138">Adición de una clase FileUpload</span><span class="sxs-lookup"><span data-stu-id="a2292-138">Add a FileUpload class</span></span>

<span data-ttu-id="a2292-139">Luego agregue una clase `FileUpload`, que está enlazada a la página para obtener los datos de programación.</span><span class="sxs-lookup"><span data-stu-id="a2292-139">Next, add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="a2292-140">Haga clic con el botón derecho en la carpeta *Models*.</span><span class="sxs-lookup"><span data-stu-id="a2292-140">Right click the *Models* folder.</span></span> <span data-ttu-id="a2292-141">Seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="a2292-141">Select **Add** > **Class**.</span></span> <span data-ttu-id="a2292-142">Asigne a la clase el nombre **FileUpload** y agregue las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="a2292-142">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="a2292-143">La clase tiene una propiedad para el título de la programación y otra para cada una de las dos versiones de la programación.</span><span class="sxs-lookup"><span data-stu-id="a2292-143">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="a2292-144">Las tres propiedades son necesarias y el título debe tener entre 3 y 60 caracteres.</span><span class="sxs-lookup"><span data-stu-id="a2292-144">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="a2292-145">Adición de una página de Razor de carga de archivos</span><span class="sxs-lookup"><span data-stu-id="a2292-145">Add a file upload Razor Page</span></span>

<span data-ttu-id="a2292-146">En la carpeta *Pages*, cree una carpeta *Schedules*.</span><span class="sxs-lookup"><span data-stu-id="a2292-146">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="a2292-147">En la carpeta *Schedules*, cree una página denominada *Index.cshtml* para cargar una programación con el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="a2292-147">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="a2292-148">Cada grupo del formulario incluye una **\<etiqueta>** que muestra el nombre de cada propiedad de clase.</span><span class="sxs-lookup"><span data-stu-id="a2292-148">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="a2292-149">Los atributos `Display` del modelo `FileUpload` proporcionan los valores de presentación de las etiquetas.</span><span class="sxs-lookup"><span data-stu-id="a2292-149">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="a2292-150">Por ejemplo, el nombre para mostrar de la propiedad `UploadPublicSchedule` se establece con `[Display(Name="Public Schedule")]` y, por tanto, muestra "Programación pública" en la etiqueta cuando se presenta el formulario.</span><span class="sxs-lookup"><span data-stu-id="a2292-150">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="a2292-151">Cada grupo del formulario incluye un **\<intervalo>** de validación.</span><span class="sxs-lookup"><span data-stu-id="a2292-151">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="a2292-152">Si la entrada del usuario no cumple los atributos de propiedad establecidos en la clase `FileUpload` o si se produce un error en alguna de las comprobaciones de validación del archivo del método `ProcessFormFile`, no se valida el modelo.</span><span class="sxs-lookup"><span data-stu-id="a2292-152">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="a2292-153">Cuando se produce un error en la validación del modelo, se presenta un útil mensaje de validación al usuario.</span><span class="sxs-lookup"><span data-stu-id="a2292-153">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="a2292-154">Por ejemplo, la propiedad `Title` se anota con `[Required]` y `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="a2292-154">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="a2292-155">Si el usuario no proporciona un título, recibe un mensaje que indica que se necesita un valor.</span><span class="sxs-lookup"><span data-stu-id="a2292-155">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="a2292-156">Si el usuario especifica un valor de menos de tres caracteres o de más de sesenta caracteres, recibe un mensaje que indica que el valor tiene una longitud incorrecta.</span><span class="sxs-lookup"><span data-stu-id="a2292-156">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="a2292-157">Si se proporciona un archivo sin contenido, aparece un mensaje que indica que el archivo está vacío.</span><span class="sxs-lookup"><span data-stu-id="a2292-157">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-code-behind-file"></a><span data-ttu-id="a2292-158">Adición del archivo de código subyacente</span><span class="sxs-lookup"><span data-stu-id="a2292-158">Add the code-behind file</span></span>

<span data-ttu-id="a2292-159">Agregue el archivo de código subyacente (*Index.cshtml.cs*) a la carpeta *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="a2292-159">Add the code-behind file (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="a2292-160">El modelo de página (`IndexModel` en *Index.cshtml.cs*) enlaza la clase `FileUpload`:</span><span class="sxs-lookup"><span data-stu-id="a2292-160">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="a2292-161">El modelo además usa una lista de las programaciones (`IList<Schedule>`) para mostrar las programaciones almacenadas en la base de datos en la página:</span><span class="sxs-lookup"><span data-stu-id="a2292-161">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="a2292-162">Cuando se carga la página con `OnGetAsync`, `Schedules` se rellena a partir de la base de datos y se usa para generar una tabla HTML de programaciones cargadas:</span><span class="sxs-lookup"><span data-stu-id="a2292-162">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="a2292-163">Cuando el formulario se publica en el servidor, se activa `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="a2292-163">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="a2292-164">Si no es válido, `Schedules` se vuelve a generar y la página se presenta con uno o más mensajes de validación que indican el motivo del error de validación de la página.</span><span class="sxs-lookup"><span data-stu-id="a2292-164">If invalid, `Schedules` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="a2292-165">Si es válido, las propiedades `FileUpload` se usan en *OnPostAsync* para completar la carga de archivos para las dos versiones de la programación y para crear un nuevo objeto `Schedule` para almacenar los datos.</span><span class="sxs-lookup"><span data-stu-id="a2292-165">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="a2292-166">La programación luego se guarda en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="a2292-166">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="a2292-167">Vinculación de una página de Razor de carga de archivos</span><span class="sxs-lookup"><span data-stu-id="a2292-167">Link the file upload Razor Page</span></span>

<span data-ttu-id="a2292-168">Abra *_Layout.cshtml* y agregue un vínculo a la barra de navegación para llegar a la página de carga de archivos:</span><span class="sxs-lookup"><span data-stu-id="a2292-168">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="a2292-169">Adición de una página para confirmar la eliminación de la programación</span><span class="sxs-lookup"><span data-stu-id="a2292-169">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="a2292-170">Cuando el usuario hace clic para eliminar una programación, se recomienda ofrecerle una oportunidad de cancelar la operación.</span><span class="sxs-lookup"><span data-stu-id="a2292-170">When the user clicks to delete a schedule, you want them to have a chance to cancel the operation.</span></span> <span data-ttu-id="a2292-171">Agregue una página de confirmación de eliminación (*Delete.cshtml*) a la carpeta *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="a2292-171">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="a2292-172">El archivo de código subyacente (*Delete.cshtml.cs*) carga una sola programación identificada por `id` en los datos de ruta de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="a2292-172">The code-behind file (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="a2292-173">Agregue el archivo *Delete.cshtml.cs* a la carpeta *Schedules*:</span><span class="sxs-lookup"><span data-stu-id="a2292-173">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="a2292-174">El método `OnPostAsync` controla la eliminación de la programación mediante su `id`:</span><span class="sxs-lookup"><span data-stu-id="a2292-174">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="a2292-175">Después de eliminar correctamente la programación, `RedirectToPage` vuelve a enviar al usuario a la página de programaciones *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a2292-175">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="a2292-176">Página de Razor Schedules activa</span><span class="sxs-lookup"><span data-stu-id="a2292-176">The working Schedules Razor Page</span></span>

<span data-ttu-id="a2292-177">Cuando se carga la página, las etiquetas y las entradas del título de la programación, la programación pública y la privada se presentan con un botón de envío:</span><span class="sxs-lookup"><span data-stu-id="a2292-177">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Página de Razor Schedules tal como se muestra en la carga inicial sin errores de validación ni campos vacíos](uploading-files/_static/browser1.png)

<span data-ttu-id="a2292-179">La selección del botón **Cargar** sin rellenar ninguno de los campos infringe los atributos `[Required]` en el modelo.</span><span class="sxs-lookup"><span data-stu-id="a2292-179">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="a2292-180">`ModelState` no es válido.</span><span class="sxs-lookup"><span data-stu-id="a2292-180">The `ModelState` is invalid.</span></span> <span data-ttu-id="a2292-181">Los mensajes de error de validación se muestran al usuario:</span><span class="sxs-lookup"><span data-stu-id="a2292-181">The validation error messages are displayed to the user:</span></span>

![Los mensajes de error de validación aparecen junto a cada control de entrada](uploading-files/_static/browser2.png)

<span data-ttu-id="a2292-183">Escriba dos letras en el campo **Título**.</span><span class="sxs-lookup"><span data-stu-id="a2292-183">Type two letters into the **Title** field.</span></span> <span data-ttu-id="a2292-184">El mensaje de validación cambia para indicar que el título debe tener entre 3 y 60 caracteres:</span><span class="sxs-lookup"><span data-stu-id="a2292-184">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Mensaje de validación de título modificado](uploading-files/_static/browser3.png)

<span data-ttu-id="a2292-186">Cuando se cargan una o más programaciones, la sección **Programaciones cargadas** presenta las programaciones cargadas:</span><span class="sxs-lookup"><span data-stu-id="a2292-186">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabla de programaciones cargadas que muestra el título de cada programación, la fecha de carga en UTC, el tamaño de archivo de versión pública y privada](uploading-files/_static/browser4.png)

<span data-ttu-id="a2292-188">El usuario puede hacer clic en el vínculo **Eliminar** desde allí para llegar a la vista de confirmación de eliminación, donde tiene una oportunidad de confirmar o cancelar la operación de eliminación.</span><span class="sxs-lookup"><span data-stu-id="a2292-188">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a2292-189">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="a2292-189">Troubleshooting</span></span>

<span data-ttu-id="a2292-190">Para más información de solución de problemas de carga de `IFormFile`, vea [Cargas de archivos en ASP.NET Core: Solución de problemas](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="a2292-190">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="a2292-191">Gracias por seguir esta introducción a las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="a2292-191">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="a2292-192">Le agradeceremos todos los comentarios que quiera hacernos.</span><span class="sxs-lookup"><span data-stu-id="a2292-192">We appreciate any comments you leave.</span></span> <span data-ttu-id="a2292-193">[Introducción a MVC y EF Core](xref:data/ef-mvc/intro) es un excelente seguimiento de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="a2292-193">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a2292-194">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a2292-194">Additional resources</span></span>

* [<span data-ttu-id="a2292-195">Cargas de archivos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2292-195">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="a2292-196">IFormFile</span><span class="sxs-lookup"><span data-stu-id="a2292-196">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="a2292-197">Anterior: Validación</span><span class="sxs-lookup"><span data-stu-id="a2292-197">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
