---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: "Negociación en ASP.NET Web API de contenido | Documentos de Microsoft"
author: MikeWasson
description: "Describe cómo ASP.NET Web API implementa negociación de contenido HTTP."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="055eb-103">Negociación de contenido en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="055eb-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="055eb-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="055eb-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="055eb-105">Este artículo describe cómo ASP.NET Web API implementa negociación de contenido.</span><span class="sxs-lookup"><span data-stu-id="055eb-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="055eb-106">La especificación de HTTP (RFC 2616) define la negociación de contenido como "el proceso de selección de la mejor representación una respuesta determinada cuando existen varias representaciones".</span><span class="sxs-lookup"><span data-stu-id="055eb-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="055eb-107">El mecanismo principal para la negociación de contenido en HTTP son estos encabezados de solicitud:</span><span class="sxs-lookup"><span data-stu-id="055eb-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="055eb-108">**Aceptar:** qué tipos de medios son aceptables para la respuesta, como "application/json", "application/xml" o un tipo de medio personalizado como &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="055eb-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="055eb-109">**Accept-Charset:** los juegos de caracteres son aceptables, como UTF-8 o ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="055eb-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="055eb-110">**Codificación aceptada:** las codificaciones de contenido son aceptables, por ejemplo, gzip.</span><span class="sxs-lookup"><span data-stu-id="055eb-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="055eb-111">**Accept-Language:** el idioma preferido de natural, como "en-us".</span><span class="sxs-lookup"><span data-stu-id="055eb-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="055eb-112">El servidor también puede buscar en otras partes de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="055eb-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="055eb-113">Por ejemplo, si la solicitud contiene un encabezado X-Requested-With, que indica una solicitud AJAX, el servidor podría predeterminado para JSON si no hay ningún encabezado Accept.</span><span class="sxs-lookup"><span data-stu-id="055eb-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="055eb-114">En este artículo, analizaremos cómo API Web utiliza los encabezados de aceptación y Accept-Charset.</span><span class="sxs-lookup"><span data-stu-id="055eb-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="055eb-115">(En este momento, no hay ninguna compatibilidad integrada para Accept-Encoding o Accept-Language.)</span><span class="sxs-lookup"><span data-stu-id="055eb-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="055eb-116">Serialización</span><span class="sxs-lookup"><span data-stu-id="055eb-116">Serialization</span></span>

