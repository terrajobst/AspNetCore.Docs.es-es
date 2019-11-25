---
title: Carga de archivos en ASP.NET Core
author: guardrex
description: Cómo usar el enlace de modelos y el streaming para cargar archivos en ASP.NET Core MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: b57ad4fe62de38085c11d7026d278cc6e0c565ce
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963154"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="bdfff-103">Carga de archivos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bdfff-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="bdfff-104">Por [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/) y [Rutger Storm](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="bdfff-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bdfff-105">ASP.NET Core admite la carga de uno o varios archivos mediante el enlace de modelos almacenado en búfer de archivos más pequeños y el streaming no almacenado en búfer de archivos de mayor tamaño.</span><span class="sxs-lookup"><span data-stu-id="bdfff-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="bdfff-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bdfff-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="bdfff-107">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="bdfff-107">Security considerations</span></span>

<span data-ttu-id="bdfff-108">Tenga precaución al proporcionar a los usuarios la capacidad de cargar archivos en un servidor.</span><span class="sxs-lookup"><span data-stu-id="bdfff-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="bdfff-109">Es posible que los atacantes intenten lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bdfff-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="bdfff-110">Ejecutar ataques por [denegación de servicio](/windows-hardware/drivers/ifs/denial-of-service).</span><span class="sxs-lookup"><span data-stu-id="bdfff-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="bdfff-111">Cargar virus o malware.</span><span class="sxs-lookup"><span data-stu-id="bdfff-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="bdfff-112">Poner en riesgo redes y servidores de otras maneras.</span><span class="sxs-lookup"><span data-stu-id="bdfff-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="bdfff-113">Estos son algunos de los pasos de seguridad con los que se reduce la probabilidad de sufrir ataques:</span><span class="sxs-lookup"><span data-stu-id="bdfff-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="bdfff-114">Cargue los archivos a un área de carga de archivos dedicada, preferiblemente una unidad que no sea de sistema.</span><span class="sxs-lookup"><span data-stu-id="bdfff-114">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="bdfff-115">Una ubicación dedicada facilita la imposición de restricciones de seguridad en los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="bdfff-115">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="bdfff-116">Deshabilite la ejecución de los permisos en la ubicación de carga de archivos.&dagger;</span><span class="sxs-lookup"><span data-stu-id="bdfff-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="bdfff-117">Los archivos cargados **no** se deben persistir en el mismo árbol de directorio que la aplicación.&dagger;</span><span class="sxs-lookup"><span data-stu-id="bdfff-117">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="bdfff-118">Use un nombre de archivo seguro determinado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="bdfff-119">No use un nombre de archivo que proporcione el usuario ni el nombre de archivo que no es de confianza del archivo cargado.&dagger; HTML codifica el nombre de archivo que no es de confianza al mostrarlo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="bdfff-120">Por ejemplo, al registrar el nombre de archivo o mostrarlo en la interfaz de usuario (Razor codifica de forma automática la salida HTML).</span><span class="sxs-lookup"><span data-stu-id="bdfff-120">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="bdfff-121">Permita solo las extensiones de archivo aprobadas para la especificación de diseño de la aplicación.&dagger;</span><span class="sxs-lookup"><span data-stu-id="bdfff-121">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="bdfff-122">Compruebe que se realizan comprobaciones del lado cliente en el servidor.&dagger; Las comprobaciones de cliente son fáciles de sortear.</span><span class="sxs-lookup"><span data-stu-id="bdfff-122">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="bdfff-123">Compruebe el tamaño de un archivo cargado.</span><span class="sxs-lookup"><span data-stu-id="bdfff-123">Check the size of an uploaded file.</span></span> <span data-ttu-id="bdfff-124">Establezca un límite de tamaño máximo para evitar cargas grandes.&dagger;</span><span class="sxs-lookup"><span data-stu-id="bdfff-124">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="bdfff-125">Cuando un archivo cargado con el mismo nombre no deba sobrescribir los archivos, vuelva a comprobar el nombre de archivo en la base de datos o en el almacenamiento físico antes de cargarlo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-125">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="bdfff-126">**Ejecute un detector de virus o malware en el contenido cargado antes de que se almacene el archivo.**</span><span class="sxs-lookup"><span data-stu-id="bdfff-126">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="bdfff-127">&dagger;La aplicación de ejemplo muestra un enfoque que cumple los criterios.</span><span class="sxs-lookup"><span data-stu-id="bdfff-127">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="bdfff-128">La carga de código malintencionado en un sistema suele ser el primer paso para ejecutar código que puede:</span><span class="sxs-lookup"><span data-stu-id="bdfff-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="bdfff-129">Obtener el control completo de un sistema.</span><span class="sxs-lookup"><span data-stu-id="bdfff-129">Completely gain control of a system.</span></span>
> * <span data-ttu-id="bdfff-130">Sobrecargar un sistema de manera que el sistema se bloquea.</span><span class="sxs-lookup"><span data-stu-id="bdfff-130">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="bdfff-131">Poner en peligro los datos del usuario o del sistema.</span><span class="sxs-lookup"><span data-stu-id="bdfff-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="bdfff-132">Aplicar grafitis a una interfaz de usuario pública.</span><span class="sxs-lookup"><span data-stu-id="bdfff-132">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="bdfff-133">Vea los siguientes recursos para más información sobre cómo reducir el área expuesta de ataques al aceptar archivos de los usuarios:</span><span class="sxs-lookup"><span data-stu-id="bdfff-133">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="bdfff-134">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Carga de archivos sin restricciones)</span><span class="sxs-lookup"><span data-stu-id="bdfff-134">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * [<span data-ttu-id="bdfff-135">Azure Security: Asegúrese de que los controles adecuados estén en vigor al aceptar archivos de usuarios</span><span class="sxs-lookup"><span data-stu-id="bdfff-135">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="bdfff-136">Para más información sobre cómo implementar medidas de seguridad, incluidos ejemplos de la aplicación de ejemplo, consulte la sección [Validación](#validation).</span><span class="sxs-lookup"><span data-stu-id="bdfff-136">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="bdfff-137">Escenarios de almacenamiento</span><span class="sxs-lookup"><span data-stu-id="bdfff-137">Storage scenarios</span></span>

<span data-ttu-id="bdfff-138">Las opciones de almacenamiento comunes para los archivos incluyen:</span><span class="sxs-lookup"><span data-stu-id="bdfff-138">Common storage options for files include:</span></span>

* <span data-ttu-id="bdfff-139">Base de datos</span><span class="sxs-lookup"><span data-stu-id="bdfff-139">Database</span></span>

  * <span data-ttu-id="bdfff-140">En el caso de cargas de archivos pequeñas, una base de datos suele ser más rápida que las opciones de almacenamiento físico (sistema de archivos o recurso compartido de red).</span><span class="sxs-lookup"><span data-stu-id="bdfff-140">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="bdfff-141">Una base de datos suele ser más conveniente que las opciones de almacenamiento físico, ya que la recuperación de un registro de base de datos para los datos de usuario puede proporcionar el contenido del archivo (por ejemplo, una imagen de avatar).</span><span class="sxs-lookup"><span data-stu-id="bdfff-141">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="bdfff-142">Una base de datos puede ser más económica que usar un servicio de almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-142">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="bdfff-143">Almacenamiento físico (sistema de archivos o recurso compartido de red)</span><span class="sxs-lookup"><span data-stu-id="bdfff-143">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="bdfff-144">Para cargas de archivos de gran tamaño:</span><span class="sxs-lookup"><span data-stu-id="bdfff-144">For large file uploads:</span></span>
    * <span data-ttu-id="bdfff-145">Los límites de base de datos pueden restringir el tamaño de la carga.</span><span class="sxs-lookup"><span data-stu-id="bdfff-145">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="bdfff-146">El almacenamiento físico suele ser menos económico que el almacenamiento en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-146">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="bdfff-147">El almacenamiento físico puede ser más económico que usar un servicio de almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-147">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="bdfff-148">El proceso de la aplicación debe tener permisos de lectura y escritura en la ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="bdfff-148">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="bdfff-149">**Nunca conceda el permiso de ejecución.**</span><span class="sxs-lookup"><span data-stu-id="bdfff-149">**Never grant execute permission.**</span></span>

