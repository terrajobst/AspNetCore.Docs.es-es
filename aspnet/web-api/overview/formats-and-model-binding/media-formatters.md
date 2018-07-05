---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formateadores de contenido multimedia en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 824fea5a8837ff8b09af832b7d2a094c7d82907b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368921"
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="bce6c-102">Formateadores de contenido multimedia en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="bce6c-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="bce6c-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bce6c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bce6c-104">Este tutorial muestra cómo admitir formatos multimedia adicionales en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="bce6c-104">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="bce6c-105">Tipos de medios de Internet</span><span class="sxs-lookup"><span data-stu-id="bce6c-105">Internet Media Types</span></span>

<span data-ttu-id="bce6c-106">Un tipo de medio, también denominado tipo MIME, identifica el formato de un elemento de datos.</span><span class="sxs-lookup"><span data-stu-id="bce6c-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="bce6c-107">Tipos de medios en HTTP, describen el formato del cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="bce6c-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="bce6c-108">Un tipo de medio consta de dos cadenas, un tipo y un subtipo.</span><span class="sxs-lookup"><span data-stu-id="bce6c-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="bce6c-109">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bce6c-109">For example:</span></span>

- <span data-ttu-id="bce6c-110">texto/html</span><span class="sxs-lookup"><span data-stu-id="bce6c-110">text/html</span></span>
- <span data-ttu-id="bce6c-111">image/png</span><span class="sxs-lookup"><span data-stu-id="bce6c-111">image/png</span></span>
- <span data-ttu-id="bce6c-112">application/json</span><span class="sxs-lookup"><span data-stu-id="bce6c-112">application/json</span></span>

