---
title: Cargas de archivos en ASP.NET Core
author: ardalis
description: "Cómo usar el enlace de modelos y la transmisión por secuencias para cargar archivos en MVC de ASP.NET Core."
keywords: "ASP.NET Core, carga de archivos de modelo de enlace, IFormFile, transmisión por secuencias"
ms.author: riande
manager: wpickett
ms.date: 7/5/2017
ms.topic: article
ms.assetid: ebc98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/file-uploads
ms.openlocfilehash: 3d42fd0657bcfb4b0fdab699bbcb572e5736688c
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="3f37c-104">Cargas de archivos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f37c-104">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="3f37c-105">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="3f37c-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="3f37c-106">Acciones de ASP.NET MVC admiten la carga de uno o más archivos con un modelo simple de enlace de archivos más pequeños o transmisión por secuencias para archivos de mayor tamaño.</span><span class="sxs-lookup"><span data-stu-id="3f37c-106">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

[<span data-ttu-id="3f37c-107">Ver o descargar el ejemplo desde GitHub</span><span class="sxs-lookup"><span data-stu-id="3f37c-107">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="3f37c-108">Cargar archivos pequeños con el enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="3f37c-108">Uploading small files with model binding</span></span>

<span data-ttu-id="3f37c-109">Para cargar archivos pequeños, puede usar un formulario HTML varias partes o construir una solicitud POST con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3f37c-109">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="3f37c-110">Un formulario de ejemplo con Razor, que es compatible con varios archivos cargados, se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="3f37c-110">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

<span data-ttu-id="3f37c-111">Para admitir cargas de archivos, deben especificar formularios HTML un `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="3f37c-111">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="3f37c-112">El `files` entrada elemento mostrado anteriormente admite la carga de varios archivos.</span><span class="sxs-lookup"><span data-stu-id="3f37c-112">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="3f37c-113">Omitir la `multiple` atributo de este elemento de entrada para permitir que solo un único archivo que se cargan.</span><span class="sxs-lookup"><span data-stu-id="3f37c-113">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="3f37c-114">El marcado anterior, se representa en un explorador como:</span><span class="sxs-lookup"><span data-stu-id="3f37c-114">The above markup renders in a browser as:</span></span>

![Formulario de carga de archivo](file-uploads/_static/upload-form.png)

<span data-ttu-id="3f37c-116">Pueden tener acceso a los archivos individuales que se cargan en el servidor a través de [enlace de modelos](xref:mvc/models/model-binding) mediante la [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interfaz.</span><span class="sxs-lookup"><span data-stu-id="3f37c-116">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="3f37c-117">`IFormFile`tiene la estructura siguiente:</span><span class="sxs-lookup"><span data-stu-id="3f37c-117">`IFormFile` has the following structure:</span></span>

```csharp
public interface IFormFile
{
    string ContentType { get; }
    string ContentDisposition { get; }
    IHeaderDictionary Headers { get; }
    long Length { get; }
    string Name { get; }
    string FileName { get; }
    Stream OpenReadStream();
    void CopyTo(Stream target);
    Task CopyToAsync(Stream target, CancellationToken cancellationToken = null);
}
```

> [!WARNING]
> <span data-ttu-id="3f37c-118">No se basan en o confiar en el `FileName` propiedad sin validación.</span><span class="sxs-lookup"><span data-stu-id="3f37c-118">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="3f37c-119">El `FileName` propiedad debe utilizarse solo para fines de presentación.</span><span class="sxs-lookup"><span data-stu-id="3f37c-119">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="3f37c-120">Al cargar archivos con el enlace de modelos y la `IFormFile` interfaz, el método de acción puede aceptar un único `IFormFile` o un `IEnumerable<IFormFile>` (o `List<IFormFile>`) que representa varios archivos.</span><span class="sxs-lookup"><span data-stu-id="3f37c-120">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="3f37c-121">En el ejemplo siguiente se recorre en iteración uno o varios de los archivos cargados, grabarlos en el sistema de archivos local y devuelve el número total y el tamaño de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="3f37c-121">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

<span data-ttu-id="3f37c-122">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3f37c-122">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]</span></span>

<span data-ttu-id="3f37c-123">Archivos cargados mediante la `IFormFile` técnica se almacenan en búfer en memoria o en disco en el servidor web antes de ser procesados.</span><span class="sxs-lookup"><span data-stu-id="3f37c-123">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="3f37c-124">Dentro del método de acción, el `IFormFile` contenido es accesible como una secuencia.</span><span class="sxs-lookup"><span data-stu-id="3f37c-124">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="3f37c-125">Además del sistema de archivos local, los archivos se pueden transmitir a [almacenamiento de blobs de Azure](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) o [Entity Framework](https://docs.microsoft.com/ef/core/index).</span><span class="sxs-lookup"><span data-stu-id="3f37c-125">In addition to the local file system, files can be streamed to [Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) or [Entity Framework](https://docs.microsoft.com/ef/core/index).</span></span>

<span data-ttu-id="3f37c-126">Para almacenar datos de archivo binario en una base de datos mediante Entity Framework, definir una propiedad de tipo `byte[]` en la entidad:</span><span class="sxs-lookup"><span data-stu-id="3f37c-126">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="3f37c-127">Especifique una propiedad ViewMode de tipo `IFormFile`:</span><span class="sxs-lookup"><span data-stu-id="3f37c-127">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="3f37c-128">`IFormFile`puede usarse directamente como un parámetro de método de acción o como una propiedad de modelo de vista, como se indicó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="3f37c-128">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="3f37c-129">Copia la `IFormFile` en una secuencia y guarde el archivo en la matriz de bytes:</span><span class="sxs-lookup"><span data-stu-id="3f37c-129">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

```csharp
// POST: /Account/Register
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Register(RegisterViewModel model)
{
    ViewData["ReturnUrl"] = returnUrl;
    if  (ModelState.IsValid)
    {
        var user = new ApplicationUser {
          UserName = model.Email,
          Email = model.Email
        };
        using (var memoryStream = new MemoryStream())
        {
            await model.AvatarImage.CopyToAsync(memoryStream);
            user.AvatarImage = memoryStream.ToArray();
        }
    // additional logic omitted
    
    // Don't rely on or trust the model.AvatarImage.FileName property 
    // without validation.
}
```

> [!NOTE]
> <span data-ttu-id="3f37c-130">Tenga cuidado al almacenar los datos binarios en bases de datos relacionales, tal y como puede ver afectado adversamente rendimiento.</span><span class="sxs-lookup"><span data-stu-id="3f37c-130">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="3f37c-131">Cargar archivos grandes con transmisión por secuencias</span><span class="sxs-lookup"><span data-stu-id="3f37c-131">Uploading large files with streaming</span></span>

<span data-ttu-id="3f37c-132">Si el tamaño o la frecuencia de cargas de archivos está causando problemas de recursos de la aplicación, tenga en cuenta la carga de archivos de transmisión por secuencias en lugar de almacenarla en búfer en su totalidad, igual que el enfoque de enlace de modelo mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="3f37c-132">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="3f37c-133">Al usar `IFormFile` y enlace de modelos es una solución más sencilla mucho, transmisión por secuencias requiere un número de pasos necesarios para implementar correctamente.</span><span class="sxs-lookup"><span data-stu-id="3f37c-133">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="3f37c-134">Cualquier archivo almacenado en búfer único superior a 64KB se moverán de la RAM a un archivo temporal en disco en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3f37c-134">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="3f37c-135">Los recursos (disco RAM) utilizados por cargas de archivos dependen del número y tamaño de carga de archivos simultáneas.</span><span class="sxs-lookup"><span data-stu-id="3f37c-135">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="3f37c-136">Transmisión por secuencias no es gran parte sobre rendimiento, sobre la escala.</span><span class="sxs-lookup"><span data-stu-id="3f37c-136">Streaming is not so much about perf, it's about scale.</span></span> <span data-ttu-id="3f37c-137">Si se intenta almacenar en búfer demasiadas cargas, el sitio se bloqueará cuando se quede sin memoria o espacio en disco.</span><span class="sxs-lookup"><span data-stu-id="3f37c-137">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="3f37c-138">En el ejemplo siguiente se muestra cómo utilizar JavaScript/Angular desea para transmitir a una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="3f37c-138">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="3f37c-139">Token antiforgery del archivo se genera utilizando un atributo de filtro personalizado y pasa en encabezados HTTP en lugar de en el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="3f37c-139">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="3f37c-140">Puesto que el método de acción procesa los datos cargados directamente, el enlace de modelos está deshabilitado por otro filtro.</span><span class="sxs-lookup"><span data-stu-id="3f37c-140">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="3f37c-141">Dentro de la acción, el contenido del formulario se lee utilizando una `MultipartReader`, que lee cada persona `MultipartSection`, procesar el archivo o almacenar el contenido según corresponda.</span><span class="sxs-lookup"><span data-stu-id="3f37c-141">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="3f37c-142">Una vez que se han leído todas las secciones, la acción realiza su propio enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="3f37c-142">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="3f37c-143">La acción inicial se carga el formulario y se guarda un token antiforgery en una cookie (a través de la `GenerateAntiforgeryTokenCookieForAjax` atributo):</span><span class="sxs-lookup"><span data-stu-id="3f37c-143">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="3f37c-144">El atributo usa integradas de ASP.NET Core [Antiforgery](xref:security/anti-request-forgery) soporte técnico para establecer una cookie con un token de solicitud:</span><span class="sxs-lookup"><span data-stu-id="3f37c-144">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

<span data-ttu-id="3f37c-145">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3f37c-145">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]</span></span>

<span data-ttu-id="3f37c-146">Angular pasa automáticamente un token antiforgery en un encabezado de solicitud denominado `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="3f37c-146">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="3f37c-147">La aplicación de MVC de ASP.NET Core está configurada para hacer referencia a este encabezado en su configuración en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f37c-147">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

<span data-ttu-id="3f37c-148">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3f37c-148">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]</span></span>

<span data-ttu-id="3f37c-149">El `DisableFormValueModelBinding` atributo, se muestra a continuación, se utiliza para deshabilitar el enlace de modelos de la `Upload` método de acción.</span><span class="sxs-lookup"><span data-stu-id="3f37c-149">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

<span data-ttu-id="3f37c-150">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3f37c-150">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]</span></span>

<span data-ttu-id="3f37c-151">Puesto que está deshabilitado el enlace de modelo, el `Upload` método de acción no acepta parámetros.</span><span class="sxs-lookup"><span data-stu-id="3f37c-151">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="3f37c-152">Funciona directamente con el `Request` propiedad de `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="3f37c-152">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="3f37c-153">Un `MultipartReader` se usa para leer cada sección.</span><span class="sxs-lookup"><span data-stu-id="3f37c-153">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="3f37c-154">El archivo se guarda con un nombre de archivo GUID y los datos de clave/valor se almacena en un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="3f37c-154">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="3f37c-155">Una vez que todas las secciones se han leído, el contenido de la `KeyValueAccumulator` se utilizan para enlazar los datos del formulario a un tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="3f37c-155">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="3f37c-156">La sección completa `Upload` método se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="3f37c-156">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

<span data-ttu-id="3f37c-157">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3f37c-157">[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="3f37c-158">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="3f37c-158">Troubleshooting</span></span>

<span data-ttu-id="3f37c-159">A continuación se muestran algunos problemas comunes que se producen al trabajar con la carga de archivos y sus posibles soluciones.</span><span class="sxs-lookup"><span data-stu-id="3f37c-159">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="3f37c-160">Error inesperado no se encontró con IIS</span><span class="sxs-lookup"><span data-stu-id="3f37c-160">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="3f37c-161">El siguiente error indica que la carga de archivos supera el servidor configurado `maxAllowedContentLength`:</span><span class="sxs-lookup"><span data-stu-id="3f37c-161">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="3f37c-162">El valor predeterminado es `30000000`, que es aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="3f37c-162">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="3f37c-163">El valor puede personalizarse mediante la edición de *web.config*:</span><span class="sxs-lookup"><span data-stu-id="3f37c-163">The value can be customized by editing *web.config*:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- This will handle requests up to 50MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="3f37c-164">Esta configuración solo se aplica a IIS.</span><span class="sxs-lookup"><span data-stu-id="3f37c-164">This setting only applies to IIS.</span></span> <span data-ttu-id="3f37c-165">El comportamiento no se produce de forma predeterminada cuando se hospedan en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f37c-165">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="3f37c-166">Para obtener más información, consulte [límites de las solicitudes \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="3f37c-166">For more information, see [Request Limits \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="3f37c-167">Excepción de referencia nula con IFormFile</span><span class="sxs-lookup"><span data-stu-id="3f37c-167">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="3f37c-168">Si el controlador es aceptación cargar archivos con `IFormFile` pero se encuentra que el valor siempre es null, confirme que el formulario HTML está especificando una `enctype` valo `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="3f37c-168">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="3f37c-169">Si este atributo no está establecido en el `<form>` elemento, no se realizará la carga de archivos y cualquier límite `IFormFile` argumentos será nulos.</span><span class="sxs-lookup"><span data-stu-id="3f37c-169">If this attribute is not set on the `<form>` element, the file upload will not occur and any bound `IFormFile` arguments will be null.</span></span>