* <span data-ttu-id="bdfff-150">Servicio de almacenamiento de datos (por ejemplo, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="bdfff-150">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="bdfff-151">Los servicios suelen ofrecer una escalabilidad y resistencia mejoradas sobre las soluciones locales que normalmente están sujetas a únicos puntos de error.</span><span class="sxs-lookup"><span data-stu-id="bdfff-151">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="bdfff-152">Los servicios pueden tener un costo menor en escenarios de infraestructura de almacenamiento de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="bdfff-152">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="bdfff-153">Para obtener más información, vea [Inicio rápido: Uso de .NET para crear un blob en el almacenamiento de objetos](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="bdfff-153">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="bdfff-154">En el tema se muestra <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, pero se puede usar <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> para guardar un <xref:System.IO.FileStream> en el almacenamiento de blobs cuando se trabaja con un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="bdfff-154">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="bdfff-155">Escenarios de carga de archivos</span><span class="sxs-lookup"><span data-stu-id="bdfff-155">File upload scenarios</span></span>

<span data-ttu-id="bdfff-156">Dos enfoques generales para cargar archivos son el almacenamiento en búfer y el streaming.</span><span class="sxs-lookup"><span data-stu-id="bdfff-156">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="bdfff-157">**Almacenamiento en búfer**</span><span class="sxs-lookup"><span data-stu-id="bdfff-157">**Buffering**</span></span>

<span data-ttu-id="bdfff-158">El archivo completo se lee en un <xref:Microsoft.AspNetCore.Http.IFormFile>, que es una representación de C# del archivo que se usa para procesar o guardar el archivo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-158">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="bdfff-159">Los recursos (disco, memoria) que se usan en las cargas de archivos dependen de la cantidad y del tamaño de las cargas de archivos que se realizan simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="bdfff-159">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="bdfff-160">Si una aplicación intenta almacenar demasiadas cargas en el búfer, el sitio se bloquea cuando se queda sin memoria o sin espacio en disco.</span><span class="sxs-lookup"><span data-stu-id="bdfff-160">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="bdfff-161">Si el tamaño o la frecuencia de las cargas de archivos está agotando los recursos de la aplicación, use el streaming.</span><span class="sxs-lookup"><span data-stu-id="bdfff-161">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="bdfff-162">Cualquier archivo almacenado en búfer único que supere los 64 KB se mueve de la memoria a un archivo temporal en el disco.</span><span class="sxs-lookup"><span data-stu-id="bdfff-162">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="bdfff-163">En las secciones siguientes de este tema se habla del almacenamiento en búfer de archivos pequeños:</span><span class="sxs-lookup"><span data-stu-id="bdfff-163">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="bdfff-164">Almacenamiento físico</span><span class="sxs-lookup"><span data-stu-id="bdfff-164">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="bdfff-165">Base de datos</span><span class="sxs-lookup"><span data-stu-id="bdfff-165">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="bdfff-166">**Streaming**</span><span class="sxs-lookup"><span data-stu-id="bdfff-166">**Streaming**</span></span>

<span data-ttu-id="bdfff-167">El archivo se recibe de una solicitud de varias partes y lo procesa o guarda directamente la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-167">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="bdfff-168">El streaming no mejora considerablemente el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="bdfff-168">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="bdfff-169">El streaming reduce las demandas de memoria o espacio en disco cuando se cargan archivos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-169">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="bdfff-170">El streaming de archivos grandes se describe en la sección [Carga de archivos de gran tamaño con streaming](#upload-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="bdfff-170">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="bdfff-171">Carga de archivos pequeños con enlace de modelos almacenado en búfer al almacenamiento físico</span><span class="sxs-lookup"><span data-stu-id="bdfff-171">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="bdfff-172">Para cargar archivos pequeños, se puede usar un formulario de varias partes o construir una solicitud POST con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bdfff-172">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="bdfff-173">En el ejemplo siguiente se muestra el uso de un formulario de Razor Pages para cargar un archivo único (*Pages/BufferedSingleFileUploadPhysical.cshtml* en la aplicación de ejemplo):</span><span class="sxs-lookup"><span data-stu-id="bdfff-173">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="bdfff-174">El ejemplo siguiente es análogo al ejemplo anterior, salvo en que:</span><span class="sxs-lookup"><span data-stu-id="bdfff-174">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="bdfff-175">([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) de JavaScript se usa para enviar los datos del formulario.</span><span class="sxs-lookup"><span data-stu-id="bdfff-175">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="bdfff-176">No hay ninguna validación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-176">There's no validation.</span></span>

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

<span data-ttu-id="bdfff-177">Para realizar la solicitud POST en JavaScript para los clientes que [no admiten Fetch API](https://caniuse.com/#feat=fetch), use uno de estos enfoques:</span><span class="sxs-lookup"><span data-stu-id="bdfff-177">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="bdfff-178">Use un Fetch Polyfill (por ejemplo, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="bdfff-178">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="bdfff-179">Use `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-179">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="bdfff-180">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bdfff-180">For example:</span></span>

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

<span data-ttu-id="bdfff-181">Para admitir las cargas de archivos, los formularios HTML deben especificar un tipo de codificación (`enctype`) de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-181">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="bdfff-182">Para que un elemento de entrada `files` admita al carga de varios archivos, proporcione el atributo `multiple` en el elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-182">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="bdfff-183">Es posible acceder a archivos individuales cargados en el servidor a través del [enlace de modelos](xref:mvc/models/model-binding) mediante <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="bdfff-183">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="bdfff-184">La aplicación de ejemplo muestra varias cargas de archivos almacenados en búfer para escenarios de almacenamiento físico y base de datos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-184">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename"></a>

> [!WARNING]
> <span data-ttu-id="bdfff-185">**No** utilice la propiedad `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> para usos distintos a la presentación y el registro.</span><span class="sxs-lookup"><span data-stu-id="bdfff-185">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="bdfff-186">Para fines de presentación y registro, codifique el nombre de archivo en HTML.</span><span class="sxs-lookup"><span data-stu-id="bdfff-186">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="bdfff-187">Un atacante puede proporcionar un nombre de archivo malintencionado, incluidas rutas de acceso completas o relativas.</span><span class="sxs-lookup"><span data-stu-id="bdfff-187">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="bdfff-188">Las aplicaciones deben:</span><span class="sxs-lookup"><span data-stu-id="bdfff-188">Applications should:</span></span>
>
> * <span data-ttu-id="bdfff-189">Quitar la ruta de acceso del nombre de archivo proporcionado por el usuario.</span><span class="sxs-lookup"><span data-stu-id="bdfff-189">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="bdfff-190">Guardar el nombre de archivo codificado en HTML, sin la ruta de acceso para la interfaz de usuario o el registro.</span><span class="sxs-lookup"><span data-stu-id="bdfff-190">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="bdfff-191">Generar un nombre de archivo aleatorio nuevo para el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="bdfff-191">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="bdfff-192">En el código siguiente se quita la ruta de acceso del nombre del archivo:</span><span class="sxs-lookup"><span data-stu-id="bdfff-192">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="bdfff-193">Los ejemplos proporcionados hasta ahora no tienen en cuenta las consideraciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="bdfff-193">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="bdfff-194">Se proporciona información adicional en las secciones siguientes y en la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="bdfff-194">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="bdfff-195">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="bdfff-195">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="bdfff-196">Validación</span><span class="sxs-lookup"><span data-stu-id="bdfff-196">Validation</span></span>](#validation)

<span data-ttu-id="bdfff-197">Al cargar archivos mediante el enlace de modelos y <xref:Microsoft.AspNetCore.Http.IFormFile>, el método de acción puede aceptar:</span><span class="sxs-lookup"><span data-stu-id="bdfff-197">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="bdfff-198">Un <xref:Microsoft.AspNetCore.Http.IFormFile> único.</span><span class="sxs-lookup"><span data-stu-id="bdfff-198">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="bdfff-199">Cualquiera de las colecciones siguientes que representan varios archivos:</span><span class="sxs-lookup"><span data-stu-id="bdfff-199">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="bdfff-200">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="bdfff-200">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="bdfff-201">El enlace coincide con los archivos de formulario por nombre.</span><span class="sxs-lookup"><span data-stu-id="bdfff-201">Binding matches form files by name.</span></span> <span data-ttu-id="bdfff-202">Por ejemplo, el valor `name` HTML en `<input type="file" name="formFile">` debe coincidir con la propiedad/el parámetro enlazado de C# (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="bdfff-202">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="bdfff-203">Para más información, consulte la sección [Coincidencia del valor de atributo de nombre con el nombre del parámetro del método POST](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="bdfff-203">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="bdfff-204">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bdfff-204">The following example:</span></span>

* <span data-ttu-id="bdfff-205">Recorre en bucle uno o más archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="bdfff-205">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="bdfff-206">Usa [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) para devolver una ruta de acceso completa de un archivo, incluido el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-206">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="bdfff-207">Guarda los archivos en el sistema de archivos local con un nombre de archivo generado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-207">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="bdfff-208">Devuelve el número total y el tamaño de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="bdfff-208">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="bdfff-209">Use `Path.GetRandomFileName` para generar un nombre de archivo sin una ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="bdfff-209">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="bdfff-210">En el ejemplo siguiente, la ruta de acceso se obtiene de la configuración:</span><span class="sxs-lookup"><span data-stu-id="bdfff-210">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="bdfff-211">La ruta de acceso pasada al <xref:System.IO.FileStream> *debe* incluir el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-211">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="bdfff-212">Si no se proporciona el nombre de archivo, se produce una <xref:System.UnauthorizedAccessException> en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="bdfff-212">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="bdfff-213">Los archivos que se cargan usando la técnica <xref:Microsoft.AspNetCore.Http.IFormFile> se almacenan en búfer en memoria o en disco en el servidor web antes de procesarse.</span><span class="sxs-lookup"><span data-stu-id="bdfff-213">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="bdfff-214">Dentro del método de acción, se puede tener acceso al contenido de <xref:Microsoft.AspNetCore.Http.IFormFile> como <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="bdfff-214">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="bdfff-215">Además del sistema de archivos local, los archivos se pueden guardar en un recurso compartido de red o en un servicio de almacenamiento de archivos, como [Azure Blob Storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="bdfff-215">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="bdfff-216">Para ver otro ejemplo que recorre en bucle varios archivos para cargar y usa nombres de archivo seguros, consulte *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-216">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="bdfff-217">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) arroja una <xref:System.IO.IOException> si se crean más de 65 535 archivos sin eliminar los archivos temporales anteriores.</span><span class="sxs-lookup"><span data-stu-id="bdfff-217">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="bdfff-218">El límite de 65 535 archivos es un límite por servidor.</span><span class="sxs-lookup"><span data-stu-id="bdfff-218">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="bdfff-219">Para más información sobre este límite en el sistema operativo Windows, consulte las notas en los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="bdfff-219">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="bdfff-220">Función GetTempFileNameA</span><span class="sxs-lookup"><span data-stu-id="bdfff-220">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="bdfff-221">Carga de archivos pequeños con enlace de modelos almacenado en búfer a una base de datos</span><span class="sxs-lookup"><span data-stu-id="bdfff-221">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="bdfff-222">Para almacenar datos de archivo binario en una base de datos con [Entity Framework](/ef/core/index), defina una propiedad de matriz <xref:System.Byte> en la entidad:</span><span class="sxs-lookup"><span data-stu-id="bdfff-222">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="bdfff-223">Especifique una propiedad de modelo de página para la clase que incluya un <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="bdfff-223">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="bdfff-224"><xref:Microsoft.AspNetCore.Http.IFormFile> se puede usar directamente como un parámetro de método de acción o como una propiedad de modelo enlazado.</span><span class="sxs-lookup"><span data-stu-id="bdfff-224"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="bdfff-225">En el ejemplo anterior se utiliza una propiedad de modelo enlazado.</span><span class="sxs-lookup"><span data-stu-id="bdfff-225">The prior example uses a bound model property.</span></span>

<span data-ttu-id="bdfff-226">`FileUpload` se usa en el formulario de Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="bdfff-226">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="bdfff-227">Cuando el formulario se publique en el servidor, copie el <xref:Microsoft.AspNetCore.Http.IFormFile> en un flujo y guárdelo como matriz de bytes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-227">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="bdfff-228">En el ejemplo siguiente, `_dbContext` almacena el contexto de base de datos de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="bdfff-228">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="bdfff-229">El ejemplo anterior es similar a un escenario que se muestra en la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bdfff-229">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="bdfff-230">*Pages/BufferedSingleFileUploadDb.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bdfff-230">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="bdfff-231">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="bdfff-231">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="bdfff-232">Tenga cuidado al almacenar los datos binarios en bases de datos relacionales, ya que esto puede repercutir adversamente en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="bdfff-232">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="bdfff-233">No se base ni confíe en la propiedad `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sin validarla.</span><span class="sxs-lookup"><span data-stu-id="bdfff-233">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="bdfff-234">La propiedad `FileName` solo debe usarse para fines de presentación y solo después de la codificación HTML.</span><span class="sxs-lookup"><span data-stu-id="bdfff-234">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="bdfff-235">Los ejemplos proporcionados no tienen en cuenta las consideraciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="bdfff-235">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="bdfff-236">Se proporciona información adicional en las secciones siguientes y en la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="bdfff-236">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="bdfff-237">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="bdfff-237">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="bdfff-238">Validación</span><span class="sxs-lookup"><span data-stu-id="bdfff-238">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="bdfff-239">Carga de archivos de gran tamaño con streaming</span><span class="sxs-lookup"><span data-stu-id="bdfff-239">Upload large files with streaming</span></span>

<span data-ttu-id="bdfff-240">En el ejemplo siguiente se muestra cómo usar JavaScript para transmitir un archivo a una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="bdfff-240">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="bdfff-241">El token de antifalsificación del archivo se genera por medio de un atributo de filtro personalizado y se pasa a los encabezados HTTP del cliente, en lugar de en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="bdfff-241">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="bdfff-242">Dado que el método de acción procesa los datos cargados directamente, el enlace de modelos del formulario se deshabilita por otro filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="bdfff-242">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="bdfff-243">Dentro de la acción, el contenido del formulario se lee usando un `MultipartReader` (que lee cada `MultipartSection` individual), de forma que el archivo se procesa o el contenido se almacena, según corresponda.</span><span class="sxs-lookup"><span data-stu-id="bdfff-243">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="bdfff-244">Una vez leídas las secciones de varias partes, la acción realiza su propio enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-244">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="bdfff-245">La respuesta de la página inicial carga el formulario y guarda un token de antifalsificación en una cookie (a través del atributo `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="bdfff-245">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="bdfff-246">El atributo usa la [compatibilidad de antifalsificación](xref:security/anti-request-forgery) integrada de ASP.NET Core para establecer una cookie con un token de solicitud:</span><span class="sxs-lookup"><span data-stu-id="bdfff-246">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="bdfff-247">El `DisableFormValueModelBindingAttribute` se usa para deshabilitar el enlace de modelos:</span><span class="sxs-lookup"><span data-stu-id="bdfff-247">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="bdfff-248">En la aplicación de ejemplo, `GenerateAntiforgeryTokenCookieAttribute` y `DisableFormValueModelBindingAttribute` se aplican como filtros a los modelos de aplicación de la página de `/StreamedSingleFileUploadDb` y `/StreamedSingleFileUploadPhysical` en `Startup.ConfigureServices` con las [convenciones de Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="bdfff-248">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="bdfff-249">Dado que el enlace de modelos no lee el formulario, los parámetros enlazados desde el formulario no se enlazan (la consulta, la ruta y el encabezado siguen funcionando).</span><span class="sxs-lookup"><span data-stu-id="bdfff-249">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="bdfff-250">El método de acción funciona directamente con la propiedad `Request`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-250">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="bdfff-251">Se usa un elemento `MultipartReader` para leer cada sección.</span><span class="sxs-lookup"><span data-stu-id="bdfff-251">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="bdfff-252">Los datos de clave-valor se almacenan en un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-252">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="bdfff-253">Una vez leídas las secciones de varias partes, el contenido del `KeyValueAccumulator` se usa para enlazar los datos del formulario a un tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-253">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="bdfff-254">El método `StreamingController.UploadDatabase` completo para streaming a una base de datos con EF Core:</span><span class="sxs-lookup"><span data-stu-id="bdfff-254">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="bdfff-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span><span class="sxs-lookup"><span data-stu-id="bdfff-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="bdfff-256">El método `StreamingController.UploadPhysical` completo para streaming a una ubicación física:</span><span class="sxs-lookup"><span data-stu-id="bdfff-256">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="bdfff-257">En la aplicación de ejemplo, las comprobaciones de validación las controla `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-257">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="bdfff-258">Validación</span><span class="sxs-lookup"><span data-stu-id="bdfff-258">Validation</span></span>

<span data-ttu-id="bdfff-259">La clase `FileHelpers` de la aplicación de ejemplo muestra varias comprobaciones de cargas de archivos de streaming y <xref:Microsoft.AspNetCore.Http.IFormFile> almacenado en búfer.</span><span class="sxs-lookup"><span data-stu-id="bdfff-259">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="bdfff-260">Para procesar cargas de archivos almacenadas en búfer de <xref:Microsoft.AspNetCore.Http.IFormFile> en la aplicación de ejemplo, consulte el método `ProcessFormFile` en el archivo *Utilities/FileHelpers.cs*.</span><span class="sxs-lookup"><span data-stu-id="bdfff-260">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="bdfff-261">Para procesar archivos de streaming, consulte el método `ProcessStreamedFile` en el mismo archivo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-261">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="bdfff-262">Los métodos de procesamiento de validación mostrados en la aplicación de ejemplo no examinan el contenido de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="bdfff-262">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="bdfff-263">En la mayoría de los escenarios de producción, se usa una API de analizador de virus/malware en el archivo antes de que el archivo esté disponible para los usuarios u otros sistemas.</span><span class="sxs-lookup"><span data-stu-id="bdfff-263">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="bdfff-264">Aunque el ejemplo de tema proporciona un ejemplo funcional de técnicas de validación, no implemente la clase `FileHelpers` en una aplicación de producción a menos que:</span><span class="sxs-lookup"><span data-stu-id="bdfff-264">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="bdfff-265">Comprenda totalmente la implementación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-265">Fully understand the implementation.</span></span>
> * <span data-ttu-id="bdfff-266">Modifique la implementación según corresponda en función del entorno y las especificaciones de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-266">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="bdfff-267">**No implemente nunca de manera indiscriminada el código de seguridad en una aplicación sin abordar estos requisitos.**</span><span class="sxs-lookup"><span data-stu-id="bdfff-267">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="bdfff-268">Validación del contenido</span><span class="sxs-lookup"><span data-stu-id="bdfff-268">Content validation</span></span>

<span data-ttu-id="bdfff-269">**Use una API de detección de virus/malware de terceros en el contenido cargado.**</span><span class="sxs-lookup"><span data-stu-id="bdfff-269">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="bdfff-270">El análisis de archivos exige recursos del servidor en escenarios de gran volumen.</span><span class="sxs-lookup"><span data-stu-id="bdfff-270">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="bdfff-271">Si disminuye el rendimiento de procesamiento de solicitudes debido al análisis de archivos, considere la posibilidad de descargar el trabajo de análisis a un [servicio en segundo plano](xref:fundamentals/host/hosted-services), posiblemente un servicio que se ejecute en otro servidor desde el servidor de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-271">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="bdfff-272">Habitualmente, los archivos cargados se almacenan en un área en cuarentena hasta que el programa de detección de virus en segundo plano los revisa.</span><span class="sxs-lookup"><span data-stu-id="bdfff-272">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="bdfff-273">Cuando se pasa un archivo, se mueve a la ubicación de almacenamiento de archivos habitual.</span><span class="sxs-lookup"><span data-stu-id="bdfff-273">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="bdfff-274">Por lo general, estos pasos se realizan junto con un registro de base de datos que indica el estado de análisis de un archivo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-274">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="bdfff-275">Mediante el uso de este enfoque, la aplicación y el servidor de aplicaciones permanecen centrados en responder a las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="bdfff-275">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="bdfff-276">Validación de la extensión del archivo</span><span class="sxs-lookup"><span data-stu-id="bdfff-276">File extension validation</span></span>

<span data-ttu-id="bdfff-277">La extensión del archivo cargado debe comprobarse con una lista de extensiones permitidas.</span><span class="sxs-lookup"><span data-stu-id="bdfff-277">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="bdfff-278">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bdfff-278">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="bdfff-279">Validación de firma del archivo</span><span class="sxs-lookup"><span data-stu-id="bdfff-279">File signature validation</span></span>

<span data-ttu-id="bdfff-280">La firma de un archivo viene determinada por los primeros bytes al principio de un archivo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-280">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="bdfff-281">Estos bytes se pueden usar para indicar si la extensión coincide con el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-281">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="bdfff-282">La aplicación de ejemplo comprueba las firmas de archivo de algunos tipos de archivo comunes.</span><span class="sxs-lookup"><span data-stu-id="bdfff-282">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="bdfff-283">En el ejemplo siguiente, la firma de archivo de una imagen JPEG se comprueba con respecto al archivo:</span><span class="sxs-lookup"><span data-stu-id="bdfff-283">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="bdfff-284">Para obtener firmas de archivo adicionales, consulte la [base de datos de firmas de archivo](https://www.filesignatures.net/) y las especificaciones de archivo oficiales.</span><span class="sxs-lookup"><span data-stu-id="bdfff-284">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="bdfff-285">Seguridad de nombre de archivo</span><span class="sxs-lookup"><span data-stu-id="bdfff-285">File name security</span></span>

<span data-ttu-id="bdfff-286">Nunca use un nombre de archivo proporcionado por el cliente para guardar un archivo en el almacenamiento físico.</span><span class="sxs-lookup"><span data-stu-id="bdfff-286">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="bdfff-287">Cree un nombre de archivo seguro para el archivo con [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) o [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) para crear una ruta de acceso completa (incluido el nombre de archivo) para el almacenamiento temporal.</span><span class="sxs-lookup"><span data-stu-id="bdfff-287">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="bdfff-288">Razor codifica automáticamente en HTML los valores de propiedad para mostrarlos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-288">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="bdfff-289">El código siguiente es seguro de usar:</span><span class="sxs-lookup"><span data-stu-id="bdfff-289">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="bdfff-290">Fuera de Razor, siempre use <xref:System.Net.WebUtility.HtmlEncode*> al contenido del nombre de archivo de una solicitud de un usuario.</span><span class="sxs-lookup"><span data-stu-id="bdfff-290">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="bdfff-291">Muchas implementaciones deben incluir una comprobación de que el archivo existe; de lo contrario, el archivo se sobrescribe por un archivo con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="bdfff-291">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="bdfff-292">Proporcione lógica adicional para satisfacer las especificaciones de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-292">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="bdfff-293">Validación del tamaño</span><span class="sxs-lookup"><span data-stu-id="bdfff-293">Size validation</span></span>

<span data-ttu-id="bdfff-294">Limite el tamaño de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="bdfff-294">Limit the size of uploaded files.</span></span>

<span data-ttu-id="bdfff-295">En la aplicación de ejemplo, el tamaño del archivo está limitado a 2 MB (se indica en bytes).</span><span class="sxs-lookup"><span data-stu-id="bdfff-295">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="bdfff-296">El límite se proporciona a través de [Configuración](xref:fundamentals/configuration/index) del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="bdfff-296">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="bdfff-297">`FileSizeLimit` se inserta en las clases `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-297">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="bdfff-298">Cuando el tamaño de un archivo supera el límite, se rechaza el archivo:</span><span class="sxs-lookup"><span data-stu-id="bdfff-298">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="bdfff-299">Coincidencia del valor de atributo de nombre con el nombre del parámetro del método POST</span><span class="sxs-lookup"><span data-stu-id="bdfff-299">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="bdfff-300">En los formularios que no son de Razor que realizan la operación POST en los datos de formulario o usan directamente `FormData` de JavaScript, el nombre especificado en el elemento del formulario o `FormData` debe coincidir con el nombre del parámetro en la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="bdfff-300">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="bdfff-301">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bdfff-301">In the following example:</span></span>

* <span data-ttu-id="bdfff-302">Cuando se usa un elemento `<input>`, el atributo `name` se establece en el valor `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-302">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="bdfff-303">Cuando se usa `FormData` en JavaScript, el nombre se establece en el valor `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-303">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="bdfff-304">Use un nombre coincidente para el parámetro del método de C# (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="bdfff-304">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="bdfff-305">Para un método de control de páginas de Razor Pages denominado `Upload`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-305">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="bdfff-306">Para un método de acción de controlador POST de MVC:</span><span class="sxs-lookup"><span data-stu-id="bdfff-306">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="bdfff-307">Configuración del servidor y de la aplicación</span><span class="sxs-lookup"><span data-stu-id="bdfff-307">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="bdfff-308">Límite de longitud del cuerpo de varias partes</span><span class="sxs-lookup"><span data-stu-id="bdfff-308">Multipart body length limit</span></span>

<span data-ttu-id="bdfff-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> establece el límite de la longitud de cada cuerpo de varias partes.</span><span class="sxs-lookup"><span data-stu-id="bdfff-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="bdfff-310">Las secciones del formulario que superan este límite inician una <xref:System.IO.InvalidDataException> cuando se analizan.</span><span class="sxs-lookup"><span data-stu-id="bdfff-310">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="bdfff-311">El valor predeterminado es 134 217 728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="bdfff-311">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="bdfff-312">Personalice el límite mediante el valor <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-312">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="bdfff-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> se utiliza para establecer el <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> para una sola página o acción.</span><span class="sxs-lookup"><span data-stu-id="bdfff-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="bdfff-314">En una aplicación de Razor Pages, aplique el filtro con una [convención](xref:razor-pages/razor-pages-conventions) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-314">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="bdfff-315">En una aplicación de Razor Pages o una aplicación MVC, aplique el filtro al método de acción o al modelo de página:</span><span class="sxs-lookup"><span data-stu-id="bdfff-315">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="bdfff-316">Tamaño máximo del cuerpo de la solicitud de Kestrel</span><span class="sxs-lookup"><span data-stu-id="bdfff-316">Kestrel maximum request body size</span></span>

<span data-ttu-id="bdfff-317">En el caso de las aplicaciones hospedadas por Kestrel, el tamaño máximo predeterminado del cuerpo de solicitud es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="bdfff-317">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="bdfff-318">Personalice el límite con la opción [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) del servidor de Kestrel:</span><span class="sxs-lookup"><span data-stu-id="bdfff-318">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="bdfff-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> se usa para establecer el valor de [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) de una sola página o acción.</span><span class="sxs-lookup"><span data-stu-id="bdfff-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="bdfff-320">En una aplicación de Razor Pages, aplique el filtro con una [convención](xref:razor-pages/razor-pages-conventions) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-320">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="bdfff-321">En una aplicación de Razor Pages o una aplicación MVC, aplique el filtro al método de acción o a la clase de control de página:</span><span class="sxs-lookup"><span data-stu-id="bdfff-321">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="bdfff-322">El `RequestSizeLimitAttribute` también se puede aplicar con la directiva [@attribute](xref:mvc/views/razor#attribute) de Razor:</span><span class="sxs-lookup"><span data-stu-id="bdfff-322">The `RequestSizeLimitAttribute` can also be applied using the [@attribute](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="bdfff-323">Otros límites de Kestrel</span><span class="sxs-lookup"><span data-stu-id="bdfff-323">Other Kestrel limits</span></span>

<span data-ttu-id="bdfff-324">Otros límites de Kestrel pueden aplicarse a las aplicaciones hospedadas por Kestrel:</span><span class="sxs-lookup"><span data-stu-id="bdfff-324">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="bdfff-325">Conexiones de cliente máximas</span><span class="sxs-lookup"><span data-stu-id="bdfff-325">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="bdfff-326">Tarifas de datos de solicitud y respuesta</span><span class="sxs-lookup"><span data-stu-id="bdfff-326">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="bdfff-327">Límite de longitud del contenido de IIS</span><span class="sxs-lookup"><span data-stu-id="bdfff-327">IIS content length limit</span></span>

<span data-ttu-id="bdfff-328">El límite predeterminado de solicitudes (`maxAllowedContentLength`) es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="bdfff-328">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="bdfff-329">Personalice el límite en el archivo *web.config*:</span><span class="sxs-lookup"><span data-stu-id="bdfff-329">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="bdfff-330">Esto solo ocurre en IIS;</span><span class="sxs-lookup"><span data-stu-id="bdfff-330">This setting only applies to IIS.</span></span> <span data-ttu-id="bdfff-331">este comportamiento no sucede de forma predeterminada cuando los archivos se hospedan en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="bdfff-331">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="bdfff-332">Para más información, consulte [Límites de solicitud \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="bdfff-332">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="bdfff-333">Las limitaciones del módulo ASP.NET Core o la presencia del módulo de filtrado de solicitudes de IIS pueden limitar las cargas a 2 GB o 4 GB.</span><span class="sxs-lookup"><span data-stu-id="bdfff-333">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="bdfff-334">Para más información, consulte el artículo que indica que [no se pueden cargar archivos con un tamaño superior a 2 GB (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="bdfff-334">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="bdfff-335">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="bdfff-335">Troubleshoot</span></span>

<span data-ttu-id="bdfff-336">Aquí incluimos algunos problemas comunes que pueden surgir al cargar archivos, así como sus posibles soluciones.</span><span class="sxs-lookup"><span data-stu-id="bdfff-336">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="bdfff-337">Error No encontrado al implementar en un servidor IIS</span><span class="sxs-lookup"><span data-stu-id="bdfff-337">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="bdfff-338">El error siguiente indica que el archivo cargado supera la longitud configurada del contenido del servidor:</span><span class="sxs-lookup"><span data-stu-id="bdfff-338">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="bdfff-339">Para más información sobre cómo aumentar el límite, consulte la sección [Límite de longitud del contenido de IIS](#iis-content-length-limit).</span><span class="sxs-lookup"><span data-stu-id="bdfff-339">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="bdfff-340">Error de conexión</span><span class="sxs-lookup"><span data-stu-id="bdfff-340">Connection failure</span></span>

<span data-ttu-id="bdfff-341">Un error de conexión y una conexión del servidor de restablecimiento probablemente indica que el archivo cargado supera el tamaño máximo del cuerpo de la solicitud de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="bdfff-341">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="bdfff-342">Para más información, consulte la sección [Tamaño máximo del cuerpo de la solicitud de Kestrel](#kestrel-maximum-request-body-size).</span><span class="sxs-lookup"><span data-stu-id="bdfff-342">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="bdfff-343">Los límites de conexión del cliente de Kestrel también pueden requerir ajustes.</span><span class="sxs-lookup"><span data-stu-id="bdfff-343">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="bdfff-344">Excepción de referencia nula con IFormFile</span><span class="sxs-lookup"><span data-stu-id="bdfff-344">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="bdfff-345">Si el controlador acepta archivos cargados con <xref:Microsoft.AspNetCore.Http.IFormFile>, pero el valor es `null`, confirme que el formulario HTML especifica un valor `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-345">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="bdfff-346">Si este atributo no está establecido en el elemento `<form>`, no se llevará a cabo la carga del archivo y cualquier argumento <xref:Microsoft.AspNetCore.Http.IFormFile> enlazado será `null`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-346">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="bdfff-347">Confirme también que la [nomenclatura de la carga en los datos de formulario coincide con la nomenclatura de la aplicación](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="bdfff-347">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="bdfff-348">El flujo era demasiado largo</span><span class="sxs-lookup"><span data-stu-id="bdfff-348">Stream was too long</span></span>

<span data-ttu-id="bdfff-349">Los ejemplos de este tema se basan en <xref:System.IO.MemoryStream> para almacenar el contenido del archivo cargado.</span><span class="sxs-lookup"><span data-stu-id="bdfff-349">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="bdfff-350">El límite de tamaño de `MemoryStream` es `int.MaxValue`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-350">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="bdfff-351">Si el escenario de carga de archivos de la aplicación requiere almacenar contenido de archivo superior a 50 MB, use un enfoque alternativo que no dependa de un único valor de `MemoryStream` para almacenar el contenido de un archivo cargado.</span><span class="sxs-lookup"><span data-stu-id="bdfff-351">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bdfff-352">ASP.NET Core admite la carga de uno o varios archivos mediante el enlace de modelos almacenado en búfer de archivos más pequeños y el streaming no almacenado en búfer de archivos de mayor tamaño.</span><span class="sxs-lookup"><span data-stu-id="bdfff-352">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="bdfff-353">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bdfff-353">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="bdfff-354">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="bdfff-354">Security considerations</span></span>

<span data-ttu-id="bdfff-355">Tenga precaución al proporcionar a los usuarios la capacidad de cargar archivos en un servidor.</span><span class="sxs-lookup"><span data-stu-id="bdfff-355">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="bdfff-356">Es posible que los atacantes intenten lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bdfff-356">Attackers may attempt to:</span></span>

* <span data-ttu-id="bdfff-357">Ejecutar ataques por [denegación de servicio](/windows-hardware/drivers/ifs/denial-of-service).</span><span class="sxs-lookup"><span data-stu-id="bdfff-357">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="bdfff-358">Cargar virus o malware.</span><span class="sxs-lookup"><span data-stu-id="bdfff-358">Upload viruses or malware.</span></span>
* <span data-ttu-id="bdfff-359">Poner en riesgo redes y servidores de otras maneras.</span><span class="sxs-lookup"><span data-stu-id="bdfff-359">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="bdfff-360">Estos son algunos de los pasos de seguridad con los que se reduce la probabilidad de sufrir ataques:</span><span class="sxs-lookup"><span data-stu-id="bdfff-360">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="bdfff-361">Cargue los archivos a un área de carga de archivos dedicada, preferiblemente una unidad que no sea de sistema.</span><span class="sxs-lookup"><span data-stu-id="bdfff-361">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="bdfff-362">Una ubicación dedicada facilita la imposición de restricciones de seguridad en los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="bdfff-362">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="bdfff-363">Deshabilite la ejecución de los permisos en la ubicación de carga de archivos.&dagger;</span><span class="sxs-lookup"><span data-stu-id="bdfff-363">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="bdfff-364">Los archivos cargados **no** se deben persistir en el mismo árbol de directorio que la aplicación.&dagger;</span><span class="sxs-lookup"><span data-stu-id="bdfff-364">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="bdfff-365">Use un nombre de archivo seguro determinado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-365">Use a safe file name determined by the app.</span></span> <span data-ttu-id="bdfff-366">No use un nombre de archivo que proporcione el usuario ni el nombre de archivo que no es de confianza del archivo cargado.&dagger; HTML codifica el nombre de archivo que no es de confianza al mostrarlo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-366">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="bdfff-367">Por ejemplo, al registrar el nombre de archivo o mostrarlo en la interfaz de usuario (Razor codifica de forma automática la salida HTML).</span><span class="sxs-lookup"><span data-stu-id="bdfff-367">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="bdfff-368">Permita solo las extensiones de archivo aprobadas para la especificación de diseño de la aplicación.&dagger;</span><span class="sxs-lookup"><span data-stu-id="bdfff-368">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="bdfff-369">Compruebe que se realizan comprobaciones del lado cliente en el servidor.&dagger; Las comprobaciones de cliente son fáciles de sortear.</span><span class="sxs-lookup"><span data-stu-id="bdfff-369">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="bdfff-370">Compruebe el tamaño de un archivo cargado.</span><span class="sxs-lookup"><span data-stu-id="bdfff-370">Check the size of an uploaded file.</span></span> <span data-ttu-id="bdfff-371">Establezca un límite de tamaño máximo para evitar cargas grandes.&dagger;</span><span class="sxs-lookup"><span data-stu-id="bdfff-371">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="bdfff-372">Cuando un archivo cargado con el mismo nombre no deba sobrescribir los archivos, vuelva a comprobar el nombre de archivo en la base de datos o en el almacenamiento físico antes de cargarlo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-372">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="bdfff-373">**Ejecute un detector de virus o malware en el contenido cargado antes de que se almacene el archivo.**</span><span class="sxs-lookup"><span data-stu-id="bdfff-373">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="bdfff-374">&dagger;La aplicación de ejemplo muestra un enfoque que cumple los criterios.</span><span class="sxs-lookup"><span data-stu-id="bdfff-374">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="bdfff-375">La carga de código malintencionado en un sistema suele ser el primer paso para ejecutar código que puede:</span><span class="sxs-lookup"><span data-stu-id="bdfff-375">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="bdfff-376">Obtener el control completo de un sistema.</span><span class="sxs-lookup"><span data-stu-id="bdfff-376">Completely gain control of a system.</span></span>
> * <span data-ttu-id="bdfff-377">Sobrecargar un sistema de manera que el sistema se bloquea.</span><span class="sxs-lookup"><span data-stu-id="bdfff-377">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="bdfff-378">Poner en peligro los datos del usuario o del sistema.</span><span class="sxs-lookup"><span data-stu-id="bdfff-378">Compromise user or system data.</span></span>
> * <span data-ttu-id="bdfff-379">Aplicar grafitis a una interfaz de usuario pública.</span><span class="sxs-lookup"><span data-stu-id="bdfff-379">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="bdfff-380">Vea los siguientes recursos para más información sobre cómo reducir el área expuesta de ataques al aceptar archivos de los usuarios:</span><span class="sxs-lookup"><span data-stu-id="bdfff-380">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="bdfff-381">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Carga de archivos sin restricciones)</span><span class="sxs-lookup"><span data-stu-id="bdfff-381">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * [<span data-ttu-id="bdfff-382">Azure Security: Asegúrese de que los controles adecuados estén en vigor al aceptar archivos de usuarios</span><span class="sxs-lookup"><span data-stu-id="bdfff-382">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="bdfff-383">Para más información sobre cómo implementar medidas de seguridad, incluidos ejemplos de la aplicación de ejemplo, consulte la sección [Validación](#validation).</span><span class="sxs-lookup"><span data-stu-id="bdfff-383">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="bdfff-384">Escenarios de almacenamiento</span><span class="sxs-lookup"><span data-stu-id="bdfff-384">Storage scenarios</span></span>

<span data-ttu-id="bdfff-385">Las opciones de almacenamiento comunes para los archivos incluyen:</span><span class="sxs-lookup"><span data-stu-id="bdfff-385">Common storage options for files include:</span></span>

* <span data-ttu-id="bdfff-386">Base de datos</span><span class="sxs-lookup"><span data-stu-id="bdfff-386">Database</span></span>

  * <span data-ttu-id="bdfff-387">En el caso de cargas de archivos pequeñas, una base de datos suele ser más rápida que las opciones de almacenamiento físico (sistema de archivos o recurso compartido de red).</span><span class="sxs-lookup"><span data-stu-id="bdfff-387">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="bdfff-388">Una base de datos suele ser más conveniente que las opciones de almacenamiento físico, ya que la recuperación de un registro de base de datos para los datos de usuario puede proporcionar el contenido del archivo (por ejemplo, una imagen de avatar).</span><span class="sxs-lookup"><span data-stu-id="bdfff-388">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="bdfff-389">Una base de datos puede ser más económica que usar un servicio de almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-389">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="bdfff-390">Almacenamiento físico (sistema de archivos o recurso compartido de red)</span><span class="sxs-lookup"><span data-stu-id="bdfff-390">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="bdfff-391">Para cargas de archivos de gran tamaño:</span><span class="sxs-lookup"><span data-stu-id="bdfff-391">For large file uploads:</span></span>
    * <span data-ttu-id="bdfff-392">Los límites de base de datos pueden restringir el tamaño de la carga.</span><span class="sxs-lookup"><span data-stu-id="bdfff-392">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="bdfff-393">El almacenamiento físico suele ser menos económico que el almacenamiento en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-393">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="bdfff-394">El almacenamiento físico puede ser más económico que usar un servicio de almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-394">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="bdfff-395">El proceso de la aplicación debe tener permisos de lectura y escritura en la ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="bdfff-395">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="bdfff-396">**Nunca conceda el permiso de ejecución.**</span><span class="sxs-lookup"><span data-stu-id="bdfff-396">**Never grant execute permission.**</span></span>

* <span data-ttu-id="bdfff-397">Servicio de almacenamiento de datos (por ejemplo, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="bdfff-397">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="bdfff-398">Los servicios suelen ofrecer una escalabilidad y resistencia mejoradas sobre las soluciones locales que normalmente están sujetas a únicos puntos de error.</span><span class="sxs-lookup"><span data-stu-id="bdfff-398">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="bdfff-399">Los servicios pueden tener un costo menor en escenarios de infraestructura de almacenamiento de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="bdfff-399">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="bdfff-400">Para obtener más información, vea [Inicio rápido: Uso de .NET para crear un blob en el almacenamiento de objetos](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="bdfff-400">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="bdfff-401">En el tema se muestra <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, pero se puede usar <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> para guardar un <xref:System.IO.FileStream> en el almacenamiento de blobs cuando se trabaja con un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="bdfff-401">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="bdfff-402">Escenarios de carga de archivos</span><span class="sxs-lookup"><span data-stu-id="bdfff-402">File upload scenarios</span></span>

<span data-ttu-id="bdfff-403">Dos enfoques generales para cargar archivos son el almacenamiento en búfer y el streaming.</span><span class="sxs-lookup"><span data-stu-id="bdfff-403">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="bdfff-404">**Almacenamiento en búfer**</span><span class="sxs-lookup"><span data-stu-id="bdfff-404">**Buffering**</span></span>

<span data-ttu-id="bdfff-405">El archivo completo se lee en un <xref:Microsoft.AspNetCore.Http.IFormFile>, que es una representación de C# del archivo que se usa para procesar o guardar el archivo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-405">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="bdfff-406">Los recursos (disco, memoria) que se usan en las cargas de archivos dependen de la cantidad y del tamaño de las cargas de archivos que se realizan simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="bdfff-406">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="bdfff-407">Si una aplicación intenta almacenar demasiadas cargas en el búfer, el sitio se bloquea cuando se queda sin memoria o sin espacio en disco.</span><span class="sxs-lookup"><span data-stu-id="bdfff-407">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="bdfff-408">Si el tamaño o la frecuencia de las cargas de archivos está agotando los recursos de la aplicación, use el streaming.</span><span class="sxs-lookup"><span data-stu-id="bdfff-408">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="bdfff-409">Cualquier archivo almacenado en búfer único que supere los 64 KB se mueve de la memoria a un archivo temporal en el disco.</span><span class="sxs-lookup"><span data-stu-id="bdfff-409">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="bdfff-410">En las secciones siguientes de este tema se habla del almacenamiento en búfer de archivos pequeños:</span><span class="sxs-lookup"><span data-stu-id="bdfff-410">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="bdfff-411">Almacenamiento físico</span><span class="sxs-lookup"><span data-stu-id="bdfff-411">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="bdfff-412">Base de datos</span><span class="sxs-lookup"><span data-stu-id="bdfff-412">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="bdfff-413">**Streaming**</span><span class="sxs-lookup"><span data-stu-id="bdfff-413">**Streaming**</span></span>

<span data-ttu-id="bdfff-414">El archivo se recibe de una solicitud de varias partes y lo procesa o guarda directamente la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-414">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="bdfff-415">El streaming no mejora considerablemente el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="bdfff-415">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="bdfff-416">El streaming reduce las demandas de memoria o espacio en disco cuando se cargan archivos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-416">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="bdfff-417">El streaming de archivos grandes se describe en la sección [Carga de archivos de gran tamaño con streaming](#upload-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="bdfff-417">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="bdfff-418">Carga de archivos pequeños con enlace de modelos almacenado en búfer al almacenamiento físico</span><span class="sxs-lookup"><span data-stu-id="bdfff-418">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="bdfff-419">Para cargar archivos pequeños, se puede usar un formulario de varias partes o construir una solicitud POST con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bdfff-419">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="bdfff-420">En el ejemplo siguiente se muestra el uso de un formulario de Razor Pages para cargar un archivo único (*Pages/BufferedSingleFileUploadPhysical.cshtml* en la aplicación de ejemplo):</span><span class="sxs-lookup"><span data-stu-id="bdfff-420">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="bdfff-421">El ejemplo siguiente es análogo al ejemplo anterior, salvo en que:</span><span class="sxs-lookup"><span data-stu-id="bdfff-421">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="bdfff-422">([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) de JavaScript se usa para enviar los datos del formulario.</span><span class="sxs-lookup"><span data-stu-id="bdfff-422">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="bdfff-423">No hay ninguna validación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-423">There's no validation.</span></span>

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

<span data-ttu-id="bdfff-424">Para realizar la solicitud POST en JavaScript para los clientes que [no admiten Fetch API](https://caniuse.com/#feat=fetch), use uno de estos enfoques:</span><span class="sxs-lookup"><span data-stu-id="bdfff-424">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="bdfff-425">Use un Fetch Polyfill (por ejemplo, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="bdfff-425">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="bdfff-426">Use `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-426">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="bdfff-427">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bdfff-427">For example:</span></span>

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

<span data-ttu-id="bdfff-428">Para admitir las cargas de archivos, los formularios HTML deben especificar un tipo de codificación (`enctype`) de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-428">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="bdfff-429">Para que un elemento de entrada `files` admita al carga de varios archivos, proporcione el atributo `multiple` en el elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-429">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="bdfff-430">Es posible acceder a archivos individuales cargados en el servidor a través del [enlace de modelos](xref:mvc/models/model-binding) mediante <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="bdfff-430">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="bdfff-431">La aplicación de ejemplo muestra varias cargas de archivos almacenados en búfer para escenarios de almacenamiento físico y base de datos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-431">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename2"></a>

> [!WARNING]
> <span data-ttu-id="bdfff-432">**No** utilice la propiedad `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> para usos distintos a la presentación y el registro.</span><span class="sxs-lookup"><span data-stu-id="bdfff-432">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="bdfff-433">Para fines de presentación y registro, codifique el nombre de archivo en HTML.</span><span class="sxs-lookup"><span data-stu-id="bdfff-433">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="bdfff-434">Un atacante puede proporcionar un nombre de archivo malintencionado, incluidas rutas de acceso completas o relativas.</span><span class="sxs-lookup"><span data-stu-id="bdfff-434">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="bdfff-435">Las aplicaciones deben:</span><span class="sxs-lookup"><span data-stu-id="bdfff-435">Applications should:</span></span>
>
> * <span data-ttu-id="bdfff-436">Quitar la ruta de acceso del nombre de archivo proporcionado por el usuario.</span><span class="sxs-lookup"><span data-stu-id="bdfff-436">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="bdfff-437">Guardar el nombre de archivo codificado en HTML, sin la ruta de acceso para la interfaz de usuario o el registro.</span><span class="sxs-lookup"><span data-stu-id="bdfff-437">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="bdfff-438">Generar un nombre de archivo aleatorio nuevo para el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="bdfff-438">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="bdfff-439">En el código siguiente se quita la ruta de acceso del nombre del archivo:</span><span class="sxs-lookup"><span data-stu-id="bdfff-439">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="bdfff-440">Los ejemplos proporcionados hasta ahora no tienen en cuenta las consideraciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="bdfff-440">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="bdfff-441">Se proporciona información adicional en las secciones siguientes y en la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="bdfff-441">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="bdfff-442">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="bdfff-442">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="bdfff-443">Validación</span><span class="sxs-lookup"><span data-stu-id="bdfff-443">Validation</span></span>](#validation)

<span data-ttu-id="bdfff-444">Al cargar archivos mediante el enlace de modelos y <xref:Microsoft.AspNetCore.Http.IFormFile>, el método de acción puede aceptar:</span><span class="sxs-lookup"><span data-stu-id="bdfff-444">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="bdfff-445">Un <xref:Microsoft.AspNetCore.Http.IFormFile> único.</span><span class="sxs-lookup"><span data-stu-id="bdfff-445">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="bdfff-446">Cualquiera de las colecciones siguientes que representan varios archivos:</span><span class="sxs-lookup"><span data-stu-id="bdfff-446">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="bdfff-447">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="bdfff-447">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="bdfff-448">El enlace coincide con los archivos de formulario por nombre.</span><span class="sxs-lookup"><span data-stu-id="bdfff-448">Binding matches form files by name.</span></span> <span data-ttu-id="bdfff-449">Por ejemplo, el valor `name` HTML en `<input type="file" name="formFile">` debe coincidir con la propiedad/el parámetro enlazado de C# (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="bdfff-449">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="bdfff-450">Para más información, consulte la sección [Coincidencia del valor de atributo de nombre con el nombre del parámetro del método POST](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="bdfff-450">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="bdfff-451">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bdfff-451">The following example:</span></span>

* <span data-ttu-id="bdfff-452">Recorre en bucle uno o más archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="bdfff-452">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="bdfff-453">Usa [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) para devolver una ruta de acceso completa de un archivo, incluido el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-453">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="bdfff-454">Guarda los archivos en el sistema de archivos local con un nombre de archivo generado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-454">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="bdfff-455">Devuelve el número total y el tamaño de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="bdfff-455">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="bdfff-456">Use `Path.GetRandomFileName` para generar un nombre de archivo sin una ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="bdfff-456">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="bdfff-457">En el ejemplo siguiente, la ruta de acceso se obtiene de la configuración:</span><span class="sxs-lookup"><span data-stu-id="bdfff-457">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="bdfff-458">La ruta de acceso pasada al <xref:System.IO.FileStream> *debe* incluir el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-458">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="bdfff-459">Si no se proporciona el nombre de archivo, se produce una <xref:System.UnauthorizedAccessException> en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="bdfff-459">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="bdfff-460">Los archivos que se cargan usando la técnica <xref:Microsoft.AspNetCore.Http.IFormFile> se almacenan en búfer en memoria o en disco en el servidor web antes de procesarse.</span><span class="sxs-lookup"><span data-stu-id="bdfff-460">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="bdfff-461">Dentro del método de acción, se puede tener acceso al contenido de <xref:Microsoft.AspNetCore.Http.IFormFile> como <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="bdfff-461">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="bdfff-462">Además del sistema de archivos local, los archivos se pueden guardar en un recurso compartido de red o en un servicio de almacenamiento de archivos, como [Azure Blob Storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="bdfff-462">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="bdfff-463">Para ver otro ejemplo que recorre en bucle varios archivos para cargar y usa nombres de archivo seguros, consulte *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-463">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="bdfff-464">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) arroja una <xref:System.IO.IOException> si se crean más de 65 535 archivos sin eliminar los archivos temporales anteriores.</span><span class="sxs-lookup"><span data-stu-id="bdfff-464">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="bdfff-465">El límite de 65 535 archivos es un límite por servidor.</span><span class="sxs-lookup"><span data-stu-id="bdfff-465">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="bdfff-466">Para más información sobre este límite en el sistema operativo Windows, consulte las notas en los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="bdfff-466">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="bdfff-467">Función GetTempFileNameA</span><span class="sxs-lookup"><span data-stu-id="bdfff-467">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="bdfff-468">Carga de archivos pequeños con enlace de modelos almacenado en búfer a una base de datos</span><span class="sxs-lookup"><span data-stu-id="bdfff-468">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="bdfff-469">Para almacenar datos de archivo binario en una base de datos con [Entity Framework](/ef/core/index), defina una propiedad de matriz <xref:System.Byte> en la entidad:</span><span class="sxs-lookup"><span data-stu-id="bdfff-469">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="bdfff-470">Especifique una propiedad de modelo de página para la clase que incluya un <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="bdfff-470">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="bdfff-471"><xref:Microsoft.AspNetCore.Http.IFormFile> se puede usar directamente como un parámetro de método de acción o como una propiedad de modelo enlazado.</span><span class="sxs-lookup"><span data-stu-id="bdfff-471"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="bdfff-472">En el ejemplo anterior se utiliza una propiedad de modelo enlazado.</span><span class="sxs-lookup"><span data-stu-id="bdfff-472">The prior example uses a bound model property.</span></span>

<span data-ttu-id="bdfff-473">`FileUpload` se usa en el formulario de Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="bdfff-473">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="bdfff-474">Cuando el formulario se publique en el servidor, copie el <xref:Microsoft.AspNetCore.Http.IFormFile> en un flujo y guárdelo como matriz de bytes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-474">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="bdfff-475">En el ejemplo siguiente, `_dbContext` almacena el contexto de base de datos de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="bdfff-475">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="bdfff-476">El ejemplo anterior es similar a un escenario que se muestra en la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bdfff-476">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="bdfff-477">*Pages/BufferedSingleFileUploadDb.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bdfff-477">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="bdfff-478">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="bdfff-478">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="bdfff-479">Tenga cuidado al almacenar los datos binarios en bases de datos relacionales, ya que esto puede repercutir adversamente en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="bdfff-479">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="bdfff-480">No se base ni confíe en la propiedad `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sin validarla.</span><span class="sxs-lookup"><span data-stu-id="bdfff-480">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="bdfff-481">La propiedad `FileName` solo debe usarse para fines de presentación y solo después de la codificación HTML.</span><span class="sxs-lookup"><span data-stu-id="bdfff-481">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="bdfff-482">Los ejemplos proporcionados no tienen en cuenta las consideraciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="bdfff-482">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="bdfff-483">Se proporciona información adicional en las secciones siguientes y en la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="bdfff-483">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="bdfff-484">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="bdfff-484">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="bdfff-485">Validación</span><span class="sxs-lookup"><span data-stu-id="bdfff-485">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="bdfff-486">Carga de archivos de gran tamaño con streaming</span><span class="sxs-lookup"><span data-stu-id="bdfff-486">Upload large files with streaming</span></span>

<span data-ttu-id="bdfff-487">En el ejemplo siguiente se muestra cómo usar JavaScript para transmitir un archivo a una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="bdfff-487">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="bdfff-488">El token de antifalsificación del archivo se genera por medio de un atributo de filtro personalizado y se pasa a los encabezados HTTP del cliente, en lugar de en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="bdfff-488">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="bdfff-489">Dado que el método de acción procesa los datos cargados directamente, el enlace de modelos del formulario se deshabilita por otro filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="bdfff-489">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="bdfff-490">Dentro de la acción, el contenido del formulario se lee usando un `MultipartReader` (que lee cada `MultipartSection` individual), de forma que el archivo se procesa o el contenido se almacena, según corresponda.</span><span class="sxs-lookup"><span data-stu-id="bdfff-490">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="bdfff-491">Una vez leídas las secciones de varias partes, la acción realiza su propio enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-491">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="bdfff-492">La respuesta de la página inicial carga el formulario y guarda un token de antifalsificación en una cookie (a través del atributo `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="bdfff-492">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="bdfff-493">El atributo usa la [compatibilidad de antifalsificación](xref:security/anti-request-forgery) integrada de ASP.NET Core para establecer una cookie con un token de solicitud:</span><span class="sxs-lookup"><span data-stu-id="bdfff-493">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="bdfff-494">El `DisableFormValueModelBindingAttribute` se usa para deshabilitar el enlace de modelos:</span><span class="sxs-lookup"><span data-stu-id="bdfff-494">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="bdfff-495">En la aplicación de ejemplo, `GenerateAntiforgeryTokenCookieAttribute` y `DisableFormValueModelBindingAttribute` se aplican como filtros a los modelos de aplicación de la página de `/StreamedSingleFileUploadDb` y `/StreamedSingleFileUploadPhysical` en `Startup.ConfigureServices` con las [convenciones de Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="bdfff-495">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="bdfff-496">Dado que el enlace de modelos no lee el formulario, los parámetros enlazados desde el formulario no se enlazan (la consulta, la ruta y el encabezado siguen funcionando).</span><span class="sxs-lookup"><span data-stu-id="bdfff-496">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="bdfff-497">El método de acción funciona directamente con la propiedad `Request`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-497">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="bdfff-498">Se usa un elemento `MultipartReader` para leer cada sección.</span><span class="sxs-lookup"><span data-stu-id="bdfff-498">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="bdfff-499">Los datos de clave-valor se almacenan en un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-499">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="bdfff-500">Una vez leídas las secciones de varias partes, el contenido del `KeyValueAccumulator` se usa para enlazar los datos del formulario a un tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-500">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="bdfff-501">El método `StreamingController.UploadDatabase` completo para streaming a una base de datos con EF Core:</span><span class="sxs-lookup"><span data-stu-id="bdfff-501">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="bdfff-502">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span><span class="sxs-lookup"><span data-stu-id="bdfff-502">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="bdfff-503">El método `StreamingController.UploadPhysical` completo para streaming a una ubicación física:</span><span class="sxs-lookup"><span data-stu-id="bdfff-503">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="bdfff-504">En la aplicación de ejemplo, las comprobaciones de validación las controla `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-504">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="bdfff-505">Validación</span><span class="sxs-lookup"><span data-stu-id="bdfff-505">Validation</span></span>

<span data-ttu-id="bdfff-506">La clase `FileHelpers` de la aplicación de ejemplo muestra varias comprobaciones de cargas de archivos de streaming y <xref:Microsoft.AspNetCore.Http.IFormFile> almacenado en búfer.</span><span class="sxs-lookup"><span data-stu-id="bdfff-506">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="bdfff-507">Para procesar cargas de archivos almacenadas en búfer de <xref:Microsoft.AspNetCore.Http.IFormFile> en la aplicación de ejemplo, consulte el método `ProcessFormFile` en el archivo *Utilities/FileHelpers.cs*.</span><span class="sxs-lookup"><span data-stu-id="bdfff-507">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="bdfff-508">Para procesar archivos de streaming, consulte el método `ProcessStreamedFile` en el mismo archivo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-508">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="bdfff-509">Los métodos de procesamiento de validación mostrados en la aplicación de ejemplo no examinan el contenido de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="bdfff-509">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="bdfff-510">En la mayoría de los escenarios de producción, se usa una API de analizador de virus/malware en el archivo antes de que el archivo esté disponible para los usuarios u otros sistemas.</span><span class="sxs-lookup"><span data-stu-id="bdfff-510">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="bdfff-511">Aunque el ejemplo de tema proporciona un ejemplo funcional de técnicas de validación, no implemente la clase `FileHelpers` en una aplicación de producción a menos que:</span><span class="sxs-lookup"><span data-stu-id="bdfff-511">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="bdfff-512">Comprenda totalmente la implementación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-512">Fully understand the implementation.</span></span>
> * <span data-ttu-id="bdfff-513">Modifique la implementación según corresponda en función del entorno y las especificaciones de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-513">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="bdfff-514">**No implemente nunca de manera indiscriminada el código de seguridad en una aplicación sin abordar estos requisitos.**</span><span class="sxs-lookup"><span data-stu-id="bdfff-514">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="bdfff-515">Validación del contenido</span><span class="sxs-lookup"><span data-stu-id="bdfff-515">Content validation</span></span>

<span data-ttu-id="bdfff-516">**Use una API de detección de virus/malware de terceros en el contenido cargado.**</span><span class="sxs-lookup"><span data-stu-id="bdfff-516">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="bdfff-517">El análisis de archivos exige recursos del servidor en escenarios de gran volumen.</span><span class="sxs-lookup"><span data-stu-id="bdfff-517">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="bdfff-518">Si disminuye el rendimiento de procesamiento de solicitudes debido al análisis de archivos, considere la posibilidad de descargar el trabajo de análisis a un [servicio en segundo plano](xref:fundamentals/host/hosted-services), posiblemente un servicio que se ejecute en otro servidor desde el servidor de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-518">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="bdfff-519">Habitualmente, los archivos cargados se almacenan en un área en cuarentena hasta que el programa de detección de virus en segundo plano los revisa.</span><span class="sxs-lookup"><span data-stu-id="bdfff-519">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="bdfff-520">Cuando se pasa un archivo, se mueve a la ubicación de almacenamiento de archivos habitual.</span><span class="sxs-lookup"><span data-stu-id="bdfff-520">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="bdfff-521">Por lo general, estos pasos se realizan junto con un registro de base de datos que indica el estado de análisis de un archivo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-521">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="bdfff-522">Mediante el uso de este enfoque, la aplicación y el servidor de aplicaciones permanecen centrados en responder a las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="bdfff-522">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="bdfff-523">Validación de la extensión del archivo</span><span class="sxs-lookup"><span data-stu-id="bdfff-523">File extension validation</span></span>

<span data-ttu-id="bdfff-524">La extensión del archivo cargado debe comprobarse con una lista de extensiones permitidas.</span><span class="sxs-lookup"><span data-stu-id="bdfff-524">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="bdfff-525">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bdfff-525">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="bdfff-526">Validación de firma del archivo</span><span class="sxs-lookup"><span data-stu-id="bdfff-526">File signature validation</span></span>

<span data-ttu-id="bdfff-527">La firma de un archivo viene determinada por los primeros bytes al principio de un archivo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-527">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="bdfff-528">Estos bytes se pueden usar para indicar si la extensión coincide con el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="bdfff-528">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="bdfff-529">La aplicación de ejemplo comprueba las firmas de archivo de algunos tipos de archivo comunes.</span><span class="sxs-lookup"><span data-stu-id="bdfff-529">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="bdfff-530">En el ejemplo siguiente, la firma de archivo de una imagen JPEG se comprueba con respecto al archivo:</span><span class="sxs-lookup"><span data-stu-id="bdfff-530">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="bdfff-531">Para obtener firmas de archivo adicionales, consulte la [base de datos de firmas de archivo](https://www.filesignatures.net/) y las especificaciones de archivo oficiales.</span><span class="sxs-lookup"><span data-stu-id="bdfff-531">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="bdfff-532">Seguridad de nombre de archivo</span><span class="sxs-lookup"><span data-stu-id="bdfff-532">File name security</span></span>

<span data-ttu-id="bdfff-533">Nunca use un nombre de archivo proporcionado por el cliente para guardar un archivo en el almacenamiento físico.</span><span class="sxs-lookup"><span data-stu-id="bdfff-533">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="bdfff-534">Cree un nombre de archivo seguro para el archivo con [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) o [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) para crear una ruta de acceso completa (incluido el nombre de archivo) para el almacenamiento temporal.</span><span class="sxs-lookup"><span data-stu-id="bdfff-534">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="bdfff-535">Razor codifica automáticamente en HTML los valores de propiedad para mostrarlos.</span><span class="sxs-lookup"><span data-stu-id="bdfff-535">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="bdfff-536">El código siguiente es seguro de usar:</span><span class="sxs-lookup"><span data-stu-id="bdfff-536">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="bdfff-537">Fuera de Razor, siempre use <xref:System.Net.WebUtility.HtmlEncode*> al contenido del nombre de archivo de una solicitud de un usuario.</span><span class="sxs-lookup"><span data-stu-id="bdfff-537">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="bdfff-538">Muchas implementaciones deben incluir una comprobación de que el archivo existe; de lo contrario, el archivo se sobrescribe por un archivo con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="bdfff-538">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="bdfff-539">Proporcione lógica adicional para satisfacer las especificaciones de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bdfff-539">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="bdfff-540">Validación del tamaño</span><span class="sxs-lookup"><span data-stu-id="bdfff-540">Size validation</span></span>

<span data-ttu-id="bdfff-541">Limite el tamaño de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="bdfff-541">Limit the size of uploaded files.</span></span>

<span data-ttu-id="bdfff-542">En la aplicación de ejemplo, el tamaño del archivo está limitado a 2 MB (se indica en bytes).</span><span class="sxs-lookup"><span data-stu-id="bdfff-542">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="bdfff-543">El límite se proporciona a través de [Configuración](xref:fundamentals/configuration/index) del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="bdfff-543">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="bdfff-544">`FileSizeLimit` se inserta en las clases `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-544">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="bdfff-545">Cuando el tamaño de un archivo supera el límite, se rechaza el archivo:</span><span class="sxs-lookup"><span data-stu-id="bdfff-545">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="bdfff-546">Coincidencia del valor de atributo de nombre con el nombre del parámetro del método POST</span><span class="sxs-lookup"><span data-stu-id="bdfff-546">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="bdfff-547">En los formularios que no son de Razor que realizan la operación POST en los datos de formulario o usan directamente `FormData` de JavaScript, el nombre especificado en el elemento del formulario o `FormData` debe coincidir con el nombre del parámetro en la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="bdfff-547">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="bdfff-548">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bdfff-548">In the following example:</span></span>

* <span data-ttu-id="bdfff-549">Cuando se usa un elemento `<input>`, el atributo `name` se establece en el valor `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-549">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="bdfff-550">Cuando se usa `FormData` en JavaScript, el nombre se establece en el valor `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-550">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="bdfff-551">Use un nombre coincidente para el parámetro del método de C# (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="bdfff-551">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="bdfff-552">Para un método de control de páginas de Razor Pages denominado `Upload`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-552">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="bdfff-553">Para un método de acción de controlador POST de MVC:</span><span class="sxs-lookup"><span data-stu-id="bdfff-553">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="bdfff-554">Configuración del servidor y de la aplicación</span><span class="sxs-lookup"><span data-stu-id="bdfff-554">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="bdfff-555">Límite de longitud del cuerpo de varias partes</span><span class="sxs-lookup"><span data-stu-id="bdfff-555">Multipart body length limit</span></span>

<span data-ttu-id="bdfff-556"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> establece el límite de la longitud de cada cuerpo de varias partes.</span><span class="sxs-lookup"><span data-stu-id="bdfff-556"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="bdfff-557">Las secciones del formulario que superan este límite inician una <xref:System.IO.InvalidDataException> cuando se analizan.</span><span class="sxs-lookup"><span data-stu-id="bdfff-557">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="bdfff-558">El valor predeterminado es 134 217 728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="bdfff-558">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="bdfff-559">Personalice el límite mediante el valor <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-559">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="bdfff-560"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> se utiliza para establecer el <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> para una sola página o acción.</span><span class="sxs-lookup"><span data-stu-id="bdfff-560"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="bdfff-561">En una aplicación de Razor Pages, aplique el filtro con una [convención](xref:razor-pages/razor-pages-conventions) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-561">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="bdfff-562">En una aplicación de Razor Pages o una aplicación MVC, aplique el filtro al método de acción o al modelo de página:</span><span class="sxs-lookup"><span data-stu-id="bdfff-562">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="bdfff-563">Tamaño máximo del cuerpo de la solicitud de Kestrel</span><span class="sxs-lookup"><span data-stu-id="bdfff-563">Kestrel maximum request body size</span></span>

<span data-ttu-id="bdfff-564">En el caso de las aplicaciones hospedadas por Kestrel, el tamaño máximo predeterminado del cuerpo de solicitud es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="bdfff-564">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="bdfff-565">Personalice el límite con la opción [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) del servidor de Kestrel:</span><span class="sxs-lookup"><span data-stu-id="bdfff-565">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="bdfff-566"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> se usa para establecer el valor de [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) de una sola página o acción.</span><span class="sxs-lookup"><span data-stu-id="bdfff-566"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="bdfff-567">En una aplicación de Razor Pages, aplique el filtro con una [convención](xref:razor-pages/razor-pages-conventions) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="bdfff-567">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="bdfff-568">En una aplicación de Razor Pages o una aplicación MVC, aplique el filtro al método de acción o a la clase de control de página:</span><span class="sxs-lookup"><span data-stu-id="bdfff-568">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="bdfff-569">Otros límites de Kestrel</span><span class="sxs-lookup"><span data-stu-id="bdfff-569">Other Kestrel limits</span></span>

<span data-ttu-id="bdfff-570">Otros límites de Kestrel pueden aplicarse a las aplicaciones hospedadas por Kestrel:</span><span class="sxs-lookup"><span data-stu-id="bdfff-570">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="bdfff-571">Conexiones de cliente máximas</span><span class="sxs-lookup"><span data-stu-id="bdfff-571">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="bdfff-572">Tarifas de datos de solicitud y respuesta</span><span class="sxs-lookup"><span data-stu-id="bdfff-572">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="bdfff-573">Límite de longitud del contenido de IIS</span><span class="sxs-lookup"><span data-stu-id="bdfff-573">IIS content length limit</span></span>

<span data-ttu-id="bdfff-574">El límite predeterminado de solicitudes (`maxAllowedContentLength`) es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="bdfff-574">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="bdfff-575">Personalice el límite en el archivo *web.config*:</span><span class="sxs-lookup"><span data-stu-id="bdfff-575">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="bdfff-576">Esto solo ocurre en IIS;</span><span class="sxs-lookup"><span data-stu-id="bdfff-576">This setting only applies to IIS.</span></span> <span data-ttu-id="bdfff-577">este comportamiento no sucede de forma predeterminada cuando los archivos se hospedan en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="bdfff-577">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="bdfff-578">Para más información, consulte [Límites de solicitud \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="bdfff-578">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="bdfff-579">Las limitaciones del módulo ASP.NET Core o la presencia del módulo de filtrado de solicitudes de IIS pueden limitar las cargas a 2 GB o 4 GB.</span><span class="sxs-lookup"><span data-stu-id="bdfff-579">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="bdfff-580">Para más información, consulte el artículo que indica que [no se pueden cargar archivos con un tamaño superior a 2 GB (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="bdfff-580">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="bdfff-581">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="bdfff-581">Troubleshoot</span></span>

<span data-ttu-id="bdfff-582">Aquí incluimos algunos problemas comunes que pueden surgir al cargar archivos, así como sus posibles soluciones.</span><span class="sxs-lookup"><span data-stu-id="bdfff-582">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="bdfff-583">Error No encontrado al implementar en un servidor IIS</span><span class="sxs-lookup"><span data-stu-id="bdfff-583">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="bdfff-584">El error siguiente indica que el archivo cargado supera la longitud configurada del contenido del servidor:</span><span class="sxs-lookup"><span data-stu-id="bdfff-584">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="bdfff-585">Para más información sobre cómo aumentar el límite, consulte la sección [Límite de longitud del contenido de IIS](#iis-content-length-limit).</span><span class="sxs-lookup"><span data-stu-id="bdfff-585">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="bdfff-586">Error de conexión</span><span class="sxs-lookup"><span data-stu-id="bdfff-586">Connection failure</span></span>

<span data-ttu-id="bdfff-587">Un error de conexión y una conexión del servidor de restablecimiento probablemente indica que el archivo cargado supera el tamaño máximo del cuerpo de la solicitud de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="bdfff-587">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="bdfff-588">Para más información, consulte la sección [Tamaño máximo del cuerpo de la solicitud de Kestrel](#kestrel-maximum-request-body-size).</span><span class="sxs-lookup"><span data-stu-id="bdfff-588">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="bdfff-589">Los límites de conexión del cliente de Kestrel también pueden requerir ajustes.</span><span class="sxs-lookup"><span data-stu-id="bdfff-589">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="bdfff-590">Excepción de referencia nula con IFormFile</span><span class="sxs-lookup"><span data-stu-id="bdfff-590">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="bdfff-591">Si el controlador acepta archivos cargados con <xref:Microsoft.AspNetCore.Http.IFormFile>, pero el valor es `null`, confirme que el formulario HTML especifica un valor `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-591">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="bdfff-592">Si este atributo no está establecido en el elemento `<form>`, no se llevará a cabo la carga del archivo y cualquier argumento <xref:Microsoft.AspNetCore.Http.IFormFile> enlazado será `null`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-592">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="bdfff-593">Confirme también que la [nomenclatura de la carga en los datos de formulario coincide con la nomenclatura de la aplicación](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="bdfff-593">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="bdfff-594">El flujo era demasiado largo</span><span class="sxs-lookup"><span data-stu-id="bdfff-594">Stream was too long</span></span>

<span data-ttu-id="bdfff-595">Los ejemplos de este tema se basan en <xref:System.IO.MemoryStream> para almacenar el contenido del archivo cargado.</span><span class="sxs-lookup"><span data-stu-id="bdfff-595">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="bdfff-596">El límite de tamaño de `MemoryStream` es `int.MaxValue`.</span><span class="sxs-lookup"><span data-stu-id="bdfff-596">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="bdfff-597">Si el escenario de carga de archivos de la aplicación requiere almacenar contenido de archivo superior a 50 MB, use un enfoque alternativo que no dependa de un único valor de `MemoryStream` para almacenar el contenido de un archivo cargado.</span><span class="sxs-lookup"><span data-stu-id="bdfff-597">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="bdfff-598">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="bdfff-598">Additional resources</span></span>

* <span data-ttu-id="bdfff-599">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Carga de archivos sin restricciones)</span><span class="sxs-lookup"><span data-stu-id="bdfff-599">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
* [<span data-ttu-id="bdfff-600">Azure Security: Marco de seguridad: Validación de entrada | Mitigaciones</span><span class="sxs-lookup"><span data-stu-id="bdfff-600">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="bdfff-601">Patrones de diseño en la nube de Azure: Patrón Valet Key</span><span class="sxs-lookup"><span data-stu-id="bdfff-601">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
