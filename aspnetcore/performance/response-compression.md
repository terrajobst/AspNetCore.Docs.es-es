---
title: Compresión de respuesta en ASP.NET Core
author: guardrex
description: Obtenga información sobre la compresión de respuesta y cómo usar Middleware de compresión de respuesta en aplicaciones de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
uid: performance/response-compression
ms.openlocfilehash: b8a84418a3258e9ac43b4eadd8564c0708590bce
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726965"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="7eaab-103">Compresión de respuesta en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7eaab-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="7eaab-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7eaab-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7eaab-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7eaab-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7eaab-106">El ancho de banda de red es un recurso limitado.</span><span class="sxs-lookup"><span data-stu-id="7eaab-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="7eaab-107">Reducir el tamaño de la respuesta normalmente aumenta la capacidad de respuesta de una aplicación, a menudo drásticamente.</span><span class="sxs-lookup"><span data-stu-id="7eaab-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="7eaab-108">Una manera de reducir los tamaños de carga es comprimir las respuestas de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="7eaab-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="7eaab-109">Cuándo usar el middleware de compresión de respuesta</span><span class="sxs-lookup"><span data-stu-id="7eaab-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="7eaab-110">Usar tecnologías de compresión de respuesta basadas en servidor en IIS, Apache o Nginx.</span><span class="sxs-lookup"><span data-stu-id="7eaab-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="7eaab-111">El rendimiento del middleware probablemente no coincidirá con el de los módulos del servidor.</span><span class="sxs-lookup"><span data-stu-id="7eaab-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="7eaab-112">El servidor [http. sys](xref:fundamentals/servers/httpsys) Server y el servidor [Kestrel](xref:fundamentals/servers/kestrel) no ofrecen actualmente compatibilidad con la compresión integrada.</span><span class="sxs-lookup"><span data-stu-id="7eaab-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="7eaab-113">Use middleware de compresión de respuesta cuando esté:</span><span class="sxs-lookup"><span data-stu-id="7eaab-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="7eaab-114">No se pueden usar las siguientes tecnologías de compresión basadas en servidor:</span><span class="sxs-lookup"><span data-stu-id="7eaab-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="7eaab-115">Módulo de compresión dinámica de IIS</span><span class="sxs-lookup"><span data-stu-id="7eaab-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="7eaab-116">Módulo de Apache mod_deflate</span><span class="sxs-lookup"><span data-stu-id="7eaab-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="7eaab-117">Compresión y descompresión de nginx</span><span class="sxs-lookup"><span data-stu-id="7eaab-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="7eaab-118">Hospedaje directo en:</span><span class="sxs-lookup"><span data-stu-id="7eaab-118">Hosting directly on:</span></span>
  * <span data-ttu-id="7eaab-119">[Servidor http. sys](xref:fundamentals/servers/httpsys) (anteriormente denominado weblistener)</span><span class="sxs-lookup"><span data-stu-id="7eaab-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="7eaab-120">Servidor de Kestrel</span><span class="sxs-lookup"><span data-stu-id="7eaab-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="7eaab-121">Compresión de las respuestas</span><span class="sxs-lookup"><span data-stu-id="7eaab-121">Response compression</span></span>

