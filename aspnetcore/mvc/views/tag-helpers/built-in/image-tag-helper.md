---
title: "Aplicación auxiliar de la etiqueta de imagen | Documentos de Microsoft"
author: pkellner
description: "Muestra cómo trabajar con la aplicación auxiliar de etiqueta de imagen"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: d0857e1926c341b2357bc824fa379c4fc30affbc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="imagetaghelper"></a><span data-ttu-id="3e61e-103">ImageTagHelper</span><span class="sxs-lookup"><span data-stu-id="3e61e-103">ImageTagHelper</span></span>

<span data-ttu-id="3e61e-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="3e61e-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="3e61e-105">Mejora de la aplicación auxiliar de etiqueta de imagen la `img` (`<img>`) etiqueta.</span><span class="sxs-lookup"><span data-stu-id="3e61e-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="3e61e-106">Requiere un `src` etiqueta, así como la `boolean` atributo `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="3e61e-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="3e61e-107">Si el origen de imagen (`src`) es un archivo estático en el servidor web de host, se anexa una memoria caché única la desactivación de cadena como parámetro de consulta para el origen de la imagen.</span><span class="sxs-lookup"><span data-stu-id="3e61e-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="3e61e-108">Esto garantiza que si cambia el archivo en el servidor web de host, una dirección URL de solicitud único se genera que incluya el parámetro de solicitud actualizada.</span><span class="sxs-lookup"><span data-stu-id="3e61e-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="3e61e-109">La memoria caché de la desactivación de cadena es un valor único que representa el valor hash del archivo de imagen estática.</span><span class="sxs-lookup"><span data-stu-id="3e61e-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="3e61e-110">Si el origen de imagen (`src`) no es un archivo estático (por ejemplo una dirección URL remota o en el archivo no existe en el servidor), el `<img>` etiqueta `src` atributo se genera sin caché la desactivación de parámetro de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="3e61e-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="3e61e-111">Atributos de aplicación auxiliar de etiqueta de imagen</span><span class="sxs-lookup"><span data-stu-id="3e61e-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="3e61e-112">versión anexar ASP</span><span class="sxs-lookup"><span data-stu-id="3e61e-112">asp-append-version</span></span>

<span data-ttu-id="3e61e-113">Cuando se especifica junto con un `src` se invoca el atributo, la aplicación auxiliar de etiqueta de imagen.</span><span class="sxs-lookup"><span data-stu-id="3e61e-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="3e61e-114">Un ejemplo de válido `img` aplicación auxiliar para etiqueta es:</span><span class="sxs-lookup"><span data-stu-id="3e61e-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="3e61e-115">Si existe el archivo estático en el directorio *... Wwwroot/images/asplogo.png* el código html generado es similar al siguiente (el valor hash será diferente):</span><span class="sxs-lookup"><span data-stu-id="3e61e-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="3e61e-116">El valor asignado al parámetro `v` es el valor hash del archivo en disco.</span><span class="sxs-lookup"><span data-stu-id="3e61e-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="3e61e-117">Si el servidor web es no se puede obtener acceso de lectura para el archivo estático al que hace referencia, no `v` parámetros se agregan a la `src` atributo.</span><span class="sxs-lookup"><span data-stu-id="3e61e-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="3e61e-118">src</span><span class="sxs-lookup"><span data-stu-id="3e61e-118">src</span></span>

<span data-ttu-id="3e61e-119">Para activar la aplicación auxiliar de etiqueta de imagen, se requiere el atributo src en la `<img>` elemento.</span><span class="sxs-lookup"><span data-stu-id="3e61e-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="3e61e-120">La aplicación auxiliar de etiqueta de imagen utiliza el `Cache` proveedor en el servidor web local para almacenar los calculados `Sha512` de un archivo determinado.</span><span class="sxs-lookup"><span data-stu-id="3e61e-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="3e61e-121">Si el archivo se vuelve a solicitar la `Sha512` no necesita volver a calcular.</span><span class="sxs-lookup"><span data-stu-id="3e61e-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="3e61e-122">La memoria caché se invalida por un monitor del archivo que se adjunta al archivo cuando el archivo `Sha512` se calcula.</span><span class="sxs-lookup"><span data-stu-id="3e61e-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3e61e-123">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3e61e-123">Additional resources</span></span>

* <xref:performance/caching/memory>
