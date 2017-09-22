---
title: Cargas de archivos en ASP.NET Core
author: ardalis
description: "Cómo usar el enlace de modelos y la transmisión por secuencias para cargar archivos en MVC de ASP.NET Core."
keywords: "ASP.NET Core, carga de archivos de modelo de enlace, IFormFile, transmisión por secuencias"
ms.author: riande
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: ebc98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/file-uploads
ms.openlocfilehash: e8608a46d6688df8da6c665a25b6f4db5f480461
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="file-uploads-in-aspnet-core"></a>Cargas de archivos en ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Acciones de ASP.NET MVC admiten la carga de uno o más archivos con un modelo simple de enlace de archivos más pequeños o transmisión por secuencias para archivos de mayor tamaño.

[Ver o descargar el ejemplo desde GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>Cargar archivos pequeños con el enlace de modelos

Para cargar archivos pequeños, puede usar un formulario HTML varias partes o construir una solicitud POST con JavaScript. Un formulario de ejemplo con Razor, que es compatible con varios archivos cargados, se muestra a continuación:

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

Para admitir cargas de archivos, deben especificar formularios HTML un `enctype` de `multipart/form-data`. El `files` entrada elemento mostrado anteriormente admite la carga de varios archivos. Omitir la `multiple` atributo de este elemento de entrada para permitir que solo un único archivo que se cargan. El marcado anterior, se representa en un explorador como:

![Formulario de carga de archivo](file-uploads/_static/upload-form.png)

Pueden tener acceso a los archivos individuales que se cargan en el servidor a través de [enlace de modelos](xref:mvc/models/model-binding) mediante la [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interfaz. `IFormFile`tiene la estructura siguiente:

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
> No se basan en o confiar en el `FileName` propiedad sin validación. El `FileName` propiedad debe utilizarse solo para fines de presentación.

Al cargar archivos con el enlace de modelos y la `IFormFile` interfaz, el método de acción puede aceptar un único `IFormFile` o un `IEnumerable<IFormFile>` (o `List<IFormFile>`) que representa varios archivos. En el ejemplo siguiente se recorre en iteración uno o varios de los archivos cargados, grabarlos en el sistema de archivos local y devuelve el número total y el tamaño de los archivos cargados.

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

Archivos cargados mediante la `IFormFile` técnica se almacenan en búfer en memoria o en disco en el servidor web antes de ser procesados. Dentro del método de acción, el `IFormFile` contenido es accesible como una secuencia. Además del sistema de archivos local, los archivos se pueden transmitir a [almacenamiento de blobs de Azure](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) o [Entity Framework](https://docs.microsoft.com/ef/core/index).

Para almacenar datos de archivo binario en una base de datos mediante Entity Framework, definir una propiedad de tipo `byte[]` en la entidad:

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

Especifique una propiedad ViewMode de tipo `IFormFile`:

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile`puede usarse directamente como un parámetro de método de acción o como una propiedad de modelo de vista, como se indicó anteriormente.

Copia la `IFormFile` en una secuencia y guarde el archivo en la matriz de bytes:

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
> Tenga cuidado al almacenar los datos binarios en bases de datos relacionales, tal y como puede ver afectado adversamente rendimiento.

## <a name="uploading-large-files-with-streaming"></a>Cargar archivos grandes con transmisión por secuencias

Si el tamaño o la frecuencia de cargas de archivos está causando problemas de recursos de la aplicación, tenga en cuenta la carga de archivos de transmisión por secuencias en lugar de almacenarla en búfer en su totalidad, igual que el enfoque de enlace de modelo mostrado anteriormente. Al usar `IFormFile` y enlace de modelos es una solución más sencilla mucho, transmisión por secuencias requiere un número de pasos necesarios para implementar correctamente.

> [!NOTE]
> Cualquier archivo almacenado en búfer único superior a 64KB se moverán de la RAM a un archivo temporal en disco en el servidor. Los recursos (disco RAM) utilizados por cargas de archivos dependen del número y tamaño de carga de archivos simultáneas. Transmisión por secuencias no es gran parte sobre rendimiento, sobre la escala. Si se intenta almacenar en búfer demasiadas cargas, el sitio se bloqueará cuando se quede sin memoria o espacio en disco.

En el ejemplo siguiente se muestra cómo utilizar JavaScript/Angular desea para transmitir a una acción de controlador. Token antiforgery del archivo se genera utilizando un atributo de filtro personalizado y pasa en encabezados HTTP en lugar de en el cuerpo de solicitud. Puesto que el método de acción procesa los datos cargados directamente, el enlace de modelos está deshabilitado por otro filtro. Dentro de la acción, el contenido del formulario se lee utilizando una `MultipartReader`, que lee cada persona `MultipartSection`, procesar el archivo o almacenar el contenido según corresponda. Una vez que se han leído todas las secciones, la acción realiza su propio enlace de modelos.

La acción inicial se carga el formulario y se guarda un token antiforgery en una cookie (a través de la `GenerateAntiforgeryTokenCookieForAjax` atributo):

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

El atributo usa integradas de ASP.NET Core [Antiforgery](xref:security/anti-request-forgery) soporte técnico para establecer una cookie con un token de solicitud:

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

Angular pasa automáticamente un token antiforgery en un encabezado de solicitud denominado `X-XSRF-TOKEN`. La aplicación de MVC de ASP.NET Core está configurada para hacer referencia a este encabezado en su configuración en *Startup.cs*:

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

El `DisableFormValueModelBinding` atributo, se muestra a continuación, se utiliza para deshabilitar el enlace de modelos de la `Upload` método de acción.

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

Puesto que está deshabilitado el enlace de modelo, el `Upload` método de acción no acepta parámetros. Funciona directamente con el `Request` propiedad de `ControllerBase`. Un `MultipartReader` se usa para leer cada sección. El archivo se guarda con un nombre de archivo GUID y los datos de clave/valor se almacena en un `KeyValueAccumulator`. Una vez que todas las secciones se han leído, el contenido de la `KeyValueAccumulator` se utilizan para enlazar los datos del formulario a un tipo de modelo.

La sección completa `Upload` método se muestra a continuación:

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>Solución de problemas

A continuación se muestran algunos problemas comunes que se producen al trabajar con la carga de archivos y sus posibles soluciones.

### <a name="unexpected-not-found-error-with-iis"></a>Error inesperado no se encontró con IIS

El siguiente error indica que la carga de archivos supera el servidor configurado `maxAllowedContentLength`:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

El valor predeterminado es `30000000`, que es aproximadamente 28,6 MB. El valor puede personalizarse mediante la edición de *web.config*:

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

Esta configuración solo se aplica a IIS. El comportamiento no se produce de forma predeterminada cuando se hospedan en Kestrel. Para obtener más información, consulte [límites de las solicitudes \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).

### <a name="null-reference-exception-with-iformfile"></a>Excepción de referencia nula con IFormFile

Si el controlador es aceptación cargar archivos con `IFormFile` pero se encuentra que el valor siempre es null, confirme que el formulario HTML está especificando una `enctype` valo `multipart/form-data`. Si este atributo no está establecido en el `<form>` elemento, no se realizará la carga de archivos y cualquier límite `IFormFile` argumentos será nulos.