<span data-ttu-id="bce6c-113">Cuando un mensaje HTTP contiene un cuerpo de entidad, el encabezado Content-Type especifica el formato del cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="bce6c-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="bce6c-114">Esto indica que el receptor a cómo analizar el contenido del cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="bce6c-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="bce6c-115">Por ejemplo, si una respuesta HTTP contiene una imagen PNG, la respuesta podría tener los siguientes encabezados.</span><span class="sxs-lookup"><span data-stu-id="bce6c-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="bce6c-116">Cuando el cliente envía un mensaje de solicitud, puede incluir un encabezado Accept.</span><span class="sxs-lookup"><span data-stu-id="bce6c-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="bce6c-117">El encabezado Accept indica que el servidor de tipos de medios que el cliente desea desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="bce6c-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="bce6c-118">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bce6c-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="bce6c-119">Este encabezado indica al servidor que desea que el cliente HTML, XHTML o XML.</span><span class="sxs-lookup"><span data-stu-id="bce6c-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="bce6c-120">El tipo de medio determina cómo la API Web serializa y deserializa el cuerpo del mensaje HTTP.</span><span class="sxs-lookup"><span data-stu-id="bce6c-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="bce6c-121">API Web tiene compatibilidad integrada para XML, JSON, BSON y datos de formato form-urlencoded y puede admitir tipos de medios adicionales escribiendo un *formateador media*.</span><span class="sxs-lookup"><span data-stu-id="bce6c-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="bce6c-122">Para crear a un formateador de medios, que se derivan de una de estas clases:</span><span class="sxs-lookup"><span data-stu-id="bce6c-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="bce6c-123">[Elemento MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="bce6c-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="bce6c-124">Esta lectura asincrónica de usos de clases y métodos de escritura.</span><span class="sxs-lookup"><span data-stu-id="bce6c-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="bce6c-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="bce6c-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="bce6c-126">Esta clase se deriva de **elemento MediaTypeFormatter** pero utiliza los métodos de lectura/escritura sychronous.</span><span class="sxs-lookup"><span data-stu-id="bce6c-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="bce6c-127">Derivar de **BufferedMediaTypeFormatter** es más sencillo, porque no hay ningún código asincrónico, pero también significa que puede bloquear el subproceso de llamada durante la E/S.</span><span class="sxs-lookup"><span data-stu-id="bce6c-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="bce6c-128">Ejemplo: Creación de un formateador de medios CSV</span><span class="sxs-lookup"><span data-stu-id="bce6c-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="bce6c-129">El ejemplo siguiente muestra un formateador de tipo de medios que puede serializar un objeto de producto a un formato de valores separados por comas (CSV).</span><span class="sxs-lookup"><span data-stu-id="bce6c-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="bce6c-130">Este ejemplo utiliza el tipo de producto definido en el tutorial [crear una API Web que admite operaciones de CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="bce6c-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="bce6c-131">Esta es la definición del objeto de producto:</span><span class="sxs-lookup"><span data-stu-id="bce6c-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="bce6c-132">Para implementar un formateador CSV, defina una clase que deriva de **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="bce6c-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="bce6c-133">En el constructor, agregue los tipos de medios que admite el formateador.</span><span class="sxs-lookup"><span data-stu-id="bce6c-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="bce6c-134">En este ejemplo, el formateador admite un único tipo de medio, &quot;texto o csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="bce6c-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="bce6c-135">Invalidar el **CanWriteType** método para indicar que escribe el formateador puede serializar:</span><span class="sxs-lookup"><span data-stu-id="bce6c-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="bce6c-136">En este ejemplo, el formateador puede serializar solo `Product` objetos, así como las colecciones de `Product` objetos.</span><span class="sxs-lookup"><span data-stu-id="bce6c-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="bce6c-137">De forma similar, reemplace el **CanReadType** método para indicar que escribe el formateador puede deserializar.</span><span class="sxs-lookup"><span data-stu-id="bce6c-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="bce6c-138">En este ejemplo, el formateador no admite la deserialización, por lo que simplemente devuelve el método **false**.</span><span class="sxs-lookup"><span data-stu-id="bce6c-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="bce6c-139">Por último, reemplace el **WriteToStream** método.</span><span class="sxs-lookup"><span data-stu-id="bce6c-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="bce6c-140">Este método serializa un tipo escribiendo en una secuencia.</span><span class="sxs-lookup"><span data-stu-id="bce6c-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="bce6c-141">Si el formateador admite la deserialización, también invalidar el **ReadFromStream** método.</span><span class="sxs-lookup"><span data-stu-id="bce6c-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="bce6c-142">Agregar a un formateador de medios a la canalización de API Web</span><span class="sxs-lookup"><span data-stu-id="bce6c-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="bce6c-143">Para agregar un tipo de medio formateador a la canalización de Web API, use el **formateadores** propiedad en el **HttpConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="bce6c-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="bce6c-144">Codificaciones de caracteres</span><span class="sxs-lookup"><span data-stu-id="bce6c-144">Character Encodings</span></span>

<span data-ttu-id="bce6c-145">Opcionalmente, un formateador de medios puede admitir varias codificaciones de caracteres, como UTF-8 o ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="bce6c-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="bce6c-146">En el constructor, agregue uno o varios [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) tipos a la **SupportedEncodings** colección.</span><span class="sxs-lookup"><span data-stu-id="bce6c-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="bce6c-147">Coloque el valor predeterminado de codificación primero.</span><span class="sxs-lookup"><span data-stu-id="bce6c-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="bce6c-148">En el **WriteToStream** y **ReadFromStream** llamar métodos, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) para seleccionar la codificación de caracteres preferida.</span><span class="sxs-lookup"><span data-stu-id="bce6c-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="bce6c-149">Este método coincide con los encabezados de solicitud con la lista de codificaciones compatibles.</span><span class="sxs-lookup"><span data-stu-id="bce6c-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="bce6c-150">Utilice el valor devuelto **Encoding** al leer o escribir desde la secuencia:</span><span class="sxs-lookup"><span data-stu-id="bce6c-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
