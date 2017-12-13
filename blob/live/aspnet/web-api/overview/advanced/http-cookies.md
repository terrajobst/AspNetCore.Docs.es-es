---
uid: web-api/overview/advanced/http-cookies
title: Las Cookies HTTP en ASP.NET Web API | Documentos de Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: e17c51946a268aa13ec035d18dc516928c9f4419
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="beb90-102">Cookies HTTP en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="beb90-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="beb90-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="beb90-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="beb90-104">En este tema se describe cómo enviar y recibir las cookies HTTP en Web API.</span><span class="sxs-lookup"><span data-stu-id="beb90-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="beb90-105">Información general sobre las Cookies HTTP</span><span class="sxs-lookup"><span data-stu-id="beb90-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="beb90-106">Esta sección ofrece una breve descripción de cómo se implementan las cookies en el nivel HTTP.</span><span class="sxs-lookup"><span data-stu-id="beb90-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="beb90-107">Para obtener más información, consulte [6265 RFC](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="beb90-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="beb90-108">Una cookie es un fragmento de datos que envía un servidor en la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="beb90-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="beb90-109">(Opcionalmente) el cliente almacena la cookie y lo devuelve en las solicitudes de subsequet.</span><span class="sxs-lookup"><span data-stu-id="beb90-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="beb90-110">Esto permite que el cliente y el servidor compartir el estado.</span><span class="sxs-lookup"><span data-stu-id="beb90-110">This allows the client and server to share state.</span></span> <span data-ttu-id="beb90-111">Para establecer una cookie, el servidor incluye un encabezado Set-Cookie en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="beb90-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="beb90-112">El formato de una cookie es un par nombre-valor, con los atributos opcionales.</span><span class="sxs-lookup"><span data-stu-id="beb90-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="beb90-113">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="beb90-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="beb90-114">Este es un ejemplo con atributos:</span><span class="sxs-lookup"><span data-stu-id="beb90-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="beb90-115">Para devolver una cookie en el servidor, el cliente contiene un encabezado de Cookie en las solicitudes posteriores.</span><span class="sxs-lookup"><span data-stu-id="beb90-115">To return a cookie to the server, the client inclues a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="beb90-116">Una respuesta HTTP puede incluir varios encabezados de Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="beb90-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="beb90-117">El cliente devuelve varias cookies con un único encabezado de Cookie.</span><span class="sxs-lookup"><span data-stu-id="beb90-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="beb90-118">El ámbito y la duración de una cookie se controlan mediante los atributos siguientes en el encabezado Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="beb90-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="beb90-119">**Dominio**: indica al cliente qué dominio debe recibir la cookie.</span><span class="sxs-lookup"><span data-stu-id="beb90-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="beb90-120">Por ejemplo, si el dominio es "ejemplo.com", el cliente devuelve la cookie para cada subdominio de example.com. Si no se especifica, el dominio es el servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="beb90-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com. If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="beb90-121">**Ruta de acceso**: restringe la cookie a la ruta de acceso especificada dentro del dominio.</span><span class="sxs-lookup"><span data-stu-id="beb90-121">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="beb90-122">Si no se especifica, se utiliza la ruta de acceso del URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="beb90-122">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="beb90-123">**Expira**: establece la fecha de expiración de la cookie.</span><span class="sxs-lookup"><span data-stu-id="beb90-123">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="beb90-124">El cliente elimina la cookie cuando expire.</span><span class="sxs-lookup"><span data-stu-id="beb90-124">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="beb90-125">**Max-Age**: establece la duración máxima de la cookie.</span><span class="sxs-lookup"><span data-stu-id="beb90-125">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="beb90-126">El cliente elimina la cookie cuando llega la antigüedad máxima.</span><span class="sxs-lookup"><span data-stu-id="beb90-126">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="beb90-127">Si ambos `Expires` y `Max-Age` se establecen, `Max-Age` tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="beb90-127">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="beb90-128">Si no está establecida, el cliente elimina la cookie cuando finaliza la sesión actual.</span><span class="sxs-lookup"><span data-stu-id="beb90-128">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="beb90-129">(El significado exacto de "sesión" viene determinado por el agente de usuario).</span><span class="sxs-lookup"><span data-stu-id="beb90-129">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="beb90-130">Sin embargo, tenga en cuenta que los clientes pueden pasar por alto las cookies.</span><span class="sxs-lookup"><span data-stu-id="beb90-130">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="beb90-131">Por ejemplo, un usuario podría deshabilitar las cookies por razones de privacidad.</span><span class="sxs-lookup"><span data-stu-id="beb90-131">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="beb90-132">Los clientes pueden eliminar las cookies antes de que caduquen o limitan el número de cookies almacenadas.</span><span class="sxs-lookup"><span data-stu-id="beb90-132">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="beb90-133">Por motivos de privacidad, los clientes a menudo rechazan cookies "third party", donde el dominio no coincide con el servidor de origen.</span><span class="sxs-lookup"><span data-stu-id="beb90-133">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="beb90-134">En resumen, el servidor no debe confiar en recibir las cookies que establece.</span><span class="sxs-lookup"><span data-stu-id="beb90-134">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="beb90-135">Cookies en API Web</span><span class="sxs-lookup"><span data-stu-id="beb90-135">Cookies in Web API</span></span>

<span data-ttu-id="beb90-136">Para agregar una cookie a una respuesta HTTP, cree un **CookieHeaderValue** instancia que representa la cookie.</span><span class="sxs-lookup"><span data-stu-id="beb90-136">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="beb90-137">A continuación, llame a la **AddCookies** método de extensión, que se define en el **System.Net.Http. HttpResponseHeadersExtensions** (clase), para agregar la cookie.</span><span class="sxs-lookup"><span data-stu-id="beb90-137">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="beb90-138">Por ejemplo, el código siguiente agrega una cookie dentro de una acción de controlador:</span><span class="sxs-lookup"><span data-stu-id="beb90-138">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="beb90-139">Tenga en cuenta que **AddCookies** toma una matriz de **CookieHeaderValue** instancias.</span><span class="sxs-lookup"><span data-stu-id="beb90-139">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="beb90-140">Para extraer las cookies de una solicitud de cliente, llame a la **GetCookies** método:</span><span class="sxs-lookup"><span data-stu-id="beb90-140">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="beb90-141">A **CookieHeaderValue** contiene una colección de **CookieState** instancias.</span><span class="sxs-lookup"><span data-stu-id="beb90-141">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="beb90-142">Cada **CookieState** representa una cookie.</span><span class="sxs-lookup"><span data-stu-id="beb90-142">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="beb90-143">Utilice el método de indizador para obtener un **CookieState** por su nombre, tal como se muestra.</span><span class="sxs-lookup"><span data-stu-id="beb90-143">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="beb90-144">Datos de la Cookie estructurado</span><span class="sxs-lookup"><span data-stu-id="beb90-144">Structured Cookie Data</span></span>

<span data-ttu-id="beb90-145">Muchos exploradores limitan número de cookies que almacenará &#8212; tanto el número total y el número por dominio.</span><span class="sxs-lookup"><span data-stu-id="beb90-145">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="beb90-146">Por lo tanto, puede ser útil colocar datos estructurados en una sola cookie, en lugar de establecer varias cookies.</span><span class="sxs-lookup"><span data-stu-id="beb90-146">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="beb90-147">RFC 6265 no define la estructura de datos de la cookie.</span><span class="sxs-lookup"><span data-stu-id="beb90-147">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="beb90-148">Mediante el **CookieHeaderValue** (clase), puede pasar una lista de pares nombre / valor para los datos de la cookie.</span><span class="sxs-lookup"><span data-stu-id="beb90-148">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="beb90-149">Estos pares de nombre-valor se codifican como datos de formulario con codificación URL en el encabezado Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="beb90-149">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="beb90-150">El código anterior produce el siguiente encabezado Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="beb90-150">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="beb90-151">El **CookieState** clase proporciona un método de indizador para leer los valores dentro de una cookie en el mensaje de solicitud:</span><span class="sxs-lookup"><span data-stu-id="beb90-151">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="beb90-152">Ejemplo: Establecer y recuperar las Cookies en un controlador de mensajes</span><span class="sxs-lookup"><span data-stu-id="beb90-152">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="beb90-153">Los ejemplos anteriores se ha explicado cómo usar las cookies desde dentro de un controlador de Web API.</span><span class="sxs-lookup"><span data-stu-id="beb90-153">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="beb90-154">Otra opción consiste en usar [controladores de mensajes](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="beb90-154">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="beb90-155">Controladores de mensajes se invocan anteriormente en la canalización de controladores.</span><span class="sxs-lookup"><span data-stu-id="beb90-155">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="beb90-156">Un controlador de mensajes puede leer las cookies de la solicitud antes de que la solicitud llegue el controlador o agregar cookies a la respuesta después de que el controlador genere la respuesta.</span><span class="sxs-lookup"><span data-stu-id="beb90-156">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="beb90-157">El código siguiente muestra un controlador de mensajes para la creación de identificadores de sesión.</span><span class="sxs-lookup"><span data-stu-id="beb90-157">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="beb90-158">El identificador de sesión se almacena en una cookie.</span><span class="sxs-lookup"><span data-stu-id="beb90-158">The session ID is stored in a cookie.</span></span> <span data-ttu-id="beb90-159">El controlador comprueba la solicitud para la cookie de sesión.</span><span class="sxs-lookup"><span data-stu-id="beb90-159">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="beb90-160">Si la solicitud no incluye la cookie, el controlador genera un identificador de sesión nuevo.</span><span class="sxs-lookup"><span data-stu-id="beb90-160">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="beb90-161">En cualquier caso, el controlador almacena el identificador de sesión en la **HttpRequestMessage.Properties** bolsa de propiedades.</span><span class="sxs-lookup"><span data-stu-id="beb90-161">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="beb90-162">También agrega la cookie de sesión a la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="beb90-162">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="beb90-163">Esta implementación no valida que el identificador de sesión desde el cliente realmente fue emitido por el servidor.</span><span class="sxs-lookup"><span data-stu-id="beb90-163">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="beb90-164">No lo use como una forma de autenticación.</span><span class="sxs-lookup"><span data-stu-id="beb90-164">Don't use it as a form of authentication!</span></span> <span data-ttu-id="beb90-165">El punto del ejemplo es mostrar la administración de cookies HTTP.</span><span class="sxs-lookup"><span data-stu-id="beb90-165">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="beb90-166">Un controlador puede obtener el identificador de sesión desde el **HttpRequestMessage.Properties** bolsa de propiedades.</span><span class="sxs-lookup"><span data-stu-id="beb90-166">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
