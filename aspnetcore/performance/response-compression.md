---
title: Compresión de respuesta en ASP.NET Core
author: guardrex
description: Obtenga información sobre la compresión de respuesta y cómo usar Middleware de compresión de respuesta en aplicaciones de ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: performance/response-compression
ms.openlocfilehash: e87480ebb81791ed233f3e2308e35e21e081824f
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248373"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="dcc3e-103">Compresión de respuesta en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dcc3e-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="dcc3e-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="dcc3e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="dcc3e-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dcc3e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="dcc3e-106">Ancho de banda de red es un recurso limitado.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="dcc3e-107">Reducir el tamaño de la respuesta normalmente aumenta la capacidad de respuesta de una aplicación, a menudo drásticamente.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="dcc3e-108">Una forma de reducir los tamaños de carga es comprimir las respuestas de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="dcc3e-109">Cuándo usar Middleware de compresión de respuestas</span><span class="sxs-lookup"><span data-stu-id="dcc3e-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="dcc3e-110">Use las tecnologías de compresión de respuesta basado en servidor en IIS, Apache o Nginx.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="dcc3e-111">El rendimiento del middleware probablemente no coincida con los módulos de servidor.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="dcc3e-112">[Servidor HTTP.sys](xref:fundamentals/servers/httpsys) server y [Kestrel](xref:fundamentals/servers/kestrel) server actualmente no ofrecen compatibilidad integrada de compresión.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="dcc3e-113">Use el Middleware de compresión de respuesta cuando haya:</span><span class="sxs-lookup"><span data-stu-id="dcc3e-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="dcc3e-114">No se puede usar las siguientes tecnologías de compresión basada en servidor:</span><span class="sxs-lookup"><span data-stu-id="dcc3e-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="dcc3e-115">Módulo de compresión dinámica de IIS</span><span class="sxs-lookup"><span data-stu-id="dcc3e-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="dcc3e-116">Apache mod_deflate module</span><span class="sxs-lookup"><span data-stu-id="dcc3e-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="dcc3e-117">Nginx compresión y descompresión</span><span class="sxs-lookup"><span data-stu-id="dcc3e-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="dcc3e-118">Que hospeda directamente en:</span><span class="sxs-lookup"><span data-stu-id="dcc3e-118">Hosting directly on:</span></span>
  * <span data-ttu-id="dcc3e-119">[Servidor HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominados WebListener)</span><span class="sxs-lookup"><span data-stu-id="dcc3e-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="dcc3e-120">Servidor kestrel</span><span class="sxs-lookup"><span data-stu-id="dcc3e-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="dcc3e-121">Compresión de las respuestas</span><span class="sxs-lookup"><span data-stu-id="dcc3e-121">Response compression</span></span>

