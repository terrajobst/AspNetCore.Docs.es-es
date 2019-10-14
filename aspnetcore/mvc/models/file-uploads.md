---
title: Carga de archivos en ASP.NET Core
author: guardrex
description: Cómo usar el enlace de modelos y el streaming para cargar archivos en ASP.NET Core MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/02/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: de8bfee22e39dfc5a6ed254cf0555887891d4590
ms.sourcegitcommit: d81912782a8b0bd164f30a516ad80f8defb5d020
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179305"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="ada6e-103">Carga de archivos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ada6e-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="ada6e-104">Por [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/) y [Rutger Storm](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="ada6e-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ada6e-105">ASP.NET Core admite la carga de uno o varios archivos mediante el enlace de modelos almacenado en búfer de archivos más pequeños y el streaming no almacenado en búfer de archivos de mayor tamaño.</span><span class="sxs-lookup"><span data-stu-id="ada6e-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="ada6e-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ada6e-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="ada6e-107">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="ada6e-107">Security considerations</span></span>

<span data-ttu-id="ada6e-108">Tenga precaución al proporcionar a los usuarios la capacidad de cargar archivos en un servidor.</span><span class="sxs-lookup"><span data-stu-id="ada6e-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="ada6e-109">Es posible que los atacantes intenten lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ada6e-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="ada6e-110">Ejecutar ataques por [denegación de servicio](/windows-hardware/drivers/ifs/denial-of-service).</span><span class="sxs-lookup"><span data-stu-id="ada6e-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="ada6e-111">Cargar virus o malware.</span><span class="sxs-lookup"><span data-stu-id="ada6e-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="ada6e-112">Poner en riesgo redes y servidores de otras maneras.</span><span class="sxs-lookup"><span data-stu-id="ada6e-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="ada6e-113">Estos son algunos de los pasos de seguridad con los que se reduce la probabilidad de sufrir ataques:</span><span class="sxs-lookup"><span data-stu-id="ada6e-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="ada6e-114">Cargue los archivos a un área de carga de archivos dedicada del sistema, de preferencia una unidad que no sea de sistema.</span><span class="sxs-lookup"><span data-stu-id="ada6e-114">Upload files to a dedicated file upload area on the system, preferably to a non-system drive.</span></span> <span data-ttu-id="ada6e-115">Usar una ubicación dedicada permite imponer de manera más sencilla restricciones de seguridad en los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="ada6e-115">Using a dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="ada6e-116">Deshabilite la ejecución de los permisos en la ubicación de carga de archivos.&dagger;</span><span class="sxs-lookup"><span data-stu-id="ada6e-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="ada6e-117">Los archivos cargados nunca deben persistir en el mismo árbol de directorio que la aplicación.&dagger;</span><span class="sxs-lookup"><span data-stu-id="ada6e-117">Never persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="ada6e-118">Use un nombre de archivo seguro determinado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="ada6e-119">No use un nombre de archivo que proporcione el usuario ni el nombre de archivo que no es de confianza del archivo cargado.&dagger; Para mostrar un nombre de archivo que no es de confianza en una interfaz de usuario o en un mensaje de registro, codifique el valor en HTML.</span><span class="sxs-lookup"><span data-stu-id="ada6e-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; To display an untrusted file name in a UI or in a logging message, HTML-encode the value.</span></span>
* <span data-ttu-id="ada6e-120">Permita únicamente un conjunto específico de extensiones de archivo aprobadas.&dagger;</span><span class="sxs-lookup"><span data-stu-id="ada6e-120">Only allow a specific set of approved file extensions.&dagger;</span></span>
* <span data-ttu-id="ada6e-121">Compruebe la firma de formato de archivo para evitar que un usuario cargue un archivo enmascarado.&dagger; Por ejemplo, no permita que un usuario cargue un archivo *.exe* con una extensión *.txt*.</span><span class="sxs-lookup"><span data-stu-id="ada6e-121">Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension.</span></span>
* <span data-ttu-id="ada6e-122">Compruebe que también se llevan a cabo comprobaciones de cliente en el servidor.&dagger; Las comprobaciones de cliente son fáciles de sortear.</span><span class="sxs-lookup"><span data-stu-id="ada6e-122">Verify that client-side checks are also performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="ada6e-123">Compruebe el tamaño de un archivo cargado y evite las cargas mayores de lo esperado.&dagger;</span><span class="sxs-lookup"><span data-stu-id="ada6e-123">Check the size of an uploaded file and prevent uploads that are larger than expected.&dagger;</span></span>
* <span data-ttu-id="ada6e-124">Cuando un archivo cargado con el mismo nombre no deba sobrescribir los archivos, vuelva a comprobar el nombre de archivo en la base de datos o en el almacenamiento físico antes de cargarlo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-124">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="ada6e-125">**Ejecute un detector de virus o malware en el contenido cargado antes de que se almacene el archivo.**</span><span class="sxs-lookup"><span data-stu-id="ada6e-125">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="ada6e-126">&dagger;La aplicación de ejemplo muestra un enfoque que cumple los criterios.</span><span class="sxs-lookup"><span data-stu-id="ada6e-126">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="ada6e-127">La carga de código malintencionado en un sistema suele ser el primer paso para ejecutar código que puede:</span><span class="sxs-lookup"><span data-stu-id="ada6e-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="ada6e-128">Obtener el control completo de un sistema.</span><span class="sxs-lookup"><span data-stu-id="ada6e-128">Completely gain control of a system.</span></span>
> * <span data-ttu-id="ada6e-129">Sobrecargar un sistema de manera que el sistema se bloquea.</span><span class="sxs-lookup"><span data-stu-id="ada6e-129">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="ada6e-130">Poner en peligro los datos del usuario o del sistema.</span><span class="sxs-lookup"><span data-stu-id="ada6e-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="ada6e-131">Aplicar grafitis a una interfaz de usuario pública.</span><span class="sxs-lookup"><span data-stu-id="ada6e-131">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="ada6e-132">Vea los siguientes recursos para más información sobre cómo reducir el área expuesta de ataques al aceptar archivos de los usuarios:</span><span class="sxs-lookup"><span data-stu-id="ada6e-132">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="ada6e-133">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Carga de archivos sin restricciones)</span><span class="sxs-lookup"><span data-stu-id="ada6e-133">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * [<span data-ttu-id="ada6e-134">Azure Security: Asegúrese de que los controles adecuados estén en vigor al aceptar archivos de usuarios</span><span class="sxs-lookup"><span data-stu-id="ada6e-134">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="ada6e-135">Para más información sobre cómo implementar medidas de seguridad, incluidos ejemplos de la aplicación de ejemplo, consulte la sección [Validación](#validation).</span><span class="sxs-lookup"><span data-stu-id="ada6e-135">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="ada6e-136">Escenarios de almacenamiento</span><span class="sxs-lookup"><span data-stu-id="ada6e-136">Storage scenarios</span></span>

<span data-ttu-id="ada6e-137">Las opciones de almacenamiento comunes para los archivos incluyen:</span><span class="sxs-lookup"><span data-stu-id="ada6e-137">Common storage options for files include:</span></span>

* <span data-ttu-id="ada6e-138">Base de datos</span><span class="sxs-lookup"><span data-stu-id="ada6e-138">Database</span></span>

  * <span data-ttu-id="ada6e-139">En el caso de cargas de archivos pequeñas, una base de datos suele ser más rápida que las opciones de almacenamiento físico (sistema de archivos o recurso compartido de red).</span><span class="sxs-lookup"><span data-stu-id="ada6e-139">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="ada6e-140">Una base de datos suele ser más conveniente que las opciones de almacenamiento físico, ya que la recuperación de un registro de base de datos para los datos de usuario puede proporcionar el contenido del archivo (por ejemplo, una imagen de avatar).</span><span class="sxs-lookup"><span data-stu-id="ada6e-140">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="ada6e-141">Una base de datos puede ser más económica que usar un servicio de almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-141">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="ada6e-142">Almacenamiento físico (sistema de archivos o recurso compartido de red)</span><span class="sxs-lookup"><span data-stu-id="ada6e-142">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="ada6e-143">Para cargas de archivos de gran tamaño:</span><span class="sxs-lookup"><span data-stu-id="ada6e-143">For large file uploads:</span></span>
    * <span data-ttu-id="ada6e-144">Los límites de base de datos pueden restringir el tamaño de la carga.</span><span class="sxs-lookup"><span data-stu-id="ada6e-144">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="ada6e-145">El almacenamiento físico suele ser menos económico que el almacenamiento en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-145">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="ada6e-146">El almacenamiento físico puede ser más económico que usar un servicio de almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-146">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="ada6e-147">El proceso de la aplicación debe tener permisos de lectura y escritura en la ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="ada6e-147">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="ada6e-148">**Nunca conceda el permiso de ejecución.**</span><span class="sxs-lookup"><span data-stu-id="ada6e-148">**Never grant execute permission.**</span></span>

