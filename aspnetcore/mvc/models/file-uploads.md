---
title: Cargas de archivos en ASP.NET Core
author: ardalis
description: Cómo usar el enlace de modelos y el streaming para cargar archivos en ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 07/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/file-uploads
ms.openlocfilehash: 7ba4f6d9e3901c310fe9fa7a70382d9243d8b347
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "32740418"
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="1789b-103">Cargas de archivos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1789b-103">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="1789b-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="1789b-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1789b-105">Entre las acciones que se pueden realizar en ASP.NET MVC está la de cargar uno o más archivos por medio de un sencillo procedimiento de enlace de modelos (archivos más pequeños) o del streaming (archivos de mayor tamaño).</span><span class="sxs-lookup"><span data-stu-id="1789b-105">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

[<span data-ttu-id="1789b-106">Ver o descargar el ejemplo desde GitHub</span><span class="sxs-lookup"><span data-stu-id="1789b-106">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="1789b-107">Cargar archivos pequeños por medio del enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="1789b-107">Uploading small files with model binding</span></span>

<span data-ttu-id="1789b-108">Para cargar archivos pequeños, se puede usar un formulario HTML de varias partes o crear una solicitud POST con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1789b-108">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="1789b-109">Este es un formulario de ejemplo en el que se usa Razor y que admite varios archivos cargados:</span><span class="sxs-lookup"><span data-stu-id="1789b-109">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

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

<span data-ttu-id="1789b-110">Para admitir las cargas de archivos, los formularios HTML deben especificar un tipo `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="1789b-110">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="1789b-111">El elemento de entrada `files` de arriba admite la carga de varios archivos.</span><span class="sxs-lookup"><span data-stu-id="1789b-111">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="1789b-112">Para admitir un único archivo cargado, solo hay que omitir el atributo `multiple` de este elemento de entrada.</span><span class="sxs-lookup"><span data-stu-id="1789b-112">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="1789b-113">El marcado anterior se muestra así en un explorador:</span><span class="sxs-lookup"><span data-stu-id="1789b-113">The above markup renders in a browser as:</span></span>

![Formulario de carga de archivos](file-uploads/_static/upload-form.png)

<span data-ttu-id="1789b-115">Se puede tener acceso a los archivos individuales cargados en el servidor a través del [enlace de modelos](xref:mvc/models/model-binding), por medio de la interfaz [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile).</span><span class="sxs-lookup"><span data-stu-id="1789b-115">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="1789b-116">`IFormFile` tiene la siguiente estructura:</span><span class="sxs-lookup"><span data-stu-id="1789b-116">`IFormFile` has the following structure:</span></span>

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
> <span data-ttu-id="1789b-117">No se base o confíe en la propiedad `FileName` sin validarla.</span><span class="sxs-lookup"><span data-stu-id="1789b-117">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="1789b-118">La propiedad `FileName` solo se debe usar con fines ilustrativos.</span><span class="sxs-lookup"><span data-stu-id="1789b-118">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="1789b-119">Al cargar archivos por medio del enlace de modelos y la interfaz `IFormFile`, el método de acción puede aceptar bien un solo `IFormFile`, bien un `IEnumerable<IFormFile>` (o `List<IFormFile>`) que represente varios archivos.</span><span class="sxs-lookup"><span data-stu-id="1789b-119">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="1789b-120">En el siguiente ejemplo se itera por uno o varios archivos cargados, estos se guardan en el sistema de archivos local y se devuelve el número total y el tamaño de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="1789b-120">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

<span data-ttu-id="1789b-121">Los archivos que se cargan usando la técnica `IFormFile` se almacenan en búfer en memoria o en disco en el servidor web antes de procesarse.</span><span class="sxs-lookup"><span data-stu-id="1789b-121">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="1789b-122">Dentro del método de acción, se puede tener acceso al contenido de `IFormFile` como una secuencia.</span><span class="sxs-lookup"><span data-stu-id="1789b-122">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="1789b-123">Aparte de al sistema de archivos local, los archivos se pueden transmitir por streaming también a [Azure Blob Storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) o a [Entity Framework](https://docs.microsoft.com/ef/core/index).</span><span class="sxs-lookup"><span data-stu-id="1789b-123">In addition to the local file system, files can be streamed to [Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) or [Entity Framework](https://docs.microsoft.com/ef/core/index).</span></span>

<span data-ttu-id="1789b-124">Para almacenar datos de archivo binario en una base de datos con Entity Framework, defina una propiedad de tipo `byte[]` en la entidad:</span><span class="sxs-lookup"><span data-stu-id="1789b-124">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="1789b-125">Especifique una propiedad ViewModel de tipo `IFormFile`:</span><span class="sxs-lookup"><span data-stu-id="1789b-125">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="1789b-126">`IFormFile` se puede usar directamente como un parámetro de método de acción o como una propiedad ViewModel, tal y como se aprecia arriba.</span><span class="sxs-lookup"><span data-stu-id="1789b-126">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="1789b-127">Copie `IFormFile` en una secuencia y guárdela en la matriz de bytes:</span><span class="sxs-lookup"><span data-stu-id="1789b-127">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

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
> <span data-ttu-id="1789b-128">Tenga cuidado al almacenar los datos binarios en bases de datos relacionales, ya que esto puede repercutir adversamente en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="1789b-128">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="1789b-129">Cargar archivos grandes con streaming</span><span class="sxs-lookup"><span data-stu-id="1789b-129">Uploading large files with streaming</span></span>

<span data-ttu-id="1789b-130">Si el tamaño o la frecuencia de las cargas de archivos están causando problemas de recursos en la aplicación, considere la posibilidad de usar el streaming para cargar archivos en lugar de almacenarlos completamente en el búfer, como ocurre con el enlace de modelos descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="1789b-130">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="1789b-131">Usar `IFormFile` y el enlace de modelos es una solución mucho más sencilla, mientras que en el streaming hay que realizar una serie de pasos para implementarlo correctamente.</span><span class="sxs-lookup"><span data-stu-id="1789b-131">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="1789b-132">Cualquier archivo individual almacenado en búfer con un tamaño superior a 64 KB se trasladará desde la memoria RAM a un archivo temporal en el disco en el servidor.</span><span class="sxs-lookup"><span data-stu-id="1789b-132">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="1789b-133">Los recursos (disco, memoria RAM) que se usan en las cargas de archivos dependen de la cantidad y del tamaño de las cargas de archivos que se realizan simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="1789b-133">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="1789b-134">En el streaming lo importante no es el rendimiento, sino la escala.</span><span class="sxs-lookup"><span data-stu-id="1789b-134">Streaming isn't so much about perf, it's about scale.</span></span> <span data-ttu-id="1789b-135">Si se intentan almacenar demasiadas cargas en búfer, el sitio se bloqueará cuando se quede sin memoria o sin espacio en disco.</span><span class="sxs-lookup"><span data-stu-id="1789b-135">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="1789b-136">En el siguiente ejemplo se describe cómo usar JavaScript/Angular para transmitir por streaming a una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="1789b-136">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="1789b-137">El token de antifalsificación del archivo se genera por medio de un atributo de filtro personalizado y se pasa en encabezados HTTP, en lugar de en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="1789b-137">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="1789b-138">Dado que el método de acción procesa los datos cargados directamente, el enlace de modelos se deshabilita por otro filtro.</span><span class="sxs-lookup"><span data-stu-id="1789b-138">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="1789b-139">Dentro de la acción, el contenido del formulario se lee usando un `MultipartReader` (que lee cada `MultipartSection` individual), de forma que el archivo se procesa o el contenido se almacena, según corresponda.</span><span class="sxs-lookup"><span data-stu-id="1789b-139">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="1789b-140">Una vez que se han leído todas las secciones, la acción realiza su enlace de modelos particular.</span><span class="sxs-lookup"><span data-stu-id="1789b-140">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="1789b-141">La acción inicial carga el formulario y guarda un token de antifalsificación en una cookie (a través del atributo `GenerateAntiforgeryTokenCookieForAjax`):</span><span class="sxs-lookup"><span data-stu-id="1789b-141">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="1789b-142">El atributo usa la compatibilidad de [antifalsificación](xref:security/anti-request-forgery) integrada de ASP.NET Core para establecer una cookie con un token de solicitud:</span><span class="sxs-lookup"><span data-stu-id="1789b-142">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

<span data-ttu-id="1789b-143">Angular pasa automáticamente un token de antifalsificación en un encabezado de solicitud denominado `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="1789b-143">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="1789b-144">La aplicación ASP.NET Core MVC está definida para hacer referencia a este encabezado en su configuración en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1789b-144">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

<span data-ttu-id="1789b-145">El atributo `DisableFormValueModelBinding`, mostrado abajo, se usa para deshabilitar el enlace de modelos en el método de acción `Upload`.</span><span class="sxs-lookup"><span data-stu-id="1789b-145">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

<span data-ttu-id="1789b-146">Como el enlace de modelos está deshabilitado, el método de acción `Upload` no acepta parámetros.</span><span class="sxs-lookup"><span data-stu-id="1789b-146">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="1789b-147">Funciona directamente con la propiedad `Request` de `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="1789b-147">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="1789b-148">Se usa un elemento `MultipartReader` para leer cada sección.</span><span class="sxs-lookup"><span data-stu-id="1789b-148">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="1789b-149">El archivo se guarda con un nombre de archivo GUID y los datos de clave/valor se almacenan en un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="1789b-149">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="1789b-150">Una vez que todas las secciones se han leído, el contenido de `KeyValueAccumulator` se usa para enlazar los datos del formulario a un tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="1789b-150">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="1789b-151">Abajo mostramos el método `Upload` completo:</span><span class="sxs-lookup"><span data-stu-id="1789b-151">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a><span data-ttu-id="1789b-152">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="1789b-152">Troubleshooting</span></span>

<span data-ttu-id="1789b-153">Aquí incluimos algunos problemas comunes que pueden surgir al cargar archivos, así como sus posibles soluciones.</span><span class="sxs-lookup"><span data-stu-id="1789b-153">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="1789b-154">Error "No encontrado" inesperado en IIS</span><span class="sxs-lookup"><span data-stu-id="1789b-154">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="1789b-155">El siguiente error indica que la carga de archivos supera el valor de `maxAllowedContentLength` configurado en el servidor:</span><span class="sxs-lookup"><span data-stu-id="1789b-155">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="1789b-156">El valor predeterminado es `30000000`, que son alrededor de 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="1789b-156">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="1789b-157">Este valor se puede personalizar editando el archivo *web.config*:</span><span class="sxs-lookup"><span data-stu-id="1789b-157">The value can be customized by editing *web.config*:</span></span>

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

<span data-ttu-id="1789b-158">Esto solo ocurre en IIS;</span><span class="sxs-lookup"><span data-stu-id="1789b-158">This setting only applies to IIS.</span></span> <span data-ttu-id="1789b-159">este comportamiento no sucede de forma predeterminada cuando los archivos se hospedan en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1789b-159">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="1789b-160">Para más información, vea [Request Limits \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/) (Límites de solicitudes).</span><span class="sxs-lookup"><span data-stu-id="1789b-160">For more information, see [Request Limits \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="1789b-161">Excepción de referencia nula con IFormFile</span><span class="sxs-lookup"><span data-stu-id="1789b-161">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="1789b-162">Si el controlador acepta archivos cargados con `IFormFile`, pero ve que el valor siempre es null, confirme que en el formulario HTML se especifica un valor `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="1789b-162">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="1789b-163">Si este atributo no está establecido en el elemento `<form>`, la carga de archivos no se llevará a cabo y cualquier argumento `IFormFile` enlazado será nulo.</span><span class="sxs-lookup"><span data-stu-id="1789b-163">If this attribute isn't set on the `<form>` element, the file upload won't occur and any bound `IFormFile` arguments will be null.</span></span>