<span data-ttu-id="dcc3e-122">Por lo general, cualquier respuesta comprimida de forma no nativa puede beneficiarse de la compresión de respuesta.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="dcc3e-123">Respuestas de forma no nativa comprimidas normalmente incluyen: CSS, JavaScript, HTML, XML y JSON.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="dcc3e-124">No debe comprimir activos comprimidos de forma nativa, como archivos PNG.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="dcc3e-125">Si se intenta comprimir aún más una respuesta cifrada de forma nativa, reducción adicional ninguna pequeño en el tiempo de tamaño y la transmisión probablemente resultar mínimo comparado con el tiempo que tardó en procesarse la compresión.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="dcc3e-126">No comprimir los archivos inferiores a aproximadamente 150-1000 bytes (según el contenido del archivo y la eficacia de compresión).</span><span class="sxs-lookup"><span data-stu-id="dcc3e-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="dcc3e-127">La sobrecarga de la compresión de archivos pequeños puede producir un archivo comprimido mayor que el archivo descomprimido.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="dcc3e-128">Cuando un cliente pueda procesar el contenido comprimido, el cliente debe informar al servidor de sus capacidades enviando el `Accept-Encoding` encabezado con la solicitud.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="dcc3e-129">Cuando un servidor envía contenido comprimido, debe incluir información en el `Content-Encoding` encabezado en cómo se codifica la respuesta comprimida.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="dcc3e-130">Contenido designaciones de codificación admitidas por el middleware se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="dcc3e-131">`Accept-Encoding` valores de encabezado</span><span class="sxs-lookup"><span data-stu-id="dcc3e-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="dcc3e-132">Middleware compatible</span><span class="sxs-lookup"><span data-stu-id="dcc3e-132">Middleware Supported</span></span> | <span data-ttu-id="dcc3e-133">Descripción</span><span class="sxs-lookup"><span data-stu-id="dcc3e-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="dcc3e-134">Sí (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="dcc3e-134">Yes (default)</span></span>        | [<span data-ttu-id="dcc3e-135">Formato de datos comprimidos Brotli</span><span class="sxs-lookup"><span data-stu-id="dcc3e-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="dcc3e-136">No</span><span class="sxs-lookup"><span data-stu-id="dcc3e-136">No</span></span>                   | [<span data-ttu-id="dcc3e-137">Formato de datos comprimidos DEFLATE</span><span class="sxs-lookup"><span data-stu-id="dcc3e-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="dcc3e-138">No</span><span class="sxs-lookup"><span data-stu-id="dcc3e-138">No</span></span>                   | [<span data-ttu-id="dcc3e-139">Intercambio de W3C XML eficaz</span><span class="sxs-lookup"><span data-stu-id="dcc3e-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="dcc3e-140">Sí</span><span class="sxs-lookup"><span data-stu-id="dcc3e-140">Yes</span></span>                  | [<span data-ttu-id="dcc3e-141">Formato de archivo GZIP</span><span class="sxs-lookup"><span data-stu-id="dcc3e-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="dcc3e-142">Sí</span><span class="sxs-lookup"><span data-stu-id="dcc3e-142">Yes</span></span>                  | <span data-ttu-id="dcc3e-143">Identificador de "Ninguna codificación": No se debe codificar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="dcc3e-144">No</span><span class="sxs-lookup"><span data-stu-id="dcc3e-144">No</span></span>                   | [<span data-ttu-id="dcc3e-145">Formato de transferencia de red para archivos de Java</span><span class="sxs-lookup"><span data-stu-id="dcc3e-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="dcc3e-146">Sí</span><span class="sxs-lookup"><span data-stu-id="dcc3e-146">Yes</span></span>                  | <span data-ttu-id="dcc3e-147">Cualquier contenido disponible no solicitados explícitamente la codificación</span><span class="sxs-lookup"><span data-stu-id="dcc3e-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="dcc3e-148">`Accept-Encoding` valores de encabezado</span><span class="sxs-lookup"><span data-stu-id="dcc3e-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="dcc3e-149">Middleware compatible</span><span class="sxs-lookup"><span data-stu-id="dcc3e-149">Middleware Supported</span></span> | <span data-ttu-id="dcc3e-150">Descripción</span><span class="sxs-lookup"><span data-stu-id="dcc3e-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="dcc3e-151">No</span><span class="sxs-lookup"><span data-stu-id="dcc3e-151">No</span></span>                   | [<span data-ttu-id="dcc3e-152">Formato de datos comprimidos Brotli</span><span class="sxs-lookup"><span data-stu-id="dcc3e-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="dcc3e-153">No</span><span class="sxs-lookup"><span data-stu-id="dcc3e-153">No</span></span>                   | [<span data-ttu-id="dcc3e-154">Formato de datos comprimidos DEFLATE</span><span class="sxs-lookup"><span data-stu-id="dcc3e-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="dcc3e-155">No</span><span class="sxs-lookup"><span data-stu-id="dcc3e-155">No</span></span>                   | [<span data-ttu-id="dcc3e-156">Intercambio de W3C XML eficaz</span><span class="sxs-lookup"><span data-stu-id="dcc3e-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="dcc3e-157">Sí (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="dcc3e-157">Yes (default)</span></span>        | [<span data-ttu-id="dcc3e-158">Formato de archivo GZIP</span><span class="sxs-lookup"><span data-stu-id="dcc3e-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="dcc3e-159">Sí</span><span class="sxs-lookup"><span data-stu-id="dcc3e-159">Yes</span></span>                  | <span data-ttu-id="dcc3e-160">Identificador de "Ninguna codificación": No se debe codificar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="dcc3e-161">No</span><span class="sxs-lookup"><span data-stu-id="dcc3e-161">No</span></span>                   | [<span data-ttu-id="dcc3e-162">Formato de transferencia de red para archivos de Java</span><span class="sxs-lookup"><span data-stu-id="dcc3e-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="dcc3e-163">Sí</span><span class="sxs-lookup"><span data-stu-id="dcc3e-163">Yes</span></span>                  | <span data-ttu-id="dcc3e-164">Cualquier contenido disponible no solicitados explícitamente la codificación</span><span class="sxs-lookup"><span data-stu-id="dcc3e-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="dcc3e-165">Para obtener más información, consulte el [IANA oficial de codificación lista del contenido](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="dcc3e-165">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="dcc3e-166">El middleware le permite agregar proveedores de compresión adicional para las instalaciones personalizadas `Accept-Encoding` valores de encabezado.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="dcc3e-167">Para obtener más información, consulte [proveedores personalizados](#custom-providers) a continuación.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="dcc3e-168">El software intermedio es capaz de reaccionar ante el valor de calidad (qvalue, `q`) cuando los envía al cliente para dar prioridad a los esquemas de compresión de ponderación.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="dcc3e-169">Para obtener más información, consulte [RFC 7231: Codificación aceptada](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="dcc3e-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="dcc3e-170">Algoritmos de compresión están sujetos a un equilibrio entre la velocidad de compresión y la eficacia de la compresión.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="dcc3e-171">*Eficacia* en este contexto se refiere al tamaño de la salida después de la compresión.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="dcc3e-172">El tamaño más pequeño se logra mediante el máximo *óptimo* compresión.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="dcc3e-173">Los encabezados implicados en la solicitud, enviar, almacenamiento en caché y recibir contenido comprimido se describen en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="dcc3e-174">Header</span><span class="sxs-lookup"><span data-stu-id="dcc3e-174">Header</span></span>             | <span data-ttu-id="dcc3e-175">Rol</span><span class="sxs-lookup"><span data-stu-id="dcc3e-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="dcc3e-176">Enviado desde el cliente al servidor para indicar la codificación esquemas aceptables para el cliente del contenido.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="dcc3e-177">Enviado desde el servidor al cliente para indicar la codificación del contenido de la carga.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="dcc3e-178">Cuando se produce la compresión, el `Content-Length` se quita el encabezado, desde los cambios de contenido del cuerpo cuando se comprime la respuesta.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="dcc3e-179">Cuando se produce la compresión, el `Content-MD5` se quita el encabezado, puesto que ha cambiado el contenido del cuerpo y el hash ya no es válido.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="dcc3e-180">Especifica el tipo MIME del contenido.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="dcc3e-181">Debe especificar cada respuesta su `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="dcc3e-182">El middleware comprueba este valor para determinar si se debe comprimir la respuesta.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="dcc3e-183">El middleware especifica un conjunto de [tipos MIME predeterminados](#mime-types) que puede codificar, pero puede reemplazar o agregar tipos MIME.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="dcc3e-184">Cuando envía el servidor con un valor de `Accept-Encoding` a los clientes y servidores proxy, el `Vary` encabezado indica al cliente o servidor proxy que debe almacenar en caché (puede variar) las respuestas en función del valor de la `Accept-Encoding` encabezado de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="dcc3e-185">El resultado de la devolución del contenido con el `Vary: Accept-Encoding` encabezado está comprimida y las que se almacenan en caché las respuestas sin comprimir por separado.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="dcc3e-186">Explore las características de Middleware de compresión de respuesta con el [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="dcc3e-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="dcc3e-187">En el ejemplo muestra:</span><span class="sxs-lookup"><span data-stu-id="dcc3e-187">The sample illustrates:</span></span>

* <span data-ttu-id="dcc3e-188">La compresión de respuestas de la aplicación con Gzip y proveedores de compresión personalizado.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="dcc3e-189">Cómo agregar un tipo MIME a la lista predeterminada de los tipos MIME para compresión.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="dcc3e-190">Paquete</span><span class="sxs-lookup"><span data-stu-id="dcc3e-190">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="dcc3e-191">Para incluir el software intermedio en un proyecto, agregue una referencia a la [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app), que incluye el [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paquete.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-191">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="dcc3e-192">Para incluir el software intermedio en un proyecto, agregue una referencia a la [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage), que incluye el [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paquete.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="dcc3e-193">Para incluir el software intermedio en un proyecto, agregue una referencia a la [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paquete.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-193">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="dcc3e-194">Configuración</span><span class="sxs-lookup"><span data-stu-id="dcc3e-194">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="dcc3e-195">El código siguiente muestra cómo habilitar el Middleware de compresión de respuestas para tipos MIME predeterminados y los proveedores de compresión ([Brotli](#brotli-compression-provider) y [Gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="dcc3e-195">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="dcc3e-196">El código siguiente muestra cómo habilitar el Middleware de compresión de respuestas para tipos MIME predeterminados y el [proveedor de compresión Gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="dcc3e-196">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

<span data-ttu-id="dcc3e-197">Notas:</span><span class="sxs-lookup"><span data-stu-id="dcc3e-197">Notes:</span></span>

* <span data-ttu-id="dcc3e-198">`app.UseResponseCompression` debe llamarse antes `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-198">`app.UseResponseCompression` must be called before `app.UseMvc`.</span></span>
* <span data-ttu-id="dcc3e-199">Usar una herramienta como [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), o [Postman](https://www.getpostman.com/) para establecer el `Accept-Encoding` encabezado de solicitud y estudiar los encabezados de respuesta, el tamaño y el cuerpo.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-199">Use a tool such as [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="dcc3e-200">Enviar una solicitud a la aplicación de ejemplo sin la `Accept-Encoding` encabezado y observe que la respuesta es sin comprimir.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-200">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="dcc3e-201">El `Content-Encoding` y `Vary` encabezados no están presentes en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-201">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Ventana de Fiddler que muestra el resultado de una solicitud sin el encabezado Accept-Encoding.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="dcc3e-204">Enviar una solicitud a la aplicación de ejemplo con el `Accept-Encoding: br` encabezado (la compresión Brotli) y observe que la respuesta está comprimida.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-204">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="dcc3e-205">El `Content-Encoding` y `Vary` encabezados están presentes en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-205">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Ventana de Fiddler que muestra el resultado de una solicitud con el encabezado Accept-Encoding y un valor de Brasil.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="dcc3e-209">Enviar una solicitud a la aplicación de ejemplo con el `Accept-Encoding: gzip` encabezado y observe que la respuesta está comprimida.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-209">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="dcc3e-210">El `Content-Encoding` y `Vary` encabezados están presentes en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-210">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Ventana de Fiddler que muestra el resultado de una solicitud con el encabezado Accept-Encoding y un valor de gzip.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="dcc3e-214">Proveedores</span><span class="sxs-lookup"><span data-stu-id="dcc3e-214">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="dcc3e-215">Proveedor de la compresión Brotli</span><span class="sxs-lookup"><span data-stu-id="dcc3e-215">Brotli Compression Provider</span></span>

<span data-ttu-id="dcc3e-216">Use la <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> para comprimir las respuestas con el [formato de datos comprimidos Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="dcc3e-216">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="dcc3e-217">Si no hay proveedores de compresión se agregan explícitamente a la <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="dcc3e-217">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="dcc3e-218">El proveedor de la compresión Brotli se agrega de forma predeterminada en la matriz de proveedores de compresión junto con el [proveedor de compresión Gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="dcc3e-218">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="dcc3e-219">Valores predeterminados de compresión para la compresión Brotli cuando el formato de datos comprimidos Brotli es compatible con el cliente.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-219">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="dcc3e-220">Si Brotli no es compatible con el cliente, la compresión predeterminada es Gzip cuando el cliente admite la compresión Gzip.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-220">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="dcc3e-221">Cuando se agregan explícitamente todos los proveedores de compresión, se debe agregar el proveedor de compresión Brotoli:</span><span class="sxs-lookup"><span data-stu-id="dcc3e-221">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="dcc3e-222">Establecer nivel con la compresión <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-222">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="dcc3e-223">El proveedor de la compresión Brotli el valor predeterminado es el nivel de compresión más rápido ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), que no podría producir la compresión más eficaz.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-223">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="dcc3e-224">Si se desea la compresión más eficaz, configure el middleware de compresión óptima.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-224">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="dcc3e-225">Nivel de compresión</span><span class="sxs-lookup"><span data-stu-id="dcc3e-225">Compression Level</span></span> | <span data-ttu-id="dcc3e-226">Descripción</span><span class="sxs-lookup"><span data-stu-id="dcc3e-226">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="dcc3e-227">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="dcc3e-227">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="dcc3e-228">Compresión debe completarse tan pronto como sea posible, incluso si la salida resultante no se comprime de forma óptima.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-228">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="dcc3e-229">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="dcc3e-229">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="dcc3e-230">No debe realizarse ninguna compresión.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-230">No compression should be performed.</span></span> |
| [<span data-ttu-id="dcc3e-231">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="dcc3e-231">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="dcc3e-232">Las respuestas se deben comprimir óptimamente, incluso si la compresión tarda más tiempo en completarse.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-232">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="dcc3e-233">Proveedor de compresión gzip</span><span class="sxs-lookup"><span data-stu-id="dcc3e-233">Gzip Compression Provider</span></span>

<span data-ttu-id="dcc3e-234">Use la <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> para comprimir las respuestas con el [formato de archivo Gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="dcc3e-234">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="dcc3e-235">Si no hay proveedores de compresión se agregan explícitamente a la <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="dcc3e-235">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="dcc3e-236">El proveedor de compresión Gzip se agrega de forma predeterminada en la matriz de proveedores de compresión junto con el [proveedor de la compresión Brotli](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="dcc3e-236">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="dcc3e-237">Valores predeterminados de compresión para la compresión Brotli cuando el formato de datos comprimidos Brotli es compatible con el cliente.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-237">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="dcc3e-238">Si Brotli no es compatible con el cliente, la compresión predeterminada es Gzip cuando el cliente admite la compresión Gzip.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-238">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="dcc3e-239">El proveedor de compresión Gzip se agrega de forma predeterminada en la matriz de proveedores de compresión.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-239">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="dcc3e-240">Compresión predeterminada en Gzip cuando el cliente admite la compresión Gzip.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-240">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="dcc3e-241">El proveedor de compresión Gzip deben agregarse cuando se agregan explícitamente todos los proveedores de compresión:</span><span class="sxs-lookup"><span data-stu-id="dcc3e-241">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

<span data-ttu-id="dcc3e-242">Establecer nivel con la compresión <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-242">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="dcc3e-243">El proveedor de compresión Gzip el valor predeterminado es el nivel de compresión más rápido ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), que no podría producir la compresión más eficaz.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-243">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="dcc3e-244">Si se desea la compresión más eficaz, configure el middleware de compresión óptima.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-244">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="dcc3e-245">Nivel de compresión</span><span class="sxs-lookup"><span data-stu-id="dcc3e-245">Compression Level</span></span> | <span data-ttu-id="dcc3e-246">Descripción</span><span class="sxs-lookup"><span data-stu-id="dcc3e-246">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="dcc3e-247">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="dcc3e-247">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="dcc3e-248">Compresión debe completarse tan pronto como sea posible, incluso si la salida resultante no se comprime de forma óptima.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-248">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="dcc3e-249">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="dcc3e-249">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="dcc3e-250">No debe realizarse ninguna compresión.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-250">No compression should be performed.</span></span> |
| [<span data-ttu-id="dcc3e-251">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="dcc3e-251">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="dcc3e-252">Las respuestas se deben comprimir óptimamente, incluso si la compresión tarda más tiempo en completarse.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-252">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="dcc3e-253">Proveedores personalizados</span><span class="sxs-lookup"><span data-stu-id="dcc3e-253">Custom providers</span></span>

<span data-ttu-id="dcc3e-254">Crear implementaciones de compresión personalizado con <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-254">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="dcc3e-255">El <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> representa el contenido que este codificación `ICompressionProvider` genera.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-255">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="dcc3e-256">El middleware usa esta información para elegir el proveedor basándose en la lista especificada en el `Accept-Encoding` encabezado de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-256">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="dcc3e-257">Uso de la aplicación de ejemplo, el cliente envía una solicitud con el `Accept-Encoding: mycustomcompression` encabezado.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-257">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="dcc3e-258">El middleware usa la implementación de compresión personalizado y devuelve la respuesta con un `Content-Encoding: mycustomcompression` encabezado.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-258">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="dcc3e-259">El cliente debe poder descomprimir la codificación personalizada en orden para una implementación de compresión personalizado para que funcione.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-259">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="dcc3e-260">Enviar una solicitud a la aplicación de ejemplo con el `Accept-Encoding: mycustomcompression` encabezado y observe los encabezados de respuesta.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-260">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="dcc3e-261">El `Vary` y `Content-Encoding` encabezados están presentes en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-261">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="dcc3e-262">El cuerpo de respuesta (no mostrado) no se comprime en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-262">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="dcc3e-263">No hay una implementación de la compresión en el `CustomCompressionProvider` clase del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-263">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="dcc3e-264">Sin embargo, el ejemplo muestra que podría implementar un algoritmo de compresión de este tipo.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-264">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Ventana de Fiddler que muestra el resultado de una solicitud con el encabezado Accept-Encoding y un valor de mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="dcc3e-267">tipos MIME</span><span class="sxs-lookup"><span data-stu-id="dcc3e-267">MIME types</span></span>

<span data-ttu-id="dcc3e-268">El middleware especifica un conjunto predeterminado de los tipos MIME para compresión:</span><span class="sxs-lookup"><span data-stu-id="dcc3e-268">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="dcc3e-269">Reemplazar o anexar los tipos MIME con las opciones de Middleware de compresión de respuesta.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-269">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="dcc3e-270">Tenga en cuenta que comodines MIME tipos, como `text/*` no se admiten.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-270">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="dcc3e-271">La aplicación de ejemplo agrega un tipo MIME para `image/svg+xml` y comprime y sirve de imagen del banner de ASP.NET Core (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="dcc3e-271">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="dcc3e-272">Compresión con el protocolo seguro</span><span class="sxs-lookup"><span data-stu-id="dcc3e-272">Compression with secure protocol</span></span>

<span data-ttu-id="dcc3e-273">Pueden controlar las respuestas comprimidas a través de conexiones seguras con el `EnableForHttps` opción, que está deshabilitada de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-273">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="dcc3e-274">Uso de compresión con páginas generadas dinámicamente puede dar lugar a problemas de seguridad, como el [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) y [infracción](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataques.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-274">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="dcc3e-275">Agregar el encabezado Vary</span><span class="sxs-lookup"><span data-stu-id="dcc3e-275">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="dcc3e-276">Si basa la compresión de respuestas en el `Accept-Encoding` encabezado, hay potencialmente varias versiones comprimidas de la respuesta y una versión sin comprimir.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-276">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="dcc3e-277">Con el fin de indicar a las memorias caché de cliente y servidor proxy que existen varias versiones y deben almacenarse los `Vary` encabezado se agrega con un `Accept-Encoding` valor.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-277">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="dcc3e-278">En ASP.NET Core 2.0 o posterior, se agrega el middleware la `Vary` automáticamente cuando se comprime la respuesta de encabezado.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-278">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="dcc3e-279">Si basa la compresión de respuestas en el `Accept-Encoding` encabezado, hay potencialmente varias versiones comprimidas de la respuesta y una versión sin comprimir.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-279">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="dcc3e-280">Con el fin de indicar a las memorias caché de cliente y servidor proxy que existen varias versiones y deben almacenarse los `Vary` encabezado se agrega con un `Accept-Encoding` valor.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-280">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="dcc3e-281">En ASP.NET Core 1.x, agregar el `Vary` encabezado a la respuesta se lleva a cabo manualmente:</span><span class="sxs-lookup"><span data-stu-id="dcc3e-281">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="dcc3e-282">Problema de middleware cuando están detrás de un proxy inverso de Nginx</span><span class="sxs-lookup"><span data-stu-id="dcc3e-282">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="dcc3e-283">Cuando una solicitud se redirigió mediante un proxy nginx, el `Accept-Encoding` se quita el encabezado.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-283">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="dcc3e-284">Eliminación de la `Accept-Encoding` encabezado evita que el middleware de compresión de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-284">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="dcc3e-285">Para obtener más información, consulte [NGINX: Compresión y descompresión](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="dcc3e-285">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="dcc3e-286">Este problema es controlando [averiguar la compresión de paso a través de Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="dcc3e-286">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="dcc3e-287">Trabajar con la compresión dinámica de IIS</span><span class="sxs-lookup"><span data-stu-id="dcc3e-287">Working with IIS dynamic compression</span></span>

<span data-ttu-id="dcc3e-288">Si tiene un módulo de compresión dinámica de IIS activo configurado en el nivel de servidor que desea deshabilitar para una aplicación, deshabilite el módulo con una adición a la *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-288">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="dcc3e-289">Para más información, vea [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Deshabilitación de módulos de IIS).</span><span class="sxs-lookup"><span data-stu-id="dcc3e-289">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="dcc3e-290">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="dcc3e-290">Troubleshooting</span></span>

<span data-ttu-id="dcc3e-291">Usar una herramienta como [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), o [Postman](https://www.getpostman.com/), que permiten establecer el `Accept-Encoding` encabezado de solicitud y estudiar los encabezados de respuesta, el tamaño y el cuerpo.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-291">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="dcc3e-292">De forma predeterminada, el Middleware de compresión de respuestas comprime las respuestas que cumplen las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="dcc3e-292">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="dcc3e-293">El `Accept-Encoding` está presente con un valor de encabezado `br`, `gzip`, `*`, o codificación personalizada que coincide con un proveedor de compresión personalizado que haya establecido.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-293">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="dcc3e-294">El valor no debe ser `identity` o tiene un valor de calidad (qvalue, `q`) de 0 (cero).</span><span class="sxs-lookup"><span data-stu-id="dcc3e-294">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="dcc3e-295">El tipo MIME (`Content-Type`) deben establecerse y debe coincidir con un tipo MIME configurado en el <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-295">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="dcc3e-296">La solicitud no debe incluir el `Content-Range` encabezado.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-296">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="dcc3e-297">La solicitud debe utilizar el protocolo no seguro (http), a menos que el protocolo seguro (https) se configura en las opciones de Middleware de compresión de respuesta.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-297">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="dcc3e-298">*Tenga en cuenta el peligro [se ha descrito anteriormente](#compression-with-secure-protocol) al habilitar la compresión de contenido segura.*</span><span class="sxs-lookup"><span data-stu-id="dcc3e-298">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="dcc3e-299">El `Accept-Encoding` está presente con un valor de encabezado `gzip`, `*`, o codificación personalizada que coincide con un proveedor de compresión personalizado que haya establecido.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-299">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="dcc3e-300">El valor no debe ser `identity` o tiene un valor de calidad (qvalue, `q`) de 0 (cero).</span><span class="sxs-lookup"><span data-stu-id="dcc3e-300">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="dcc3e-301">El tipo MIME (`Content-Type`) deben establecerse y debe coincidir con un tipo MIME configurado en el <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-301">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="dcc3e-302">La solicitud no debe incluir el `Content-Range` encabezado.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-302">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="dcc3e-303">La solicitud debe utilizar el protocolo no seguro (http), a menos que el protocolo seguro (https) se configura en las opciones de Middleware de compresión de respuesta.</span><span class="sxs-lookup"><span data-stu-id="dcc3e-303">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="dcc3e-304">*Tenga en cuenta el peligro [se ha descrito anteriormente](#compression-with-secure-protocol) al habilitar la compresión de contenido segura.*</span><span class="sxs-lookup"><span data-stu-id="dcc3e-304">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="dcc3e-305">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="dcc3e-305">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="dcc3e-306">Mozilla Developer Network: Accept-Encoding</span><span class="sxs-lookup"><span data-stu-id="dcc3e-306">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="dcc3e-307">Sección RFC 7231 3.1.2.1: Códigos de contenido</span><span class="sxs-lookup"><span data-stu-id="dcc3e-307">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="dcc3e-308">RFC 7230 sección 4.2.3: Codificación gzip</span><span class="sxs-lookup"><span data-stu-id="dcc3e-308">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="dcc3e-309">Versión de especificación del formato de archivo GZIP 4.3</span><span class="sxs-lookup"><span data-stu-id="dcc3e-309">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
