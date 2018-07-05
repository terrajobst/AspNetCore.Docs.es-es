---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Compatibilidad con BSON en ASP.NET Web API 2.1 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7c5e763d92295a83b3431e9ec6e305b07f8a64d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370499"
---
<a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="89299-102">Compatibilidad con BSON en ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="89299-102">BSON Support in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="89299-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="89299-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="89299-104">Web API 2.1 incluye compatibilidad con BSON.</span><span class="sxs-lookup"><span data-stu-id="89299-104">Web API 2.1 introduces support for BSON.</span></span> <span data-ttu-id="89299-105">Este tema muestra cómo usar BSON en el controlador de API de Web (servidor) y en una aplicación cliente. NET.</span><span class="sxs-lookup"><span data-stu-id="89299-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span>

## <a name="what-is-bson"></a><span data-ttu-id="89299-106">¿Qué es BSON?</span><span class="sxs-lookup"><span data-stu-id="89299-106">What is BSON?</span></span>

<span data-ttu-id="89299-107">[BSON](http://bsonspec.org/) es un formato de serialización binaria.</span><span class="sxs-lookup"><span data-stu-id="89299-107">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="89299-108">"BSON" es el acrónimo "Binario"JSON", pero BSON y JSON se serializan de forma muy diferente.</span><span class="sxs-lookup"><span data-stu-id="89299-108">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="89299-109">Como BSON solo es "JSON similar a", porque los objetos se representan como pares de nombre-valor, similares a JSON.</span><span class="sxs-lookup"><span data-stu-id="89299-109">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="89299-110">A diferencia de JSON, los tipos de datos numéricos se almacenan como bytes, cadenas no</span><span class="sxs-lookup"><span data-stu-id="89299-110">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="89299-111">BSON se diseñó para ser ligero, fácil de explorar y rápido para la codificación y descodificación.</span><span class="sxs-lookup"><span data-stu-id="89299-111">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="89299-112">BSON es comparable en tamaño a JSON.</span><span class="sxs-lookup"><span data-stu-id="89299-112">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="89299-113">Dependiendo de los datos, una carga BSON puede ser menor o mayor que una carga JSON.</span><span class="sxs-lookup"><span data-stu-id="89299-113">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="89299-114">Para serializar los datos binarios, como un archivo de imagen BSON es menor que JSON, porque los datos binarios no están codificado en base64.</span><span class="sxs-lookup"><span data-stu-id="89299-114">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="89299-115">Son fáciles de analizar porque los elementos van precedidos de un campo de longitud, por lo que un analizador puede omitir los elementos sin decodificarlos documentos BSON.</span><span class="sxs-lookup"><span data-stu-id="89299-115">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="89299-116">Codificación y descodificación son eficaces, porque los tipos de datos numéricos se almacenan como números, cadenas no.</span><span class="sxs-lookup"><span data-stu-id="89299-116">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="89299-117">Los clientes nativos, como las aplicaciones cliente. NET, pueden beneficiarse del uso BSON en lugar de los formatos basados en texto como JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="89299-117">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="89299-118">Para los clientes de explorador, probablemente deseará quedarse con JSON, porque JavaScript puede convertir directamente la carga de JSON.</span><span class="sxs-lookup"><span data-stu-id="89299-118">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="89299-119">Afortunadamente, la API Web usa [negociación de contenido](content-negotiation.md), por lo que puede admitir ambos formatos y permitir que el cliente elija su API.</span><span class="sxs-lookup"><span data-stu-id="89299-119">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="89299-120">Habilitar BSON en el servidor</span><span class="sxs-lookup"><span data-stu-id="89299-120">Enabling BSON on the Server</span></span>

<span data-ttu-id="89299-121">En la configuración de la API Web, agregue el **BsonMediaTypeFormatter** a la colección de formateadores.</span><span class="sxs-lookup"><span data-stu-id="89299-121">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="89299-122">Ahora, si el cliente solicita "application/bson", API Web usará al formateador BSON.</span><span class="sxs-lookup"><span data-stu-id="89299-122">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="89299-123">Para asociar BSON con otros tipos de medios, agregarlos a la colección SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="89299-123">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="89299-124">El código siguiente agrega "application/vnd.contoso" para los tipos de medios admitidos:</span><span class="sxs-lookup"><span data-stu-id="89299-124">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="89299-125">Sesión de HTTP de ejemplo</span><span class="sxs-lookup"><span data-stu-id="89299-125">Example HTTP Session</span></span>

<span data-ttu-id="89299-126">En este ejemplo, vamos a usar un controlador de API Web simple además de la clase del modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="89299-126">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="89299-127">Un cliente puede enviar la solicitud HTTP siguiente:</span><span class="sxs-lookup"><span data-stu-id="89299-127">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="89299-128">Esta es la respuesta:</span><span class="sxs-lookup"><span data-stu-id="89299-128">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="89299-129">Aquí he reemplazado los datos binarios con &quot;.&quot; caracteres.</span><span class="sxs-lookup"><span data-stu-id="89299-129">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="89299-130">Captura de pantalla siguiente muestra Fiddler los valores sin formato hexadecimales.</span><span class="sxs-lookup"><span data-stu-id="89299-130">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="89299-131">Uso de BSON con HttpClient</span><span class="sxs-lookup"><span data-stu-id="89299-131">Using BSON with HttpClient</span></span>

<span data-ttu-id="89299-132">Las aplicaciones de los clientes de .NET pueden utilizar el formateador BSON con **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="89299-132">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="89299-133">Para obtener más información acerca de **HttpClient**, consulte [una llamada a una Web API desde un cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="89299-133">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="89299-134">El código siguiente envía una solicitud GET que acepta BSON y, a continuación, deserializa la carga BSON en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="89299-134">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="89299-135">Para solicitar BSON desde el servidor, establezca el encabezado Accept a "application/bson":</span><span class="sxs-lookup"><span data-stu-id="89299-135">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="89299-136">Para deserializar el cuerpo de respuesta, utilice el **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="89299-136">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="89299-137">Este formateador no está en la colección de formateadores de forma predeterminada, por lo que debe especificarla al leer el cuerpo de respuesta:</span><span class="sxs-lookup"><span data-stu-id="89299-137">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="89299-138">El ejemplo siguiente muestra cómo enviar una solicitud POST que contiene BSON.</span><span class="sxs-lookup"><span data-stu-id="89299-138">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="89299-139">Gran parte de este código es el mismo que el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="89299-139">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="89299-140">Sin embargo, en el **PostAsync** método, especifique **BsonMediaTypeFormatter** como el formateador:</span><span class="sxs-lookup"><span data-stu-id="89299-140">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="89299-141">Serializar tipos primitivos de nivel superior</span><span class="sxs-lookup"><span data-stu-id="89299-141">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="89299-142">Todos los documentos BSON es una lista de pares clave/valor. La especificación de BSON no define una sintaxis para serializar un único valor sin formato, como un entero o cadena.</span><span class="sxs-lookup"><span data-stu-id="89299-142">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="89299-143">Para evitar esta limitación, la **BsonMediaTypeFormatter** trata los tipos primitivos como un caso especial.</span><span class="sxs-lookup"><span data-stu-id="89299-143">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="89299-144">Antes de serializar, convierte el valor en un par clave/valor con la clave "Value".</span><span class="sxs-lookup"><span data-stu-id="89299-144">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="89299-145">Por ejemplo, suponga que el controlador de API devuelve un entero:</span><span class="sxs-lookup"><span data-stu-id="89299-145">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="89299-146">Antes de serializar, el formateador BSON lo convierte en el siguiente par clave-valor:</span><span class="sxs-lookup"><span data-stu-id="89299-146">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="89299-147">Al deserializar, el formateador convierte los datos al valor original.</span><span class="sxs-lookup"><span data-stu-id="89299-147">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="89299-148">Sin embargo, los clientes que usan un analizador BSON diferente necesitará controlar este caso, si su API web devuelve los valores sin formato.</span><span class="sxs-lookup"><span data-stu-id="89299-148">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="89299-149">En general, debería considerar devolver datos estructurados, en lugar de valores sin procesar.</span><span class="sxs-lookup"><span data-stu-id="89299-149">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="89299-150">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="89299-150">Additional Resources</span></span>

[<span data-ttu-id="89299-151">Ejemplo de la API BSON de Web</span><span class="sxs-lookup"><span data-stu-id="89299-151">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="89299-152">Formateadores de contenido multimedia</span><span class="sxs-lookup"><span data-stu-id="89299-152">Media Formatters</span></span>](media-formatters.md)
