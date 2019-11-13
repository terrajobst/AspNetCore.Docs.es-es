---
title: Carga de archivos en ASP.NET Core
author: guardrex
description: Cómo usar el enlace de modelos y el streaming para cargar archivos en ASP.NET Core MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/31/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: 04e7533aa190a4875d3f66e8665fec16abec48b3
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73462941"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="671af-103">Carga de archivos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="671af-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="671af-104">Por [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/) y [Rutger Storm](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="671af-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="671af-105">ASP.NET Core admite la carga de uno o varios archivos mediante el enlace de modelos almacenado en búfer de archivos más pequeños y el streaming no almacenado en búfer de archivos de mayor tamaño.</span><span class="sxs-lookup"><span data-stu-id="671af-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="671af-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="671af-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="671af-107">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="671af-107">Security considerations</span></span>

<span data-ttu-id="671af-108">Tenga precaución al proporcionar a los usuarios la capacidad de cargar archivos en un servidor.</span><span class="sxs-lookup"><span data-stu-id="671af-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="671af-109">Es posible que los atacantes intenten lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="671af-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="671af-110">Ejecutar ataques por [denegación de servicio](/windows-hardware/drivers/ifs/denial-of-service).</span><span class="sxs-lookup"><span data-stu-id="671af-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="671af-111">Cargar virus o malware.</span><span class="sxs-lookup"><span data-stu-id="671af-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="671af-112">Poner en riesgo redes y servidores de otras maneras.</span><span class="sxs-lookup"><span data-stu-id="671af-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="671af-113">Estos son algunos de los pasos de seguridad con los que se reduce la probabilidad de sufrir ataques:</span><span class="sxs-lookup"><span data-stu-id="671af-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="671af-114">Cargue los archivos a un área de carga de archivos dedicada, preferiblemente una unidad que no sea de sistema.</span><span class="sxs-lookup"><span data-stu-id="671af-114">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="671af-115">Una ubicación dedicada facilita la imposición de restricciones de seguridad en los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="671af-115">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="671af-116">Deshabilite la ejecución de los permisos en la ubicación de carga de archivos.&dagger;</span><span class="sxs-lookup"><span data-stu-id="671af-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="671af-117">Los archivos cargados **no** se deben persistir en el mismo árbol de directorio que la aplicación.&dagger;</span><span class="sxs-lookup"><span data-stu-id="671af-117">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="671af-118">Use un nombre de archivo seguro determinado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="671af-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="671af-119">No use un nombre de archivo que proporcione el usuario ni el nombre de archivo que no es de confianza del archivo cargado.&dagger; HTML codifica el nombre de archivo que no es de confianza al mostrarlo.</span><span class="sxs-lookup"><span data-stu-id="671af-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="671af-120">Por ejemplo, al registrar el nombre de archivo o mostrarlo en la interfaz de usuario (Razor codifica de forma automática la salida HTML).</span><span class="sxs-lookup"><span data-stu-id="671af-120">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="671af-121">Permita solo las extensiones de archivo aprobadas para la especificación de diseño de la aplicación.&dagger;</span><span class="sxs-lookup"><span data-stu-id="671af-121">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="671af-122">Compruebe que se realizan comprobaciones del lado cliente en el servidor.&dagger; Las comprobaciones de cliente son fáciles de sortear.</span><span class="sxs-lookup"><span data-stu-id="671af-122">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="671af-123">Compruebe el tamaño de un archivo cargado.</span><span class="sxs-lookup"><span data-stu-id="671af-123">Check the size of an uploaded file.</span></span> <span data-ttu-id="671af-124">Establezca un límite de tamaño máximo para evitar cargas grandes.&dagger;</span><span class="sxs-lookup"><span data-stu-id="671af-124">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="671af-125">Cuando un archivo cargado con el mismo nombre no deba sobrescribir los archivos, vuelva a comprobar el nombre de archivo en la base de datos o en el almacenamiento físico antes de cargarlo.</span><span class="sxs-lookup"><span data-stu-id="671af-125">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="671af-126">**Ejecute un detector de virus o malware en el contenido cargado antes de que se almacene el archivo.**</span><span class="sxs-lookup"><span data-stu-id="671af-126">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="671af-127">&dagger;La aplicación de ejemplo muestra un enfoque que cumple los criterios.</span><span class="sxs-lookup"><span data-stu-id="671af-127">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="671af-128">La carga de código malintencionado en un sistema suele ser el primer paso para ejecutar código que puede:</span><span class="sxs-lookup"><span data-stu-id="671af-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="671af-129">Obtener el control completo de un sistema.</span><span class="sxs-lookup"><span data-stu-id="671af-129">Completely gain control of a system.</span></span>
> * <span data-ttu-id="671af-130">Sobrecargar un sistema de manera que el sistema se bloquea.</span><span class="sxs-lookup"><span data-stu-id="671af-130">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="671af-131">Poner en peligro los datos del usuario o del sistema.</span><span class="sxs-lookup"><span data-stu-id="671af-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="671af-132">Aplicar grafitis a una interfaz de usuario pública.</span><span class="sxs-lookup"><span data-stu-id="671af-132">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="671af-133">Vea los siguientes recursos para más información sobre cómo reducir el área expuesta de ataques al aceptar archivos de los usuarios:</span><span class="sxs-lookup"><span data-stu-id="671af-133">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="671af-134">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Carga de archivos sin restricciones)</span><span class="sxs-lookup"><span data-stu-id="671af-134">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * [<span data-ttu-id="671af-135">Azure Security: Asegúrese de que los controles adecuados estén en vigor al aceptar archivos de usuarios</span><span class="sxs-lookup"><span data-stu-id="671af-135">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="671af-136">Para más información sobre cómo implementar medidas de seguridad, incluidos ejemplos de la aplicación de ejemplo, consulte la sección [Validación](#validation).</span><span class="sxs-lookup"><span data-stu-id="671af-136">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="671af-137">Escenarios de almacenamiento</span><span class="sxs-lookup"><span data-stu-id="671af-137">Storage scenarios</span></span>

<span data-ttu-id="671af-138">Las opciones de almacenamiento comunes para los archivos incluyen:</span><span class="sxs-lookup"><span data-stu-id="671af-138">Common storage options for files include:</span></span>

* <span data-ttu-id="671af-139">Base de datos</span><span class="sxs-lookup"><span data-stu-id="671af-139">Database</span></span>

  * <span data-ttu-id="671af-140">En el caso de cargas de archivos pequeñas, una base de datos suele ser más rápida que las opciones de almacenamiento físico (sistema de archivos o recurso compartido de red).</span><span class="sxs-lookup"><span data-stu-id="671af-140">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="671af-141">Una base de datos suele ser más conveniente que las opciones de almacenamiento físico, ya que la recuperación de un registro de base de datos para los datos de usuario puede proporcionar el contenido del archivo (por ejemplo, una imagen de avatar).</span><span class="sxs-lookup"><span data-stu-id="671af-141">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="671af-142">Una base de datos puede ser más económica que usar un servicio de almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="671af-142">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="671af-143">Almacenamiento físico (sistema de archivos o recurso compartido de red)</span><span class="sxs-lookup"><span data-stu-id="671af-143">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="671af-144">Para cargas de archivos de gran tamaño:</span><span class="sxs-lookup"><span data-stu-id="671af-144">For large file uploads:</span></span>
    * <span data-ttu-id="671af-145">Los límites de base de datos pueden restringir el tamaño de la carga.</span><span class="sxs-lookup"><span data-stu-id="671af-145">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="671af-146">El almacenamiento físico suele ser menos económico que el almacenamiento en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="671af-146">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="671af-147">El almacenamiento físico puede ser más económico que usar un servicio de almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="671af-147">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="671af-148">El proceso de la aplicación debe tener permisos de lectura y escritura en la ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="671af-148">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="671af-149">**Nunca conceda el permiso de ejecución.**</span><span class="sxs-lookup"><span data-stu-id="671af-149">**Never grant execute permission.**</span></span>

* <span data-ttu-id="671af-150">Servicio de almacenamiento de datos (por ejemplo, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="671af-150">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="671af-151">Los servicios suelen ofrecer una escalabilidad y resistencia mejoradas sobre las soluciones locales que normalmente están sujetas a únicos puntos de error.</span><span class="sxs-lookup"><span data-stu-id="671af-151">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="671af-152">Los servicios pueden tener un costo menor en escenarios de infraestructura de almacenamiento de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="671af-152">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="671af-153">Para obtener más información, vea [Inicio rápido: Uso de .NET para crear un blob en el almacenamiento de objetos](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="671af-153">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="671af-154">En el tema se muestra <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, pero se puede usar <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> para guardar un <xref:System.IO.FileStream> en el almacenamiento de blobs cuando se trabaja con un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="671af-154">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="671af-155">Escenarios de carga de archivos</span><span class="sxs-lookup"><span data-stu-id="671af-155">File upload scenarios</span></span>

<span data-ttu-id="671af-156">Dos enfoques generales para cargar archivos son el almacenamiento en búfer y el streaming.</span><span class="sxs-lookup"><span data-stu-id="671af-156">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="671af-157">**Almacenamiento en búfer**</span><span class="sxs-lookup"><span data-stu-id="671af-157">**Buffering**</span></span>

<span data-ttu-id="671af-158">El archivo completo se lee en un <xref:Microsoft.AspNetCore.Http.IFormFile>, que es una representación de C# del archivo que se usa para procesar o guardar el archivo.</span><span class="sxs-lookup"><span data-stu-id="671af-158">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="671af-159">Los recursos (disco, memoria) que se usan en las cargas de archivos dependen de la cantidad y del tamaño de las cargas de archivos que se realizan simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="671af-159">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="671af-160">Si una aplicación intenta almacenar demasiadas cargas en el búfer, el sitio se bloquea cuando se queda sin memoria o sin espacio en disco.</span><span class="sxs-lookup"><span data-stu-id="671af-160">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="671af-161">Si el tamaño o la frecuencia de las cargas de archivos está agotando los recursos de la aplicación, use el streaming.</span><span class="sxs-lookup"><span data-stu-id="671af-161">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="671af-162">Cualquier archivo almacenado en búfer único que supere los 64 KB se mueve de la memoria a un archivo temporal en el disco.</span><span class="sxs-lookup"><span data-stu-id="671af-162">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="671af-163">En las secciones siguientes de este tema se habla del almacenamiento en búfer de archivos pequeños:</span><span class="sxs-lookup"><span data-stu-id="671af-163">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="671af-164">Almacenamiento físico</span><span class="sxs-lookup"><span data-stu-id="671af-164">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="671af-165">Base de datos</span><span class="sxs-lookup"><span data-stu-id="671af-165">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="671af-166">**Streaming**</span><span class="sxs-lookup"><span data-stu-id="671af-166">**Streaming**</span></span>

<span data-ttu-id="671af-167">El archivo se recibe de una solicitud de varias partes y lo procesa o guarda directamente la aplicación.</span><span class="sxs-lookup"><span data-stu-id="671af-167">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="671af-168">El streaming no mejora considerablemente el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="671af-168">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="671af-169">El streaming reduce las demandas de memoria o espacio en disco cuando se cargan archivos.</span><span class="sxs-lookup"><span data-stu-id="671af-169">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="671af-170">El streaming de archivos grandes se describe en la sección [Carga de archivos de gran tamaño con streaming](#upload-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="671af-170">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="671af-171">Carga de archivos pequeños con enlace de modelos almacenado en búfer al almacenamiento físico</span><span class="sxs-lookup"><span data-stu-id="671af-171">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="671af-172">Para cargar archivos pequeños, se puede usar un formulario de varias partes o construir una solicitud POST con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="671af-172">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="671af-173">En el ejemplo siguiente se muestra el uso de un formulario de Razor Pages para cargar un archivo único (*Pages/BufferedSingleFileUploadPhysical.cshtml* en la aplicación de ejemplo):</span><span class="sxs-lookup"><span data-stu-id="671af-173">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

<span data-ttu-id="671af-174">El ejemplo siguiente es análogo al ejemplo anterior, salvo en que:</span><span class="sxs-lookup"><span data-stu-id="671af-174">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="671af-175">([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) de JavaScript se usa para enviar los datos del formulario.</span><span class="sxs-lookup"><span data-stu-id="671af-175">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="671af-176">No hay ninguna validación.</span><span class="sxs-lookup"><span data-stu-id="671af-176">There's no validation.</span></span>

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

<span data-ttu-id="671af-177">Para realizar la solicitud POST en JavaScript para los clientes que [no admiten Fetch API](https://caniuse.com/#feat=fetch), use uno de estos enfoques:</span><span class="sxs-lookup"><span data-stu-id="671af-177">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="671af-178">Use un Fetch Polyfill (por ejemplo, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="671af-178">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="671af-179">Use `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="671af-179">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="671af-180">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="671af-180">For example:</span></span>

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

<span data-ttu-id="671af-181">Para admitir las cargas de archivos, los formularios HTML deben especificar un tipo de codificación (`enctype`) de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="671af-181">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="671af-182">Para que un elemento de entrada `files` admita al carga de varios archivos, proporcione el atributo `multiple` en el elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="671af-182">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="671af-183">Es posible acceder a archivos individuales cargados en el servidor a través del [enlace de modelos](xref:mvc/models/model-binding) mediante <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="671af-183">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="671af-184">La aplicación de ejemplo muestra varias cargas de archivos almacenados en búfer para escenarios de almacenamiento físico y base de datos.</span><span class="sxs-lookup"><span data-stu-id="671af-184">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename"></a>

> [!WARNING]
> <span data-ttu-id="671af-185">**No** utilice la propiedad `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> para usos distintos a la presentación y el registro.</span><span class="sxs-lookup"><span data-stu-id="671af-185">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="671af-186">Para fines de presentación y registro, codifique el nombre de archivo en HTML.</span><span class="sxs-lookup"><span data-stu-id="671af-186">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="671af-187">Un atacante puede proporcionar un nombre de archivo malintencionado, incluidas rutas de acceso completas o relativas.</span><span class="sxs-lookup"><span data-stu-id="671af-187">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="671af-188">Las aplicaciones deben:</span><span class="sxs-lookup"><span data-stu-id="671af-188">Applications should:</span></span>
>
> * <span data-ttu-id="671af-189">Quitar la ruta de acceso del nombre de archivo proporcionado por el usuario.</span><span class="sxs-lookup"><span data-stu-id="671af-189">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="671af-190">Guardar el nombre de archivo codificado en HTML, sin la ruta de acceso para la interfaz de usuario o el registro.</span><span class="sxs-lookup"><span data-stu-id="671af-190">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="671af-191">Generar un nombre de archivo aleatorio nuevo para el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="671af-191">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="671af-192">En el código siguiente se quita la ruta de acceso del nombre del archivo:</span><span class="sxs-lookup"><span data-stu-id="671af-192">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="671af-193">Los ejemplos proporcionados hasta ahora no tienen en cuenta las consideraciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="671af-193">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="671af-194">Se proporciona información adicional en las secciones siguientes y en la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="671af-194">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="671af-195">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="671af-195">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="671af-196">Validación</span><span class="sxs-lookup"><span data-stu-id="671af-196">Validation</span></span>](#validation)

<span data-ttu-id="671af-197">Al cargar archivos mediante el enlace de modelos y <xref:Microsoft.AspNetCore.Http.IFormFile>, el método de acción puede aceptar:</span><span class="sxs-lookup"><span data-stu-id="671af-197">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="671af-198">Un <xref:Microsoft.AspNetCore.Http.IFormFile> único.</span><span class="sxs-lookup"><span data-stu-id="671af-198">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="671af-199">Cualquiera de las colecciones siguientes que representan varios archivos:</span><span class="sxs-lookup"><span data-stu-id="671af-199">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="671af-200">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="671af-200">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="671af-201">El enlace coincide con los archivos de formulario por nombre.</span><span class="sxs-lookup"><span data-stu-id="671af-201">Binding matches form files by name.</span></span> <span data-ttu-id="671af-202">Por ejemplo, el valor `name` HTML en `<input type="file" name="formFile">` debe coincidir con la propiedad/el parámetro enlazado de C# (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="671af-202">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="671af-203">Para más información, consulte la sección [Coincidencia del valor de atributo de nombre con el nombre del parámetro del método POST](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="671af-203">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="671af-204">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="671af-204">The following example:</span></span>

* <span data-ttu-id="671af-205">Recorre en bucle uno o más archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="671af-205">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="671af-206">Usa [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) para devolver una ruta de acceso completa de un archivo, incluido el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="671af-206">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="671af-207">Guarda los archivos en el sistema de archivos local con un nombre de archivo generado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="671af-207">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="671af-208">Devuelve el número total y el tamaño de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="671af-208">Returns the total number and size of files uploaded.</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

<span data-ttu-id="671af-209">Use `Path.GetRandomFileName` para generar un nombre de archivo sin una ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="671af-209">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="671af-210">En el ejemplo siguiente, la ruta de acceso se obtiene de la configuración:</span><span class="sxs-lookup"><span data-stu-id="671af-210">In the following example, the path is obtained from configuration:</span></span>

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

<span data-ttu-id="671af-211">La ruta de acceso pasada al <xref:System.IO.FileStream> *debe* incluir el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="671af-211">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="671af-212">Si no se proporciona el nombre de archivo, se produce una <xref:System.UnauthorizedAccessException> en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="671af-212">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="671af-213">Los archivos que se cargan usando la técnica <xref:Microsoft.AspNetCore.Http.IFormFile> se almacenan en búfer en memoria o en disco en el servidor web antes de procesarse.</span><span class="sxs-lookup"><span data-stu-id="671af-213">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="671af-214">Dentro del método de acción, se puede tener acceso al contenido de <xref:Microsoft.AspNetCore.Http.IFormFile> como <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="671af-214">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="671af-215">Además del sistema de archivos local, los archivos se pueden guardar en un recurso compartido de red o en un servicio de almacenamiento de archivos, como [Azure Blob Storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="671af-215">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="671af-216">Para ver otro ejemplo que recorre en bucle varios archivos para cargar y usa nombres de archivo seguros, consulte *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="671af-216">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="671af-217">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) arroja una <xref:System.IO.IOException> si se crean más de 65 535 archivos sin eliminar los archivos temporales anteriores.</span><span class="sxs-lookup"><span data-stu-id="671af-217">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="671af-218">El límite de 65 535 archivos es un límite por servidor.</span><span class="sxs-lookup"><span data-stu-id="671af-218">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="671af-219">Para más información sobre este límite en el sistema operativo Windows, consulte las notas en los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="671af-219">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="671af-220">Función GetTempFileNameA</span><span class="sxs-lookup"><span data-stu-id="671af-220">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="671af-221">Carga de archivos pequeños con enlace de modelos almacenado en búfer a una base de datos</span><span class="sxs-lookup"><span data-stu-id="671af-221">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="671af-222">Para almacenar datos de archivo binario en una base de datos con [Entity Framework](/ef/core/index), defina una propiedad de matriz <xref:System.Byte> en la entidad:</span><span class="sxs-lookup"><span data-stu-id="671af-222">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="671af-223">Especifique una propiedad de modelo de página para la clase que incluya un <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="671af-223">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="671af-224"><xref:Microsoft.AspNetCore.Http.IFormFile> se puede usar directamente como un parámetro de método de acción o como una propiedad de modelo enlazado.</span><span class="sxs-lookup"><span data-stu-id="671af-224"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="671af-225">En el ejemplo anterior se utiliza una propiedad de modelo enlazado.</span><span class="sxs-lookup"><span data-stu-id="671af-225">The prior example uses a bound model property.</span></span>

<span data-ttu-id="671af-226">`FileUpload` se usa en el formulario de Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="671af-226">The `FileUpload` is used in the Razor Pages form:</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

<span data-ttu-id="671af-227">Cuando el formulario se publique en el servidor, copie el <xref:Microsoft.AspNetCore.Http.IFormFile> en un flujo y guárdelo como matriz de bytes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="671af-227">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="671af-228">En el ejemplo siguiente, `_dbContext` almacena el contexto de base de datos de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="671af-228">In the following example, `_dbContext` stores the app's database context:</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

<span data-ttu-id="671af-229">El ejemplo anterior es similar a un escenario que se muestra en la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="671af-229">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="671af-230">*Pages/BufferedSingleFileUploadDb.cshtml*</span><span class="sxs-lookup"><span data-stu-id="671af-230">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="671af-231">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="671af-231">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="671af-232">Tenga cuidado al almacenar los datos binarios en bases de datos relacionales, ya que esto puede repercutir adversamente en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="671af-232">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="671af-233">No se base ni confíe en la propiedad `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sin validarla.</span><span class="sxs-lookup"><span data-stu-id="671af-233">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="671af-234">La propiedad `FileName` solo debe usarse para fines de presentación y solo después de la codificación HTML.</span><span class="sxs-lookup"><span data-stu-id="671af-234">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="671af-235">Los ejemplos proporcionados no tienen en cuenta las consideraciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="671af-235">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="671af-236">Se proporciona información adicional en las secciones siguientes y en la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="671af-236">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="671af-237">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="671af-237">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="671af-238">Validación</span><span class="sxs-lookup"><span data-stu-id="671af-238">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="671af-239">Carga de archivos de gran tamaño con streaming</span><span class="sxs-lookup"><span data-stu-id="671af-239">Upload large files with streaming</span></span>

<span data-ttu-id="671af-240">En el ejemplo siguiente se muestra cómo usar JavaScript para transmitir un archivo a una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="671af-240">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="671af-241">El token de antifalsificación del archivo se genera por medio de un atributo de filtro personalizado y se pasa a los encabezados HTTP del cliente, en lugar de en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="671af-241">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="671af-242">Dado que el método de acción procesa los datos cargados directamente, el enlace de modelos del formulario se deshabilita por otro filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="671af-242">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="671af-243">Dentro de la acción, el contenido del formulario se lee usando un `MultipartReader` (que lee cada `MultipartSection` individual), de forma que el archivo se procesa o el contenido se almacena, según corresponda.</span><span class="sxs-lookup"><span data-stu-id="671af-243">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="671af-244">Una vez leídas las secciones de varias partes, la acción realiza su propio enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="671af-244">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="671af-245">La respuesta de la página inicial carga el formulario y guarda un token de antifalsificación en una cookie (a través del atributo `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="671af-245">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="671af-246">El atributo usa la [compatibilidad de antifalsificación](xref:security/anti-request-forgery) integrada de ASP.NET Core para establecer una cookie con un token de solicitud:</span><span class="sxs-lookup"><span data-stu-id="671af-246">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="671af-247">El `DisableFormValueModelBindingAttribute` se usa para deshabilitar el enlace de modelos:</span><span class="sxs-lookup"><span data-stu-id="671af-247">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="671af-248">En la aplicación de ejemplo, `GenerateAntiforgeryTokenCookieAttribute` y `DisableFormValueModelBindingAttribute` se aplican como filtros a los modelos de aplicación de la página de `/StreamedSingleFileUploadDb` y `/StreamedSingleFileUploadPhysical` en `Startup.ConfigureServices` con las [convenciones de Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="671af-248">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="671af-249">Dado que el enlace de modelos no lee el formulario, los parámetros enlazados desde el formulario no se enlazan (la consulta, la ruta y el encabezado siguen funcionando).</span><span class="sxs-lookup"><span data-stu-id="671af-249">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="671af-250">El método de acción funciona directamente con la propiedad `Request`.</span><span class="sxs-lookup"><span data-stu-id="671af-250">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="671af-251">Se usa un elemento `MultipartReader` para leer cada sección.</span><span class="sxs-lookup"><span data-stu-id="671af-251">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="671af-252">Los datos de clave-valor se almacenan en un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="671af-252">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="671af-253">Una vez leídas las secciones de varias partes, el contenido del `KeyValueAccumulator` se usa para enlazar los datos del formulario a un tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="671af-253">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="671af-254">El método `StreamingController.UploadDatabase` completo para streaming a una base de datos con EF Core:</span><span class="sxs-lookup"><span data-stu-id="671af-254">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="671af-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span><span class="sxs-lookup"><span data-stu-id="671af-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="671af-256">El método `StreamingController.UploadPhysical` completo para streaming a una ubicación física:</span><span class="sxs-lookup"><span data-stu-id="671af-256">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="671af-257">En la aplicación de ejemplo, las comprobaciones de validación las controla `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="671af-257">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="671af-258">Validación</span><span class="sxs-lookup"><span data-stu-id="671af-258">Validation</span></span>

<span data-ttu-id="671af-259">La clase `FileHelpers` de la aplicación de ejemplo muestra varias comprobaciones de cargas de archivos de streaming y <xref:Microsoft.AspNetCore.Http.IFormFile> almacenado en búfer.</span><span class="sxs-lookup"><span data-stu-id="671af-259">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="671af-260">Para procesar cargas de archivos almacenadas en búfer de <xref:Microsoft.AspNetCore.Http.IFormFile> en la aplicación de ejemplo, consulte el método `ProcessFormFile` en el archivo *Utilities/FileHelpers.cs*.</span><span class="sxs-lookup"><span data-stu-id="671af-260">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="671af-261">Para procesar archivos de streaming, consulte el método `ProcessStreamedFile` en el mismo archivo.</span><span class="sxs-lookup"><span data-stu-id="671af-261">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="671af-262">Los métodos de procesamiento de validación mostrados en la aplicación de ejemplo no examinan el contenido de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="671af-262">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="671af-263">En la mayoría de los escenarios de producción, se usa una API de analizador de virus/malware en el archivo antes de que el archivo esté disponible para los usuarios u otros sistemas.</span><span class="sxs-lookup"><span data-stu-id="671af-263">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="671af-264">Aunque el ejemplo de tema proporciona un ejemplo funcional de técnicas de validación, no implemente la clase `FileHelpers` en una aplicación de producción a menos que:</span><span class="sxs-lookup"><span data-stu-id="671af-264">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="671af-265">Comprenda totalmente la implementación.</span><span class="sxs-lookup"><span data-stu-id="671af-265">Fully understand the implementation.</span></span>
> * <span data-ttu-id="671af-266">Modifique la implementación según corresponda en función del entorno y las especificaciones de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="671af-266">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="671af-267">**No implemente nunca de manera indiscriminada el código de seguridad en una aplicación sin abordar estos requisitos.**</span><span class="sxs-lookup"><span data-stu-id="671af-267">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="671af-268">Validación del contenido</span><span class="sxs-lookup"><span data-stu-id="671af-268">Content validation</span></span>

<span data-ttu-id="671af-269">**Use una API de detección de virus/malware de terceros en el contenido cargado.**</span><span class="sxs-lookup"><span data-stu-id="671af-269">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="671af-270">El análisis de archivos exige recursos del servidor en escenarios de gran volumen.</span><span class="sxs-lookup"><span data-stu-id="671af-270">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="671af-271">Si disminuye el rendimiento de procesamiento de solicitudes debido al análisis de archivos, considere la posibilidad de descargar el trabajo de análisis a un [servicio en segundo plano](xref:fundamentals/host/hosted-services), posiblemente un servicio que se ejecute en otro servidor desde el servidor de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="671af-271">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="671af-272">Habitualmente, los archivos cargados se almacenan en un área en cuarentena hasta que el programa de detección de virus en segundo plano los revisa.</span><span class="sxs-lookup"><span data-stu-id="671af-272">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="671af-273">Cuando se pasa un archivo, se mueve a la ubicación de almacenamiento de archivos habitual.</span><span class="sxs-lookup"><span data-stu-id="671af-273">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="671af-274">Por lo general, estos pasos se realizan junto con un registro de base de datos que indica el estado de análisis de un archivo.</span><span class="sxs-lookup"><span data-stu-id="671af-274">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="671af-275">Mediante el uso de este enfoque, la aplicación y el servidor de aplicaciones permanecen centrados en responder a las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="671af-275">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="671af-276">Validación de la extensión del archivo</span><span class="sxs-lookup"><span data-stu-id="671af-276">File extension validation</span></span>

<span data-ttu-id="671af-277">La extensión del archivo cargado debe comprobarse con una lista de extensiones permitidas.</span><span class="sxs-lookup"><span data-stu-id="671af-277">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="671af-278">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="671af-278">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="671af-279">Validación de firma del archivo</span><span class="sxs-lookup"><span data-stu-id="671af-279">File signature validation</span></span>

<span data-ttu-id="671af-280">La firma de un archivo viene determinada por los primeros bytes al principio de un archivo.</span><span class="sxs-lookup"><span data-stu-id="671af-280">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="671af-281">Estos bytes se pueden usar para indicar si la extensión coincide con el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="671af-281">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="671af-282">La aplicación de ejemplo comprueba las firmas de archivo de algunos tipos de archivo comunes.</span><span class="sxs-lookup"><span data-stu-id="671af-282">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="671af-283">En el ejemplo siguiente, la firma de archivo de una imagen JPEG se comprueba con respecto al archivo:</span><span class="sxs-lookup"><span data-stu-id="671af-283">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

<span data-ttu-id="671af-284">Para obtener firmas de archivo adicionales, consulte la [base de datos de firmas de archivo](https://www.filesignatures.net/) y las especificaciones de archivo oficiales.</span><span class="sxs-lookup"><span data-stu-id="671af-284">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="671af-285">Seguridad de nombre de archivo</span><span class="sxs-lookup"><span data-stu-id="671af-285">File name security</span></span>

<span data-ttu-id="671af-286">Nunca use un nombre de archivo proporcionado por el cliente para guardar un archivo en el almacenamiento físico.</span><span class="sxs-lookup"><span data-stu-id="671af-286">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="671af-287">Cree un nombre de archivo seguro para el archivo con [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) o [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) para crear una ruta de acceso completa (incluido el nombre de archivo) para el almacenamiento temporal.</span><span class="sxs-lookup"><span data-stu-id="671af-287">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="671af-288">Razor codifica automáticamente en HTML los valores de propiedad para mostrarlos.</span><span class="sxs-lookup"><span data-stu-id="671af-288">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="671af-289">El código siguiente es seguro de usar:</span><span class="sxs-lookup"><span data-stu-id="671af-289">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="671af-290">Fuera de Razor, siempre use <xref:System.Net.WebUtility.HtmlEncode*> al contenido del nombre de archivo de una solicitud de un usuario.</span><span class="sxs-lookup"><span data-stu-id="671af-290">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="671af-291">Muchas implementaciones deben incluir una comprobación de que el archivo existe; de lo contrario, el archivo se sobrescribe por un archivo con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="671af-291">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="671af-292">Proporcione lógica adicional para satisfacer las especificaciones de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="671af-292">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="671af-293">Validación del tamaño</span><span class="sxs-lookup"><span data-stu-id="671af-293">Size validation</span></span>

<span data-ttu-id="671af-294">Limite el tamaño de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="671af-294">Limit the size of uploaded files.</span></span>

<span data-ttu-id="671af-295">En la aplicación de ejemplo, el tamaño del archivo está limitado a 2 MB (se indica en bytes).</span><span class="sxs-lookup"><span data-stu-id="671af-295">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="671af-296">El límite se proporciona a través de [Configuración](xref:fundamentals/configuration/index) del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="671af-296">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="671af-297">`FileSizeLimit` se inserta en las clases `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="671af-297">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

<span data-ttu-id="671af-298">Cuando el tamaño de un archivo supera el límite, se rechaza el archivo:</span><span class="sxs-lookup"><span data-stu-id="671af-298">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="671af-299">Coincidencia del valor de atributo de nombre con el nombre del parámetro del método POST</span><span class="sxs-lookup"><span data-stu-id="671af-299">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="671af-300">En los formularios que no son de Razor que realizan la operación POST en los datos de formulario o usan directamente `FormData` de JavaScript, el nombre especificado en el elemento del formulario o `FormData` debe coincidir con el nombre del parámetro en la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="671af-300">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="671af-301">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="671af-301">In the following example:</span></span>

* <span data-ttu-id="671af-302">Cuando se usa un elemento `<input>`, el atributo `name` se establece en el valor `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="671af-302">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="671af-303">Cuando se usa `FormData` en JavaScript, el nombre se establece en el valor `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="671af-303">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="671af-304">Use un nombre coincidente para el parámetro del método de C# (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="671af-304">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="671af-305">Para un método de control de páginas de Razor Pages denominado `Upload`:</span><span class="sxs-lookup"><span data-stu-id="671af-305">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="671af-306">Para un método de acción de controlador POST de MVC:</span><span class="sxs-lookup"><span data-stu-id="671af-306">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="671af-307">Configuración del servidor y de la aplicación</span><span class="sxs-lookup"><span data-stu-id="671af-307">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="671af-308">Límite de longitud del cuerpo de varias partes</span><span class="sxs-lookup"><span data-stu-id="671af-308">Multipart body length limit</span></span>

<span data-ttu-id="671af-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> establece el límite de la longitud de cada cuerpo de varias partes.</span><span class="sxs-lookup"><span data-stu-id="671af-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="671af-310">Las secciones del formulario que superan este límite inician una <xref:System.IO.InvalidDataException> cuando se analizan.</span><span class="sxs-lookup"><span data-stu-id="671af-310">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="671af-311">El valor predeterminado es 134 217 728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="671af-311">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="671af-312">Personalice el límite mediante el valor <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="671af-312">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="671af-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> se utiliza para establecer el <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> para una sola página o acción.</span><span class="sxs-lookup"><span data-stu-id="671af-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="671af-314">En una aplicación de Razor Pages, aplique el filtro con una [convención](xref:razor-pages/razor-pages-conventions) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="671af-314">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRazorPages()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model.Filters.Add(
                    new RequestFormLimitsAttribute()
                    {
                        // Set the limit to 256 MB
                        MultipartBodyLengthLimit = 268435456
                    });
    });
```

<span data-ttu-id="671af-315">En una aplicación de Razor Pages o una aplicación MVC, aplique el filtro al método de acción o al modelo de página:</span><span class="sxs-lookup"><span data-stu-id="671af-315">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="671af-316">Tamaño máximo del cuerpo de la solicitud de Kestrel</span><span class="sxs-lookup"><span data-stu-id="671af-316">Kestrel maximum request body size</span></span>

<span data-ttu-id="671af-317">En el caso de las aplicaciones hospedadas por Kestrel, el tamaño máximo predeterminado del cuerpo de solicitud es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="671af-317">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="671af-318">Personalice el límite con la opción [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) del servidor de Kestrel:</span><span class="sxs-lookup"><span data-stu-id="671af-318">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="671af-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> se usa para establecer el valor de [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) de una sola página o acción.</span><span class="sxs-lookup"><span data-stu-id="671af-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="671af-320">En una aplicación de Razor Pages, aplique el filtro con una [convención](xref:razor-pages/razor-pages-conventions) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="671af-320">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRazorPages()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model =>
                {
                    // Handle requests up to 50 MB
                    model.Filters.Add(
                        new RequestSizeLimitAttribute(52428800));
                });
    });
```

<span data-ttu-id="671af-321">En una aplicación de Razor Pages o una aplicación MVC, aplique el filtro al método de acción o a la clase de control de página:</span><span class="sxs-lookup"><span data-stu-id="671af-321">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="671af-322">El `RequestSizeLimitAttribute` también se puede aplicar con la directiva [@attribute](xref:mvc/views/razor#attribute) de Razor:</span><span class="sxs-lookup"><span data-stu-id="671af-322">The `RequestSizeLimitAttribute` can also be applied using the [@attribute](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="671af-323">Otros límites de Kestrel</span><span class="sxs-lookup"><span data-stu-id="671af-323">Other Kestrel limits</span></span>

<span data-ttu-id="671af-324">Otros límites de Kestrel pueden aplicarse a las aplicaciones hospedadas por Kestrel:</span><span class="sxs-lookup"><span data-stu-id="671af-324">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="671af-325">Conexiones de cliente máximas</span><span class="sxs-lookup"><span data-stu-id="671af-325">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="671af-326">Tarifas de datos de solicitud y respuesta</span><span class="sxs-lookup"><span data-stu-id="671af-326">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="671af-327">Límite de longitud del contenido de IIS</span><span class="sxs-lookup"><span data-stu-id="671af-327">IIS content length limit</span></span>

<span data-ttu-id="671af-328">El límite predeterminado de solicitudes (`maxAllowedContentLength`) es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="671af-328">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="671af-329">Personalice el límite en el archivo *web.config*:</span><span class="sxs-lookup"><span data-stu-id="671af-329">Customize the limit in the *web.config* file:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="671af-330">Esto solo ocurre en IIS;</span><span class="sxs-lookup"><span data-stu-id="671af-330">This setting only applies to IIS.</span></span> <span data-ttu-id="671af-331">este comportamiento no sucede de forma predeterminada cuando los archivos se hospedan en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="671af-331">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="671af-332">Para más información, consulte [Límites de solicitud \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="671af-332">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="671af-333">Las limitaciones del módulo ASP.NET Core o la presencia del módulo de filtrado de solicitudes de IIS pueden limitar las cargas a 2 GB o 4 GB.</span><span class="sxs-lookup"><span data-stu-id="671af-333">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="671af-334">Para más información, consulte el artículo que indica que [no se pueden cargar archivos con un tamaño superior a 2 GB (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="671af-334">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="671af-335">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="671af-335">Troubleshoot</span></span>

<span data-ttu-id="671af-336">Aquí incluimos algunos problemas comunes que pueden surgir al cargar archivos, así como sus posibles soluciones.</span><span class="sxs-lookup"><span data-stu-id="671af-336">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="671af-337">Error No encontrado al implementar en un servidor IIS</span><span class="sxs-lookup"><span data-stu-id="671af-337">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="671af-338">El error siguiente indica que el archivo cargado supera la longitud configurada del contenido del servidor:</span><span class="sxs-lookup"><span data-stu-id="671af-338">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="671af-339">Para más información sobre cómo aumentar el límite, consulte la sección [Límite de longitud del contenido de IIS](#iis-content-length-limit).</span><span class="sxs-lookup"><span data-stu-id="671af-339">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="671af-340">Error de conexión</span><span class="sxs-lookup"><span data-stu-id="671af-340">Connection failure</span></span>

<span data-ttu-id="671af-341">Un error de conexión y una conexión del servidor de restablecimiento probablemente indica que el archivo cargado supera el tamaño máximo del cuerpo de la solicitud de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="671af-341">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="671af-342">Para más información, consulte la sección [Tamaño máximo del cuerpo de la solicitud de Kestrel](#kestrel-maximum-request-body-size).</span><span class="sxs-lookup"><span data-stu-id="671af-342">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="671af-343">Los límites de conexión del cliente de Kestrel también pueden requerir ajustes.</span><span class="sxs-lookup"><span data-stu-id="671af-343">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="671af-344">Excepción de referencia nula con IFormFile</span><span class="sxs-lookup"><span data-stu-id="671af-344">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="671af-345">Si el controlador acepta archivos cargados con <xref:Microsoft.AspNetCore.Http.IFormFile>, pero el valor es `null`, confirme que el formulario HTML especifica un valor `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="671af-345">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="671af-346">Si este atributo no está establecido en el elemento `<form>`, no se llevará a cabo la carga del archivo y cualquier argumento <xref:Microsoft.AspNetCore.Http.IFormFile> enlazado será `null`.</span><span class="sxs-lookup"><span data-stu-id="671af-346">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="671af-347">Confirme también que la [nomenclatura de la carga en los datos de formulario coincide con la nomenclatura de la aplicación](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="671af-347">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="671af-348">ASP.NET Core admite la carga de uno o varios archivos mediante el enlace de modelos almacenado en búfer de archivos más pequeños y el streaming no almacenado en búfer de archivos de mayor tamaño.</span><span class="sxs-lookup"><span data-stu-id="671af-348">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="671af-349">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="671af-349">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="671af-350">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="671af-350">Security considerations</span></span>

<span data-ttu-id="671af-351">Tenga precaución al proporcionar a los usuarios la capacidad de cargar archivos en un servidor.</span><span class="sxs-lookup"><span data-stu-id="671af-351">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="671af-352">Es posible que los atacantes intenten lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="671af-352">Attackers may attempt to:</span></span>

* <span data-ttu-id="671af-353">Ejecutar ataques por [denegación de servicio](/windows-hardware/drivers/ifs/denial-of-service).</span><span class="sxs-lookup"><span data-stu-id="671af-353">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="671af-354">Cargar virus o malware.</span><span class="sxs-lookup"><span data-stu-id="671af-354">Upload viruses or malware.</span></span>
* <span data-ttu-id="671af-355">Poner en riesgo redes y servidores de otras maneras.</span><span class="sxs-lookup"><span data-stu-id="671af-355">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="671af-356">Estos son algunos de los pasos de seguridad con los que se reduce la probabilidad de sufrir ataques:</span><span class="sxs-lookup"><span data-stu-id="671af-356">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="671af-357">Cargue los archivos a un área de carga de archivos dedicada, preferiblemente una unidad que no sea de sistema.</span><span class="sxs-lookup"><span data-stu-id="671af-357">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="671af-358">Una ubicación dedicada facilita la imposición de restricciones de seguridad en los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="671af-358">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="671af-359">Deshabilite la ejecución de los permisos en la ubicación de carga de archivos.&dagger;</span><span class="sxs-lookup"><span data-stu-id="671af-359">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="671af-360">Los archivos cargados **no** se deben persistir en el mismo árbol de directorio que la aplicación.&dagger;</span><span class="sxs-lookup"><span data-stu-id="671af-360">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="671af-361">Use un nombre de archivo seguro determinado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="671af-361">Use a safe file name determined by the app.</span></span> <span data-ttu-id="671af-362">No use un nombre de archivo que proporcione el usuario ni el nombre de archivo que no es de confianza del archivo cargado.&dagger; HTML codifica el nombre de archivo que no es de confianza al mostrarlo.</span><span class="sxs-lookup"><span data-stu-id="671af-362">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="671af-363">Por ejemplo, al registrar el nombre de archivo o mostrarlo en la interfaz de usuario (Razor codifica de forma automática la salida HTML).</span><span class="sxs-lookup"><span data-stu-id="671af-363">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="671af-364">Permita solo las extensiones de archivo aprobadas para la especificación de diseño de la aplicación.&dagger;</span><span class="sxs-lookup"><span data-stu-id="671af-364">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="671af-365">Compruebe que se realizan comprobaciones del lado cliente en el servidor.&dagger; Las comprobaciones de cliente son fáciles de sortear.</span><span class="sxs-lookup"><span data-stu-id="671af-365">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="671af-366">Compruebe el tamaño de un archivo cargado.</span><span class="sxs-lookup"><span data-stu-id="671af-366">Check the size of an uploaded file.</span></span> <span data-ttu-id="671af-367">Establezca un límite de tamaño máximo para evitar cargas grandes.&dagger;</span><span class="sxs-lookup"><span data-stu-id="671af-367">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="671af-368">Cuando un archivo cargado con el mismo nombre no deba sobrescribir los archivos, vuelva a comprobar el nombre de archivo en la base de datos o en el almacenamiento físico antes de cargarlo.</span><span class="sxs-lookup"><span data-stu-id="671af-368">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="671af-369">**Ejecute un detector de virus o malware en el contenido cargado antes de que se almacene el archivo.**</span><span class="sxs-lookup"><span data-stu-id="671af-369">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="671af-370">&dagger;La aplicación de ejemplo muestra un enfoque que cumple los criterios.</span><span class="sxs-lookup"><span data-stu-id="671af-370">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="671af-371">La carga de código malintencionado en un sistema suele ser el primer paso para ejecutar código que puede:</span><span class="sxs-lookup"><span data-stu-id="671af-371">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="671af-372">Obtener el control completo de un sistema.</span><span class="sxs-lookup"><span data-stu-id="671af-372">Completely gain control of a system.</span></span>
> * <span data-ttu-id="671af-373">Sobrecargar un sistema de manera que el sistema se bloquea.</span><span class="sxs-lookup"><span data-stu-id="671af-373">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="671af-374">Poner en peligro los datos del usuario o del sistema.</span><span class="sxs-lookup"><span data-stu-id="671af-374">Compromise user or system data.</span></span>
> * <span data-ttu-id="671af-375">Aplicar grafitis a una interfaz de usuario pública.</span><span class="sxs-lookup"><span data-stu-id="671af-375">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="671af-376">Vea los siguientes recursos para más información sobre cómo reducir el área expuesta de ataques al aceptar archivos de los usuarios:</span><span class="sxs-lookup"><span data-stu-id="671af-376">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="671af-377">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Carga de archivos sin restricciones)</span><span class="sxs-lookup"><span data-stu-id="671af-377">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * [<span data-ttu-id="671af-378">Azure Security: Asegúrese de que los controles adecuados estén en vigor al aceptar archivos de usuarios</span><span class="sxs-lookup"><span data-stu-id="671af-378">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="671af-379">Para más información sobre cómo implementar medidas de seguridad, incluidos ejemplos de la aplicación de ejemplo, consulte la sección [Validación](#validation).</span><span class="sxs-lookup"><span data-stu-id="671af-379">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="671af-380">Escenarios de almacenamiento</span><span class="sxs-lookup"><span data-stu-id="671af-380">Storage scenarios</span></span>

<span data-ttu-id="671af-381">Las opciones de almacenamiento comunes para los archivos incluyen:</span><span class="sxs-lookup"><span data-stu-id="671af-381">Common storage options for files include:</span></span>

* <span data-ttu-id="671af-382">Base de datos</span><span class="sxs-lookup"><span data-stu-id="671af-382">Database</span></span>

  * <span data-ttu-id="671af-383">En el caso de cargas de archivos pequeñas, una base de datos suele ser más rápida que las opciones de almacenamiento físico (sistema de archivos o recurso compartido de red).</span><span class="sxs-lookup"><span data-stu-id="671af-383">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="671af-384">Una base de datos suele ser más conveniente que las opciones de almacenamiento físico, ya que la recuperación de un registro de base de datos para los datos de usuario puede proporcionar el contenido del archivo (por ejemplo, una imagen de avatar).</span><span class="sxs-lookup"><span data-stu-id="671af-384">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="671af-385">Una base de datos puede ser más económica que usar un servicio de almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="671af-385">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="671af-386">Almacenamiento físico (sistema de archivos o recurso compartido de red)</span><span class="sxs-lookup"><span data-stu-id="671af-386">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="671af-387">Para cargas de archivos de gran tamaño:</span><span class="sxs-lookup"><span data-stu-id="671af-387">For large file uploads:</span></span>
    * <span data-ttu-id="671af-388">Los límites de base de datos pueden restringir el tamaño de la carga.</span><span class="sxs-lookup"><span data-stu-id="671af-388">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="671af-389">El almacenamiento físico suele ser menos económico que el almacenamiento en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="671af-389">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="671af-390">El almacenamiento físico puede ser más económico que usar un servicio de almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="671af-390">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="671af-391">El proceso de la aplicación debe tener permisos de lectura y escritura en la ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="671af-391">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="671af-392">**Nunca conceda el permiso de ejecución.**</span><span class="sxs-lookup"><span data-stu-id="671af-392">**Never grant execute permission.**</span></span>

* <span data-ttu-id="671af-393">Servicio de almacenamiento de datos (por ejemplo, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="671af-393">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="671af-394">Los servicios suelen ofrecer una escalabilidad y resistencia mejoradas sobre las soluciones locales que normalmente están sujetas a únicos puntos de error.</span><span class="sxs-lookup"><span data-stu-id="671af-394">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="671af-395">Los servicios pueden tener un costo menor en escenarios de infraestructura de almacenamiento de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="671af-395">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="671af-396">Para obtener más información, vea [Inicio rápido: Uso de .NET para crear un blob en el almacenamiento de objetos](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="671af-396">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="671af-397">En el tema se muestra <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, pero se puede usar <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> para guardar un <xref:System.IO.FileStream> en el almacenamiento de blobs cuando se trabaja con un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="671af-397">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="671af-398">Escenarios de carga de archivos</span><span class="sxs-lookup"><span data-stu-id="671af-398">File upload scenarios</span></span>

<span data-ttu-id="671af-399">Dos enfoques generales para cargar archivos son el almacenamiento en búfer y el streaming.</span><span class="sxs-lookup"><span data-stu-id="671af-399">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="671af-400">**Almacenamiento en búfer**</span><span class="sxs-lookup"><span data-stu-id="671af-400">**Buffering**</span></span>

<span data-ttu-id="671af-401">El archivo completo se lee en un <xref:Microsoft.AspNetCore.Http.IFormFile>, que es una representación de C# del archivo que se usa para procesar o guardar el archivo.</span><span class="sxs-lookup"><span data-stu-id="671af-401">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="671af-402">Los recursos (disco, memoria) que se usan en las cargas de archivos dependen de la cantidad y del tamaño de las cargas de archivos que se realizan simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="671af-402">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="671af-403">Si una aplicación intenta almacenar demasiadas cargas en el búfer, el sitio se bloquea cuando se queda sin memoria o sin espacio en disco.</span><span class="sxs-lookup"><span data-stu-id="671af-403">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="671af-404">Si el tamaño o la frecuencia de las cargas de archivos está agotando los recursos de la aplicación, use el streaming.</span><span class="sxs-lookup"><span data-stu-id="671af-404">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="671af-405">Cualquier archivo almacenado en búfer único que supere los 64 KB se mueve de la memoria a un archivo temporal en el disco.</span><span class="sxs-lookup"><span data-stu-id="671af-405">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="671af-406">En las secciones siguientes de este tema se habla del almacenamiento en búfer de archivos pequeños:</span><span class="sxs-lookup"><span data-stu-id="671af-406">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="671af-407">Almacenamiento físico</span><span class="sxs-lookup"><span data-stu-id="671af-407">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="671af-408">Base de datos</span><span class="sxs-lookup"><span data-stu-id="671af-408">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="671af-409">**Streaming**</span><span class="sxs-lookup"><span data-stu-id="671af-409">**Streaming**</span></span>

<span data-ttu-id="671af-410">El archivo se recibe de una solicitud de varias partes y lo procesa o guarda directamente la aplicación.</span><span class="sxs-lookup"><span data-stu-id="671af-410">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="671af-411">El streaming no mejora considerablemente el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="671af-411">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="671af-412">El streaming reduce las demandas de memoria o espacio en disco cuando se cargan archivos.</span><span class="sxs-lookup"><span data-stu-id="671af-412">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="671af-413">El streaming de archivos grandes se describe en la sección [Carga de archivos de gran tamaño con streaming](#upload-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="671af-413">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="671af-414">Carga de archivos pequeños con enlace de modelos almacenado en búfer al almacenamiento físico</span><span class="sxs-lookup"><span data-stu-id="671af-414">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="671af-415">Para cargar archivos pequeños, se puede usar un formulario de varias partes o construir una solicitud POST con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="671af-415">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="671af-416">En el ejemplo siguiente se muestra el uso de un formulario de Razor Pages para cargar un archivo único (*Pages/BufferedSingleFileUploadPhysical.cshtml* en la aplicación de ejemplo):</span><span class="sxs-lookup"><span data-stu-id="671af-416">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

<span data-ttu-id="671af-417">El ejemplo siguiente es análogo al ejemplo anterior, salvo en que:</span><span class="sxs-lookup"><span data-stu-id="671af-417">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="671af-418">([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) de JavaScript se usa para enviar los datos del formulario.</span><span class="sxs-lookup"><span data-stu-id="671af-418">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="671af-419">No hay ninguna validación.</span><span class="sxs-lookup"><span data-stu-id="671af-419">There's no validation.</span></span>

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

<span data-ttu-id="671af-420">Para realizar la solicitud POST en JavaScript para los clientes que [no admiten Fetch API](https://caniuse.com/#feat=fetch), use uno de estos enfoques:</span><span class="sxs-lookup"><span data-stu-id="671af-420">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="671af-421">Use un Fetch Polyfill (por ejemplo, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="671af-421">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="671af-422">Use `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="671af-422">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="671af-423">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="671af-423">For example:</span></span>

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

<span data-ttu-id="671af-424">Para admitir las cargas de archivos, los formularios HTML deben especificar un tipo de codificación (`enctype`) de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="671af-424">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="671af-425">Para que un elemento de entrada `files` admita al carga de varios archivos, proporcione el atributo `multiple` en el elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="671af-425">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="671af-426">Es posible acceder a archivos individuales cargados en el servidor a través del [enlace de modelos](xref:mvc/models/model-binding) mediante <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="671af-426">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="671af-427">La aplicación de ejemplo muestra varias cargas de archivos almacenados en búfer para escenarios de almacenamiento físico y base de datos.</span><span class="sxs-lookup"><span data-stu-id="671af-427">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename2"></a>

> [!WARNING]
> <span data-ttu-id="671af-428">**No** utilice la propiedad `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> para usos distintos a la presentación y el registro.</span><span class="sxs-lookup"><span data-stu-id="671af-428">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="671af-429">Para fines de presentación y registro, codifique el nombre de archivo en HTML.</span><span class="sxs-lookup"><span data-stu-id="671af-429">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="671af-430">Un atacante puede proporcionar un nombre de archivo malintencionado, incluidas rutas de acceso completas o relativas.</span><span class="sxs-lookup"><span data-stu-id="671af-430">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="671af-431">Las aplicaciones deben:</span><span class="sxs-lookup"><span data-stu-id="671af-431">Applications should:</span></span>
>
> * <span data-ttu-id="671af-432">Quitar la ruta de acceso del nombre de archivo proporcionado por el usuario.</span><span class="sxs-lookup"><span data-stu-id="671af-432">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="671af-433">Guardar el nombre de archivo codificado en HTML, sin la ruta de acceso para la interfaz de usuario o el registro.</span><span class="sxs-lookup"><span data-stu-id="671af-433">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="671af-434">Generar un nombre de archivo aleatorio nuevo para el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="671af-434">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="671af-435">En el código siguiente se quita la ruta de acceso del nombre del archivo:</span><span class="sxs-lookup"><span data-stu-id="671af-435">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="671af-436">Los ejemplos proporcionados hasta ahora no tienen en cuenta las consideraciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="671af-436">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="671af-437">Se proporciona información adicional en las secciones siguientes y en la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="671af-437">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="671af-438">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="671af-438">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="671af-439">Validación</span><span class="sxs-lookup"><span data-stu-id="671af-439">Validation</span></span>](#validation)

<span data-ttu-id="671af-440">Al cargar archivos mediante el enlace de modelos y <xref:Microsoft.AspNetCore.Http.IFormFile>, el método de acción puede aceptar:</span><span class="sxs-lookup"><span data-stu-id="671af-440">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="671af-441">Un <xref:Microsoft.AspNetCore.Http.IFormFile> único.</span><span class="sxs-lookup"><span data-stu-id="671af-441">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="671af-442">Cualquiera de las colecciones siguientes que representan varios archivos:</span><span class="sxs-lookup"><span data-stu-id="671af-442">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="671af-443">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="671af-443">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="671af-444">El enlace coincide con los archivos de formulario por nombre.</span><span class="sxs-lookup"><span data-stu-id="671af-444">Binding matches form files by name.</span></span> <span data-ttu-id="671af-445">Por ejemplo, el valor `name` HTML en `<input type="file" name="formFile">` debe coincidir con la propiedad/el parámetro enlazado de C# (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="671af-445">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="671af-446">Para más información, consulte la sección [Coincidencia del valor de atributo de nombre con el nombre del parámetro del método POST](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="671af-446">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="671af-447">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="671af-447">The following example:</span></span>

* <span data-ttu-id="671af-448">Recorre en bucle uno o más archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="671af-448">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="671af-449">Usa [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) para devolver una ruta de acceso completa de un archivo, incluido el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="671af-449">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="671af-450">Guarda los archivos en el sistema de archivos local con un nombre de archivo generado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="671af-450">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="671af-451">Devuelve el número total y el tamaño de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="671af-451">Returns the total number and size of files uploaded.</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

<span data-ttu-id="671af-452">Use `Path.GetRandomFileName` para generar un nombre de archivo sin una ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="671af-452">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="671af-453">En el ejemplo siguiente, la ruta de acceso se obtiene de la configuración:</span><span class="sxs-lookup"><span data-stu-id="671af-453">In the following example, the path is obtained from configuration:</span></span>

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

<span data-ttu-id="671af-454">La ruta de acceso pasada al <xref:System.IO.FileStream> *debe* incluir el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="671af-454">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="671af-455">Si no se proporciona el nombre de archivo, se produce una <xref:System.UnauthorizedAccessException> en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="671af-455">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="671af-456">Los archivos que se cargan usando la técnica <xref:Microsoft.AspNetCore.Http.IFormFile> se almacenan en búfer en memoria o en disco en el servidor web antes de procesarse.</span><span class="sxs-lookup"><span data-stu-id="671af-456">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="671af-457">Dentro del método de acción, se puede tener acceso al contenido de <xref:Microsoft.AspNetCore.Http.IFormFile> como <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="671af-457">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="671af-458">Además del sistema de archivos local, los archivos se pueden guardar en un recurso compartido de red o en un servicio de almacenamiento de archivos, como [Azure Blob Storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="671af-458">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="671af-459">Para ver otro ejemplo que recorre en bucle varios archivos para cargar y usa nombres de archivo seguros, consulte *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="671af-459">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="671af-460">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) arroja una <xref:System.IO.IOException> si se crean más de 65 535 archivos sin eliminar los archivos temporales anteriores.</span><span class="sxs-lookup"><span data-stu-id="671af-460">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="671af-461">El límite de 65 535 archivos es un límite por servidor.</span><span class="sxs-lookup"><span data-stu-id="671af-461">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="671af-462">Para más información sobre este límite en el sistema operativo Windows, consulte las notas en los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="671af-462">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="671af-463">Función GetTempFileNameA</span><span class="sxs-lookup"><span data-stu-id="671af-463">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="671af-464">Carga de archivos pequeños con enlace de modelos almacenado en búfer a una base de datos</span><span class="sxs-lookup"><span data-stu-id="671af-464">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="671af-465">Para almacenar datos de archivo binario en una base de datos con [Entity Framework](/ef/core/index), defina una propiedad de matriz <xref:System.Byte> en la entidad:</span><span class="sxs-lookup"><span data-stu-id="671af-465">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="671af-466">Especifique una propiedad de modelo de página para la clase que incluya un <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="671af-466">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="671af-467"><xref:Microsoft.AspNetCore.Http.IFormFile> se puede usar directamente como un parámetro de método de acción o como una propiedad de modelo enlazado.</span><span class="sxs-lookup"><span data-stu-id="671af-467"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="671af-468">En el ejemplo anterior se utiliza una propiedad de modelo enlazado.</span><span class="sxs-lookup"><span data-stu-id="671af-468">The prior example uses a bound model property.</span></span>

<span data-ttu-id="671af-469">`FileUpload` se usa en el formulario de Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="671af-469">The `FileUpload` is used in the Razor Pages form:</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

<span data-ttu-id="671af-470">Cuando el formulario se publique en el servidor, copie el <xref:Microsoft.AspNetCore.Http.IFormFile> en un flujo y guárdelo como matriz de bytes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="671af-470">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="671af-471">En el ejemplo siguiente, `_dbContext` almacena el contexto de base de datos de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="671af-471">In the following example, `_dbContext` stores the app's database context:</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

<span data-ttu-id="671af-472">El ejemplo anterior es similar a un escenario que se muestra en la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="671af-472">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="671af-473">*Pages/BufferedSingleFileUploadDb.cshtml*</span><span class="sxs-lookup"><span data-stu-id="671af-473">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="671af-474">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="671af-474">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="671af-475">Tenga cuidado al almacenar los datos binarios en bases de datos relacionales, ya que esto puede repercutir adversamente en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="671af-475">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="671af-476">No se base ni confíe en la propiedad `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sin validarla.</span><span class="sxs-lookup"><span data-stu-id="671af-476">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="671af-477">La propiedad `FileName` solo debe usarse para fines de presentación y solo después de la codificación HTML.</span><span class="sxs-lookup"><span data-stu-id="671af-477">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="671af-478">Los ejemplos proporcionados no tienen en cuenta las consideraciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="671af-478">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="671af-479">Se proporciona información adicional en las secciones siguientes y en la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="671af-479">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="671af-480">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="671af-480">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="671af-481">Validación</span><span class="sxs-lookup"><span data-stu-id="671af-481">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="671af-482">Carga de archivos de gran tamaño con streaming</span><span class="sxs-lookup"><span data-stu-id="671af-482">Upload large files with streaming</span></span>

<span data-ttu-id="671af-483">En el ejemplo siguiente se muestra cómo usar JavaScript para transmitir un archivo a una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="671af-483">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="671af-484">El token de antifalsificación del archivo se genera por medio de un atributo de filtro personalizado y se pasa a los encabezados HTTP del cliente, en lugar de en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="671af-484">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="671af-485">Dado que el método de acción procesa los datos cargados directamente, el enlace de modelos del formulario se deshabilita por otro filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="671af-485">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="671af-486">Dentro de la acción, el contenido del formulario se lee usando un `MultipartReader` (que lee cada `MultipartSection` individual), de forma que el archivo se procesa o el contenido se almacena, según corresponda.</span><span class="sxs-lookup"><span data-stu-id="671af-486">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="671af-487">Una vez leídas las secciones de varias partes, la acción realiza su propio enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="671af-487">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="671af-488">La respuesta de la página inicial carga el formulario y guarda un token de antifalsificación en una cookie (a través del atributo `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="671af-488">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="671af-489">El atributo usa la [compatibilidad de antifalsificación](xref:security/anti-request-forgery) integrada de ASP.NET Core para establecer una cookie con un token de solicitud:</span><span class="sxs-lookup"><span data-stu-id="671af-489">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="671af-490">El `DisableFormValueModelBindingAttribute` se usa para deshabilitar el enlace de modelos:</span><span class="sxs-lookup"><span data-stu-id="671af-490">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="671af-491">En la aplicación de ejemplo, `GenerateAntiforgeryTokenCookieAttribute` y `DisableFormValueModelBindingAttribute` se aplican como filtros a los modelos de aplicación de la página de `/StreamedSingleFileUploadDb` y `/StreamedSingleFileUploadPhysical` en `Startup.ConfigureServices` con las [convenciones de Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="671af-491">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="671af-492">Dado que el enlace de modelos no lee el formulario, los parámetros enlazados desde el formulario no se enlazan (la consulta, la ruta y el encabezado siguen funcionando).</span><span class="sxs-lookup"><span data-stu-id="671af-492">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="671af-493">El método de acción funciona directamente con la propiedad `Request`.</span><span class="sxs-lookup"><span data-stu-id="671af-493">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="671af-494">Se usa un elemento `MultipartReader` para leer cada sección.</span><span class="sxs-lookup"><span data-stu-id="671af-494">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="671af-495">Los datos de clave-valor se almacenan en un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="671af-495">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="671af-496">Una vez leídas las secciones de varias partes, el contenido del `KeyValueAccumulator` se usa para enlazar los datos del formulario a un tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="671af-496">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="671af-497">El método `StreamingController.UploadDatabase` completo para streaming a una base de datos con EF Core:</span><span class="sxs-lookup"><span data-stu-id="671af-497">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="671af-498">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span><span class="sxs-lookup"><span data-stu-id="671af-498">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="671af-499">El método `StreamingController.UploadPhysical` completo para streaming a una ubicación física:</span><span class="sxs-lookup"><span data-stu-id="671af-499">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="671af-500">En la aplicación de ejemplo, las comprobaciones de validación las controla `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="671af-500">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="671af-501">Validación</span><span class="sxs-lookup"><span data-stu-id="671af-501">Validation</span></span>

<span data-ttu-id="671af-502">La clase `FileHelpers` de la aplicación de ejemplo muestra varias comprobaciones de cargas de archivos de streaming y <xref:Microsoft.AspNetCore.Http.IFormFile> almacenado en búfer.</span><span class="sxs-lookup"><span data-stu-id="671af-502">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="671af-503">Para procesar cargas de archivos almacenadas en búfer de <xref:Microsoft.AspNetCore.Http.IFormFile> en la aplicación de ejemplo, consulte el método `ProcessFormFile` en el archivo *Utilities/FileHelpers.cs*.</span><span class="sxs-lookup"><span data-stu-id="671af-503">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="671af-504">Para procesar archivos de streaming, consulte el método `ProcessStreamedFile` en el mismo archivo.</span><span class="sxs-lookup"><span data-stu-id="671af-504">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="671af-505">Los métodos de procesamiento de validación mostrados en la aplicación de ejemplo no examinan el contenido de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="671af-505">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="671af-506">En la mayoría de los escenarios de producción, se usa una API de analizador de virus/malware en el archivo antes de que el archivo esté disponible para los usuarios u otros sistemas.</span><span class="sxs-lookup"><span data-stu-id="671af-506">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="671af-507">Aunque el ejemplo de tema proporciona un ejemplo funcional de técnicas de validación, no implemente la clase `FileHelpers` en una aplicación de producción a menos que:</span><span class="sxs-lookup"><span data-stu-id="671af-507">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="671af-508">Comprenda totalmente la implementación.</span><span class="sxs-lookup"><span data-stu-id="671af-508">Fully understand the implementation.</span></span>
> * <span data-ttu-id="671af-509">Modifique la implementación según corresponda en función del entorno y las especificaciones de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="671af-509">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="671af-510">**No implemente nunca de manera indiscriminada el código de seguridad en una aplicación sin abordar estos requisitos.**</span><span class="sxs-lookup"><span data-stu-id="671af-510">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="671af-511">Validación del contenido</span><span class="sxs-lookup"><span data-stu-id="671af-511">Content validation</span></span>

<span data-ttu-id="671af-512">**Use una API de detección de virus/malware de terceros en el contenido cargado.**</span><span class="sxs-lookup"><span data-stu-id="671af-512">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="671af-513">El análisis de archivos exige recursos del servidor en escenarios de gran volumen.</span><span class="sxs-lookup"><span data-stu-id="671af-513">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="671af-514">Si disminuye el rendimiento de procesamiento de solicitudes debido al análisis de archivos, considere la posibilidad de descargar el trabajo de análisis a un [servicio en segundo plano](xref:fundamentals/host/hosted-services), posiblemente un servicio que se ejecute en otro servidor desde el servidor de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="671af-514">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="671af-515">Habitualmente, los archivos cargados se almacenan en un área en cuarentena hasta que el programa de detección de virus en segundo plano los revisa.</span><span class="sxs-lookup"><span data-stu-id="671af-515">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="671af-516">Cuando se pasa un archivo, se mueve a la ubicación de almacenamiento de archivos habitual.</span><span class="sxs-lookup"><span data-stu-id="671af-516">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="671af-517">Por lo general, estos pasos se realizan junto con un registro de base de datos que indica el estado de análisis de un archivo.</span><span class="sxs-lookup"><span data-stu-id="671af-517">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="671af-518">Mediante el uso de este enfoque, la aplicación y el servidor de aplicaciones permanecen centrados en responder a las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="671af-518">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="671af-519">Validación de la extensión del archivo</span><span class="sxs-lookup"><span data-stu-id="671af-519">File extension validation</span></span>

<span data-ttu-id="671af-520">La extensión del archivo cargado debe comprobarse con una lista de extensiones permitidas.</span><span class="sxs-lookup"><span data-stu-id="671af-520">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="671af-521">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="671af-521">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="671af-522">Validación de firma del archivo</span><span class="sxs-lookup"><span data-stu-id="671af-522">File signature validation</span></span>

<span data-ttu-id="671af-523">La firma de un archivo viene determinada por los primeros bytes al principio de un archivo.</span><span class="sxs-lookup"><span data-stu-id="671af-523">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="671af-524">Estos bytes se pueden usar para indicar si la extensión coincide con el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="671af-524">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="671af-525">La aplicación de ejemplo comprueba las firmas de archivo de algunos tipos de archivo comunes.</span><span class="sxs-lookup"><span data-stu-id="671af-525">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="671af-526">En el ejemplo siguiente, la firma de archivo de una imagen JPEG se comprueba con respecto al archivo:</span><span class="sxs-lookup"><span data-stu-id="671af-526">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

<span data-ttu-id="671af-527">Para obtener firmas de archivo adicionales, consulte la [base de datos de firmas de archivo](https://www.filesignatures.net/) y las especificaciones de archivo oficiales.</span><span class="sxs-lookup"><span data-stu-id="671af-527">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="671af-528">Seguridad de nombre de archivo</span><span class="sxs-lookup"><span data-stu-id="671af-528">File name security</span></span>

<span data-ttu-id="671af-529">Nunca use un nombre de archivo proporcionado por el cliente para guardar un archivo en el almacenamiento físico.</span><span class="sxs-lookup"><span data-stu-id="671af-529">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="671af-530">Cree un nombre de archivo seguro para el archivo con [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) o [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) para crear una ruta de acceso completa (incluido el nombre de archivo) para el almacenamiento temporal.</span><span class="sxs-lookup"><span data-stu-id="671af-530">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="671af-531">Razor codifica automáticamente en HTML los valores de propiedad para mostrarlos.</span><span class="sxs-lookup"><span data-stu-id="671af-531">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="671af-532">El código siguiente es seguro de usar:</span><span class="sxs-lookup"><span data-stu-id="671af-532">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="671af-533">Fuera de Razor, siempre use <xref:System.Net.WebUtility.HtmlEncode*> al contenido del nombre de archivo de una solicitud de un usuario.</span><span class="sxs-lookup"><span data-stu-id="671af-533">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="671af-534">Muchas implementaciones deben incluir una comprobación de que el archivo existe; de lo contrario, el archivo se sobrescribe por un archivo con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="671af-534">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="671af-535">Proporcione lógica adicional para satisfacer las especificaciones de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="671af-535">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="671af-536">Validación del tamaño</span><span class="sxs-lookup"><span data-stu-id="671af-536">Size validation</span></span>

<span data-ttu-id="671af-537">Limite el tamaño de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="671af-537">Limit the size of uploaded files.</span></span>

<span data-ttu-id="671af-538">En la aplicación de ejemplo, el tamaño del archivo está limitado a 2 MB (se indica en bytes).</span><span class="sxs-lookup"><span data-stu-id="671af-538">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="671af-539">El límite se proporciona a través de [Configuración](xref:fundamentals/configuration/index) del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="671af-539">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="671af-540">`FileSizeLimit` se inserta en las clases `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="671af-540">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

<span data-ttu-id="671af-541">Cuando el tamaño de un archivo supera el límite, se rechaza el archivo:</span><span class="sxs-lookup"><span data-stu-id="671af-541">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="671af-542">Coincidencia del valor de atributo de nombre con el nombre del parámetro del método POST</span><span class="sxs-lookup"><span data-stu-id="671af-542">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="671af-543">En los formularios que no son de Razor que realizan la operación POST en los datos de formulario o usan directamente `FormData` de JavaScript, el nombre especificado en el elemento del formulario o `FormData` debe coincidir con el nombre del parámetro en la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="671af-543">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="671af-544">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="671af-544">In the following example:</span></span>

* <span data-ttu-id="671af-545">Cuando se usa un elemento `<input>`, el atributo `name` se establece en el valor `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="671af-545">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="671af-546">Cuando se usa `FormData` en JavaScript, el nombre se establece en el valor `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="671af-546">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="671af-547">Use un nombre coincidente para el parámetro del método de C# (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="671af-547">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="671af-548">Para un método de control de páginas de Razor Pages denominado `Upload`:</span><span class="sxs-lookup"><span data-stu-id="671af-548">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="671af-549">Para un método de acción de controlador POST de MVC:</span><span class="sxs-lookup"><span data-stu-id="671af-549">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="671af-550">Configuración del servidor y de la aplicación</span><span class="sxs-lookup"><span data-stu-id="671af-550">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="671af-551">Límite de longitud del cuerpo de varias partes</span><span class="sxs-lookup"><span data-stu-id="671af-551">Multipart body length limit</span></span>

<span data-ttu-id="671af-552"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> establece el límite de la longitud de cada cuerpo de varias partes.</span><span class="sxs-lookup"><span data-stu-id="671af-552"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="671af-553">Las secciones del formulario que superan este límite inician una <xref:System.IO.InvalidDataException> cuando se analizan.</span><span class="sxs-lookup"><span data-stu-id="671af-553">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="671af-554">El valor predeterminado es 134 217 728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="671af-554">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="671af-555">Personalice el límite mediante el valor <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="671af-555">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="671af-556"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> se utiliza para establecer el <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> para una sola página o acción.</span><span class="sxs-lookup"><span data-stu-id="671af-556"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="671af-557">En una aplicación de Razor Pages, aplique el filtro con una [convención](xref:razor-pages/razor-pages-conventions) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="671af-557">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model.Filters.Add(
                    new RequestFormLimitsAttribute()
                    {
                        // Set the limit to 256 MB
                        MultipartBodyLengthLimit = 268435456
                    });
    })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="671af-558">En una aplicación de Razor Pages o una aplicación MVC, aplique el filtro al método de acción o al modelo de página:</span><span class="sxs-lookup"><span data-stu-id="671af-558">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="671af-559">Tamaño máximo del cuerpo de la solicitud de Kestrel</span><span class="sxs-lookup"><span data-stu-id="671af-559">Kestrel maximum request body size</span></span>

<span data-ttu-id="671af-560">En el caso de las aplicaciones hospedadas por Kestrel, el tamaño máximo predeterminado del cuerpo de solicitud es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="671af-560">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="671af-561">Personalice el límite con la opción [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) del servidor de Kestrel:</span><span class="sxs-lookup"><span data-stu-id="671af-561">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        });
```

<span data-ttu-id="671af-562"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> se usa para establecer el valor de [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) de una sola página o acción.</span><span class="sxs-lookup"><span data-stu-id="671af-562"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="671af-563">En una aplicación de Razor Pages, aplique el filtro con una [convención](xref:razor-pages/razor-pages-conventions) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="671af-563">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model =>
                {
                    // Handle requests up to 50 MB
                    model.Filters.Add(
                        new RequestSizeLimitAttribute(52428800));
                });
    })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="671af-564">En una aplicación de Razor Pages o una aplicación MVC, aplique el filtro al método de acción o a la clase de control de página:</span><span class="sxs-lookup"><span data-stu-id="671af-564">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="671af-565">Otros límites de Kestrel</span><span class="sxs-lookup"><span data-stu-id="671af-565">Other Kestrel limits</span></span>

<span data-ttu-id="671af-566">Otros límites de Kestrel pueden aplicarse a las aplicaciones hospedadas por Kestrel:</span><span class="sxs-lookup"><span data-stu-id="671af-566">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="671af-567">Conexiones de cliente máximas</span><span class="sxs-lookup"><span data-stu-id="671af-567">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="671af-568">Tarifas de datos de solicitud y respuesta</span><span class="sxs-lookup"><span data-stu-id="671af-568">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="671af-569">Límite de longitud del contenido de IIS</span><span class="sxs-lookup"><span data-stu-id="671af-569">IIS content length limit</span></span>

<span data-ttu-id="671af-570">El límite predeterminado de solicitudes (`maxAllowedContentLength`) es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="671af-570">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="671af-571">Personalice el límite en el archivo *web.config*:</span><span class="sxs-lookup"><span data-stu-id="671af-571">Customize the limit in the *web.config* file:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="671af-572">Esto solo ocurre en IIS;</span><span class="sxs-lookup"><span data-stu-id="671af-572">This setting only applies to IIS.</span></span> <span data-ttu-id="671af-573">este comportamiento no sucede de forma predeterminada cuando los archivos se hospedan en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="671af-573">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="671af-574">Para más información, consulte [Límites de solicitud \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="671af-574">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="671af-575">Las limitaciones del módulo ASP.NET Core o la presencia del módulo de filtrado de solicitudes de IIS pueden limitar las cargas a 2 GB o 4 GB.</span><span class="sxs-lookup"><span data-stu-id="671af-575">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="671af-576">Para más información, consulte el artículo que indica que [no se pueden cargar archivos con un tamaño superior a 2 GB (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="671af-576">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="671af-577">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="671af-577">Troubleshoot</span></span>

<span data-ttu-id="671af-578">Aquí incluimos algunos problemas comunes que pueden surgir al cargar archivos, así como sus posibles soluciones.</span><span class="sxs-lookup"><span data-stu-id="671af-578">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="671af-579">Error No encontrado al implementar en un servidor IIS</span><span class="sxs-lookup"><span data-stu-id="671af-579">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="671af-580">El error siguiente indica que el archivo cargado supera la longitud configurada del contenido del servidor:</span><span class="sxs-lookup"><span data-stu-id="671af-580">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="671af-581">Para más información sobre cómo aumentar el límite, consulte la sección [Límite de longitud del contenido de IIS](#iis-content-length-limit).</span><span class="sxs-lookup"><span data-stu-id="671af-581">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="671af-582">Error de conexión</span><span class="sxs-lookup"><span data-stu-id="671af-582">Connection failure</span></span>

<span data-ttu-id="671af-583">Un error de conexión y una conexión del servidor de restablecimiento probablemente indica que el archivo cargado supera el tamaño máximo del cuerpo de la solicitud de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="671af-583">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="671af-584">Para más información, consulte la sección [Tamaño máximo del cuerpo de la solicitud de Kestrel](#kestrel-maximum-request-body-size).</span><span class="sxs-lookup"><span data-stu-id="671af-584">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="671af-585">Los límites de conexión del cliente de Kestrel también pueden requerir ajustes.</span><span class="sxs-lookup"><span data-stu-id="671af-585">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="671af-586">Excepción de referencia nula con IFormFile</span><span class="sxs-lookup"><span data-stu-id="671af-586">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="671af-587">Si el controlador acepta archivos cargados con <xref:Microsoft.AspNetCore.Http.IFormFile>, pero el valor es `null`, confirme que el formulario HTML especifica un valor `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="671af-587">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="671af-588">Si este atributo no está establecido en el elemento `<form>`, no se llevará a cabo la carga del archivo y cualquier argumento <xref:Microsoft.AspNetCore.Http.IFormFile> enlazado será `null`.</span><span class="sxs-lookup"><span data-stu-id="671af-588">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="671af-589">Confirme también que la [nomenclatura de la carga en los datos de formulario coincide con la nomenclatura de la aplicación](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="671af-589">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="671af-590">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="671af-590">Additional resources</span></span>

* <span data-ttu-id="671af-591">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Carga de archivos sin restricciones)</span><span class="sxs-lookup"><span data-stu-id="671af-591">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
* [<span data-ttu-id="671af-592">Azure Security: Marco de seguridad: Validación de entrada | Mitigaciones</span><span class="sxs-lookup"><span data-stu-id="671af-592">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="671af-593">Patrones de diseño en la nube de Azure: Patrón Valet Key</span><span class="sxs-lookup"><span data-stu-id="671af-593">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
