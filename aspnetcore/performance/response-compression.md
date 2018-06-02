---
title: Middleware de compresión de respuesta para ASP.NET Core
author: guardrex
description: Obtenga información acerca de la compresión de respuesta y cómo usar el Middleware de compresión de respuesta en aplicaciones de ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/response-compression
ms.openlocfilehash: 152799500577dd09247bcee8c87cde39ca20aa79
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729579"
---
# <a name="response-compression-middleware-for-aspnet-core"></a><span data-ttu-id="d9ca4-103">Middleware de compresión de respuesta para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9ca4-103">Response Compression Middleware for ASP.NET Core</span></span>

<span data-ttu-id="d9ca4-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d9ca4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d9ca4-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d9ca4-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d9ca4-106">Ancho de banda de red es un recurso limitado.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="d9ca4-107">Reducir el tamaño de la respuesta normalmente aumenta la capacidad de respuesta de una aplicación, a menudo drásticamente.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="d9ca4-108">Una manera de reducir el tamaño de carga es comprimir las respuestas de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="d9ca4-109">Cuándo utilizar Middleware de compresión de respuesta</span><span class="sxs-lookup"><span data-stu-id="d9ca4-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="d9ca4-110">Utilizar tecnologías de compresión de respuesta basada en servidor en IIS, Apache o Nginx.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="d9ca4-111">El rendimiento del middleware de probablemente no coincida con el de los módulos del servidor.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="d9ca4-112">[Servidor HTTP.sys](xref:fundamentals/servers/httpsys) y [Kestrel](xref:fundamentals/servers/kestrel) actualmente no ofrecen compatibilidad con la compresión integrada.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) and [Kestrel](xref:fundamentals/servers/kestrel) don't currently offer built-in compression support.</span></span>

