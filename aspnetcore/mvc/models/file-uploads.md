---
title: Cargas de archivos en ASP.NET Core
author: ardalis
description: Cómo usar el enlace de modelos y el streaming para cargar archivos en ASP.NET Core MVC.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: mvc/models/file-uploads
ms.openlocfilehash: 5e6e2cd5fac25e2abe27915c2f4caa64b13e90bd
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56409988"
---
# <a name="file-uploads-in-aspnet-core"></a>Cargas de archivos en ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Entre las acciones que se pueden realizar en ASP.NET MVC está la de cargar uno o más archivos por medio de un sencillo procedimiento de enlace de modelos (archivos más pequeños) o del streaming (archivos de mayor tamaño).

[Ver o descargar el ejemplo desde GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>Cargar archivos pequeños por medio del enlace de modelos

Para cargar archivos pequeños, se puede usar un formulario HTML de varias partes o crear una solicitud POST con JavaScript. Este es un formulario de ejemplo en el que se usa Razor y que admite varios archivos cargados:

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

Para admitir las cargas de archivos, los formularios HTML deben especificar un tipo `enctype` de `multipart/form-data`. El elemento de entrada `files` de arriba admite la carga de varios archivos. Para admitir un único archivo cargado, solo hay que omitir el atributo `multiple` de este elemento de entrada. El marcado anterior se muestra así en un explorador:

![Formulario de carga de archivos](file-uploads/_static/upload-form.png)

Se puede tener acceso a los archivos individuales cargados en el servidor a través del [enlace de modelos](xref:mvc/models/model-binding), por medio de la interfaz [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile). `IFormFile` tiene la siguiente estructura:

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
> No se base o confíe en la propiedad `FileName` sin validarla. La propiedad `FileName` solo se debe usar con fines ilustrativos.

Al cargar archivos por medio del enlace de modelos y la interfaz `IFormFile`, el método de acción puede aceptar bien un solo `IFormFile`, bien un `IEnumerable<IFormFile>` (o `List<IFormFile>`) que represente varios archivos. En el siguiente ejemplo se itera por uno o varios archivos cargados, estos se guardan en el sistema de archivos local y se devuelve el número total y el tamaño de los archivos cargados.

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

Los archivos que se cargan usando la técnica `IFormFile` se almacenan en búfer en memoria o en disco en el servidor web antes de procesarse. Dentro del método de acción, se puede tener acceso al contenido de `IFormFile` como una secuencia. Aparte de al sistema de archivos local, los archivos se pueden transmitir por streaming también a [Azure Blob Storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs) o a [Entity Framework](/ef/core/index).

Para almacenar datos de archivo binario en una base de datos con Entity Framework, defina una propiedad de tipo `byte[]` en la entidad:

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

Especifique una propiedad ViewModel de tipo `IFormFile`:

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile` se puede usar directamente como un parámetro de método de acción o como una propiedad ViewModel, tal y como se aprecia arriba.

Copie `IFormFile` en una secuencia y guárdela en la matriz de bytes:

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
        var user = new ApplicationUser 
        {
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
> Tenga cuidado al almacenar los datos binarios en bases de datos relacionales, ya que esto puede repercutir adversamente en el rendimiento.

## <a name="uploading-large-files-with-streaming"></a>Cargar archivos grandes con streaming

Si el tamaño o la frecuencia de las cargas de archivos están causando problemas de recursos en la aplicación, considere la posibilidad de usar el streaming para cargar archivos en lugar de almacenarlos completamente en el búfer, como ocurre con el enlace de modelos descrito anteriormente. Usar `IFormFile` y el enlace de modelos es una solución mucho más sencilla, mientras que en el streaming hay que realizar una serie de pasos para implementarlo correctamente.

> [!NOTE]
> Cualquier archivo individual almacenado en búfer con un tamaño superior a 64 KB se trasladará desde la memoria RAM a un archivo temporal en el disco en el servidor. Los recursos (disco, memoria RAM) que se usan en las cargas de archivos dependen de la cantidad y del tamaño de las cargas de archivos que se realizan simultáneamente. En el streaming lo importante no es el rendimiento, sino la escala. Si se intentan almacenar demasiadas cargas en búfer, el sitio se bloqueará cuando se quede sin memoria o sin espacio en disco.

En el siguiente ejemplo se describe cómo usar JavaScript/Angular para transmitir por streaming a una acción de controlador. El token de antifalsificación del archivo se genera por medio de un atributo de filtro personalizado y se pasa en encabezados HTTP, en lugar de en el cuerpo de la solicitud. Dado que el método de acción procesa los datos cargados directamente, el enlace de modelos se deshabilita por otro filtro. Dentro de la acción, el contenido del formulario se lee usando un `MultipartReader` (que lee cada `MultipartSection` individual), de forma que el archivo se procesa o el contenido se almacena, según corresponda. Una vez que se han leído todas las secciones, la acción realiza su enlace de modelos particular.

La acción inicial carga el formulario y guarda un token de antifalsificación en una cookie (a través del atributo `GenerateAntiforgeryTokenCookieForAjax`):

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

El atributo usa la compatibilidad de [antifalsificación](xref:security/anti-request-forgery) integrada de ASP.NET Core para establecer una cookie con un token de solicitud:

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

Angular pasa automáticamente un token de antifalsificación en un encabezado de solicitud denominado `X-XSRF-TOKEN`. La aplicación ASP.NET Core MVC está definida para hacer referencia a este encabezado en su configuración en *Startup.cs*:

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

El atributo `DisableFormValueModelBinding`, mostrado abajo, se usa para deshabilitar el enlace de modelos en el método de acción `Upload`.

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

Como el enlace de modelos está deshabilitado, el método de acción `Upload` no acepta parámetros. Funciona directamente con la propiedad `Request` de `ControllerBase`. Se usa un elemento `MultipartReader` para leer cada sección. El archivo se guarda con un nombre de archivo GUID y los datos de clave/valor se almacenan en un `KeyValueAccumulator`. Una vez que todas las secciones se han leído, el contenido de `KeyValueAccumulator` se usa para enlazar los datos del formulario a un tipo de modelo.

Abajo mostramos el método `Upload` completo:

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>Solución de problemas

Aquí incluimos algunos problemas comunes que pueden surgir al cargar archivos, así como sus posibles soluciones.

### <a name="unexpected-not-found-error-with-iis"></a>Error "No encontrado" inesperado en IIS

El siguiente error indica que la carga de archivos supera el valor de `maxAllowedContentLength` configurado en el servidor:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

El valor predeterminado es `30000000`, que son alrededor de 28,6 MB. Este valor se puede personalizar editando el archivo *web.config*:

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

Esto solo ocurre en IIS; este comportamiento no sucede de forma predeterminada cuando los archivos se hospedan en Kestrel. Para más información, vea [Request Limits \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/) (Límites de solicitudes).

### <a name="null-reference-exception-with-iformfile"></a>Excepción de referencia nula con IFormFile

Si el controlador acepta archivos cargados con `IFormFile`, pero ve que el valor siempre es null, confirme que en el formulario HTML se especifica un valor `enctype` de `multipart/form-data`. Si este atributo no está establecido en el elemento `<form>`, la carga de archivos no se llevará a cabo y cualquier argumento `IFormFile` enlazado será nulo.
