---
title: Operaciones de solicitud y respuesta en ASP.NET Core
author: jkotalik
description: Aprenda a leer el cuerpo de la solicitud y a escribir el cuerpo de respuesta en ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jukotali
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/middleware/request-response
ms.openlocfilehash: 0c321dad256e239b61907980c09d2c088c1407ff
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538571"
---
# <a name="request-and-response-operations-in-aspnet-core"></a><span data-ttu-id="75817-103">Operaciones de solicitud y respuesta en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="75817-103">Request and response operations in ASP.NET Core</span></span>

<span data-ttu-id="75817-104">Por [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="75817-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="75817-105">En este artículo se explica cómo leer el cuerpo de la solicitud y escribir el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="75817-105">This article explains how to read from the request body and write to the response body.</span></span> <span data-ttu-id="75817-106">Es posible que al escribir middleware deba escribir código para estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="75817-106">You might need to write code for these operations when you're writing middleware.</span></span> <span data-ttu-id="75817-107">Si no es así, lo normal es que no tenga que escribir este código porque las operaciones se realizan mediante MVC y Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="75817-107">Otherwise, you typically don't have to write this code because the operations are handled by MVC and Razor Pages.</span></span>

<span data-ttu-id="75817-108">En ASP.NET Core 3.0, hay dos abstracciones para los cuerpos de solicitud y respuesta: <xref:System.IO.Stream> y <xref:System.IO.Pipelines.Pipe>.</span><span class="sxs-lookup"><span data-stu-id="75817-108">In ASP.NET Core 3.0, there are two abstractions for the request and response bodies: <xref:System.IO.Stream> and <xref:System.IO.Pipelines.Pipe>.</span></span> <span data-ttu-id="75817-109">Para leer la solicitud, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) es un objeto <xref:System.IO.Stream> y `HttpRequest.BodyPipe` es un objeto <xref:System.IO.Pipelines.PipeReader>.</span><span class="sxs-lookup"><span data-stu-id="75817-109">For request reading, [HttpRequest.Body](xref:Microsoft.AspNetCore.Http.HttpRequest.Body) is a <xref:System.IO.Stream>, and `HttpRequest.BodyPipe` is a <xref:System.IO.Pipelines.PipeReader>.</span></span> <span data-ttu-id="75817-110">Para escribir la respuesta, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) es un objeto `HttpResponse.BodyPipe` y es un objeto <xref:System.IO.Pipelines.PipeWriter>.</span><span class="sxs-lookup"><span data-stu-id="75817-110">For response writing, [HttpResponse.Body](xref:Microsoft.AspNetCore.Http.HttpResponse.Body) is a `HttpResponse.BodyPipe` is a <xref:System.IO.Pipelines.PipeWriter>.</span></span>

<span data-ttu-id="75817-111">Se recomiendan canalizaciones a través de secuencias.</span><span class="sxs-lookup"><span data-stu-id="75817-111">We recommend pipelines over streams.</span></span> <span data-ttu-id="75817-112">Las secuencias pueden ser más fáciles de usar en el caso de algunas operaciones sencillas, pero las canalizaciones son más ventajosas para el rendimiento y son más fáciles de usar en la mayoría de los casos.</span><span class="sxs-lookup"><span data-stu-id="75817-112">Streams can be easier to use for some simple operations, but pipelines have a performance advantage and are easier to use in most scenarios.</span></span> <span data-ttu-id="75817-113">En 3.0, ASP.NET Core está empezando a usar internamente canalizaciones en lugar de secuencias.</span><span class="sxs-lookup"><span data-stu-id="75817-113">In 3.0, ASP.NET Core is starting to use pipelines instead of streams internally.</span></span> <span data-ttu-id="75817-114">Ejemplos:</span><span class="sxs-lookup"><span data-stu-id="75817-114">Examples include:</span></span>

- `FormReader`
- `TextReader`
- `TexWriter`
- `HttpResponse.WriteAsync`

<span data-ttu-id="75817-115">Las secuencias no van a desaparecer.</span><span class="sxs-lookup"><span data-stu-id="75817-115">Streams aren't going away.</span></span> <span data-ttu-id="75817-116">Se seguirán usando en todo .NET, y además muchos tipos de secuencias no tienen equivalentes de canalización, como `FileStreams` y `ResponseCompression`.</span><span class="sxs-lookup"><span data-stu-id="75817-116">They continue to be used throughout .NET, and many stream types don't have pipe equivalents, like `FileStreams` and `ResponseCompression`.</span></span>

## <a name="stream-examples"></a><span data-ttu-id="75817-117">Ejemplos de secuencias</span><span class="sxs-lookup"><span data-stu-id="75817-117">Stream examples</span></span>