<span data-ttu-id="d9ca4-113">Use Middleware de compresión de respuesta cuando esté:</span><span class="sxs-lookup"><span data-stu-id="d9ca4-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="d9ca4-114">No se puede usar las siguientes tecnologías de compresión basada en servidor:</span><span class="sxs-lookup"><span data-stu-id="d9ca4-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="d9ca4-115">Módulo de compresión dinámica de IIS</span><span class="sxs-lookup"><span data-stu-id="d9ca4-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="d9ca4-116">Módulo de mod_deflate de Apache</span><span class="sxs-lookup"><span data-stu-id="d9ca4-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="d9ca4-117">Nginx compresión y descompresión</span><span class="sxs-lookup"><span data-stu-id="d9ca4-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="d9ca4-118">Hospeda directamente en:</span><span class="sxs-lookup"><span data-stu-id="d9ca4-118">Hosting directly on:</span></span>
  * <span data-ttu-id="d9ca4-119">[Servidor HTTP.sys](xref:fundamentals/servers/httpsys) (anteriormente denominados [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="d9ca4-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * [<span data-ttu-id="d9ca4-120">Kestrel</span><span class="sxs-lookup"><span data-stu-id="d9ca4-120">Kestrel</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="d9ca4-121">Compresión de respuesta</span><span class="sxs-lookup"><span data-stu-id="d9ca4-121">Response compression</span></span>

<span data-ttu-id="d9ca4-122">Por lo general, cualquier respuesta que comprimen de forma nativa puede beneficiarse de compresión de respuesta.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="d9ca4-123">Respuestas de forma no nativa comprimidas normalmente son: CSS, JavaScript, HTML, XML y JSON.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="d9ca4-124">No debe comprimir activos comprimidos de forma nativa, como archivos PNG.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="d9ca4-125">Si se intentan volver a comprimir aún más una respuesta cifrada de forma nativa, cualquier pequeña reducción adicional en tiempo de tamaño y la transmisión probablemente resultar mínimo comparado con el tiempo que tardó en procesarse la compresión.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="d9ca4-126">No comprimir los archivos de menos de aproximadamente 150-1000 bytes (según el contenido del archivo y la eficacia de compresión).</span><span class="sxs-lookup"><span data-stu-id="d9ca4-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="d9ca4-127">La sobrecarga de la compresión de archivos pequeños puede generar un archivo comprimido mayor que el archivo sin comprimir.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="d9ca4-128">Cuando un cliente puede procesar contenido comprimido, el cliente debe informar al servidor de sus capacidades mediante el envío de la `Accept-Encoding` encabezado con la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="d9ca4-129">Cuando un servidor envía contenido comprimido, debe incluir información de la `Content-Encoding` encabezado en cómo se codifican las respuestas comprimidas.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="d9ca4-130">Contenido designaciones de codificación admitidas por el middleware se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="d9ca4-131">`Accept-Encoding` valores de encabezado</span><span class="sxs-lookup"><span data-stu-id="d9ca4-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="d9ca4-132">Software intermedio compatible</span><span class="sxs-lookup"><span data-stu-id="d9ca4-132">Middleware Supported</span></span> | <span data-ttu-id="d9ca4-133">Descripción</span><span class="sxs-lookup"><span data-stu-id="d9ca4-133">Description</span></span>                                                 |
| ------------------------------- | :------------------: | ----------------------------------------------------------- |
| `br`                            | <span data-ttu-id="d9ca4-134">No</span><span class="sxs-lookup"><span data-stu-id="d9ca4-134">No</span></span>                   | <span data-ttu-id="d9ca4-135">Formato de los datos comprimidos de Brotli</span><span class="sxs-lookup"><span data-stu-id="d9ca4-135">Brotli Compressed Data Format</span></span>                               |
| `compress`                      | <span data-ttu-id="d9ca4-136">No</span><span class="sxs-lookup"><span data-stu-id="d9ca4-136">No</span></span>                   | <span data-ttu-id="d9ca4-137">Formato de datos de "comprimir" de UNIX</span><span class="sxs-lookup"><span data-stu-id="d9ca4-137">UNIX "compress" data format</span></span>                                 |
| `deflate`                       | <span data-ttu-id="d9ca4-138">No</span><span class="sxs-lookup"><span data-stu-id="d9ca4-138">No</span></span>                   | <span data-ttu-id="d9ca4-139">"deflate" datos comprimidos en el formato de datos de "zlib"</span><span class="sxs-lookup"><span data-stu-id="d9ca4-139">"deflate" compressed data inside the "zlib" data format</span></span>     |
| `exi`                           | <span data-ttu-id="d9ca4-140">No</span><span class="sxs-lookup"><span data-stu-id="d9ca4-140">No</span></span>                   | <span data-ttu-id="d9ca4-141">Intercambio eficaz de XML de W3C</span><span class="sxs-lookup"><span data-stu-id="d9ca4-141">W3C Efficient XML Interchange</span></span>                               |
| `gzip`                          | <span data-ttu-id="d9ca4-142">Sí (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="d9ca4-142">Yes (default)</span></span>        | <span data-ttu-id="d9ca4-143">formato de archivo GZIP</span><span class="sxs-lookup"><span data-stu-id="d9ca4-143">gzip file format</span></span>                                            |
| `identity`                      | <span data-ttu-id="d9ca4-144">Sí</span><span class="sxs-lookup"><span data-stu-id="d9ca4-144">Yes</span></span>                  | <span data-ttu-id="d9ca4-145">Identificador de "Sin codificación": no se debe codificar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-145">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="d9ca4-146">No</span><span class="sxs-lookup"><span data-stu-id="d9ca4-146">No</span></span>                   | <span data-ttu-id="d9ca4-147">Formato de transferencia de red para archivos de Java</span><span class="sxs-lookup"><span data-stu-id="d9ca4-147">Network Transfer Format for Java Archives</span></span>                   |
| `*`                             | <span data-ttu-id="d9ca4-148">Sí</span><span class="sxs-lookup"><span data-stu-id="d9ca4-148">Yes</span></span>                  | <span data-ttu-id="d9ca4-149">Cualquier contenido disponible no codificación explícitamente solicitada</span><span class="sxs-lookup"><span data-stu-id="d9ca4-149">Any available content encoding not explicitly requested</span></span>     |

<span data-ttu-id="d9ca4-150">Para obtener más información, consulte el [lista de codificación oficial contenido de IANA](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="d9ca4-150">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="d9ca4-151">El middleware le permite agregar proveedores de compresión adicional para personalizado `Accept-Encoding` valores de encabezado.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-151">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="d9ca4-152">Para obtener más información, consulte [proveedores personalizados](#custom-providers) a continuación.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-152">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="d9ca4-153">El software intermedio es capaz de reaccionar ante el valor de calidad (qvalue, `q`) ponderación cuando enviada por el cliente para dar prioridad a los esquemas de compresión.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-153">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="d9ca4-154">Para obtener más información, consulte [RFC 7231: codificación aceptada](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="d9ca4-154">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="d9ca4-155">Algoritmos de compresión se están sujetas a un equilibrio entre la velocidad de compresión y la eficacia de la compresión.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-155">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="d9ca4-156">*Eficacia* en este contexto hace referencia al tamaño de la salida después de la compresión.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-156">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="d9ca4-157">El tamaño más pequeño se logra mediante la mayoría *óptimo* compresión.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-157">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="d9ca4-158">Los encabezados implicados en la solicitud, enviar, almacenamiento en caché y recibir contenido comprimido se describen en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-158">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="d9ca4-159">Header</span><span class="sxs-lookup"><span data-stu-id="d9ca4-159">Header</span></span>             | <span data-ttu-id="d9ca4-160">Rol</span><span class="sxs-lookup"><span data-stu-id="d9ca4-160">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="d9ca4-161">Envía desde el cliente al servidor para indicar el contenido aceptable para el cliente de esquemas de codificación.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-161">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="d9ca4-162">Envía desde el servidor al cliente para indicar la codificación del contenido de la carga.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-162">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="d9ca4-163">Cuando se produce la compresión, el `Content-Length` se quita el encabezado, desde los cambios de contenido del cuerpo cuando se comprime la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-163">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="d9ca4-164">Cuando se produce la compresión, el `Content-MD5` se quita el encabezado, puesto que ha cambiado el contenido del cuerpo y el hash ya no es válido.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-164">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="d9ca4-165">Especifica el tipo MIME del contenido.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-165">Specifies the MIME type of the content.</span></span> <span data-ttu-id="d9ca4-166">Debe especificar cada respuesta su `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-166">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="d9ca4-167">El middleware comprueba este valor para determinar si se debe comprimir la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-167">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="d9ca4-168">El middleware especifica un conjunto de [predeterminado tipos MIME](#mime-types) que puede codificar, pero puede reemplazar o agregar un tipo MIME.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-168">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="d9ca4-169">Cuando envía el servidor con un valor de `Accept-Encoding` a los clientes y servidores proxy, la `Vary` encabezado indica al cliente o proxy que debe almacenar en caché (varían) las respuestas en función del valor de la `Accept-Encoding` encabezado de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-169">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="d9ca4-170">El resultado de la devolución del contenido con el `Vary: Accept-Encoding` encabezado es que ambos comprimen y sin comprimir las respuestas se almacenan en caché por separado.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-170">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="d9ca4-171">Puede explorar las características del Middleware de compresión de respuesta con el [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="d9ca4-171">You can explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="d9ca4-172">El ejemplo muestra:</span><span class="sxs-lookup"><span data-stu-id="d9ca4-172">The sample illustrates:</span></span>

* <span data-ttu-id="d9ca4-173">La compresión de las respuestas de aplicación mediante gzip y proveedores de compresión personalizada.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-173">The compression of app responses using gzip and custom compression providers.</span></span>
* <span data-ttu-id="d9ca4-174">Cómo agregar un tipo MIME a la lista predeterminada de tipos MIME para la compresión.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-174">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="d9ca4-175">Package</span><span class="sxs-lookup"><span data-stu-id="d9ca4-175">Package</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d9ca4-176">Para incluir el software intermedio en el proyecto, agregue una referencia a la [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) paquete.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-176">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span> <span data-ttu-id="d9ca4-177">Esta característica está disponible para aplicaciones que tienen como destino ASP.NET Core 1.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-177">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="d9ca4-178">Para incluir el software intermedio en el proyecto, agregue una referencia a la [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) empaquetar o usar el [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="d9ca4-178">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package or use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="d9ca4-179">Para incluir el software intermedio en el proyecto, agregue una referencia a la [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) empaquetar o usar el [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d9ca4-179">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package or use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="d9ca4-180">Configuración</span><span class="sxs-lookup"><span data-stu-id="d9ca4-180">Configuration</span></span>

<span data-ttu-id="d9ca4-181">El código siguiente muestra cómo habilitar el Middleware de compresión de respuesta con la compresión de gzip predeterminada y para los tipos MIME predeterminado.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-181">The following code shows how to enable the Response Compression Middleware with the default gzip compression and for default MIME types.</span></span>

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

> [!NOTE]
> <span data-ttu-id="d9ca4-182">Utilice una herramienta como [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), o [Postman](https://www.getpostman.com/) para establecer el `Accept-Encoding` encabezado de solicitud y estudiar los encabezados de respuesta, el tamaño y el cuerpo.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-182">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="d9ca4-183">Enviar una solicitud a la aplicación de ejemplo sin la `Accept-Encoding` encabezado y observe que la respuesta es sin comprimir.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-183">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="d9ca4-184">El `Content-Encoding` y `Vary` encabezados no están presentes en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-184">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Ventana de Fiddler que muestra el resultado de una solicitud sin el encabezado Accept-Encoding.](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="d9ca4-187">Enviar una solicitud a la aplicación de ejemplo con el `Accept-Encoding: gzip` encabezado y observe que la respuesta está comprimida.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-187">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="d9ca4-188">El `Content-Encoding` y `Vary` encabezados están presentes en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-188">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Ventana de Fiddler Mostrar resultado de una solicitud con el encabezado Accept-Encoding y un valor de gzip.](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="d9ca4-192">Proveedores</span><span class="sxs-lookup"><span data-stu-id="d9ca4-192">Providers</span></span>

### <a name="gzipcompressionprovider"></a><span data-ttu-id="d9ca4-193">GzipCompressionProvider</span><span class="sxs-lookup"><span data-stu-id="d9ca4-193">GzipCompressionProvider</span></span>

<span data-ttu-id="d9ca4-194">Use la [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) para comprimir las respuestas con gzip.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-194">Use the [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) to compress responses with gzip.</span></span> <span data-ttu-id="d9ca4-195">Esto es el proveedor de compresión predeterminado si no se especifica ninguno.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-195">This is the default compression provider if none are specified.</span></span> <span data-ttu-id="d9ca4-196">Puede establecer la compresión de nivel con el [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).</span><span class="sxs-lookup"><span data-stu-id="d9ca4-196">You can set the compression level with the [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).</span></span>

<span data-ttu-id="d9ca4-197">El proveedor de compresión gzip usa de forma predeterminada el nivel de compresión más rápido ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), que no puede producir la compresión más eficaz.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-197">The gzip compression provider defaults to the fastest compression level ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="d9ca4-198">Si se desea la compresión más eficaz, puede configurar el middleware de compresión óptima.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-198">If the most efficient compression is desired, you can configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="d9ca4-199">Nivel de compresión</span><span class="sxs-lookup"><span data-stu-id="d9ca4-199">Compression Level</span></span> | <span data-ttu-id="d9ca4-200">Descripción</span><span class="sxs-lookup"><span data-stu-id="d9ca4-200">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="d9ca4-201">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="d9ca4-201">CompressionLevel.Fastest</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="d9ca4-202">Compresión debe completar lo más rápido posible, incluso si el resultado no está comprimido un rendimiento óptimo.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-202">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="d9ca4-203">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="d9ca4-203">CompressionLevel.NoCompression</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="d9ca4-204">Es necesario realizar ninguna compresión.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-204">No compression should be performed.</span></span> |
| [<span data-ttu-id="d9ca4-205">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="d9ca4-205">CompressionLevel.Optimal</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="d9ca4-206">Las respuestas se deben comprimir un rendimiento óptimo, incluso si la compresión tarda más tiempo en completarse.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-206">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d9ca4-207">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d9ca4-207">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d9ca4-208">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d9ca4-208">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

---

## <a name="mime-types"></a><span data-ttu-id="d9ca4-209">tipos MIME</span><span class="sxs-lookup"><span data-stu-id="d9ca4-209">MIME types</span></span>

<span data-ttu-id="d9ca4-210">El middleware especifica un conjunto predeterminado de tipos MIME para la compresión:</span><span class="sxs-lookup"><span data-stu-id="d9ca4-210">The middleware specifies a default set of MIME types for compression:</span></span>

* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

<span data-ttu-id="d9ca4-211">Puede reemplazar o anexar los tipos MIME con las opciones de Middleware de compresión de respuesta.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-211">You can replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="d9ca4-212">Tenga en cuenta que MIME de comodines tipos, como `text/*` no son compatibles.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-212">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="d9ca4-213">La aplicación de ejemplo agrega un tipo MIME para `image/svg+xml` y comprime y sirve de imagen de titular de ASP.NET Core (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="d9ca4-213">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d9ca4-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d9ca4-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d9ca4-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d9ca4-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

---

### <a name="custom-providers"></a><span data-ttu-id="d9ca4-216">Proveedores personalizados</span><span class="sxs-lookup"><span data-stu-id="d9ca4-216">Custom providers</span></span>

<span data-ttu-id="d9ca4-217">Puede crear implementaciones de compresión personalizada con [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider).</span><span class="sxs-lookup"><span data-stu-id="d9ca4-217">You can create custom compression implementations with [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider).</span></span> <span data-ttu-id="d9ca4-218">El [EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) representa la codificación que el control de contenido `ICompressionProvider` genera.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-218">The [EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="d9ca4-219">El middleware que usa esta información para elegir el proveedor basándose en la lista especificada en el `Accept-Encoding` encabezado de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-219">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="d9ca4-220">Mediante la aplicación de ejemplo, el cliente envía una solicitud con el `Accept-Encoding: mycustomcompression` encabezado.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-220">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="d9ca4-221">El software intermedio utiliza la implementación de compresión personalizada y devuelve la respuesta con un `Content-Encoding: mycustomcompression` encabezado.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-221">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="d9ca4-222">El cliente debe poder descomprimir la codificación personalizada en orden para una implementación de la compresión personalizado para que funcione.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-222">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d9ca4-223">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d9ca4-223">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d9ca4-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d9ca4-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

<span data-ttu-id="d9ca4-225">Enviar una solicitud a la aplicación de ejemplo con el `Accept-Encoding: mycustomcompression` encabezado y observe los encabezados de respuesta.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-225">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="d9ca4-226">El `Vary` y `Content-Encoding` encabezados están presentes en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-226">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="d9ca4-227">El cuerpo de respuesta (no mostrado) no está comprimido en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-227">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="d9ca4-228">No hay una implementación de la compresión en el `CustomCompressionProvider` clase del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-228">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="d9ca4-229">Sin embargo, en el ejemplo se muestra donde se implementa un algoritmo de compresión de este tipo.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-229">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Ventana de Fiddler Mostrar resultado de una solicitud con el encabezado Accept-Encoding y un valor de mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="d9ca4-232">Compresión con protocolo seguro</span><span class="sxs-lookup"><span data-stu-id="d9ca4-232">Compression with secure protocol</span></span>

<span data-ttu-id="d9ca4-233">Las respuestas comprimidas a través de conexiones seguras pueden controlarse con el `EnableForHttps` opción, que está deshabilitada de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-233">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="d9ca4-234">Utilice la compresión con páginas generadas dinámicamente puede provocar problemas de seguridad como la [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) y [infracción](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataques.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-234">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="d9ca4-235">Agregar el encabezado Vary</span><span class="sxs-lookup"><span data-stu-id="d9ca4-235">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d9ca4-236">Al comprimir las respuestas según el `Accept-Encoding` encabezado, hay potencialmente varias versiones comprimidas de la respuesta y una versión sin comprimir.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-236">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="d9ca4-237">Para indicar a las memorias caché de cliente y servidor proxy que existen varias versiones y deben almacenarse el `Vary` encabezado se agrega con un `Accept-Encoding` valor.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-237">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="d9ca4-238">En el núcleo de ASP.NET 2.0 o posterior, el middleware agrega el `Vary` encabezado automáticamente cuando se comprime la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-238">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d9ca4-239">Al comprimir las respuestas según el `Accept-Encoding` encabezado, hay potencialmente varias versiones comprimidas de la respuesta y una versión sin comprimir.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-239">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="d9ca4-240">Para indicar a las memorias caché de cliente y servidor proxy que existen varias versiones y deben almacenarse el `Vary` encabezado se agrega con un `Accept-Encoding` valor.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-240">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="d9ca4-241">En ASP.NET Core 1.x, agregar el `Vary` encabezado en la respuesta se realiza manualmente:</span><span class="sxs-lookup"><span data-stu-id="d9ca4-241">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

<span data-ttu-id="d9ca4-242">[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="d9ca4-242">[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="d9ca4-243">Problema de middleware al estar detrás de un proxy inverso Nginx</span><span class="sxs-lookup"><span data-stu-id="d9ca4-243">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="d9ca4-244">Una vez procesada por Nginx, una solicitud de la `Accept-Encoding` se quita el encabezado.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-244">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="d9ca4-245">Esto evita que el middleware de la compresión de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-245">This prevents the middleware from compressing the response.</span></span> <span data-ttu-id="d9ca4-246">Para obtener más información, consulte [NGINX: compresión y descompresión](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="d9ca4-246">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="d9ca4-247">Este problema se realiza un seguimiento por [averiguar compresión de paso a través de Nginx (BasicMiddleware n.º 123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="d9ca4-247">This issue is tracked by [Figure out pass-through compression for Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="d9ca4-248">Trabajar con la compresión dinámica de IIS</span><span class="sxs-lookup"><span data-stu-id="d9ca4-248">Working with IIS dynamic compression</span></span>

<span data-ttu-id="d9ca4-249">Si tiene un módulo de compresión dinámica de IIS activo configurado en el nivel de servidor que desea deshabilitar para una aplicación, puede hacerlo con un complemento de la *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-249">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, you can do so with an addition to your *web.config* file.</span></span> <span data-ttu-id="d9ca4-250">Para más información, vea [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Deshabilitación de módulos de IIS).</span><span class="sxs-lookup"><span data-stu-id="d9ca4-250">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="d9ca4-251">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="d9ca4-251">Troubleshooting</span></span>

<span data-ttu-id="d9ca4-252">Utilice una herramienta como [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), o [Postman](https://www.getpostman.com/), que permite establecer el `Accept-Encoding` encabezado de solicitud y estudiar los encabezados de respuesta, el tamaño y el cuerpo.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-252">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="d9ca4-253">El Middleware de compresión de respuesta comprime las respuestas que cumplen las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="d9ca4-253">The Response Compression Middleware compresses responses that meet the following conditions:</span></span>

* <span data-ttu-id="d9ca4-254">El `Accept-Encoding` encabezado está presente con un valor de `gzip`, `*`, o codificación personalizada que coincida con un proveedor de compresión personalizada que ha establecido.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-254">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="d9ca4-255">El valor no debe ser `identity` o tener un valor de calidad (qvalue, `q`) de 0 (cero).</span><span class="sxs-lookup"><span data-stu-id="d9ca4-255">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="d9ca4-256">El tipo MIME (`Content-Type`) debe establecerse y debe coincidir con un tipo MIME configurado en el [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).</span><span class="sxs-lookup"><span data-stu-id="d9ca4-256">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).</span></span>
* <span data-ttu-id="d9ca4-257">La solicitud no debe incluir el `Content-Range` encabezado.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-257">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="d9ca4-258">La solicitud debe utilizar inseguro protocolo (http), a menos que el protocolo seguro (https) se configura en las opciones de Middleware de compresión de respuesta.</span><span class="sxs-lookup"><span data-stu-id="d9ca4-258">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="d9ca4-259">*Tenga en cuenta el peligro [se ha descrito anteriormente](#compression-with-secure-protocol) al habilitar la compresión de contenido segura.*</span><span class="sxs-lookup"><span data-stu-id="d9ca4-259">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d9ca4-260">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d9ca4-260">Additional resources</span></span>

* [<span data-ttu-id="d9ca4-261">Inicio de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="d9ca4-261">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="d9ca4-262">Middleware</span><span class="sxs-lookup"><span data-stu-id="d9ca4-262">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="d9ca4-263">Red de desarrollador de Mozilla: Codificación aceptada</span><span class="sxs-lookup"><span data-stu-id="d9ca4-263">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="d9ca4-264">RFC 7231 sección 3.1.2.1: Códigos contenidos</span><span class="sxs-lookup"><span data-stu-id="d9ca4-264">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="d9ca4-265">RFC 7230 sección 4.2.3: Codificación Gzip</span><span class="sxs-lookup"><span data-stu-id="d9ca4-265">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="d9ca4-266">Versión 4.3 de la especificación de formato del archivo GZIP</span><span class="sxs-lookup"><span data-stu-id="d9ca4-266">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