<span data-ttu-id="7eaab-122">Normalmente, cualquier respuesta no comprimida de forma nativa puede beneficiarse de la compresión de respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="7eaab-123">Las respuestas no comprimidas de forma nativa suelen ser: CSS, JavaScript, HTML, XML y JSON.</span><span class="sxs-lookup"><span data-stu-id="7eaab-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="7eaab-124">No debe comprimir los recursos comprimidos de forma nativa, como los archivos PNG.</span><span class="sxs-lookup"><span data-stu-id="7eaab-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="7eaab-125">Si intenta comprimir aún más una respuesta comprimida de forma nativa, es probable que cualquier reducción adicional en el tamaño y el tiempo de transmisión esté sobrecurrida en el tiempo que se tarda en procesar la compresión.</span><span class="sxs-lookup"><span data-stu-id="7eaab-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="7eaab-126">No comprimir archivos de menos de 150-1000 bytes (según el contenido del archivo y la eficacia de la compresión).</span><span class="sxs-lookup"><span data-stu-id="7eaab-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="7eaab-127">La sobrecarga de comprimir archivos pequeños puede generar un archivo comprimido mayor que el archivo sin comprimir.</span><span class="sxs-lookup"><span data-stu-id="7eaab-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="7eaab-128">Cuando un cliente puede procesar contenido comprimido, el cliente debe informar al servidor de sus capacidades mediante el envío del encabezado de `Accept-Encoding` con la solicitud.</span><span class="sxs-lookup"><span data-stu-id="7eaab-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="7eaab-129">Cuando un servidor envía contenido comprimido, debe incluir información en el encabezado `Content-Encoding` sobre cómo se codifica la respuesta comprimida.</span><span class="sxs-lookup"><span data-stu-id="7eaab-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="7eaab-130">En la tabla siguiente se muestran las designaciones de codificación de contenido que admite el middleware.</span><span class="sxs-lookup"><span data-stu-id="7eaab-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="7eaab-131">`Accept-Encoding` valores de encabezado</span><span class="sxs-lookup"><span data-stu-id="7eaab-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="7eaab-132">Middleware compatible</span><span class="sxs-lookup"><span data-stu-id="7eaab-132">Middleware Supported</span></span> | <span data-ttu-id="7eaab-133">Descripción</span><span class="sxs-lookup"><span data-stu-id="7eaab-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="7eaab-134">Sí (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="7eaab-134">Yes (default)</span></span>        | [<span data-ttu-id="7eaab-135">Brotli formato de datos comprimidos</span><span class="sxs-lookup"><span data-stu-id="7eaab-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="7eaab-136">No</span><span class="sxs-lookup"><span data-stu-id="7eaab-136">No</span></span>                   | [<span data-ttu-id="7eaab-137">Desinflar formato de datos comprimidos</span><span class="sxs-lookup"><span data-stu-id="7eaab-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="7eaab-138">No</span><span class="sxs-lookup"><span data-stu-id="7eaab-138">No</span></span>                   | [<span data-ttu-id="7eaab-139">Intercambio XML eficaz de W3C</span><span class="sxs-lookup"><span data-stu-id="7eaab-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="7eaab-140">Sí</span><span class="sxs-lookup"><span data-stu-id="7eaab-140">Yes</span></span>                  | [<span data-ttu-id="7eaab-141">Formato de archivo gzip</span><span class="sxs-lookup"><span data-stu-id="7eaab-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="7eaab-142">Sí</span><span class="sxs-lookup"><span data-stu-id="7eaab-142">Yes</span></span>                  | <span data-ttu-id="7eaab-143">Identificador de "sin codificación": no se debe codificar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="7eaab-144">No</span><span class="sxs-lookup"><span data-stu-id="7eaab-144">No</span></span>                   | [<span data-ttu-id="7eaab-145">Formato de transferencia de red para archivos de Java</span><span class="sxs-lookup"><span data-stu-id="7eaab-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="7eaab-146">Sí</span><span class="sxs-lookup"><span data-stu-id="7eaab-146">Yes</span></span>                  | <span data-ttu-id="7eaab-147">Cualquier codificación de contenido disponible no se ha solicitado explícitamente</span><span class="sxs-lookup"><span data-stu-id="7eaab-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="7eaab-148">`Accept-Encoding` valores de encabezado</span><span class="sxs-lookup"><span data-stu-id="7eaab-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="7eaab-149">Middleware compatible</span><span class="sxs-lookup"><span data-stu-id="7eaab-149">Middleware Supported</span></span> | <span data-ttu-id="7eaab-150">Descripción</span><span class="sxs-lookup"><span data-stu-id="7eaab-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="7eaab-151">No</span><span class="sxs-lookup"><span data-stu-id="7eaab-151">No</span></span>                   | [<span data-ttu-id="7eaab-152">Brotli formato de datos comprimidos</span><span class="sxs-lookup"><span data-stu-id="7eaab-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="7eaab-153">No</span><span class="sxs-lookup"><span data-stu-id="7eaab-153">No</span></span>                   | [<span data-ttu-id="7eaab-154">Desinflar formato de datos comprimidos</span><span class="sxs-lookup"><span data-stu-id="7eaab-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="7eaab-155">No</span><span class="sxs-lookup"><span data-stu-id="7eaab-155">No</span></span>                   | [<span data-ttu-id="7eaab-156">Intercambio XML eficaz de W3C</span><span class="sxs-lookup"><span data-stu-id="7eaab-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="7eaab-157">Sí (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="7eaab-157">Yes (default)</span></span>        | [<span data-ttu-id="7eaab-158">Formato de archivo gzip</span><span class="sxs-lookup"><span data-stu-id="7eaab-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="7eaab-159">Sí</span><span class="sxs-lookup"><span data-stu-id="7eaab-159">Yes</span></span>                  | <span data-ttu-id="7eaab-160">Identificador de "sin codificación": no se debe codificar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="7eaab-161">No</span><span class="sxs-lookup"><span data-stu-id="7eaab-161">No</span></span>                   | [<span data-ttu-id="7eaab-162">Formato de transferencia de red para archivos de Java</span><span class="sxs-lookup"><span data-stu-id="7eaab-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="7eaab-163">Sí</span><span class="sxs-lookup"><span data-stu-id="7eaab-163">Yes</span></span>                  | <span data-ttu-id="7eaab-164">Cualquier codificación de contenido disponible no se ha solicitado explícitamente</span><span class="sxs-lookup"><span data-stu-id="7eaab-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="7eaab-165">Para obtener más información, consulte la [lista de codificación de contenido oficial de IANA](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="7eaab-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="7eaab-166">El middleware permite agregar proveedores de compresión adicionales para los valores de encabezado de `Accept-Encoding` personalizado.</span><span class="sxs-lookup"><span data-stu-id="7eaab-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="7eaab-167">Para obtener más información, vea [proveedores personalizados](#custom-providers) a continuación.</span><span class="sxs-lookup"><span data-stu-id="7eaab-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="7eaab-168">El middleware es capaz de reaccionar a la ponderación del valor de calidad (qvalue, `q`) cuando lo envía el cliente para priorizar los esquemas de compresión.</span><span class="sxs-lookup"><span data-stu-id="7eaab-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="7eaab-169">Para obtener más información, consulte [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="7eaab-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="7eaab-170">Los algoritmos de compresión están sujetos a un equilibrio entre la velocidad de compresión y la eficacia de la compresión.</span><span class="sxs-lookup"><span data-stu-id="7eaab-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="7eaab-171">La *eficacia* en este contexto hace referencia al tamaño de la salida después de la compresión.</span><span class="sxs-lookup"><span data-stu-id="7eaab-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="7eaab-172">La compresión más *óptima* consigue el menor tamaño.</span><span class="sxs-lookup"><span data-stu-id="7eaab-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="7eaab-173">En la tabla siguiente se describen los encabezados implicados en la solicitud, el envío, el almacenamiento en caché y la recepción de contenido comprimido.</span><span class="sxs-lookup"><span data-stu-id="7eaab-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="7eaab-174">Header</span><span class="sxs-lookup"><span data-stu-id="7eaab-174">Header</span></span>             | <span data-ttu-id="7eaab-175">Rol</span><span class="sxs-lookup"><span data-stu-id="7eaab-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="7eaab-176">Se envía desde el cliente al servidor para indicar los esquemas de codificación de contenido aceptables para el cliente.</span><span class="sxs-lookup"><span data-stu-id="7eaab-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="7eaab-177">Se envía desde el servidor al cliente para indicar la codificación del contenido en la carga.</span><span class="sxs-lookup"><span data-stu-id="7eaab-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="7eaab-178">Cuando se produce la compresión, se quita el encabezado `Content-Length`, ya que el contenido del cuerpo cambia cuando se comprime la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="7eaab-179">Cuando se produce la compresión, se quita el encabezado de `Content-MD5`, ya que el contenido del cuerpo ha cambiado y el hash ya no es válido.</span><span class="sxs-lookup"><span data-stu-id="7eaab-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="7eaab-180">Especifica el tipo MIME del contenido.</span><span class="sxs-lookup"><span data-stu-id="7eaab-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="7eaab-181">Cada respuesta debe especificar su `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="7eaab-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="7eaab-182">El middleware comprueba este valor para determinar si se debe comprimir la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="7eaab-183">El middleware especifica un conjunto de [tipos MIME predeterminados](#mime-types) que se pueden codificar, pero puede reemplazar o agregar tipos MIME.</span><span class="sxs-lookup"><span data-stu-id="7eaab-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="7eaab-184">Cuando lo envía el servidor con un valor de `Accept-Encoding` a clientes y servidores proxy, el encabezado de `Vary` indica al cliente o al proxy que debe almacenar en caché (variar) respuestas en función del valor del encabezado `Accept-Encoding` de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="7eaab-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="7eaab-185">El resultado de devolver el contenido con el encabezado `Vary: Accept-Encoding` es que las respuestas comprimidas y sin comprimir se almacenan en caché por separado.</span><span class="sxs-lookup"><span data-stu-id="7eaab-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="7eaab-186">Explore las características del middleware de compresión de respuesta con la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="7eaab-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="7eaab-187">En el ejemplo se muestra:</span><span class="sxs-lookup"><span data-stu-id="7eaab-187">The sample illustrates:</span></span>

* <span data-ttu-id="7eaab-188">Compresión de las respuestas de la aplicación mediante gzip y proveedores de compresión personalizados.</span><span class="sxs-lookup"><span data-stu-id="7eaab-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="7eaab-189">Cómo agregar un tipo MIME a la lista predeterminada de tipos MIME para la compresión.</span><span class="sxs-lookup"><span data-stu-id="7eaab-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="7eaab-190">Package</span><span class="sxs-lookup"><span data-stu-id="7eaab-190">Package</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7eaab-191">El middleware de compresión de respuesta lo proporciona el paquete [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) , que se incluye implícitamente en ASP.net Core aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="7eaab-191">Response Compression Middleware is provided by the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package, which is implicitly included in ASP.NET Core apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7eaab-192">Para incluir el middleware en un proyecto, agregue una referencia al [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), que incluye el paquete [Microsoft. AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) .</span><span class="sxs-lookup"><span data-stu-id="7eaab-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="7eaab-193">Configuración de</span><span class="sxs-lookup"><span data-stu-id="7eaab-193">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7eaab-194">En el código siguiente se muestra cómo habilitar el middleware de compresión de respuesta para los tipos MIME predeterminados y los proveedores de compresión ([Brotli](#brotli-compression-provider) y [gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="7eaab-194">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7eaab-195">En el código siguiente se muestra cómo habilitar el middleware de compresión de respuesta para los tipos MIME predeterminados y el [proveedor de compresión gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="7eaab-195">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

::: moniker-end

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

<span data-ttu-id="7eaab-196">Notas:</span><span class="sxs-lookup"><span data-stu-id="7eaab-196">Notes:</span></span>

* <span data-ttu-id="7eaab-197">se debe llamar a `app.UseResponseCompression` antes que cualquier software intermedio que comprime las respuestas.</span><span class="sxs-lookup"><span data-stu-id="7eaab-197">`app.UseResponseCompression` must be called before any middleware that compresses responses.</span></span> <span data-ttu-id="7eaab-198">Para obtener más información, vea <xref:fundamentals/middleware/index#middleware-order>.</span><span class="sxs-lookup"><span data-stu-id="7eaab-198">For more information, see <xref:fundamentals/middleware/index#middleware-order>.</span></span>
* <span data-ttu-id="7eaab-199">Use una herramienta como [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)o [Postman](https://www.getpostman.com/) para establecer el encabezado de solicitud `Accept-Encoding` y estudiar los encabezados de respuesta, el tamaño y el cuerpo.</span><span class="sxs-lookup"><span data-stu-id="7eaab-199">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="7eaab-200">Envíe una solicitud a la aplicación de ejemplo sin el encabezado `Accept-Encoding` y observe que la respuesta se ha descomprimido.</span><span class="sxs-lookup"><span data-stu-id="7eaab-200">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="7eaab-201">Los encabezados `Content-Encoding` y `Vary` no están presentes en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-201">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Ventana de Fiddler que muestra el resultado de una solicitud sin el encabezado Accept-Encoding.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7eaab-204">Envíe una solicitud a la aplicación de ejemplo con el encabezado `Accept-Encoding: br` (compresión Brotli) y observe que la respuesta está comprimida.</span><span class="sxs-lookup"><span data-stu-id="7eaab-204">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="7eaab-205">Los encabezados `Content-Encoding` y `Vary` están presentes en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-205">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Ventana de Fiddler que muestra el resultado de una solicitud con el encabezado Accept-Encoding y un valor de br.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7eaab-209">Envíe una solicitud a la aplicación de ejemplo con el encabezado `Accept-Encoding: gzip` y observe que la respuesta está comprimida.</span><span class="sxs-lookup"><span data-stu-id="7eaab-209">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="7eaab-210">Los encabezados `Content-Encoding` y `Vary` están presentes en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-210">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Ventana de Fiddler que muestra el resultado de una solicitud con el encabezado Accept-Encoding y un valor de gzip.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="7eaab-214">Proveedores</span><span class="sxs-lookup"><span data-stu-id="7eaab-214">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="7eaab-215">Proveedor de compresión Brotli</span><span class="sxs-lookup"><span data-stu-id="7eaab-215">Brotli Compression Provider</span></span>

<span data-ttu-id="7eaab-216">Use el <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> para comprimir las respuestas con el [formato de datos comprimidos Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="7eaab-216">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="7eaab-217">Si no se agregan explícitamente proveedores de compresión al <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="7eaab-217">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="7eaab-218">El proveedor de compresión Brotli se agrega de forma predeterminada a la matriz de proveedores de compresión junto con el [proveedor de compresión gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="7eaab-218">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="7eaab-219">La compresión toma como valor predeterminado la compresión Brotli cuando el cliente admite el formato de datos comprimidos Brotli.</span><span class="sxs-lookup"><span data-stu-id="7eaab-219">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="7eaab-220">Si el cliente no admite Brotli, el valor predeterminado de la compresión es gzip cuando el cliente admite la compresión gzip.</span><span class="sxs-lookup"><span data-stu-id="7eaab-220">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="7eaab-221">El proveedor de compresión Brotoli debe agregarse cuando se agreguen explícitamente proveedores de compresión:</span><span class="sxs-lookup"><span data-stu-id="7eaab-221">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7eaab-222">Establezca el nivel de compresión con <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="7eaab-222">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="7eaab-223">El proveedor de compresión Brotli tiene como valor predeterminado el nivel de compresión más rápido ([CompressionLevel. Fast](xref:System.IO.Compression.CompressionLevel)), que podría no producir la compresión más eficaz.</span><span class="sxs-lookup"><span data-stu-id="7eaab-223">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="7eaab-224">Si se desea la compresión más eficaz, configure el middleware para una compresión óptima.</span><span class="sxs-lookup"><span data-stu-id="7eaab-224">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="7eaab-225">Nivel de compresión</span><span class="sxs-lookup"><span data-stu-id="7eaab-225">Compression Level</span></span> | <span data-ttu-id="7eaab-226">Descripción</span><span class="sxs-lookup"><span data-stu-id="7eaab-226">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="7eaab-227">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="7eaab-227">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7eaab-228">La compresión debe completarse tan pronto como sea posible, incluso si la salida resultante no se comprime de forma óptima.</span><span class="sxs-lookup"><span data-stu-id="7eaab-228">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="7eaab-229">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="7eaab-229">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7eaab-230">No se debe realizar ninguna compresión.</span><span class="sxs-lookup"><span data-stu-id="7eaab-230">No compression should be performed.</span></span> |
| [<span data-ttu-id="7eaab-231">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="7eaab-231">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7eaab-232">Las respuestas se deben comprimir óptimamente, incluso si la compresión tarda más tiempo en completarse.</span><span class="sxs-lookup"><span data-stu-id="7eaab-232">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<BrotliCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

::: moniker-end

### <a name="gzip-compression-provider"></a><span data-ttu-id="7eaab-233">Proveedor de compresión gzip</span><span class="sxs-lookup"><span data-stu-id="7eaab-233">Gzip Compression Provider</span></span>

<span data-ttu-id="7eaab-234">Use el <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> para comprimir las respuestas con el [formato de archivo gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="7eaab-234">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="7eaab-235">Si no se agregan explícitamente proveedores de compresión al <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="7eaab-235">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="7eaab-236">El proveedor de compresión gzip se agrega de forma predeterminada a la matriz de proveedores de compresión junto con el [proveedor de compresión Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="7eaab-236">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="7eaab-237">La compresión toma como valor predeterminado la compresión Brotli cuando el cliente admite el formato de datos comprimidos Brotli.</span><span class="sxs-lookup"><span data-stu-id="7eaab-237">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="7eaab-238">Si el cliente no admite Brotli, el valor predeterminado de la compresión es gzip cuando el cliente admite la compresión gzip.</span><span class="sxs-lookup"><span data-stu-id="7eaab-238">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="7eaab-239">El proveedor de compresión gzip se agrega de forma predeterminada a la matriz de proveedores de compresión.</span><span class="sxs-lookup"><span data-stu-id="7eaab-239">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="7eaab-240">De forma predeterminada, la compresión es gzip cuando el cliente admite la compresión gzip.</span><span class="sxs-lookup"><span data-stu-id="7eaab-240">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="7eaab-241">El proveedor de compresión gzip debe agregarse cuando se agreguen explícitamente proveedores de compresión:</span><span class="sxs-lookup"><span data-stu-id="7eaab-241">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

<span data-ttu-id="7eaab-242">Establezca el nivel de compresión con <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="7eaab-242">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="7eaab-243">El proveedor de compresión gzip tiene como valor predeterminado el nivel de compresión más rápido ([CompressionLevel. Fast](xref:System.IO.Compression.CompressionLevel)), que podría no producir la compresión más eficaz.</span><span class="sxs-lookup"><span data-stu-id="7eaab-243">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="7eaab-244">Si se desea la compresión más eficaz, configure el middleware para una compresión óptima.</span><span class="sxs-lookup"><span data-stu-id="7eaab-244">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="7eaab-245">Nivel de compresión</span><span class="sxs-lookup"><span data-stu-id="7eaab-245">Compression Level</span></span> | <span data-ttu-id="7eaab-246">Descripción</span><span class="sxs-lookup"><span data-stu-id="7eaab-246">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="7eaab-247">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="7eaab-247">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7eaab-248">La compresión debe completarse tan pronto como sea posible, incluso si la salida resultante no se comprime de forma óptima.</span><span class="sxs-lookup"><span data-stu-id="7eaab-248">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="7eaab-249">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="7eaab-249">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7eaab-250">No se debe realizar ninguna compresión.</span><span class="sxs-lookup"><span data-stu-id="7eaab-250">No compression should be performed.</span></span> |
| [<span data-ttu-id="7eaab-251">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="7eaab-251">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7eaab-252">Las respuestas se deben comprimir óptimamente, incluso si la compresión tarda más tiempo en completarse.</span><span class="sxs-lookup"><span data-stu-id="7eaab-252">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<GzipCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

### <a name="custom-providers"></a><span data-ttu-id="7eaab-253">Proveedores personalizados</span><span class="sxs-lookup"><span data-stu-id="7eaab-253">Custom providers</span></span>

<span data-ttu-id="7eaab-254">Cree implementaciones de compresión personalizadas con <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="7eaab-254">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="7eaab-255">La <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> representa la codificación de contenido que produce este `ICompressionProvider`.</span><span class="sxs-lookup"><span data-stu-id="7eaab-255">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="7eaab-256">El middleware usa esta información para elegir el proveedor en función de la lista especificada en el encabezado `Accept-Encoding` de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="7eaab-256">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="7eaab-257">Mediante el uso de la aplicación de ejemplo, el cliente envía una solicitud con el encabezado `Accept-Encoding: mycustomcompression`.</span><span class="sxs-lookup"><span data-stu-id="7eaab-257">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="7eaab-258">El middleware usa la implementación de la compresión personalizada y devuelve la respuesta con un encabezado `Content-Encoding: mycustomcompression`.</span><span class="sxs-lookup"><span data-stu-id="7eaab-258">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="7eaab-259">El cliente debe ser capaz de descomprimir la codificación personalizada para que funcione una implementación de compresión personalizada.</span><span class="sxs-lookup"><span data-stu-id="7eaab-259">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="7eaab-260">Envíe una solicitud a la aplicación de ejemplo con el encabezado `Accept-Encoding: mycustomcompression` y observe los encabezados de respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-260">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="7eaab-261">Los encabezados `Vary` y `Content-Encoding` están presentes en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-261">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="7eaab-262">El cuerpo de la respuesta (no se muestra) no se comprime en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="7eaab-262">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="7eaab-263">No hay ninguna implementación de compresión en la clase `CustomCompressionProvider` del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="7eaab-263">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="7eaab-264">Sin embargo, en el ejemplo se muestra dónde implementaría este tipo de algoritmo de compresión.</span><span class="sxs-lookup"><span data-stu-id="7eaab-264">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Ventana de Fiddler que muestra el resultado de una solicitud con el encabezado Accept-Encoding y un valor de mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="7eaab-267">tipos MIME</span><span class="sxs-lookup"><span data-stu-id="7eaab-267">MIME types</span></span>

<span data-ttu-id="7eaab-268">El middleware especifica un conjunto predeterminado de tipos MIME para la compresión:</span><span class="sxs-lookup"><span data-stu-id="7eaab-268">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="7eaab-269">Reemplazar o anexar tipos MIME con las opciones de middleware de compresión de respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-269">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="7eaab-270">Tenga en cuenta que no se admiten los tipos MIME comodín, como `text/*`.</span><span class="sxs-lookup"><span data-stu-id="7eaab-270">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="7eaab-271">La aplicación de ejemplo agrega un tipo MIME para `image/svg+xml` y comprime y sirve la imagen de banner ASP.NET Core (*banner. svg*).</span><span class="sxs-lookup"><span data-stu-id="7eaab-271">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="7eaab-272">Compresión con protocolo seguro</span><span class="sxs-lookup"><span data-stu-id="7eaab-272">Compression with secure protocol</span></span>

<span data-ttu-id="7eaab-273">Las respuestas comprimidas a través de conexiones seguras se pueden controlar con la opción `EnableForHttps`, que está deshabilitada de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7eaab-273">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="7eaab-274">El uso de la compresión con páginas generadas dinámicamente puede provocar problemas de seguridad, como los ataques de [delitos](https://wikipedia.org/wiki/CRIME_(security_exploit)) y [brechas](https://wikipedia.org/wiki/BREACH_(security_exploit)) .</span><span class="sxs-lookup"><span data-stu-id="7eaab-274">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="7eaab-275">Agregar el encabezado Vary</span><span class="sxs-lookup"><span data-stu-id="7eaab-275">Adding the Vary header</span></span>

<span data-ttu-id="7eaab-276">Al comprimir las respuestas en función del encabezado de `Accept-Encoding`, puede haber varias versiones comprimidas de la respuesta y una versión sin comprimir.</span><span class="sxs-lookup"><span data-stu-id="7eaab-276">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="7eaab-277">Para indicar a las memorias caché de cliente y proxy que existen varias versiones y deben almacenarse, se agrega el encabezado `Vary` con un valor `Accept-Encoding`.</span><span class="sxs-lookup"><span data-stu-id="7eaab-277">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="7eaab-278">En ASP.NET Core 2,0 o posterior, el middleware agrega el encabezado de `Vary` automáticamente cuando se comprime la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-278">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="7eaab-279">Problema de middleware al estar detrás de un proxy inverso de nginx</span><span class="sxs-lookup"><span data-stu-id="7eaab-279">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="7eaab-280">Cuando una solicitud es un proxy por Nginx, se quita el encabezado `Accept-Encoding`.</span><span class="sxs-lookup"><span data-stu-id="7eaab-280">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="7eaab-281">La eliminación del encabezado de `Accept-Encoding` impide que el middleware comprima la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-281">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="7eaab-282">Para obtener más información, vea [nginx: compresión y descompresión](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="7eaab-282">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="7eaab-283">Se realiza un seguimiento de este problema mediante la [compresión de paso a través de Nginx (ASPNET/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="7eaab-283">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="7eaab-284">Trabajar con la compresión dinámica de IIS</span><span class="sxs-lookup"><span data-stu-id="7eaab-284">Working with IIS dynamic compression</span></span>

<span data-ttu-id="7eaab-285">Si tiene un módulo de compresión dinámica de IIS activo configurado en el nivel de servidor que quiere deshabilitar para una aplicación, deshabilite el módulo con una adición al archivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="7eaab-285">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="7eaab-286">Para más información, vea [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Deshabilitación de módulos de IIS).</span><span class="sxs-lookup"><span data-stu-id="7eaab-286">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7eaab-287">Solucionar problemas</span><span class="sxs-lookup"><span data-stu-id="7eaab-287">Troubleshooting</span></span>

<span data-ttu-id="7eaab-288">Use una herramienta como [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)o [Postman](https://www.getpostman.com/), que le permite establecer el encabezado de solicitud `Accept-Encoding` y estudiar los encabezados de respuesta, el tamaño y el cuerpo.</span><span class="sxs-lookup"><span data-stu-id="7eaab-288">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="7eaab-289">De forma predeterminada, el middleware de compresión de respuesta comprime las respuestas que cumplen las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="7eaab-289">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="7eaab-290">El encabezado `Accept-Encoding` está presente con un valor de `br`, `gzip`, `*`o codificación personalizada que coincide con un proveedor de compresión personalizado que ha establecido.</span><span class="sxs-lookup"><span data-stu-id="7eaab-290">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="7eaab-291">El valor no debe ser `identity` o tener un valor de calidad (qvalue, `q`) de 0 (cero).</span><span class="sxs-lookup"><span data-stu-id="7eaab-291">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="7eaab-292">Se debe establecer el tipo MIME (`Content-Type`) y debe coincidir con un tipo MIME configurado en el <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="7eaab-292">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="7eaab-293">La solicitud no debe incluir el encabezado `Content-Range`.</span><span class="sxs-lookup"><span data-stu-id="7eaab-293">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="7eaab-294">La solicitud debe usar el Protocolo no seguro (http), a menos que se configure el protocolo seguro (https) en las opciones de middleware de compresión de respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-294">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="7eaab-295">*Tenga en cuenta el riesgo [descrito anteriormente](#compression-with-secure-protocol) al habilitar la compresión de contenido segura.*</span><span class="sxs-lookup"><span data-stu-id="7eaab-295">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="7eaab-296">El encabezado `Accept-Encoding` está presente con un valor de `gzip`, `*`o codificación personalizada que coincide con un proveedor de compresión personalizado que ha establecido.</span><span class="sxs-lookup"><span data-stu-id="7eaab-296">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="7eaab-297">El valor no debe ser `identity` o tener un valor de calidad (qvalue, `q`) de 0 (cero).</span><span class="sxs-lookup"><span data-stu-id="7eaab-297">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="7eaab-298">Se debe establecer el tipo MIME (`Content-Type`) y debe coincidir con un tipo MIME configurado en el <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="7eaab-298">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="7eaab-299">La solicitud no debe incluir el encabezado `Content-Range`.</span><span class="sxs-lookup"><span data-stu-id="7eaab-299">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="7eaab-300">La solicitud debe usar el Protocolo no seguro (http), a menos que se configure el protocolo seguro (https) en las opciones de middleware de compresión de respuesta.</span><span class="sxs-lookup"><span data-stu-id="7eaab-300">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="7eaab-301">*Tenga en cuenta el riesgo [descrito anteriormente](#compression-with-secure-protocol) al habilitar la compresión de contenido segura.*</span><span class="sxs-lookup"><span data-stu-id="7eaab-301">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="7eaab-302">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7eaab-302">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="7eaab-303">Red del desarrollador de Mozilla: codificación de aceptación</span><span class="sxs-lookup"><span data-stu-id="7eaab-303">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="7eaab-304">Sección 3.1.2.1 de RFC 7231: codificación de contenido</span><span class="sxs-lookup"><span data-stu-id="7eaab-304">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="7eaab-305">Sección de RFC 7230 4.2.3: codificación gzip</span><span class="sxs-lookup"><span data-stu-id="7eaab-305">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="7eaab-306">Versión de especificación de formato de archivo GZIP 4,3</span><span class="sxs-lookup"><span data-stu-id="7eaab-306">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