<span data-ttu-id="055eb-117">Si un controlador de Web API devuelve un recurso como tipo de CLR, la canalización, serializa el valor devuelto y lo escribe en el cuerpo de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="055eb-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="055eb-118">Por ejemplo, considere la siguiente acción del controlador:</span><span class="sxs-lookup"><span data-stu-id="055eb-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="055eb-119">Un cliente puede enviar esta solicitud HTTP:</span><span class="sxs-lookup"><span data-stu-id="055eb-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="055eb-120">En respuesta, el servidor puede enviar:</span><span class="sxs-lookup"><span data-stu-id="055eb-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="055eb-121">En este ejemplo, el cliente solicitó JSON, Javascript o "cualquier" (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="055eb-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="055eb-122">El servidor respondió con una representación JSON de la `Product` objeto.</span><span class="sxs-lookup"><span data-stu-id="055eb-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="055eb-123">Tenga en cuenta que el encabezado Content-Type de la respuesta se establece en &quot;application/json&quot;.</span><span class="sxs-lookup"><span data-stu-id="055eb-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="055eb-124">También puede devolver un controlador de un **HttpResponseMessage** objeto.</span><span class="sxs-lookup"><span data-stu-id="055eb-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="055eb-125">Para especificar un objeto CLR para el cuerpo de respuesta, llame a la **CreateResponse** método de extensión:</span><span class="sxs-lookup"><span data-stu-id="055eb-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="055eb-126">Esta opción proporciona mayor control sobre los detalles de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="055eb-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="055eb-127">Puede establecer el código de estado, agregar encabezados HTTP y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="055eb-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="055eb-128">El objeto que serializa el recurso se denomina un *formateador del contenido multimedia*.</span><span class="sxs-lookup"><span data-stu-id="055eb-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="055eb-129">Formateadores de contenido multimedia que se derivan de la **elemento MediaTypeFormatter** clase.</span><span class="sxs-lookup"><span data-stu-id="055eb-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="055eb-130">API Web proporciona a formateadores de contenido multimedia para XML y JSON, y puede crear formateadores personalizados para admitir otros tipos de medios.</span><span class="sxs-lookup"><span data-stu-id="055eb-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="055eb-131">Para obtener información sobre cómo escribir un formateador personalizado, consulte [formateadores de contenido multimedia](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="055eb-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="055eb-132">Administración de contenido funciona de negociación</span><span class="sxs-lookup"><span data-stu-id="055eb-132">How Content Negotiation Works</span></span>

<span data-ttu-id="055eb-133">En primer lugar, la canalización Obtiene la **IContentNegotiator** desde el **HttpConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="055eb-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="055eb-134">También obtiene la lista de formateadores de contenido multimedia desde el **HttpConfiguration.Formatters** colección.</span><span class="sxs-lookup"><span data-stu-id="055eb-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="055eb-135">A continuación, llama a la canalización **IContentNegotiatior.Negotiate**, pasando:</span><span class="sxs-lookup"><span data-stu-id="055eb-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="055eb-136">El tipo de objeto que se va a serializar</span><span class="sxs-lookup"><span data-stu-id="055eb-136">The type of object to serialize</span></span>
- <span data-ttu-id="055eb-137">La colección de formateadores de contenido multimedia</span><span class="sxs-lookup"><span data-stu-id="055eb-137">The collection of media formatters</span></span>
- <span data-ttu-id="055eb-138">La solicitud HTTP</span><span class="sxs-lookup"><span data-stu-id="055eb-138">The HTTP request</span></span>

<span data-ttu-id="055eb-139">El **Negotiate** método devuelve dos piezas de información:</span><span class="sxs-lookup"><span data-stu-id="055eb-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="055eb-140">Formateador que va a utilizar</span><span class="sxs-lookup"><span data-stu-id="055eb-140">Which formatter to use</span></span>
- <span data-ttu-id="055eb-141">El tipo de medio de la respuesta</span><span class="sxs-lookup"><span data-stu-id="055eb-141">The media type for the response</span></span>

<span data-ttu-id="055eb-142">Si no se encuentra ningún formateador, la **Negotiate** método **null**y el error de cliente recibe HTTP 406 (no aceptable).</span><span class="sxs-lookup"><span data-stu-id="055eb-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="055eb-143">El código siguiente muestra cómo un controlador puede invocar directamente negociación de contenido:</span><span class="sxs-lookup"><span data-stu-id="055eb-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="055eb-144">Este código es equivalente a lo que el que la canalización realiza de forma automática.</span><span class="sxs-lookup"><span data-stu-id="055eb-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="055eb-145">Negociador de contenido predeterminado</span><span class="sxs-lookup"><span data-stu-id="055eb-145">Default Content Negotiator</span></span>

<span data-ttu-id="055eb-146">El **DefaultContentNegotiator** clase proporciona la implementación predeterminada de **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="055eb-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="055eb-147">Utiliza varios criterios para seleccionar a un formateador.</span><span class="sxs-lookup"><span data-stu-id="055eb-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="055eb-148">En primer lugar, el formateador debe ser capaz de serializar el tipo.</span><span class="sxs-lookup"><span data-stu-id="055eb-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="055eb-149">Esto se comprueba mediante una llamada a **MediaTypeFormatter.CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="055eb-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="055eb-150">A continuación, el negociador de contenido examina cada formateador y da como resultado coincide con la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="055eb-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="055eb-151">Para evaluar la búsqueda de coincidencias, el negociador de contenido examina dos cosas en el formateador:</span><span class="sxs-lookup"><span data-stu-id="055eb-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="055eb-152">El **SupportedMediaTypes** colección, que contiene una lista de tipos de medios admitidos.</span><span class="sxs-lookup"><span data-stu-id="055eb-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="055eb-153">El negociador de contenido intenta hacer coincidir esta lista en el encabezado de aceptación de solicitud.</span><span class="sxs-lookup"><span data-stu-id="055eb-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="055eb-154">Tenga en cuenta que el encabezado Accept puede incluir intervalos.</span><span class="sxs-lookup"><span data-stu-id="055eb-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="055eb-155">Por ejemplo, "text/plain" es una coincidencia para el texto /\* o \* / \*.</span><span class="sxs-lookup"><span data-stu-id="055eb-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="055eb-156">El **MediaTypeMappings** colección, que contiene una lista de **MediaTypeMapping** objetos.</span><span class="sxs-lookup"><span data-stu-id="055eb-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="055eb-157">El **MediaTypeMapping** clase proporciona una forma genérica para que coincida con las solicitudes HTTP con tipos de medios.</span><span class="sxs-lookup"><span data-stu-id="055eb-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="055eb-158">Por ejemplo, puede asignar un encabezado HTTP personalizado para un tipo de medio determinado.</span><span class="sxs-lookup"><span data-stu-id="055eb-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="055eb-159">Si hay varias coincide, la coincidencia con el más alto gana factor de calidad.</span><span class="sxs-lookup"><span data-stu-id="055eb-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="055eb-160">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="055eb-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="055eb-161">En este ejemplo, application/json tiene un factor de calidad implícita de 1,0, por lo que es preferible a través de la aplicación/xml.</span><span class="sxs-lookup"><span data-stu-id="055eb-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="055eb-162">Si se encuentra ninguna coincidencia, el negociador de contenido intenta hacer coincidir en el tipo de medio del cuerpo de la solicitud, si lo hay.</span><span class="sxs-lookup"><span data-stu-id="055eb-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="055eb-163">Por ejemplo, si la solicitud contiene datos JSON, examina el negociador de contenido para un formateador JSON.</span><span class="sxs-lookup"><span data-stu-id="055eb-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="055eb-164">Si todavía no hay ninguna coincidencia, el negociador de contenido simplemente toma al primer formateador que puede serializar el tipo.</span><span class="sxs-lookup"><span data-stu-id="055eb-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="055eb-165">Al seleccionar una codificación de caracteres</span><span class="sxs-lookup"><span data-stu-id="055eb-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="055eb-166">Una vez que se selecciona un formateador, el negociador de contenido elige la mejor codificación de caracteres examinando el **SupportedEncodings** propiedad en el formateador y hacerla coincidir con el encabezado Accept-Charset en la solicitud (si existe).</span><span class="sxs-lookup"><span data-stu-id="055eb-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