<span data-ttu-id="75817-118">Imagine que quiere crear un middleware que lee el cuerpo de la solicitud entero como una lista de cadenas, que se divide en nuevas líneas.</span><span class="sxs-lookup"><span data-stu-id="75817-118">Suppose you want to create a middleware that reads the entire request body as a list of strings, splitting on new lines.</span></span> <span data-ttu-id="75817-119">Una implementación de secuencias sencilla podría parecerse al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="75817-119">A simple stream implementation might look like the following example:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStream)]

<span data-ttu-id="75817-120">Este código funciona, pero hay algunos problemas:</span><span class="sxs-lookup"><span data-stu-id="75817-120">This code works, but there are some issues:</span></span>

- <span data-ttu-id="75817-121">Antes de realizar la anexión a `StringBuilder`, en el ejemplo se crea otra cadena (`encodedString`) que se desecha inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="75817-121">Before appending to the `StringBuilder`, the example creates another string (`encodedString`) that is thrown away immediately.</span></span> <span data-ttu-id="75817-122">Este proceso se produce con todos los bytes de la secuencia, por lo que el resultado es la asignación de memoria adicional del tamaño del cuerpo de la solicitud entera.</span><span class="sxs-lookup"><span data-stu-id="75817-122">This process occurs for all bytes in the stream, so the result is extra memory allocation the size of the entire request body.</span></span>
- <span data-ttu-id="75817-123">En el ejemplo se lee toda la cadena antes de la división en nuevas líneas.</span><span class="sxs-lookup"><span data-stu-id="75817-123">The example reads the entire string before splitting on new lines.</span></span> <span data-ttu-id="75817-124">Sería más eficaz buscar nuevas líneas en la matriz de bytes.</span><span class="sxs-lookup"><span data-stu-id="75817-124">It would be more efficient to check for new lines in the byte array.</span></span>

<span data-ttu-id="75817-125">En este ejemplo se corrigen algunos de estos problemas:</span><span class="sxs-lookup"><span data-stu-id="75817-125">Here's an example that fixes some of these issues:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringsFromStreamMoreEfficient)]

<span data-ttu-id="75817-126">En este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="75817-126">This example:</span></span>

- <span data-ttu-id="75817-127">No se almacena en búfer todo el cuerpo de la solicitud en `StringBuilder` a menos que no haya ningún carácter de nueva línea.</span><span class="sxs-lookup"><span data-stu-id="75817-127">Doesn't buffer the entire request body in a `StringBuilder` unless there aren't any newline characters.</span></span>
- <span data-ttu-id="75817-128">No se llama a `Split` en la cadena.</span><span class="sxs-lookup"><span data-stu-id="75817-128">Doesn't call `Split` on the string.</span></span>

<span data-ttu-id="75817-129">Sin embargo, todavía hay algunos problemas:</span><span class="sxs-lookup"><span data-stu-id="75817-129">However, there are still are a few issues:</span></span>

- <span data-ttu-id="75817-130">Si los caracteres de nueva línea están dispersos, gran parte del cuerpo de la solicitud se almacena en búfer en la cadena.</span><span class="sxs-lookup"><span data-stu-id="75817-130">If newline characters are sparse, much of the request body is buffered in the string .</span></span>
- <span data-ttu-id="75817-131">Se siguen creando cadenas (`remainingString`) y se agregan al búfer de cadenas, lo que resulta en una asignación adicional.</span><span class="sxs-lookup"><span data-stu-id="75817-131">It still creates strings (`remainingString`) and adds them to the string buffer, which results in an extra allocation.</span></span>

<span data-ttu-id="75817-132">Estos problemas se pueden corregir, pero el código se vuelve cada vez más complicado y las mejoras son pocas.</span><span class="sxs-lookup"><span data-stu-id="75817-132">These issues are fixable, but the code is becoming more and more complicated with little improvement.</span></span> <span data-ttu-id="75817-133">Las canalizaciones ofrecen una manera de resolver estos problemas con una complejidad mínima del código.</span><span class="sxs-lookup"><span data-stu-id="75817-133">Pipelines provide a way to solve these problems with minimal code complexity.</span></span>

## <a name="pipelines"></a><span data-ttu-id="75817-134">Canalizaciones</span><span class="sxs-lookup"><span data-stu-id="75817-134">Pipelines</span></span>

<span data-ttu-id="75817-135">En el ejemplo siguiente se muestra cómo se puede gestionar el mismo escenario mediante un objeto `PipeReader`:</span><span class="sxs-lookup"><span data-stu-id="75817-135">The following example shows how the same scenario can be handled using a `PipeReader`:</span></span>

[!code-csharp[](request-response/samples/3.x/RequestResponseSample/Startup.cs?name=GetListOfStringFromPipe)]

<span data-ttu-id="75817-136">En este ejemplo se solucionan muchos problemas que tenían las implementaciones de secuencias:</span><span class="sxs-lookup"><span data-stu-id="75817-136">This example fixes many issues that the streams implementations had:</span></span>

