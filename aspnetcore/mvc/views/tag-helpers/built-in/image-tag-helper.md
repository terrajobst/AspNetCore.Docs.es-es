---
title: "Aplicación auxiliar de etiquetas de imagen en ASP.NET Core"
author: pkellner
description: "Muestra cómo trabajar con la aplicación auxiliar de etiquetas de imagen"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 75bddd01a95f3ae0b1ea19de0eb64ad3b9066319
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="imagetaghelper"></a><span data-ttu-id="65a77-103">Aplicación auxiliar de etiquetas de imagen</span><span class="sxs-lookup"><span data-stu-id="65a77-103">ImageTagHelper</span></span>

<span data-ttu-id="65a77-104">Por [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="65a77-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="65a77-105">La aplicación auxiliar de etiquetas de imagen es una mejora de la etiqueta `img` (`<img>`).</span><span class="sxs-lookup"><span data-stu-id="65a77-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="65a77-106">Requiere una etiqueta `src`, así como el atributo `boolean` `asp-append-version`.</span><span class="sxs-lookup"><span data-stu-id="65a77-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="65a77-107">Si el origen de la imagen (`src`) es un archivo estático en el servidor web del host, se anexa una cadena única de bloqueo de la caché como parámetro de consulta a ese origen de la imagen.</span><span class="sxs-lookup"><span data-stu-id="65a77-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="65a77-108">De este modo, si el archivo en el servidor web del host cambia, se generará una dirección URL de solicitud única que incluye el parámetro de solicitud actualizada.</span><span class="sxs-lookup"><span data-stu-id="65a77-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="65a77-109">La cadena de limpieza de memoria caché es un valor único que representa el valor hash del archivo de imagen estático.</span><span class="sxs-lookup"><span data-stu-id="65a77-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="65a77-110">Si el origen de la imagen (`src`) no es un archivo estático (por ejemplo, es una dirección URL remota o se trata de un archivo que no existe en el servidor) el atributo `src` de la etiqueta `<img>` se genera sin parámetro de cadena de consulta de limpieza de caché.</span><span class="sxs-lookup"><span data-stu-id="65a77-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="65a77-111">Atributos de la aplicación auxiliar de etiquetas de imagen</span><span class="sxs-lookup"><span data-stu-id="65a77-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="65a77-112">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="65a77-112">asp-append-version</span></span>

<span data-ttu-id="65a77-113">Cuando se especifica junto con un atributo `src`, se invoca la aplicación auxiliar de etiquetas de imagen.</span><span class="sxs-lookup"><span data-stu-id="65a77-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="65a77-114">Este es un ejemplo de aplicación auxiliar de etiquetas `img` válido:</span><span class="sxs-lookup"><span data-stu-id="65a77-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="65a77-115">Si el archivo estático existe en el directorio *..wwwroot/images/asplogo.png*, el código HTML generado es similar al siguiente (el valor hash será diferente):</span><span class="sxs-lookup"><span data-stu-id="65a77-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="65a77-116">El valor asignado al parámetro `v` es el valor hash del archivo almacenado en disco.</span><span class="sxs-lookup"><span data-stu-id="65a77-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="65a77-117">Si el servidor web no es capaz de obtener acceso de lectura al archivo estático al que se hace referencia, no se agregará ningún parámetro `v` al atributo `src`.</span><span class="sxs-lookup"><span data-stu-id="65a77-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="65a77-118">src</span><span class="sxs-lookup"><span data-stu-id="65a77-118">src</span></span>

<span data-ttu-id="65a77-119">Para activar la aplicación auxiliar de etiquetas de imagen, se requiere el atributo src en el elemento `<img>`.</span><span class="sxs-lookup"><span data-stu-id="65a77-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="65a77-120">La aplicación auxiliar de etiquetas de imagen usa el proveedor `Cache` en el servidor web local para almacenar el valor de `Sha512` calculado de un archivo determinado.</span><span class="sxs-lookup"><span data-stu-id="65a77-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="65a77-121">Si el archivo se vuelve a solicitar, no es necesario volver a calcular `Sha512`.</span><span class="sxs-lookup"><span data-stu-id="65a77-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="65a77-122">La memoria caché queda invalidada por un monitor del archivo que se adjunta al archivo cuando el valor de `Sha512` del archivo se calcula.</span><span class="sxs-lookup"><span data-stu-id="65a77-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="65a77-123">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="65a77-123">Additional resources</span></span>

* <xref:performance/caching/memory>
