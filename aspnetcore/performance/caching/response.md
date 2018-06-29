---
title: Las respuestas en caché de ASP.NET Core
author: rick-anderson
description: Aprenda a usar el almacenamiento en caché a menores requisitos de ancho de banda de respuesta y aumentar el rendimiento de las aplicaciones de ASP.NET Core.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: c53ae3f6ab8d26588533772dd4fdacb36ec12059
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077769"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="40bf9-103">Las respuestas en caché de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40bf9-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="40bf9-104">Por [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="40bf9-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="40bf9-105">Las respuestas en caché en las páginas de Razor está disponible en ASP.NET Core 2.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="40bf9-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="40bf9-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="40bf9-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="40bf9-107">Las respuestas en caché reducen el número de solicitudes de que un cliente o proxy se realiza en un servidor web.</span><span class="sxs-lookup"><span data-stu-id="40bf9-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="40bf9-108">Las respuestas en caché también reducen la cantidad de trabajo realiza el servidor web para generar una respuesta.</span><span class="sxs-lookup"><span data-stu-id="40bf9-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="40bf9-109">Las respuestas en caché se controlan mediante encabezados que especifican cómo desea que cliente, el proxy y middleware para la memoria caché las respuestas.</span><span class="sxs-lookup"><span data-stu-id="40bf9-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="40bf9-110">El servidor web puede almacenar en caché las respuestas al agregar [Middleware de almacenamiento en caché de respuesta](xref:performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="40bf9-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="40bf9-111">Almacenamiento en caché de respuesta basado en HTTP</span><span class="sxs-lookup"><span data-stu-id="40bf9-111">HTTP-based response caching</span></span>

<span data-ttu-id="40bf9-112">El [especificación HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234) describe el comportamiento de las cachés de Internet.</span><span class="sxs-lookup"><span data-stu-id="40bf9-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="40bf9-113">El encabezado HTTP principal que se usa para almacenar en caché es [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), que se utiliza para especificar la caché *directivas*.</span><span class="sxs-lookup"><span data-stu-id="40bf9-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="40bf9-114">Las directivas de controlan el comportamiento de almacenamiento en caché las solicitudes lleguen su desde los clientes a los servidores a medida que y respuestas lleguen su de servidores a los clientes.</span><span class="sxs-lookup"><span data-stu-id="40bf9-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="40bf9-115">Mover las solicitudes y respuestas a través de servidores proxy y servidores proxy deben también ajustarse a la especificación HTTP 1.1 almacenamiento en memoria caché.</span><span class="sxs-lookup"><span data-stu-id="40bf9-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="40bf9-116">Common `Cache-Control` directivas se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="40bf9-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="40bf9-117">Directiva</span><span class="sxs-lookup"><span data-stu-id="40bf9-117">Directive</span></span>                                                       | <span data-ttu-id="40bf9-118">Acción</span><span class="sxs-lookup"><span data-stu-id="40bf9-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="40bf9-119">public</span><span class="sxs-lookup"><span data-stu-id="40bf9-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="40bf9-120">Una memoria caché puede almacenar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="40bf9-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="40bf9-121">private</span><span class="sxs-lookup"><span data-stu-id="40bf9-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="40bf9-122">No se debe almacenar la respuesta de una memoria caché compartida.</span><span class="sxs-lookup"><span data-stu-id="40bf9-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="40bf9-123">Una caché privada puede almacenar y reutilizar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="40bf9-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="40bf9-124">max-age</span><span class="sxs-lookup"><span data-stu-id="40bf9-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="40bf9-125">El cliente no aceptará una respuesta cuya antigüedad es superior al número especificado de segundos.</span><span class="sxs-lookup"><span data-stu-id="40bf9-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="40bf9-126">Ejemplos: `max-age=60` (60 segundos), `max-age=2592000` (1 mes)</span><span class="sxs-lookup"><span data-stu-id="40bf9-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="40bf9-127">sin caché</span><span class="sxs-lookup"><span data-stu-id="40bf9-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="40bf9-128">**En las solicitudes**: una memoria caché no debe usar una respuesta almacenada para satisfacer la solicitud.</span><span class="sxs-lookup"><span data-stu-id="40bf9-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="40bf9-129">Nota: El servidor de origen vuelve a genera la respuesta para el cliente y el software intermedio actualiza la respuesta almacenada en la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="40bf9-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="40bf9-130">**En las respuestas**: la respuesta no debe usarse para una solicitud posterior sin la validación en el servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="40bf9-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="40bf9-131">ningún almacén</span><span class="sxs-lookup"><span data-stu-id="40bf9-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="40bf9-132">**En las solicitudes**: una memoria caché no debe almacenar la solicitud.</span><span class="sxs-lookup"><span data-stu-id="40bf9-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="40bf9-133">**En las respuestas**: una memoria caché no debe almacenar cualquier parte de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="40bf9-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="40bf9-134">Otros encabezados de caché que desempeñan un papel en el almacenamiento en caché se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="40bf9-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="40bf9-135">Header</span><span class="sxs-lookup"><span data-stu-id="40bf9-135">Header</span></span>                                                     | <span data-ttu-id="40bf9-136">Función</span><span class="sxs-lookup"><span data-stu-id="40bf9-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="40bf9-137">Edad</span><span class="sxs-lookup"><span data-stu-id="40bf9-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="40bf9-138">Una estimación de la cantidad de tiempo en segundos desde que se generó la respuesta o se ha validado correctamente en el servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="40bf9-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="40bf9-139">Expira</span><span class="sxs-lookup"><span data-stu-id="40bf9-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="40bf9-140">La fecha y hora después de que la respuesta se considera obsoleta.</span><span class="sxs-lookup"><span data-stu-id="40bf9-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="40bf9-141">Pragma</span><span class="sxs-lookup"><span data-stu-id="40bf9-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="40bf9-142">Existe para hacia atrás compatibilidad con HTTP/1.0 almacena en memoria caché de configuración `no-cache` comportamiento.</span><span class="sxs-lookup"><span data-stu-id="40bf9-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="40bf9-143">Si el `Cache-Control` encabezado está presente, el `Pragma` se omite el encabezado.</span><span class="sxs-lookup"><span data-stu-id="40bf9-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="40bf9-144">Variar</span><span class="sxs-lookup"><span data-stu-id="40bf9-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="40bf9-145">Especifica que una respuesta almacenada en caché no se debe enviar a menos que todos los de la `Vary` coinciden con campos de encabezado de solicitud original de la respuesta almacenada en caché y la solicitud nuevo.</span><span class="sxs-lookup"><span data-stu-id="40bf9-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="40bf9-146">Las directivas de Control de caché de solicitud de aspectos de almacenamiento en caché basado en HTTP</span><span class="sxs-lookup"><span data-stu-id="40bf9-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="40bf9-147">El [especificación HTTP 1.1 almacenamiento en memoria caché para el encabezado Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) requiere una memoria caché que se respeten válido `Cache-Control` encabezado enviado por el cliente.</span><span class="sxs-lookup"><span data-stu-id="40bf9-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="40bf9-148">Un cliente puede realizar las solicitudes con un `no-cache` valor del encabezado y se fuerza el servidor para generar una nueva respuesta para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="40bf9-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="40bf9-149">Siempre respetando cliente `Cache-Control` encabezados de solicitud tiene sentido si considera que el objetivo del almacenamiento en caché de HTTP.</span><span class="sxs-lookup"><span data-stu-id="40bf9-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="40bf9-150">En la especificación oficial, el almacenamiento en caché está pensado para reducir la sobrecarga de red y latencia de satisfacer las solicitudes a través de una red de clientes, servidores proxy y servidores.</span><span class="sxs-lookup"><span data-stu-id="40bf9-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="40bf9-151">No es necesariamente una manera de controlar la carga en un servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="40bf9-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="40bf9-152">No hay ningún control del desarrollador actual sobre este comportamiento de almacenamiento en caché cuando se usa el [Middleware de almacenamiento en caché de respuesta](xref:performance/caching/middleware) porque el middleware se adhiere a los oficiales especificación el almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="40bf9-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="40bf9-153">[Mejoras futuras en el middleware](https://github.com/aspnet/ResponseCaching/issues/96) permitirá configurar el middleware para omitir una solicitud `Cache-Control` encabezado al decidir atender una respuesta almacenada en caché.</span><span class="sxs-lookup"><span data-stu-id="40bf9-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="40bf9-154">Esto le ofrece la oportunidad para controlar mejor la carga en el servidor cuando se usa el middleware.</span><span class="sxs-lookup"><span data-stu-id="40bf9-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="40bf9-155">Otras tecnologías de almacenamiento en caché de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40bf9-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="40bf9-156">Almacenamiento en caché en memoria</span><span class="sxs-lookup"><span data-stu-id="40bf9-156">In-memory caching</span></span>

<span data-ttu-id="40bf9-157">Almacenamiento en caché en memoria utiliza memoria del servidor para almacenar los datos almacenados en caché.</span><span class="sxs-lookup"><span data-stu-id="40bf9-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="40bf9-158">Este tipo de almacenamiento en caché es adecuado para un único servidor o varios servidores mediante *sesiones permanentes*.</span><span class="sxs-lookup"><span data-stu-id="40bf9-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="40bf9-159">Sesiones permanentes significa que siempre se enrutan las solicitudes de un cliente en el mismo servidor para su procesamiento.</span><span class="sxs-lookup"><span data-stu-id="40bf9-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="40bf9-160">Para obtener más información, consulte [almacenar en memoria caché en memoria](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="40bf9-160">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="40bf9-161">Caché distribuida</span><span class="sxs-lookup"><span data-stu-id="40bf9-161">Distributed Cache</span></span>

<span data-ttu-id="40bf9-162">Usar una memoria caché distribuida para almacenar datos en la memoria cuando la aplicación se hospeda en una granja de servidores en la nube o el servidor.</span><span class="sxs-lookup"><span data-stu-id="40bf9-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="40bf9-163">La memoria caché se comparte entre los servidores que procesan las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="40bf9-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="40bf9-164">Un cliente puede enviar una solicitud que controla cualquier servidor en el grupo si los datos almacenados en caché para el cliente están disponibles.</span><span class="sxs-lookup"><span data-stu-id="40bf9-164">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="40bf9-165">ASP.NET Core ofrece SQL Server y las cachés de Redis distribuida.</span><span class="sxs-lookup"><span data-stu-id="40bf9-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="40bf9-166">Para obtener más información, consulte [trabajar con una memoria caché distribuida](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="40bf9-166">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="40bf9-167">Aplicación auxiliar de etiqueta de caché</span><span class="sxs-lookup"><span data-stu-id="40bf9-167">Cache Tag Helper</span></span>

<span data-ttu-id="40bf9-168">Puede almacenar en caché el contenido de una vista MVC o Razor página a la aplicación auxiliar de etiqueta de caché.</span><span class="sxs-lookup"><span data-stu-id="40bf9-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="40bf9-169">La aplicación auxiliar de etiqueta de caché usa almacenamiento en caché en memoria para almacenar datos.</span><span class="sxs-lookup"><span data-stu-id="40bf9-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="40bf9-170">Para obtener más información, consulte [aplicación auxiliar de la etiqueta de caché en MVC de ASP.NET Core](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="40bf9-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="40bf9-171">Aplicación auxiliar de etiquetas de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="40bf9-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="40bf9-172">Puede almacenar en caché el contenido de una vista MVC o la página de Razor en nube distribuida o escenarios de granja de servidores web con el Ayudante de etiqueta de caché distribuida.</span><span class="sxs-lookup"><span data-stu-id="40bf9-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="40bf9-173">El Ayudante de etiqueta de caché distribuida usa SQL Server o Redis para almacenar datos.</span><span class="sxs-lookup"><span data-stu-id="40bf9-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="40bf9-174">Para obtener más información, consulte [auxiliar de etiqueta de caché distribuida](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="40bf9-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="40bf9-175">Atributo ResponseCache</span><span class="sxs-lookup"><span data-stu-id="40bf9-175">ResponseCache attribute</span></span>

<span data-ttu-id="40bf9-176">El [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) especifica los parámetros necesarios para establecer encabezados apropiados en las respuestas en caché.</span><span class="sxs-lookup"><span data-stu-id="40bf9-176">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="40bf9-177">Deshabilitar el almacenamiento en caché para el contenido que contiene información para clientes autenticados.</span><span class="sxs-lookup"><span data-stu-id="40bf9-177">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="40bf9-178">Sólo debe habilitarse el almacenamiento en caché para el contenido que no cambia en función de la identidad de un usuario o si un usuario ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="40bf9-178">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="40bf9-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varía la respuesta almacenada en los valores de la lista de claves de consulta determinada.</span><span class="sxs-lookup"><span data-stu-id="40bf9-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="40bf9-180">Cuando un valor único de `*` es siempre el middleware varía las respuestas por todos los parámetros de cadena de consulta de solicitud.</span><span class="sxs-lookup"><span data-stu-id="40bf9-180">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="40bf9-181">`VaryByQueryKeys` requiere ASP.NET Core 1.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="40bf9-181">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="40bf9-182">El Middleware de almacenamiento en caché de la respuesta debe estar habilitado para establecer el `VaryByQueryKeys` propiedad; en caso contrario, se produce una excepción en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="40bf9-182">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="40bf9-183">No hay un encabezado HTTP correspondiente para el `VaryByQueryKeys` propiedad.</span><span class="sxs-lookup"><span data-stu-id="40bf9-183">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="40bf9-184">La propiedad es una característica HTTP controlada el Middleware de almacenamiento en caché de respuesta.</span><span class="sxs-lookup"><span data-stu-id="40bf9-184">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="40bf9-185">Para que el middleware atender una respuesta almacenada en caché, la cadena de consulta y el valor de cadena de consulta deben coincidir con una solicitud anterior.</span><span class="sxs-lookup"><span data-stu-id="40bf9-185">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="40bf9-186">Por ejemplo, considere la posibilidad de la secuencia de las solicitudes y resultados que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="40bf9-186">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="40bf9-187">Solicitud</span><span class="sxs-lookup"><span data-stu-id="40bf9-187">Request</span></span>                          | <span data-ttu-id="40bf9-188">Resultado</span><span class="sxs-lookup"><span data-stu-id="40bf9-188">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="40bf9-189">Devuelta por el servidor</span><span class="sxs-lookup"><span data-stu-id="40bf9-189">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="40bf9-190">Procedentes de middleware</span><span class="sxs-lookup"><span data-stu-id="40bf9-190">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="40bf9-191">Devuelta por el servidor</span><span class="sxs-lookup"><span data-stu-id="40bf9-191">Returned from server</span></span>     |

<span data-ttu-id="40bf9-192">La primera solicitud es devuelto por el servidor y en memoria caché de middleware.</span><span class="sxs-lookup"><span data-stu-id="40bf9-192">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="40bf9-193">Dado que la cadena de consulta coincide con la solicitud anterior, se devuelve la segunda solicitud middleware.</span><span class="sxs-lookup"><span data-stu-id="40bf9-193">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="40bf9-194">La tercera solicitud no se encuentra en la caché de middleware porque el valor de cadena de consulta no coincide con una solicitud anterior.</span><span class="sxs-lookup"><span data-stu-id="40bf9-194">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="40bf9-195">El `ResponseCacheAttribute` se utiliza para crear y configurar (a través de `IFilterFactory`) un [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="40bf9-195">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="40bf9-196">El `ResponseCacheFilter` realiza el trabajo de actualización de los encabezados HTTP adecuados y características de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="40bf9-196">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="40bf9-197">El filtro:</span><span class="sxs-lookup"><span data-stu-id="40bf9-197">The filter:</span></span>

* <span data-ttu-id="40bf9-198">Quita los encabezados existentes para `Vary`, `Cache-Control`, y `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="40bf9-198">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="40bf9-199">Escribe los encabezados adecuados en función de las propiedades establecidas en el `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="40bf9-199">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="40bf9-200">Actualiza la respuesta si el almacenamiento en caché característica HTTP `VaryByQueryKeys` se establece.</span><span class="sxs-lookup"><span data-stu-id="40bf9-200">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="40bf9-201">Variar</span><span class="sxs-lookup"><span data-stu-id="40bf9-201">Vary</span></span>

<span data-ttu-id="40bf9-202">Este encabezado solo cuando se escribe el `VaryByHeader` se establece la propiedad.</span><span class="sxs-lookup"><span data-stu-id="40bf9-202">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="40bf9-203">Se establece en el `Vary` valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="40bf9-203">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="40bf9-204">El siguiente ejemplo se utiliza la `VaryByHeader` propiedad:</span><span class="sxs-lookup"><span data-stu-id="40bf9-204">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="40bf9-205">Puede ver los encabezados de respuesta con las herramientas de red de su explorador.</span><span class="sxs-lookup"><span data-stu-id="40bf9-205">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="40bf9-206">La siguiente imagen muestra la F12 de borde de salida en el **red** ficha cuando el `About2` se actualiza el método de acción:</span><span class="sxs-lookup"><span data-stu-id="40bf9-206">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![Arista F12 salida en la pestaña de la red cuando se llama al método de acción About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="40bf9-208">NoStore y Location.None</span><span class="sxs-lookup"><span data-stu-id="40bf9-208">NoStore and Location.None</span></span>

<span data-ttu-id="40bf9-209">`NoStore` invalida la mayoría de las demás propiedades.</span><span class="sxs-lookup"><span data-stu-id="40bf9-209">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="40bf9-210">Cuando esta propiedad se establece en `true`, `Cache-Control` encabezado se establece en `no-store`.</span><span class="sxs-lookup"><span data-stu-id="40bf9-210">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="40bf9-211">Si `Location` está establecido en `None`:</span><span class="sxs-lookup"><span data-stu-id="40bf9-211">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="40bf9-212">El valor de `Cache-Control` está establecido en `no-store,no-cache`.</span><span class="sxs-lookup"><span data-stu-id="40bf9-212">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="40bf9-213">El valor de `Pragma` está establecido en `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="40bf9-213">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="40bf9-214">Si `NoStore` es `false` y `Location` es `None`, `Cache-Control` y `Pragma` se establecen en `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="40bf9-214">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="40bf9-215">Normalmente establece `NoStore` a `true` en las páginas de error.</span><span class="sxs-lookup"><span data-stu-id="40bf9-215">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="40bf9-216">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="40bf9-216">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="40bf9-217">Esto genera los encabezados siguientes:</span><span class="sxs-lookup"><span data-stu-id="40bf9-217">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="40bf9-218">Ubicación y la duración</span><span class="sxs-lookup"><span data-stu-id="40bf9-218">Location and Duration</span></span>

<span data-ttu-id="40bf9-219">Para habilitar el almacenamiento en caché, `Duration` debe establecerse en un valor positivo y `Location` debe ser `Any` (valor predeterminado) o `Client`.</span><span class="sxs-lookup"><span data-stu-id="40bf9-219">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="40bf9-220">En este caso, el `Cache-Control` encabezado se establece en el valor de ubicación seguido por el `max-age` de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="40bf9-220">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="40bf9-221">`Location`de opciones de `Any` y `Client` traducir `Cache-Control` valores de encabezado `public` y `private`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="40bf9-221">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="40bf9-222">Como se indicó anteriormente, establecer `Location` a `None` establece ambas `Cache-Control` y `Pragma` encabezados para `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="40bf9-222">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="40bf9-223">A continuación se muestra un ejemplo que muestra los encabezados genera estableciendo `Duration` y deja el valor predeterminado `Location` valor:</span><span class="sxs-lookup"><span data-stu-id="40bf9-223">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="40bf9-224">Esto produce el siguiente encabezado:</span><span class="sxs-lookup"><span data-stu-id="40bf9-224">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="40bf9-225">Perfiles de memoria caché</span><span class="sxs-lookup"><span data-stu-id="40bf9-225">Cache profiles</span></span>

<span data-ttu-id="40bf9-226">En lugar de duplicar `ResponseCache` configuración en muchos atributos de acción de controlador, perfiles de memoria caché se puede configurar como opciones al configurar MVC en la `ConfigureServices` método `Startup`.</span><span class="sxs-lookup"><span data-stu-id="40bf9-226">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="40bf9-227">Valores que se encuentran en un perfil de caché que se hace referencia se utilizan como los valores predeterminados por el `ResponseCache` de atributo y se reemplazan por las propiedades especificadas en el atributo.</span><span class="sxs-lookup"><span data-stu-id="40bf9-227">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="40bf9-228">Cómo configurar un perfil de caché:</span><span class="sxs-lookup"><span data-stu-id="40bf9-228">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="40bf9-229">Hacer referencia a un perfil de caché:</span><span class="sxs-lookup"><span data-stu-id="40bf9-229">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="40bf9-230">El `ResponseCache` atributo se puede aplicar tanto a las acciones (métodos) y controladores (clases).</span><span class="sxs-lookup"><span data-stu-id="40bf9-230">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="40bf9-231">Atributos de nivel de método invalidan la configuración especificada en los atributos de nivel de clase.</span><span class="sxs-lookup"><span data-stu-id="40bf9-231">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="40bf9-232">En el ejemplo anterior, un atributo de nivel de clase especifica una duración de 30 segundos, mientras que un atributo de nivel de método hace referencia a un perfil de caché con una duración establecida en 60 segundos.</span><span class="sxs-lookup"><span data-stu-id="40bf9-232">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="40bf9-233">El encabezado resultante:</span><span class="sxs-lookup"><span data-stu-id="40bf9-233">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="40bf9-234">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="40bf9-234">Additional resources</span></span>

* [<span data-ttu-id="40bf9-235">Almacenar las respuestas en las cachés</span><span class="sxs-lookup"><span data-stu-id="40bf9-235">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="40bf9-236">Control de caché</span><span class="sxs-lookup"><span data-stu-id="40bf9-236">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="40bf9-237">Almacenamiento en caché en memoria</span><span class="sxs-lookup"><span data-stu-id="40bf9-237">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="40bf9-238">Trabajar con una memoria caché distribuida</span><span class="sxs-lookup"><span data-stu-id="40bf9-238">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="40bf9-239">Detectar cambios con tokens de cambio</span><span class="sxs-lookup"><span data-stu-id="40bf9-239">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="40bf9-240">Middleware de almacenamiento en caché de respuestas</span><span class="sxs-lookup"><span data-stu-id="40bf9-240">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="40bf9-241">Aplicación auxiliar de etiquetas de caché</span><span class="sxs-lookup"><span data-stu-id="40bf9-241">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="40bf9-242">Aplicación auxiliar de etiquetas de caché distribuida</span><span class="sxs-lookup"><span data-stu-id="40bf9-242">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