- <span data-ttu-id="75817-137">No es necesario un búfer de cadenas porque `PipeReader` gestiona los bytes que no se han usado.</span><span class="sxs-lookup"><span data-stu-id="75817-137">There is no need for a string buffer because the `PipeReader` handles bytes that haven't been used.</span></span>
- <span data-ttu-id="75817-138">Las cadenas codificadas se agregan directamente a la lista de cadenas devueltas.</span><span class="sxs-lookup"><span data-stu-id="75817-138">Encoded strings are directly added to the list of returned strings.</span></span>
- <span data-ttu-id="75817-139">La creación de cadenas es de libre asignación, además de la memoria usada por la cadena (excepto la llamada `ToArray()`).</span><span class="sxs-lookup"><span data-stu-id="75817-139">String creation is allocation-free besides the memory used by the string (except the `ToArray()` call).</span></span>

## <a name="adapters"></a><span data-ttu-id="75817-140">Adaptadores</span><span class="sxs-lookup"><span data-stu-id="75817-140">Adapters</span></span>

<span data-ttu-id="75817-141">Ahora que ambas propiedades, `Body` y `BodyPipe`, están disponibles para `HttpRequest` y `HttpResponse`, ¿qué ocurre cuando se establece `Body` en una secuencia diferente?</span><span class="sxs-lookup"><span data-stu-id="75817-141">Now that both `Body` and `BodyPipe` properties are available for `HttpRequest` and `HttpResponse`, what happens when you set `Body` to a different stream?</span></span> <span data-ttu-id="75817-142">En 3.0, un nuevo conjunto de adaptadores se adaptan automáticamente al tipo del otro.</span><span class="sxs-lookup"><span data-stu-id="75817-142">In 3.0, a new set of adapters automatically adapt each type to the other.</span></span> <span data-ttu-id="75817-143">Por ejemplo, si establece `HttpRequest.Body` en una nueva secuencia, `HttpRequest.BodyPipe` se establece automáticamente en un nuevo objeto `PipeReader` que encapsula a `HttpRequest.Body`.</span><span class="sxs-lookup"><span data-stu-id="75817-143">For example, if you set `HttpRequest.Body` to a new stream, `HttpRequest.BodyPipe` is automatically set to a new `PipeReader` that wraps `HttpRequest.Body`.</span></span> <span data-ttu-id="75817-144">El mismo comportamiento se aplica a la configuración de la propiedad `BodyPipe`.</span><span class="sxs-lookup"><span data-stu-id="75817-144">The same behavior applies to setting the `BodyPipe` property.</span></span> <span data-ttu-id="75817-145">Si `HttpResponse.BodyPipe` se establece en un nuevo objeto `PipeWriter`, `HttpResponse.Body` se establece automáticamente en una nueva secuencia que encapsula a `HttpResponse.BodyPipe`.</span><span class="sxs-lookup"><span data-stu-id="75817-145">If `HttpResponse.BodyPipe` is set to a new `PipeWriter`, the `HttpResponse.Body` is automatically set to a new stream that wraps `HttpResponse.BodyPipe`.</span></span>

## <a name="startasync"></a><span data-ttu-id="75817-146">StartAsync</span><span class="sxs-lookup"><span data-stu-id="75817-146">StartAsync</span></span>

<span data-ttu-id="75817-147">`HttpResponse.StartAsync` es una novedad de la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="75817-147">`HttpResponse.StartAsync` is new in 3.0.</span></span> <span data-ttu-id="75817-148">Se usa para indicar que los encabezados no se pueden modificar y para ejecutar devoluciones de llamada `OnStarting`.</span><span class="sxs-lookup"><span data-stu-id="75817-148">It is used to indicate that headers are unmodifiable and to run `OnStarting` callbacks.</span></span> <span data-ttu-id="75817-149">En 3.0-preview3, tiene que llamar a `StartAsync` antes de usar `HttpRequest.BodyPipe`, y en futuras versiones, será una recomendación.</span><span class="sxs-lookup"><span data-stu-id="75817-149">In 3.0-preview3, you have to call `StartAsync` before using `HttpRequest.BodyPipe`, and in future releases, it will be a recommendation.</span></span> <span data-ttu-id="75817-150">Al usar Kestrel como servidor, si se llama a StartAsync antes de usar el objeto `PipeReader` se garantiza que la memoria que devuelve `GetMemory` pertenecerá al objeto interno <xref:System.IO.Pipelines.Pipe> de Kestrel en lugar de a un búfer externo.</span><span class="sxs-lookup"><span data-stu-id="75817-150">When using Kestrel as a server, calling StartAsync before using the `PipeReader` guarantees that memory returned by `GetMemory` will belong to Kestrel's internal <xref:System.IO.Pipelines.Pipe> rather than an external buffer.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75817-151">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="75817-151">Additional resources</span></span>

- [<span data-ttu-id="75817-152">Introducción a System.IO.Pipelines</span><span class="sxs-lookup"><span data-stu-id="75817-152">Introducing System.IO.Pipelines</span></span>](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/)
- <xref:fundamentals/middleware/write>