* <span data-ttu-id="ada6e-149">Servicio de almacenamiento de datos (por ejemplo, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="ada6e-149">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="ada6e-150">Los servicios suelen ofrecer una escalabilidad y resistencia mejoradas sobre las soluciones locales que normalmente están sujetas a únicos puntos de error.</span><span class="sxs-lookup"><span data-stu-id="ada6e-150">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="ada6e-151">Los servicios pueden tener un costo menor en escenarios de infraestructura de almacenamiento de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="ada6e-151">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="ada6e-152">Para obtener más información, vea [Inicio rápido: Uso de .NET para crear un blob en el almacenamiento de objetos](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="ada6e-152">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="ada6e-153">En el tema se muestra <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, pero se puede usar <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> para guardar un <xref:System.IO.FileStream> en el almacenamiento de blobs cuando se trabaja con un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="ada6e-153">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="ada6e-154">Escenarios de carga de archivos</span><span class="sxs-lookup"><span data-stu-id="ada6e-154">File upload scenarios</span></span>

<span data-ttu-id="ada6e-155">Dos enfoques generales para cargar archivos son el almacenamiento en búfer y el streaming.</span><span class="sxs-lookup"><span data-stu-id="ada6e-155">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="ada6e-156">**Almacenamiento en búfer**</span><span class="sxs-lookup"><span data-stu-id="ada6e-156">**Buffering**</span></span>

<span data-ttu-id="ada6e-157">El archivo completo se lee en un <xref:Microsoft.AspNetCore.Http.IFormFile>, que es una representación de C# del archivo que se usa para procesar o guardar el archivo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-157">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="ada6e-158">Los recursos (disco, memoria) que se usan en las cargas de archivos dependen de la cantidad y del tamaño de las cargas de archivos que se realizan simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="ada6e-158">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="ada6e-159">Si una aplicación intenta almacenar demasiadas cargas en el búfer, el sitio se bloquea cuando se queda sin memoria o sin espacio en disco.</span><span class="sxs-lookup"><span data-stu-id="ada6e-159">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="ada6e-160">Si el tamaño o la frecuencia de las cargas de archivos está agotando los recursos de la aplicación, use el streaming.</span><span class="sxs-lookup"><span data-stu-id="ada6e-160">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="ada6e-161">Cualquier archivo almacenado en búfer único que supere los 64 KB se mueve de la memoria a un archivo temporal en el disco.</span><span class="sxs-lookup"><span data-stu-id="ada6e-161">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="ada6e-162">En las secciones siguientes de este tema se habla del almacenamiento en búfer de archivos pequeños:</span><span class="sxs-lookup"><span data-stu-id="ada6e-162">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="ada6e-163">Almacenamiento físico</span><span class="sxs-lookup"><span data-stu-id="ada6e-163">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="ada6e-164">Base de datos</span><span class="sxs-lookup"><span data-stu-id="ada6e-164">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="ada6e-165">**Streaming**</span><span class="sxs-lookup"><span data-stu-id="ada6e-165">**Streaming**</span></span>

<span data-ttu-id="ada6e-166">El archivo se recibe de una solicitud de varias partes y lo procesa o guarda directamente la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-166">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="ada6e-167">El streaming no mejora considerablemente el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ada6e-167">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="ada6e-168">El streaming reduce las demandas de memoria o espacio en disco cuando se cargan archivos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-168">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="ada6e-169">El streaming de archivos grandes se describe en la sección [Carga de archivos de gran tamaño con streaming](#upload-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="ada6e-169">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="ada6e-170">Carga de archivos pequeños con enlace de modelos almacenado en búfer al almacenamiento físico</span><span class="sxs-lookup"><span data-stu-id="ada6e-170">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="ada6e-171">Para cargar archivos pequeños, se puede usar un formulario de varias partes o construir una solicitud POST con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ada6e-171">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="ada6e-172">En el ejemplo siguiente se muestra el uso de un formulario de Razor Pages para cargar un archivo único (*Pages/BufferedSingleFileUploadPhysical.cshtml* en la aplicación de ejemplo):</span><span class="sxs-lookup"><span data-stu-id="ada6e-172">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="ada6e-173">El ejemplo siguiente es análogo al ejemplo anterior, salvo en que:</span><span class="sxs-lookup"><span data-stu-id="ada6e-173">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="ada6e-174">([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) de JavaScript se usa para enviar los datos del formulario.</span><span class="sxs-lookup"><span data-stu-id="ada6e-174">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="ada6e-175">No hay ninguna validación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-175">There's no validation.</span></span>

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

<span data-ttu-id="ada6e-176">Para realizar la solicitud POST en JavaScript para los clientes que [no admiten Fetch API](https://caniuse.com/#feat=fetch), use uno de estos enfoques:</span><span class="sxs-lookup"><span data-stu-id="ada6e-176">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="ada6e-177">Use un Fetch Polyfill (por ejemplo, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="ada6e-177">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="ada6e-178">Use `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="ada6e-178">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="ada6e-179">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ada6e-179">For example:</span></span>

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

<span data-ttu-id="ada6e-180">Para admitir las cargas de archivos, los formularios HTML deben especificar un tipo de codificación (`enctype`) de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="ada6e-180">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="ada6e-181">Para que un elemento de entrada `files` admita al carga de varios archivos, proporcione el atributo `multiple` en el elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-181">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="ada6e-182">Es posible acceder a archivos individuales cargados en el servidor a través del [enlace de modelos](xref:mvc/models/model-binding) mediante <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="ada6e-182">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="ada6e-183">La aplicación de ejemplo muestra varias cargas de archivos almacenados en búfer para escenarios de almacenamiento físico y base de datos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-183">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

> [!WARNING]
> <span data-ttu-id="ada6e-184">No se base ni confíe en la propiedad `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sin validarla.</span><span class="sxs-lookup"><span data-stu-id="ada6e-184">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="ada6e-185">La propiedad `FileName` solo debe usarse para fines de presentación y solo después de codificar el valor de HTML.</span><span class="sxs-lookup"><span data-stu-id="ada6e-185">The `FileName` property should only be used for display purposes and only after HTML encoding the value.</span></span>
>
> <span data-ttu-id="ada6e-186">Los ejemplos proporcionados hasta ahora no tienen en cuenta las consideraciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="ada6e-186">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="ada6e-187">Se proporciona información adicional en las secciones siguientes y en la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="ada6e-187">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="ada6e-188">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="ada6e-188">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="ada6e-189">Validación</span><span class="sxs-lookup"><span data-stu-id="ada6e-189">Validation</span></span>](#validation)

<span data-ttu-id="ada6e-190">Al cargar archivos mediante el enlace de modelos y <xref:Microsoft.AspNetCore.Http.IFormFile>, el método de acción puede aceptar:</span><span class="sxs-lookup"><span data-stu-id="ada6e-190">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="ada6e-191">Un <xref:Microsoft.AspNetCore.Http.IFormFile> único.</span><span class="sxs-lookup"><span data-stu-id="ada6e-191">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="ada6e-192">Cualquiera de las colecciones siguientes que representan varios archivos:</span><span class="sxs-lookup"><span data-stu-id="ada6e-192">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="ada6e-193">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="ada6e-193">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="ada6e-194">El enlace coincide con los archivos de formulario por nombre.</span><span class="sxs-lookup"><span data-stu-id="ada6e-194">Binding matches form files by name.</span></span> <span data-ttu-id="ada6e-195">Por ejemplo, el valor `name` HTML en `<input type="file" name="formFile">` debe coincidir con la propiedad/el parámetro enlazado de C# (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="ada6e-195">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="ada6e-196">Para más información, consulte la sección [Coincidencia del valor de atributo de nombre con el nombre del parámetro del método POST](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="ada6e-196">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="ada6e-197">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ada6e-197">The following example:</span></span>

* <span data-ttu-id="ada6e-198">Recorre en bucle uno o más archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="ada6e-198">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="ada6e-199">Usa [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) para devolver una ruta de acceso completa de un archivo, incluido el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-199">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="ada6e-200">Guarda los archivos en el sistema de archivos local con un nombre de archivo generado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-200">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="ada6e-201">Devuelve el número total y el tamaño de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="ada6e-201">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="ada6e-202">Use `Path.GetRandomFileName` para generar un nombre de archivo sin una ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="ada6e-202">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="ada6e-203">En el ejemplo siguiente, la ruta de acceso se obtiene de la configuración:</span><span class="sxs-lookup"><span data-stu-id="ada6e-203">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="ada6e-204">La ruta de acceso pasada al <xref:System.IO.FileStream> *debe* incluir el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-204">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="ada6e-205">Si no se proporciona el nombre de archivo, se produce una <xref:System.UnauthorizedAccessException> en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="ada6e-205">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="ada6e-206">Los archivos que se cargan usando la técnica <xref:Microsoft.AspNetCore.Http.IFormFile> se almacenan en búfer en memoria o en disco en el servidor web antes de procesarse.</span><span class="sxs-lookup"><span data-stu-id="ada6e-206">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="ada6e-207">Dentro del método de acción, se puede tener acceso al contenido de <xref:Microsoft.AspNetCore.Http.IFormFile> como <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="ada6e-207">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="ada6e-208">Además del sistema de archivos local, los archivos se pueden guardar en un recurso compartido de red o en un servicio de almacenamiento de archivos, como [Azure Blob Storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="ada6e-208">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="ada6e-209">Para ver otro ejemplo que recorre en bucle varios archivos para cargar y usa nombres de archivo seguros, consulte *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-209">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="ada6e-210">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) arroja una <xref:System.IO.IOException> si se crean más de 65 535 archivos sin eliminar los archivos temporales anteriores.</span><span class="sxs-lookup"><span data-stu-id="ada6e-210">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="ada6e-211">El límite de 65 535 archivos es un límite por servidor.</span><span class="sxs-lookup"><span data-stu-id="ada6e-211">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="ada6e-212">Para más información sobre este límite en el sistema operativo Windows, consulte las notas en los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="ada6e-212">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="ada6e-213">GetTempFileNameA function</span><span class="sxs-lookup"><span data-stu-id="ada6e-213">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="ada6e-214">Carga de archivos pequeños con enlace de modelos almacenado en búfer a una base de datos</span><span class="sxs-lookup"><span data-stu-id="ada6e-214">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="ada6e-215">Para almacenar datos de archivo binario en una base de datos con [Entity Framework](/ef/core/index), defina una propiedad de matriz <xref:System.Byte> en la entidad:</span><span class="sxs-lookup"><span data-stu-id="ada6e-215">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="ada6e-216">Especifique una propiedad de modelo de página para la clase que incluya un <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="ada6e-216">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="ada6e-217"><xref:Microsoft.AspNetCore.Http.IFormFile> se puede usar directamente como un parámetro de método de acción o como una propiedad de modelo enlazado.</span><span class="sxs-lookup"><span data-stu-id="ada6e-217"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="ada6e-218">En el ejemplo anterior se utiliza una propiedad de modelo enlazado.</span><span class="sxs-lookup"><span data-stu-id="ada6e-218">The prior example uses a bound model property.</span></span>

<span data-ttu-id="ada6e-219">`FileUpload` se usa en el formulario de Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="ada6e-219">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="ada6e-220">Cuando el formulario se publique en el servidor, copie el <xref:Microsoft.AspNetCore.Http.IFormFile> en un flujo y guárdelo como matriz de bytes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-220">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="ada6e-221">En el ejemplo siguiente, `_dbContext` almacena el contexto de base de datos de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="ada6e-221">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="ada6e-222">El ejemplo anterior es similar a un escenario que se muestra en la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ada6e-222">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="ada6e-223">*Pages/BufferedSingleFileUploadDb.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ada6e-223">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="ada6e-224">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="ada6e-224">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="ada6e-225">Tenga cuidado al almacenar los datos binarios en bases de datos relacionales, ya que esto puede repercutir adversamente en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ada6e-225">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="ada6e-226">No se base ni confíe en la propiedad `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sin validarla.</span><span class="sxs-lookup"><span data-stu-id="ada6e-226">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="ada6e-227">La propiedad `FileName` solo debe usarse para fines de presentación y solo después de la codificación HTML.</span><span class="sxs-lookup"><span data-stu-id="ada6e-227">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="ada6e-228">Los ejemplos proporcionados no tienen en cuenta las consideraciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="ada6e-228">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="ada6e-229">Se proporciona información adicional en las secciones siguientes y en la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="ada6e-229">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="ada6e-230">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="ada6e-230">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="ada6e-231">Validación</span><span class="sxs-lookup"><span data-stu-id="ada6e-231">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="ada6e-232">Carga de archivos de gran tamaño con streaming</span><span class="sxs-lookup"><span data-stu-id="ada6e-232">Upload large files with streaming</span></span>

<span data-ttu-id="ada6e-233">En el ejemplo siguiente se muestra cómo usar JavaScript para transmitir un archivo a una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="ada6e-233">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="ada6e-234">El token de antifalsificación del archivo se genera por medio de un atributo de filtro personalizado y se pasa a los encabezados HTTP del cliente, en lugar de en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="ada6e-234">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="ada6e-235">Dado que el método de acción procesa los datos cargados directamente, el enlace de modelos del formulario se deshabilita por otro filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="ada6e-235">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="ada6e-236">Dentro de la acción, el contenido del formulario se lee usando un `MultipartReader` (que lee cada `MultipartSection` individual), de forma que el archivo se procesa o el contenido se almacena, según corresponda.</span><span class="sxs-lookup"><span data-stu-id="ada6e-236">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="ada6e-237">Una vez leídas las secciones de varias partes, la acción realiza su propio enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-237">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="ada6e-238">La respuesta de la página inicial carga el formulario y guarda un token de antifalsificación en una cookie (a través del atributo `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="ada6e-238">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="ada6e-239">El atributo usa la [compatibilidad de antifalsificación](xref:security/anti-request-forgery) integrada de ASP.NET Core para establecer una cookie con un token de solicitud:</span><span class="sxs-lookup"><span data-stu-id="ada6e-239">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="ada6e-240">El `DisableFormValueModelBindingAttribute` se usa para deshabilitar el enlace de modelos:</span><span class="sxs-lookup"><span data-stu-id="ada6e-240">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="ada6e-241">En la aplicación de ejemplo, `GenerateAntiforgeryTokenCookieAttribute` y `DisableFormValueModelBindingAttribute` se aplican como filtros a los modelos de aplicación de la página de `/StreamedSingleFileUploadDb` y `/StreamedSingleFileUploadPhysical` en `Startup.ConfigureServices` con las [convenciones de Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="ada6e-241">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="ada6e-242">Dado que el enlace de modelos no lee el formulario, los parámetros enlazados desde el formulario no se enlazan (la consulta, la ruta y el encabezado siguen funcionando).</span><span class="sxs-lookup"><span data-stu-id="ada6e-242">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="ada6e-243">El método de acción funciona directamente con la propiedad `Request`.</span><span class="sxs-lookup"><span data-stu-id="ada6e-243">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="ada6e-244">Se usa un elemento `MultipartReader` para leer cada sección.</span><span class="sxs-lookup"><span data-stu-id="ada6e-244">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="ada6e-245">Los datos de clave-valor se almacenan en un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="ada6e-245">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="ada6e-246">Una vez leídas las secciones de varias partes, el contenido del `KeyValueAccumulator` se usa para enlazar los datos del formulario a un tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-246">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="ada6e-247">El método `StreamingController.UploadDatabase` completo para streaming a una base de datos con EF Core:</span><span class="sxs-lookup"><span data-stu-id="ada6e-247">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="ada6e-248">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span><span class="sxs-lookup"><span data-stu-id="ada6e-248">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="ada6e-249">El método `StreamingController.UploadPhysical` completo para streaming a una ubicación física:</span><span class="sxs-lookup"><span data-stu-id="ada6e-249">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="ada6e-250">En la aplicación de ejemplo, las comprobaciones de validación las controla `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="ada6e-250">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="ada6e-251">Validación</span><span class="sxs-lookup"><span data-stu-id="ada6e-251">Validation</span></span>

<span data-ttu-id="ada6e-252">La clase `FileHelpers` de la aplicación de ejemplo muestra varias comprobaciones de cargas de archivos de streaming y <xref:Microsoft.AspNetCore.Http.IFormFile> almacenado en búfer.</span><span class="sxs-lookup"><span data-stu-id="ada6e-252">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="ada6e-253">Para procesar cargas de archivos almacenadas en búfer de <xref:Microsoft.AspNetCore.Http.IFormFile> en la aplicación de ejemplo, consulte el método `ProcessFormFile` en el archivo *Utilities/FileHelpers.cs*.</span><span class="sxs-lookup"><span data-stu-id="ada6e-253">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="ada6e-254">Para procesar archivos de streaming, consulte el método `ProcessStreamedFile` en el mismo archivo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-254">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="ada6e-255">Los métodos de procesamiento de validación mostrados en la aplicación de ejemplo no examinan el contenido de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="ada6e-255">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="ada6e-256">En la mayoría de los escenarios de producción, se usa una API de analizador de virus/malware en el archivo antes de que el archivo esté disponible para los usuarios u otros sistemas.</span><span class="sxs-lookup"><span data-stu-id="ada6e-256">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="ada6e-257">Aunque el ejemplo de tema proporciona un ejemplo funcional de técnicas de validación, no implemente la clase `FileHelpers` en una aplicación de producción a menos que:</span><span class="sxs-lookup"><span data-stu-id="ada6e-257">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="ada6e-258">Comprenda totalmente la implementación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-258">Fully understand the implementation.</span></span>
> * <span data-ttu-id="ada6e-259">Modifique la implementación según corresponda en función del entorno y las especificaciones de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-259">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="ada6e-260">**No implemente nunca de manera indiscriminada el código de seguridad en una aplicación sin abordar estos requisitos.**</span><span class="sxs-lookup"><span data-stu-id="ada6e-260">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="ada6e-261">Validación del contenido</span><span class="sxs-lookup"><span data-stu-id="ada6e-261">Content validation</span></span>

<span data-ttu-id="ada6e-262">**Use una API de detección de virus/malware de terceros en el contenido cargado.**</span><span class="sxs-lookup"><span data-stu-id="ada6e-262">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="ada6e-263">El análisis de archivos exige recursos del servidor en escenarios de gran volumen.</span><span class="sxs-lookup"><span data-stu-id="ada6e-263">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="ada6e-264">Si disminuye el rendimiento de procesamiento de solicitudes debido al análisis de archivos, considere la posibilidad de descargar el trabajo de análisis a un [servicio en segundo plano](xref:fundamentals/host/hosted-services), posiblemente un servicio que se ejecute en otro servidor desde el servidor de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-264">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="ada6e-265">Habitualmente, los archivos cargados se almacenan en un área en cuarentena hasta que el programa de detección de virus en segundo plano los revisa.</span><span class="sxs-lookup"><span data-stu-id="ada6e-265">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="ada6e-266">Cuando se pasa un archivo, se mueve a la ubicación de almacenamiento de archivos habitual.</span><span class="sxs-lookup"><span data-stu-id="ada6e-266">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="ada6e-267">Por lo general, estos pasos se realizan junto con un registro de base de datos que indica el estado de análisis de un archivo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-267">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="ada6e-268">Mediante el uso de este enfoque, la aplicación y el servidor de aplicaciones permanecen centrados en responder a las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="ada6e-268">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="ada6e-269">Validación de la extensión del archivo</span><span class="sxs-lookup"><span data-stu-id="ada6e-269">File extension validation</span></span>

<span data-ttu-id="ada6e-270">La extensión del archivo cargado debe comprobarse con una lista de extensiones permitidas.</span><span class="sxs-lookup"><span data-stu-id="ada6e-270">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="ada6e-271">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ada6e-271">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="ada6e-272">Validación de firma del archivo</span><span class="sxs-lookup"><span data-stu-id="ada6e-272">File signature validation</span></span>

<span data-ttu-id="ada6e-273">La firma de un archivo viene determinada por los primeros bytes al principio de un archivo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-273">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="ada6e-274">Estos bytes se pueden usar para indicar si la extensión coincide con el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-274">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="ada6e-275">La aplicación de ejemplo comprueba las firmas de archivo de algunos tipos de archivo comunes.</span><span class="sxs-lookup"><span data-stu-id="ada6e-275">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="ada6e-276">En el ejemplo siguiente, la firma de archivo de una imagen JPEG se comprueba con respecto al archivo:</span><span class="sxs-lookup"><span data-stu-id="ada6e-276">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="ada6e-277">Para obtener firmas de archivo adicionales, consulte la [base de datos de firmas de archivo](https://www.filesignatures.net/) y las especificaciones de archivo oficiales.</span><span class="sxs-lookup"><span data-stu-id="ada6e-277">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="ada6e-278">Seguridad de nombre de archivo</span><span class="sxs-lookup"><span data-stu-id="ada6e-278">File name security</span></span>

<span data-ttu-id="ada6e-279">Nunca use un nombre de archivo proporcionado por el cliente para guardar un archivo en el almacenamiento físico.</span><span class="sxs-lookup"><span data-stu-id="ada6e-279">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="ada6e-280">Cree un nombre de archivo seguro para el archivo con [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) o [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) para crear una ruta de acceso completa (incluido el nombre de archivo) para el almacenamiento temporal.</span><span class="sxs-lookup"><span data-stu-id="ada6e-280">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="ada6e-281">Razor codifica automáticamente en HTML los valores de propiedad para mostrarlos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-281">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="ada6e-282">El código siguiente es seguro de usar:</span><span class="sxs-lookup"><span data-stu-id="ada6e-282">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="ada6e-283">Fuera de Razor, siempre use <xref:System.Net.WebUtility.HtmlEncode*> al contenido del nombre de archivo de una solicitud de un usuario.</span><span class="sxs-lookup"><span data-stu-id="ada6e-283">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="ada6e-284">Muchas implementaciones deben incluir una comprobación de que el archivo existe; de lo contrario, el archivo se sobrescribe por un archivo con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="ada6e-284">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="ada6e-285">Proporcione lógica adicional para satisfacer las especificaciones de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-285">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="ada6e-286">Validación del tamaño</span><span class="sxs-lookup"><span data-stu-id="ada6e-286">Size validation</span></span>

<span data-ttu-id="ada6e-287">Limite el tamaño de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="ada6e-287">Limit the size of uploaded files.</span></span>

<span data-ttu-id="ada6e-288">En la aplicación de ejemplo, el tamaño del archivo está limitado a 2 MB (se indica en bytes).</span><span class="sxs-lookup"><span data-stu-id="ada6e-288">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="ada6e-289">El límite se proporciona a través de [Configuración](xref:fundamentals/configuration/index) del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="ada6e-289">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="ada6e-290">`FileSizeLimit` se inserta en las clases `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-290">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="ada6e-291">Cuando el tamaño de un archivo supera el límite, se rechaza el archivo:</span><span class="sxs-lookup"><span data-stu-id="ada6e-291">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="ada6e-292">Coincidencia del valor de atributo de nombre con el nombre del parámetro del método POST</span><span class="sxs-lookup"><span data-stu-id="ada6e-292">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="ada6e-293">En los formularios que no son de Razor que realizan la operación POST en los datos de formulario o usan directamente `FormData` de JavaScript, el nombre especificado en el elemento del formulario o `FormData` debe coincidir con el nombre del parámetro en la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="ada6e-293">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="ada6e-294">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ada6e-294">In the following example:</span></span>

* <span data-ttu-id="ada6e-295">Cuando se usa un elemento `<input>`, el atributo `name` se establece en el valor `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-295">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="ada6e-296">Cuando se usa `FormData` en JavaScript, el nombre se establece en el valor `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-296">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="ada6e-297">Use un nombre coincidente para el parámetro del método de C# (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="ada6e-297">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="ada6e-298">Para un método de control de páginas de Razor Pages denominado `Upload`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-298">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="ada6e-299">Para un método de acción de controlador POST de MVC:</span><span class="sxs-lookup"><span data-stu-id="ada6e-299">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="ada6e-300">Configuración del servidor y de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ada6e-300">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="ada6e-301">Límite de longitud del cuerpo de varias partes</span><span class="sxs-lookup"><span data-stu-id="ada6e-301">Multipart body length limit</span></span>

<span data-ttu-id="ada6e-302"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> establece el límite de la longitud de cada cuerpo de varias partes.</span><span class="sxs-lookup"><span data-stu-id="ada6e-302"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="ada6e-303">Las secciones del formulario que superan este límite inician una <xref:System.IO.InvalidDataException> cuando se analizan.</span><span class="sxs-lookup"><span data-stu-id="ada6e-303">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="ada6e-304">El valor predeterminado es 134 217 728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="ada6e-304">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="ada6e-305">Personalice el límite mediante el valor <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-305">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="ada6e-306"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> se utiliza para establecer el <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> para una sola página o acción.</span><span class="sxs-lookup"><span data-stu-id="ada6e-306"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="ada6e-307">En una aplicación de Razor Pages, aplique el filtro con una [convención](xref:razor-pages/razor-pages-conventions) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-307">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="ada6e-308">En una aplicación de Razor Pages o una aplicación MVC, aplique el filtro al método de acción o al modelo de página:</span><span class="sxs-lookup"><span data-stu-id="ada6e-308">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="ada6e-309">Tamaño máximo del cuerpo de la solicitud de Kestrel</span><span class="sxs-lookup"><span data-stu-id="ada6e-309">Kestrel maximum request body size</span></span>

<span data-ttu-id="ada6e-310">En el caso de las aplicaciones hospedadas por Kestrel, el tamaño máximo predeterminado del cuerpo de solicitud es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="ada6e-310">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="ada6e-311">Personalice el límite con la opción [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) del servidor de Kestrel:</span><span class="sxs-lookup"><span data-stu-id="ada6e-311">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="ada6e-312"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> se usa para establecer el valor de [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) de una sola página o acción.</span><span class="sxs-lookup"><span data-stu-id="ada6e-312"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="ada6e-313">En una aplicación de Razor Pages, aplique el filtro con una [convención](xref:razor-pages/razor-pages-conventions) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-313">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="ada6e-314">En una aplicación de Razor Pages o una aplicación MVC, aplique el filtro al método de acción o a la clase de control de página:</span><span class="sxs-lookup"><span data-stu-id="ada6e-314">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="ada6e-315">El `RequestSizeLimitAttribute` también se puede aplicar con la directiva [@attribute](xref:mvc/views/razor#attribute) de Razor:</span><span class="sxs-lookup"><span data-stu-id="ada6e-315">The `RequestSizeLimitAttribute` can also be applied using the [@attribute](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="ada6e-316">Otros límites de Kestrel</span><span class="sxs-lookup"><span data-stu-id="ada6e-316">Other Kestrel limits</span></span>

<span data-ttu-id="ada6e-317">Otros límites de Kestrel pueden aplicarse a las aplicaciones hospedadas por Kestrel:</span><span class="sxs-lookup"><span data-stu-id="ada6e-317">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="ada6e-318">Conexiones de cliente máximas</span><span class="sxs-lookup"><span data-stu-id="ada6e-318">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="ada6e-319">Tarifas de datos de solicitud y respuesta</span><span class="sxs-lookup"><span data-stu-id="ada6e-319">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="ada6e-320">Límite de longitud del contenido de IIS</span><span class="sxs-lookup"><span data-stu-id="ada6e-320">IIS content length limit</span></span>

<span data-ttu-id="ada6e-321">El límite predeterminado de solicitudes (`maxAllowedContentLength`) es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="ada6e-321">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="ada6e-322">Personalice el límite en el archivo *web.config*:</span><span class="sxs-lookup"><span data-stu-id="ada6e-322">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="ada6e-323">Esto solo ocurre en IIS;</span><span class="sxs-lookup"><span data-stu-id="ada6e-323">This setting only applies to IIS.</span></span> <span data-ttu-id="ada6e-324">este comportamiento no sucede de forma predeterminada cuando los archivos se hospedan en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ada6e-324">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="ada6e-325">Para más información, consulte [Límites de solicitud \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="ada6e-325">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="ada6e-326">Las limitaciones del módulo ASP.NET Core o la presencia del módulo de filtrado de solicitudes de IIS pueden limitar las cargas a 2 GB o 4 GB.</span><span class="sxs-lookup"><span data-stu-id="ada6e-326">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="ada6e-327">Para más información, consulte el artículo que indica que [no se pueden cargar archivos con un tamaño superior a 2 GB (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="ada6e-327">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="ada6e-328">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="ada6e-328">Troubleshoot</span></span>

<span data-ttu-id="ada6e-329">Aquí incluimos algunos problemas comunes que pueden surgir al cargar archivos, así como sus posibles soluciones.</span><span class="sxs-lookup"><span data-stu-id="ada6e-329">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="ada6e-330">Error No encontrado al implementar en un servidor IIS</span><span class="sxs-lookup"><span data-stu-id="ada6e-330">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="ada6e-331">El error siguiente indica que el archivo cargado supera la longitud configurada del contenido del servidor:</span><span class="sxs-lookup"><span data-stu-id="ada6e-331">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="ada6e-332">Para más información sobre cómo aumentar el límite, consulte la sección [Límite de longitud del contenido de IIS](#iis-content-length-limit).</span><span class="sxs-lookup"><span data-stu-id="ada6e-332">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="ada6e-333">Error de conexión</span><span class="sxs-lookup"><span data-stu-id="ada6e-333">Connection failure</span></span>

<span data-ttu-id="ada6e-334">Un error de conexión y una conexión del servidor de restablecimiento probablemente indica que el archivo cargado supera el tamaño máximo del cuerpo de la solicitud de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ada6e-334">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="ada6e-335">Para más información, consulte la sección [Tamaño máximo del cuerpo de la solicitud de Kestrel](#kestrel-maximum-request-body-size).</span><span class="sxs-lookup"><span data-stu-id="ada6e-335">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="ada6e-336">Los límites de conexión del cliente de Kestrel también pueden requerir ajustes.</span><span class="sxs-lookup"><span data-stu-id="ada6e-336">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="ada6e-337">Excepción de referencia nula con IFormFile</span><span class="sxs-lookup"><span data-stu-id="ada6e-337">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="ada6e-338">Si el controlador acepta archivos cargados con <xref:Microsoft.AspNetCore.Http.IFormFile>, pero el valor es `null`, confirme que el formulario HTML especifica un valor `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="ada6e-338">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="ada6e-339">Si este atributo no está establecido en el elemento `<form>`, no se llevará a cabo la carga del archivo y cualquier argumento <xref:Microsoft.AspNetCore.Http.IFormFile> enlazado será `null`.</span><span class="sxs-lookup"><span data-stu-id="ada6e-339">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="ada6e-340">Confirme también que la [nomenclatura de la carga en los datos de formulario coincide con la nomenclatura de la aplicación](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="ada6e-340">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ada6e-341">ASP.NET Core admite la carga de uno o varios archivos mediante el enlace de modelos almacenado en búfer de archivos más pequeños y el streaming no almacenado en búfer de archivos de mayor tamaño.</span><span class="sxs-lookup"><span data-stu-id="ada6e-341">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="ada6e-342">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ada6e-342">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="ada6e-343">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="ada6e-343">Security considerations</span></span>

<span data-ttu-id="ada6e-344">Tenga precaución al proporcionar a los usuarios la capacidad de cargar archivos en un servidor.</span><span class="sxs-lookup"><span data-stu-id="ada6e-344">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="ada6e-345">Es posible que los atacantes intenten lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ada6e-345">Attackers may attempt to:</span></span>

* <span data-ttu-id="ada6e-346">Ejecutar ataques por [denegación de servicio](/windows-hardware/drivers/ifs/denial-of-service).</span><span class="sxs-lookup"><span data-stu-id="ada6e-346">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="ada6e-347">Cargar virus o malware.</span><span class="sxs-lookup"><span data-stu-id="ada6e-347">Upload viruses or malware.</span></span>
* <span data-ttu-id="ada6e-348">Poner en riesgo redes y servidores de otras maneras.</span><span class="sxs-lookup"><span data-stu-id="ada6e-348">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="ada6e-349">Estos son algunos de los pasos de seguridad con los que se reduce la probabilidad de sufrir ataques:</span><span class="sxs-lookup"><span data-stu-id="ada6e-349">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="ada6e-350">Cargue los archivos a un área de carga de archivos dedicada del sistema, de preferencia una unidad que no sea de sistema.</span><span class="sxs-lookup"><span data-stu-id="ada6e-350">Upload files to a dedicated file upload area on the system, preferably to a non-system drive.</span></span> <span data-ttu-id="ada6e-351">Usar una ubicación dedicada permite imponer de manera más sencilla restricciones de seguridad en los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="ada6e-351">Using a dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="ada6e-352">Deshabilite la ejecución de los permisos en la ubicación de carga de archivos.&dagger;</span><span class="sxs-lookup"><span data-stu-id="ada6e-352">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="ada6e-353">Los archivos cargados nunca deben persistir en el mismo árbol de directorio que la aplicación.&dagger;</span><span class="sxs-lookup"><span data-stu-id="ada6e-353">Never persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="ada6e-354">Use un nombre de archivo seguro determinado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-354">Use a safe file name determined by the app.</span></span> <span data-ttu-id="ada6e-355">No use un nombre de archivo que proporcione el usuario ni el nombre de archivo que no es de confianza del archivo cargado.&dagger; Para mostrar un nombre de archivo que no es de confianza en una interfaz de usuario o en un mensaje de registro, codifique el valor en HTML.</span><span class="sxs-lookup"><span data-stu-id="ada6e-355">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; To display an untrusted file name in a UI or in a logging message, HTML-encode the value.</span></span>
* <span data-ttu-id="ada6e-356">Permita únicamente un conjunto específico de extensiones de archivo aprobadas.&dagger;</span><span class="sxs-lookup"><span data-stu-id="ada6e-356">Only allow a specific set of approved file extensions.&dagger;</span></span>
* <span data-ttu-id="ada6e-357">Compruebe la firma de formato de archivo para evitar que un usuario cargue un archivo enmascarado.&dagger; Por ejemplo, no permita que un usuario cargue un archivo *.exe* con una extensión *.txt*.</span><span class="sxs-lookup"><span data-stu-id="ada6e-357">Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension.</span></span>
* <span data-ttu-id="ada6e-358">Compruebe que también se llevan a cabo comprobaciones de cliente en el servidor.&dagger; Las comprobaciones de cliente son fáciles de sortear.</span><span class="sxs-lookup"><span data-stu-id="ada6e-358">Verify that client-side checks are also performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="ada6e-359">Compruebe el tamaño de un archivo cargado y evite las cargas mayores de lo esperado.&dagger;</span><span class="sxs-lookup"><span data-stu-id="ada6e-359">Check the size of an uploaded file and prevent uploads that are larger than expected.&dagger;</span></span>
* <span data-ttu-id="ada6e-360">Cuando un archivo cargado con el mismo nombre no deba sobrescribir los archivos, vuelva a comprobar el nombre de archivo en la base de datos o en el almacenamiento físico antes de cargarlo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-360">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="ada6e-361">**Ejecute un detector de virus o malware en el contenido cargado antes de que se almacene el archivo.**</span><span class="sxs-lookup"><span data-stu-id="ada6e-361">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="ada6e-362">&dagger;La aplicación de ejemplo muestra un enfoque que cumple los criterios.</span><span class="sxs-lookup"><span data-stu-id="ada6e-362">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="ada6e-363">La carga de código malintencionado en un sistema suele ser el primer paso para ejecutar código que puede:</span><span class="sxs-lookup"><span data-stu-id="ada6e-363">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="ada6e-364">Obtener el control completo de un sistema.</span><span class="sxs-lookup"><span data-stu-id="ada6e-364">Completely gain control of a system.</span></span>
> * <span data-ttu-id="ada6e-365">Sobrecargar un sistema de manera que el sistema se bloquea.</span><span class="sxs-lookup"><span data-stu-id="ada6e-365">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="ada6e-366">Poner en peligro los datos del usuario o del sistema.</span><span class="sxs-lookup"><span data-stu-id="ada6e-366">Compromise user or system data.</span></span>
> * <span data-ttu-id="ada6e-367">Aplicar grafitis a una interfaz de usuario pública.</span><span class="sxs-lookup"><span data-stu-id="ada6e-367">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="ada6e-368">Vea los siguientes recursos para más información sobre cómo reducir el área expuesta de ataques al aceptar archivos de los usuarios:</span><span class="sxs-lookup"><span data-stu-id="ada6e-368">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * <span data-ttu-id="ada6e-369">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Carga de archivos sin restricciones)</span><span class="sxs-lookup"><span data-stu-id="ada6e-369">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
> * [<span data-ttu-id="ada6e-370">Azure Security: Asegúrese de que los controles adecuados estén en vigor al aceptar archivos de usuarios</span><span class="sxs-lookup"><span data-stu-id="ada6e-370">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="ada6e-371">Para más información sobre cómo implementar medidas de seguridad, incluidos ejemplos de la aplicación de ejemplo, consulte la sección [Validación](#validation).</span><span class="sxs-lookup"><span data-stu-id="ada6e-371">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="ada6e-372">Escenarios de almacenamiento</span><span class="sxs-lookup"><span data-stu-id="ada6e-372">Storage scenarios</span></span>

<span data-ttu-id="ada6e-373">Las opciones de almacenamiento comunes para los archivos incluyen:</span><span class="sxs-lookup"><span data-stu-id="ada6e-373">Common storage options for files include:</span></span>

* <span data-ttu-id="ada6e-374">Base de datos</span><span class="sxs-lookup"><span data-stu-id="ada6e-374">Database</span></span>

  * <span data-ttu-id="ada6e-375">En el caso de cargas de archivos pequeñas, una base de datos suele ser más rápida que las opciones de almacenamiento físico (sistema de archivos o recurso compartido de red).</span><span class="sxs-lookup"><span data-stu-id="ada6e-375">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="ada6e-376">Una base de datos suele ser más conveniente que las opciones de almacenamiento físico, ya que la recuperación de un registro de base de datos para los datos de usuario puede proporcionar el contenido del archivo (por ejemplo, una imagen de avatar).</span><span class="sxs-lookup"><span data-stu-id="ada6e-376">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="ada6e-377">Una base de datos puede ser más económica que usar un servicio de almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-377">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="ada6e-378">Almacenamiento físico (sistema de archivos o recurso compartido de red)</span><span class="sxs-lookup"><span data-stu-id="ada6e-378">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="ada6e-379">Para cargas de archivos de gran tamaño:</span><span class="sxs-lookup"><span data-stu-id="ada6e-379">For large file uploads:</span></span>
    * <span data-ttu-id="ada6e-380">Los límites de base de datos pueden restringir el tamaño de la carga.</span><span class="sxs-lookup"><span data-stu-id="ada6e-380">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="ada6e-381">El almacenamiento físico suele ser menos económico que el almacenamiento en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-381">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="ada6e-382">El almacenamiento físico puede ser más económico que usar un servicio de almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-382">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="ada6e-383">El proceso de la aplicación debe tener permisos de lectura y escritura en la ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="ada6e-383">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="ada6e-384">**Nunca conceda el permiso de ejecución.**</span><span class="sxs-lookup"><span data-stu-id="ada6e-384">**Never grant execute permission.**</span></span>

* <span data-ttu-id="ada6e-385">Servicio de almacenamiento de datos (por ejemplo, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="ada6e-385">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="ada6e-386">Los servicios suelen ofrecer una escalabilidad y resistencia mejoradas sobre las soluciones locales que normalmente están sujetas a únicos puntos de error.</span><span class="sxs-lookup"><span data-stu-id="ada6e-386">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="ada6e-387">Los servicios pueden tener un costo menor en escenarios de infraestructura de almacenamiento de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="ada6e-387">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="ada6e-388">Para obtener más información, vea [Inicio rápido: Uso de .NET para crear un blob en el almacenamiento de objetos](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="ada6e-388">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="ada6e-389">En el tema se muestra <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, pero se puede usar <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> para guardar un <xref:System.IO.FileStream> en el almacenamiento de blobs cuando se trabaja con un <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="ada6e-389">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="ada6e-390">Escenarios de carga de archivos</span><span class="sxs-lookup"><span data-stu-id="ada6e-390">File upload scenarios</span></span>

<span data-ttu-id="ada6e-391">Dos enfoques generales para cargar archivos son el almacenamiento en búfer y el streaming.</span><span class="sxs-lookup"><span data-stu-id="ada6e-391">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="ada6e-392">**Almacenamiento en búfer**</span><span class="sxs-lookup"><span data-stu-id="ada6e-392">**Buffering**</span></span>

<span data-ttu-id="ada6e-393">El archivo completo se lee en un <xref:Microsoft.AspNetCore.Http.IFormFile>, que es una representación de C# del archivo que se usa para procesar o guardar el archivo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-393">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="ada6e-394">Los recursos (disco, memoria) que se usan en las cargas de archivos dependen de la cantidad y del tamaño de las cargas de archivos que se realizan simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="ada6e-394">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="ada6e-395">Si una aplicación intenta almacenar demasiadas cargas en el búfer, el sitio se bloquea cuando se queda sin memoria o sin espacio en disco.</span><span class="sxs-lookup"><span data-stu-id="ada6e-395">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="ada6e-396">Si el tamaño o la frecuencia de las cargas de archivos está agotando los recursos de la aplicación, use el streaming.</span><span class="sxs-lookup"><span data-stu-id="ada6e-396">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="ada6e-397">Cualquier archivo almacenado en búfer único que supere los 64 KB se mueve de la memoria a un archivo temporal en el disco.</span><span class="sxs-lookup"><span data-stu-id="ada6e-397">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="ada6e-398">En las secciones siguientes de este tema se habla del almacenamiento en búfer de archivos pequeños:</span><span class="sxs-lookup"><span data-stu-id="ada6e-398">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="ada6e-399">Almacenamiento físico</span><span class="sxs-lookup"><span data-stu-id="ada6e-399">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="ada6e-400">Base de datos</span><span class="sxs-lookup"><span data-stu-id="ada6e-400">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="ada6e-401">**Streaming**</span><span class="sxs-lookup"><span data-stu-id="ada6e-401">**Streaming**</span></span>

<span data-ttu-id="ada6e-402">El archivo se recibe de una solicitud de varias partes y lo procesa o guarda directamente la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-402">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="ada6e-403">El streaming no mejora considerablemente el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ada6e-403">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="ada6e-404">El streaming reduce las demandas de memoria o espacio en disco cuando se cargan archivos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-404">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="ada6e-405">El streaming de archivos grandes se describe en la sección [Carga de archivos de gran tamaño con streaming](#upload-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="ada6e-405">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="ada6e-406">Carga de archivos pequeños con enlace de modelos almacenado en búfer al almacenamiento físico</span><span class="sxs-lookup"><span data-stu-id="ada6e-406">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="ada6e-407">Para cargar archivos pequeños, se puede usar un formulario de varias partes o construir una solicitud POST con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ada6e-407">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="ada6e-408">En el ejemplo siguiente se muestra el uso de un formulario de Razor Pages para cargar un archivo único (*Pages/BufferedSingleFileUploadPhysical.cshtml* en la aplicación de ejemplo):</span><span class="sxs-lookup"><span data-stu-id="ada6e-408">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="ada6e-409">El ejemplo siguiente es análogo al ejemplo anterior, salvo en que:</span><span class="sxs-lookup"><span data-stu-id="ada6e-409">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="ada6e-410">([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) de JavaScript se usa para enviar los datos del formulario.</span><span class="sxs-lookup"><span data-stu-id="ada6e-410">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="ada6e-411">No hay ninguna validación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-411">There's no validation.</span></span>

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

<span data-ttu-id="ada6e-412">Para realizar la solicitud POST en JavaScript para los clientes que [no admiten Fetch API](https://caniuse.com/#feat=fetch), use uno de estos enfoques:</span><span class="sxs-lookup"><span data-stu-id="ada6e-412">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="ada6e-413">Use un Fetch Polyfill (por ejemplo, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="ada6e-413">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="ada6e-414">Use `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="ada6e-414">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="ada6e-415">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ada6e-415">For example:</span></span>

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

<span data-ttu-id="ada6e-416">Para admitir las cargas de archivos, los formularios HTML deben especificar un tipo de codificación (`enctype`) de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="ada6e-416">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="ada6e-417">Para que un elemento de entrada `files` admita al carga de varios archivos, proporcione el atributo `multiple` en el elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-417">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="ada6e-418">Es posible acceder a archivos individuales cargados en el servidor a través del [enlace de modelos](xref:mvc/models/model-binding) mediante <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="ada6e-418">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="ada6e-419">La aplicación de ejemplo muestra varias cargas de archivos almacenados en búfer para escenarios de almacenamiento físico y base de datos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-419">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

> [!WARNING]
> <span data-ttu-id="ada6e-420">No se base ni confíe en la propiedad `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sin validarla.</span><span class="sxs-lookup"><span data-stu-id="ada6e-420">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="ada6e-421">La propiedad `FileName` solo debe usarse para fines de presentación y solo después de codificar el valor de HTML.</span><span class="sxs-lookup"><span data-stu-id="ada6e-421">The `FileName` property should only be used for display purposes and only after HTML encoding the value.</span></span>
>
> <span data-ttu-id="ada6e-422">Los ejemplos proporcionados hasta ahora no tienen en cuenta las consideraciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="ada6e-422">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="ada6e-423">Se proporciona información adicional en las secciones siguientes y en la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="ada6e-423">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="ada6e-424">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="ada6e-424">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="ada6e-425">Validación</span><span class="sxs-lookup"><span data-stu-id="ada6e-425">Validation</span></span>](#validation)

<span data-ttu-id="ada6e-426">Al cargar archivos mediante el enlace de modelos y <xref:Microsoft.AspNetCore.Http.IFormFile>, el método de acción puede aceptar:</span><span class="sxs-lookup"><span data-stu-id="ada6e-426">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="ada6e-427">Un <xref:Microsoft.AspNetCore.Http.IFormFile> único.</span><span class="sxs-lookup"><span data-stu-id="ada6e-427">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="ada6e-428">Cualquiera de las colecciones siguientes que representan varios archivos:</span><span class="sxs-lookup"><span data-stu-id="ada6e-428">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="ada6e-429">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="ada6e-429">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="ada6e-430">El enlace coincide con los archivos de formulario por nombre.</span><span class="sxs-lookup"><span data-stu-id="ada6e-430">Binding matches form files by name.</span></span> <span data-ttu-id="ada6e-431">Por ejemplo, el valor `name` HTML en `<input type="file" name="formFile">` debe coincidir con la propiedad/el parámetro enlazado de C# (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="ada6e-431">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="ada6e-432">Para más información, consulte la sección [Coincidencia del valor de atributo de nombre con el nombre del parámetro del método POST](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="ada6e-432">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="ada6e-433">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ada6e-433">The following example:</span></span>

* <span data-ttu-id="ada6e-434">Recorre en bucle uno o más archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="ada6e-434">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="ada6e-435">Usa [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) para devolver una ruta de acceso completa de un archivo, incluido el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-435">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="ada6e-436">Guarda los archivos en el sistema de archivos local con un nombre de archivo generado por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-436">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="ada6e-437">Devuelve el número total y el tamaño de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="ada6e-437">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="ada6e-438">Use `Path.GetRandomFileName` para generar un nombre de archivo sin una ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="ada6e-438">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="ada6e-439">En el ejemplo siguiente, la ruta de acceso se obtiene de la configuración:</span><span class="sxs-lookup"><span data-stu-id="ada6e-439">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="ada6e-440">La ruta de acceso pasada al <xref:System.IO.FileStream> *debe* incluir el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-440">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="ada6e-441">Si no se proporciona el nombre de archivo, se produce una <xref:System.UnauthorizedAccessException> en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="ada6e-441">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="ada6e-442">Los archivos que se cargan usando la técnica <xref:Microsoft.AspNetCore.Http.IFormFile> se almacenan en búfer en memoria o en disco en el servidor web antes de procesarse.</span><span class="sxs-lookup"><span data-stu-id="ada6e-442">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="ada6e-443">Dentro del método de acción, se puede tener acceso al contenido de <xref:Microsoft.AspNetCore.Http.IFormFile> como <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="ada6e-443">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="ada6e-444">Además del sistema de archivos local, los archivos se pueden guardar en un recurso compartido de red o en un servicio de almacenamiento de archivos, como [Azure Blob Storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="ada6e-444">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="ada6e-445">Para ver otro ejemplo que recorre en bucle varios archivos para cargar y usa nombres de archivo seguros, consulte *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-445">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="ada6e-446">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) arroja una <xref:System.IO.IOException> si se crean más de 65 535 archivos sin eliminar los archivos temporales anteriores.</span><span class="sxs-lookup"><span data-stu-id="ada6e-446">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="ada6e-447">El límite de 65 535 archivos es un límite por servidor.</span><span class="sxs-lookup"><span data-stu-id="ada6e-447">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="ada6e-448">Para más información sobre este límite en el sistema operativo Windows, consulte las notas en los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="ada6e-448">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="ada6e-449">Función GetTempFileNameA</span><span class="sxs-lookup"><span data-stu-id="ada6e-449">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="ada6e-450">Carga de archivos pequeños con enlace de modelos almacenado en búfer a una base de datos</span><span class="sxs-lookup"><span data-stu-id="ada6e-450">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="ada6e-451">Para almacenar datos de archivo binario en una base de datos con [Entity Framework](/ef/core/index), defina una propiedad de matriz <xref:System.Byte> en la entidad:</span><span class="sxs-lookup"><span data-stu-id="ada6e-451">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="ada6e-452">Especifique una propiedad de modelo de página para la clase que incluya un <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="ada6e-452">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="ada6e-453"><xref:Microsoft.AspNetCore.Http.IFormFile> se puede usar directamente como un parámetro de método de acción o como una propiedad de modelo enlazado.</span><span class="sxs-lookup"><span data-stu-id="ada6e-453"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="ada6e-454">En el ejemplo anterior se utiliza una propiedad de modelo enlazado.</span><span class="sxs-lookup"><span data-stu-id="ada6e-454">The prior example uses a bound model property.</span></span>

<span data-ttu-id="ada6e-455">`FileUpload` se usa en el formulario de Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="ada6e-455">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="ada6e-456">Cuando el formulario se publique en el servidor, copie el <xref:Microsoft.AspNetCore.Http.IFormFile> en un flujo y guárdelo como matriz de bytes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-456">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="ada6e-457">En el ejemplo siguiente, `_dbContext` almacena el contexto de base de datos de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="ada6e-457">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="ada6e-458">El ejemplo anterior es similar a un escenario que se muestra en la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ada6e-458">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="ada6e-459">*Pages/BufferedSingleFileUploadDb.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ada6e-459">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="ada6e-460">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="ada6e-460">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="ada6e-461">Tenga cuidado al almacenar los datos binarios en bases de datos relacionales, ya que esto puede repercutir adversamente en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ada6e-461">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="ada6e-462">No se base ni confíe en la propiedad `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sin validarla.</span><span class="sxs-lookup"><span data-stu-id="ada6e-462">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="ada6e-463">La propiedad `FileName` solo debe usarse para fines de presentación y solo después de la codificación HTML.</span><span class="sxs-lookup"><span data-stu-id="ada6e-463">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="ada6e-464">Los ejemplos proporcionados no tienen en cuenta las consideraciones de seguridad.</span><span class="sxs-lookup"><span data-stu-id="ada6e-464">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="ada6e-465">Se proporciona información adicional en las secciones siguientes y en la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="ada6e-465">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="ada6e-466">Consideraciones de seguridad</span><span class="sxs-lookup"><span data-stu-id="ada6e-466">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="ada6e-467">Validación</span><span class="sxs-lookup"><span data-stu-id="ada6e-467">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="ada6e-468">Carga de archivos de gran tamaño con streaming</span><span class="sxs-lookup"><span data-stu-id="ada6e-468">Upload large files with streaming</span></span>

<span data-ttu-id="ada6e-469">En el ejemplo siguiente se muestra cómo usar JavaScript para transmitir un archivo a una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="ada6e-469">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="ada6e-470">El token de antifalsificación del archivo se genera por medio de un atributo de filtro personalizado y se pasa a los encabezados HTTP del cliente, en lugar de en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="ada6e-470">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="ada6e-471">Dado que el método de acción procesa los datos cargados directamente, el enlace de modelos del formulario se deshabilita por otro filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="ada6e-471">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="ada6e-472">Dentro de la acción, el contenido del formulario se lee usando un `MultipartReader` (que lee cada `MultipartSection` individual), de forma que el archivo se procesa o el contenido se almacena, según corresponda.</span><span class="sxs-lookup"><span data-stu-id="ada6e-472">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="ada6e-473">Una vez leídas las secciones de varias partes, la acción realiza su propio enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-473">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="ada6e-474">La respuesta de la página inicial carga el formulario y guarda un token de antifalsificación en una cookie (a través del atributo `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="ada6e-474">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="ada6e-475">El atributo usa la [compatibilidad de antifalsificación](xref:security/anti-request-forgery) integrada de ASP.NET Core para establecer una cookie con un token de solicitud:</span><span class="sxs-lookup"><span data-stu-id="ada6e-475">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="ada6e-476">El `DisableFormValueModelBindingAttribute` se usa para deshabilitar el enlace de modelos:</span><span class="sxs-lookup"><span data-stu-id="ada6e-476">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="ada6e-477">En la aplicación de ejemplo, `GenerateAntiforgeryTokenCookieAttribute` y `DisableFormValueModelBindingAttribute` se aplican como filtros a los modelos de aplicación de la página de `/StreamedSingleFileUploadDb` y `/StreamedSingleFileUploadPhysical` en `Startup.ConfigureServices` con las [convenciones de Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="ada6e-477">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="ada6e-478">Dado que el enlace de modelos no lee el formulario, los parámetros enlazados desde el formulario no se enlazan (la consulta, la ruta y el encabezado siguen funcionando).</span><span class="sxs-lookup"><span data-stu-id="ada6e-478">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="ada6e-479">El método de acción funciona directamente con la propiedad `Request`.</span><span class="sxs-lookup"><span data-stu-id="ada6e-479">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="ada6e-480">Se usa un elemento `MultipartReader` para leer cada sección.</span><span class="sxs-lookup"><span data-stu-id="ada6e-480">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="ada6e-481">Los datos de clave-valor se almacenan en un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="ada6e-481">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="ada6e-482">Una vez leídas las secciones de varias partes, el contenido del `KeyValueAccumulator` se usa para enlazar los datos del formulario a un tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-482">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="ada6e-483">El método `StreamingController.UploadDatabase` completo para streaming a una base de datos con EF Core:</span><span class="sxs-lookup"><span data-stu-id="ada6e-483">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="ada6e-484">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span><span class="sxs-lookup"><span data-stu-id="ada6e-484">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="ada6e-485">El método `StreamingController.UploadPhysical` completo para streaming a una ubicación física:</span><span class="sxs-lookup"><span data-stu-id="ada6e-485">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="ada6e-486">En la aplicación de ejemplo, las comprobaciones de validación las controla `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="ada6e-486">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="ada6e-487">Validación</span><span class="sxs-lookup"><span data-stu-id="ada6e-487">Validation</span></span>

<span data-ttu-id="ada6e-488">La clase `FileHelpers` de la aplicación de ejemplo muestra varias comprobaciones de cargas de archivos de streaming y <xref:Microsoft.AspNetCore.Http.IFormFile> almacenado en búfer.</span><span class="sxs-lookup"><span data-stu-id="ada6e-488">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="ada6e-489">Para procesar cargas de archivos almacenadas en búfer de <xref:Microsoft.AspNetCore.Http.IFormFile> en la aplicación de ejemplo, consulte el método `ProcessFormFile` en el archivo *Utilities/FileHelpers.cs*.</span><span class="sxs-lookup"><span data-stu-id="ada6e-489">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="ada6e-490">Para procesar archivos de streaming, consulte el método `ProcessStreamedFile` en el mismo archivo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-490">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="ada6e-491">Los métodos de procesamiento de validación mostrados en la aplicación de ejemplo no examinan el contenido de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="ada6e-491">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="ada6e-492">En la mayoría de los escenarios de producción, se usa una API de analizador de virus/malware en el archivo antes de que el archivo esté disponible para los usuarios u otros sistemas.</span><span class="sxs-lookup"><span data-stu-id="ada6e-492">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="ada6e-493">Aunque el ejemplo de tema proporciona un ejemplo funcional de técnicas de validación, no implemente la clase `FileHelpers` en una aplicación de producción a menos que:</span><span class="sxs-lookup"><span data-stu-id="ada6e-493">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="ada6e-494">Comprenda totalmente la implementación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-494">Fully understand the implementation.</span></span>
> * <span data-ttu-id="ada6e-495">Modifique la implementación según corresponda en función del entorno y las especificaciones de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-495">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="ada6e-496">**No implemente nunca de manera indiscriminada el código de seguridad en una aplicación sin abordar estos requisitos.**</span><span class="sxs-lookup"><span data-stu-id="ada6e-496">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="ada6e-497">Validación del contenido</span><span class="sxs-lookup"><span data-stu-id="ada6e-497">Content validation</span></span>

<span data-ttu-id="ada6e-498">**Use una API de detección de virus/malware de terceros en el contenido cargado.**</span><span class="sxs-lookup"><span data-stu-id="ada6e-498">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="ada6e-499">El análisis de archivos exige recursos del servidor en escenarios de gran volumen.</span><span class="sxs-lookup"><span data-stu-id="ada6e-499">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="ada6e-500">Si disminuye el rendimiento de procesamiento de solicitudes debido al análisis de archivos, considere la posibilidad de descargar el trabajo de análisis a un [servicio en segundo plano](xref:fundamentals/host/hosted-services), posiblemente un servicio que se ejecute en otro servidor desde el servidor de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-500">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="ada6e-501">Habitualmente, los archivos cargados se almacenan en un área en cuarentena hasta que el programa de detección de virus en segundo plano los revisa.</span><span class="sxs-lookup"><span data-stu-id="ada6e-501">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="ada6e-502">Cuando se pasa un archivo, se mueve a la ubicación de almacenamiento de archivos habitual.</span><span class="sxs-lookup"><span data-stu-id="ada6e-502">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="ada6e-503">Por lo general, estos pasos se realizan junto con un registro de base de datos que indica el estado de análisis de un archivo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-503">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="ada6e-504">Mediante el uso de este enfoque, la aplicación y el servidor de aplicaciones permanecen centrados en responder a las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="ada6e-504">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="ada6e-505">Validación de la extensión del archivo</span><span class="sxs-lookup"><span data-stu-id="ada6e-505">File extension validation</span></span>

<span data-ttu-id="ada6e-506">La extensión del archivo cargado debe comprobarse con una lista de extensiones permitidas.</span><span class="sxs-lookup"><span data-stu-id="ada6e-506">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="ada6e-507">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ada6e-507">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="ada6e-508">Validación de firma del archivo</span><span class="sxs-lookup"><span data-stu-id="ada6e-508">File signature validation</span></span>

<span data-ttu-id="ada6e-509">La firma de un archivo viene determinada por los primeros bytes al principio de un archivo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-509">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="ada6e-510">Estos bytes se pueden usar para indicar si la extensión coincide con el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="ada6e-510">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="ada6e-511">La aplicación de ejemplo comprueba las firmas de archivo de algunos tipos de archivo comunes.</span><span class="sxs-lookup"><span data-stu-id="ada6e-511">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="ada6e-512">En el ejemplo siguiente, la firma de archivo de una imagen JPEG se comprueba con respecto al archivo:</span><span class="sxs-lookup"><span data-stu-id="ada6e-512">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="ada6e-513">Para obtener firmas de archivo adicionales, consulte la [base de datos de firmas de archivo](https://www.filesignatures.net/) y las especificaciones de archivo oficiales.</span><span class="sxs-lookup"><span data-stu-id="ada6e-513">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="ada6e-514">Seguridad de nombre de archivo</span><span class="sxs-lookup"><span data-stu-id="ada6e-514">File name security</span></span>

<span data-ttu-id="ada6e-515">Nunca use un nombre de archivo proporcionado por el cliente para guardar un archivo en el almacenamiento físico.</span><span class="sxs-lookup"><span data-stu-id="ada6e-515">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="ada6e-516">Cree un nombre de archivo seguro para el archivo con [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) o [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) para crear una ruta de acceso completa (incluido el nombre de archivo) para el almacenamiento temporal.</span><span class="sxs-lookup"><span data-stu-id="ada6e-516">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="ada6e-517">Razor codifica automáticamente en HTML los valores de propiedad para mostrarlos.</span><span class="sxs-lookup"><span data-stu-id="ada6e-517">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="ada6e-518">El código siguiente es seguro de usar:</span><span class="sxs-lookup"><span data-stu-id="ada6e-518">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="ada6e-519">Fuera de Razor, siempre use <xref:System.Net.WebUtility.HtmlEncode*> al contenido del nombre de archivo de una solicitud de un usuario.</span><span class="sxs-lookup"><span data-stu-id="ada6e-519">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="ada6e-520">Muchas implementaciones deben incluir una comprobación de que el archivo existe; de lo contrario, el archivo se sobrescribe por un archivo con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="ada6e-520">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="ada6e-521">Proporcione lógica adicional para satisfacer las especificaciones de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ada6e-521">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="ada6e-522">Validación del tamaño</span><span class="sxs-lookup"><span data-stu-id="ada6e-522">Size validation</span></span>

<span data-ttu-id="ada6e-523">Limite el tamaño de los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="ada6e-523">Limit the size of uploaded files.</span></span>

<span data-ttu-id="ada6e-524">En la aplicación de ejemplo, el tamaño del archivo está limitado a 2 MB (se indica en bytes).</span><span class="sxs-lookup"><span data-stu-id="ada6e-524">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="ada6e-525">El límite se proporciona a través de [Configuración](xref:fundamentals/configuration/index) del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="ada6e-525">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="ada6e-526">`FileSizeLimit` se inserta en las clases `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-526">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="ada6e-527">Cuando el tamaño de un archivo supera el límite, se rechaza el archivo:</span><span class="sxs-lookup"><span data-stu-id="ada6e-527">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="ada6e-528">Coincidencia del valor de atributo de nombre con el nombre del parámetro del método POST</span><span class="sxs-lookup"><span data-stu-id="ada6e-528">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="ada6e-529">En los formularios que no son de Razor que realizan la operación POST en los datos de formulario o usan directamente `FormData` de JavaScript, el nombre especificado en el elemento del formulario o `FormData` debe coincidir con el nombre del parámetro en la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="ada6e-529">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="ada6e-530">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ada6e-530">In the following example:</span></span>

* <span data-ttu-id="ada6e-531">Cuando se usa un elemento `<input>`, el atributo `name` se establece en el valor `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-531">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="ada6e-532">Cuando se usa `FormData` en JavaScript, el nombre se establece en el valor `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-532">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="ada6e-533">Use un nombre coincidente para el parámetro del método de C# (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="ada6e-533">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="ada6e-534">Para un método de control de páginas de Razor Pages denominado `Upload`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-534">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="ada6e-535">Para un método de acción de controlador POST de MVC:</span><span class="sxs-lookup"><span data-stu-id="ada6e-535">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="ada6e-536">Configuración del servidor y de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ada6e-536">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="ada6e-537">Límite de longitud del cuerpo de varias partes</span><span class="sxs-lookup"><span data-stu-id="ada6e-537">Multipart body length limit</span></span>

<span data-ttu-id="ada6e-538"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> establece el límite de la longitud de cada cuerpo de varias partes.</span><span class="sxs-lookup"><span data-stu-id="ada6e-538"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="ada6e-539">Las secciones del formulario que superan este límite inician una <xref:System.IO.InvalidDataException> cuando se analizan.</span><span class="sxs-lookup"><span data-stu-id="ada6e-539">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="ada6e-540">El valor predeterminado es 134 217 728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="ada6e-540">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="ada6e-541">Personalice el límite mediante el valor <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-541">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="ada6e-542"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> se utiliza para establecer el <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> para una sola página o acción.</span><span class="sxs-lookup"><span data-stu-id="ada6e-542"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="ada6e-543">En una aplicación de Razor Pages, aplique el filtro con una [convención](xref:razor-pages/razor-pages-conventions) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-543">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="ada6e-544">En una aplicación de Razor Pages o una aplicación MVC, aplique el filtro al método de acción o al modelo de página:</span><span class="sxs-lookup"><span data-stu-id="ada6e-544">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="ada6e-545">Tamaño máximo del cuerpo de la solicitud de Kestrel</span><span class="sxs-lookup"><span data-stu-id="ada6e-545">Kestrel maximum request body size</span></span>

<span data-ttu-id="ada6e-546">En el caso de las aplicaciones hospedadas por Kestrel, el tamaño máximo predeterminado del cuerpo de solicitud es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="ada6e-546">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="ada6e-547">Personalice el límite con la opción [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) del servidor de Kestrel:</span><span class="sxs-lookup"><span data-stu-id="ada6e-547">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="ada6e-548"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> se usa para establecer el valor de [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) de una sola página o acción.</span><span class="sxs-lookup"><span data-stu-id="ada6e-548"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="ada6e-549">En una aplicación de Razor Pages, aplique el filtro con una [convención](xref:razor-pages/razor-pages-conventions) en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ada6e-549">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="ada6e-550">En una aplicación de Razor Pages o una aplicación MVC, aplique el filtro al método de acción o a la clase de control de página:</span><span class="sxs-lookup"><span data-stu-id="ada6e-550">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="ada6e-551">Otros límites de Kestrel</span><span class="sxs-lookup"><span data-stu-id="ada6e-551">Other Kestrel limits</span></span>

<span data-ttu-id="ada6e-552">Otros límites de Kestrel pueden aplicarse a las aplicaciones hospedadas por Kestrel:</span><span class="sxs-lookup"><span data-stu-id="ada6e-552">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="ada6e-553">Conexiones de cliente máximas</span><span class="sxs-lookup"><span data-stu-id="ada6e-553">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="ada6e-554">Tarifas de datos de solicitud y respuesta</span><span class="sxs-lookup"><span data-stu-id="ada6e-554">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="ada6e-555">Límite de longitud del contenido de IIS</span><span class="sxs-lookup"><span data-stu-id="ada6e-555">IIS content length limit</span></span>

<span data-ttu-id="ada6e-556">El límite predeterminado de solicitudes (`maxAllowedContentLength`) es 30 000 000 bytes, que son aproximadamente 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="ada6e-556">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="ada6e-557">Personalice el límite en el archivo *web.config*:</span><span class="sxs-lookup"><span data-stu-id="ada6e-557">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="ada6e-558">Esto solo ocurre en IIS;</span><span class="sxs-lookup"><span data-stu-id="ada6e-558">This setting only applies to IIS.</span></span> <span data-ttu-id="ada6e-559">este comportamiento no sucede de forma predeterminada cuando los archivos se hospedan en Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ada6e-559">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="ada6e-560">Para más información, consulte [Límites de solicitud \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="ada6e-560">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="ada6e-561">Las limitaciones del módulo ASP.NET Core o la presencia del módulo de filtrado de solicitudes de IIS pueden limitar las cargas a 2 GB o 4 GB.</span><span class="sxs-lookup"><span data-stu-id="ada6e-561">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="ada6e-562">Para más información, consulte el artículo que indica que [no se pueden cargar archivos con un tamaño superior a 2 GB (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="ada6e-562">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="ada6e-563">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="ada6e-563">Troubleshoot</span></span>

<span data-ttu-id="ada6e-564">Aquí incluimos algunos problemas comunes que pueden surgir al cargar archivos, así como sus posibles soluciones.</span><span class="sxs-lookup"><span data-stu-id="ada6e-564">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="ada6e-565">Error No encontrado al implementar en un servidor IIS</span><span class="sxs-lookup"><span data-stu-id="ada6e-565">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="ada6e-566">El error siguiente indica que el archivo cargado supera la longitud configurada del contenido del servidor:</span><span class="sxs-lookup"><span data-stu-id="ada6e-566">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="ada6e-567">Para más información sobre cómo aumentar el límite, consulte la sección [Límite de longitud del contenido de IIS](#iis-content-length-limit).</span><span class="sxs-lookup"><span data-stu-id="ada6e-567">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="ada6e-568">Error de conexión</span><span class="sxs-lookup"><span data-stu-id="ada6e-568">Connection failure</span></span>

<span data-ttu-id="ada6e-569">Un error de conexión y una conexión del servidor de restablecimiento probablemente indica que el archivo cargado supera el tamaño máximo del cuerpo de la solicitud de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ada6e-569">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="ada6e-570">Para más información, consulte la sección [Tamaño máximo del cuerpo de la solicitud de Kestrel](#kestrel-maximum-request-body-size).</span><span class="sxs-lookup"><span data-stu-id="ada6e-570">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="ada6e-571">Los límites de conexión del cliente de Kestrel también pueden requerir ajustes.</span><span class="sxs-lookup"><span data-stu-id="ada6e-571">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="ada6e-572">Excepción de referencia nula con IFormFile</span><span class="sxs-lookup"><span data-stu-id="ada6e-572">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="ada6e-573">Si el controlador acepta archivos cargados con <xref:Microsoft.AspNetCore.Http.IFormFile>, pero el valor es `null`, confirme que el formulario HTML especifica un valor `enctype` de `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="ada6e-573">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="ada6e-574">Si este atributo no está establecido en el elemento `<form>`, no se llevará a cabo la carga del archivo y cualquier argumento <xref:Microsoft.AspNetCore.Http.IFormFile> enlazado será `null`.</span><span class="sxs-lookup"><span data-stu-id="ada6e-574">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="ada6e-575">Confirme también que la [nomenclatura de la carga en los datos de formulario coincide con la nomenclatura de la aplicación](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="ada6e-575">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="ada6e-576">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ada6e-576">Additional resources</span></span>

* <span data-ttu-id="ada6e-577">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Carga de archivos sin restricciones)</span><span class="sxs-lookup"><span data-stu-id="ada6e-577">[Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload)</span></span>
* [<span data-ttu-id="ada6e-578">Azure Security: Marco de seguridad: Validación de entrada | Mitigaciones</span><span class="sxs-lookup"><span data-stu-id="ada6e-578">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="ada6e-579">Patrones de diseño en la nube de Azure: Patrón Valet Key</span><span class="sxs-lookup"><span data-stu-id="ada6e-579">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
