---
title: "Estado de sesión y la aplicación de ASP.NET Core"
author: rick-anderson
description: "Enfoques para la conservación de aplicación y el estado de usuario (sesión) entre las solicitudes."
keywords: "Núcleo de ASP.NET, estado de la aplicación, el estado de sesión, la cadena de consulta, registrar"
ms.author: riande
manager: wpickett
ms.date: 10/08/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f9c1d10101d23e105c4a8af41d851f69b1b6a175
ms.sourcegitcommit: 9c27fa0f0c57ad611aa43f63afb9b9c9571d4a94
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="c8204-104">Introducción al estado de sesión y la aplicación de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8204-104">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="c8204-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), y [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="c8204-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="c8204-106">HTTP es un protocolo sin estado.</span><span class="sxs-lookup"><span data-stu-id="c8204-106">HTTP is a stateless protocol.</span></span> <span data-ttu-id="c8204-107">Un servidor web trata cada solicitud HTTP como una solicitud independiente y no retiene los valores de usuario de las solicitudes anteriores.</span><span class="sxs-lookup"><span data-stu-id="c8204-107">A web server treats each HTTP request as an independent request and does not retain user values from previous requests.</span></span> <span data-ttu-id="c8204-108">Este artículo describe diferentes maneras de mantener la aplicación y el estado de sesión entre las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="c8204-108">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="c8204-109">Estado de sesión</span><span class="sxs-lookup"><span data-stu-id="c8204-109">Session state</span></span>

<span data-ttu-id="c8204-110">El estado de sesión es una característica de ASP.NET Core que se puede usar para guardar y almacenar datos de usuario mientras el usuario explora la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="c8204-110">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="c8204-111">Estado de sesión que consta de una diccionario o tabla hash en el servidor, conserva los datos de todas las solicitudes desde un explorador.</span><span class="sxs-lookup"><span data-stu-id="c8204-111">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="c8204-112">Los datos de sesión está respaldados por una memoria caché.</span><span class="sxs-lookup"><span data-stu-id="c8204-112">The session data is backed by a cache.</span></span>

<span data-ttu-id="c8204-113">ASP.NET Core mantiene el estado de sesión proporcionando una cookie que contiene el identificador de sesión, que se envía al servidor con cada solicitud de cliente.</span><span class="sxs-lookup"><span data-stu-id="c8204-113">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="c8204-114">El servidor utiliza el identificador de sesión para capturar los datos de sesión.</span><span class="sxs-lookup"><span data-stu-id="c8204-114">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="c8204-115">Dado que la cookie de sesión es específica del explorador, no puede compartir las sesiones entre los exploradores.</span><span class="sxs-lookup"><span data-stu-id="c8204-115">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="c8204-116">Las cookies de sesión se eliminan cuando finaliza la sesión del explorador.</span><span class="sxs-lookup"><span data-stu-id="c8204-116">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="c8204-117">Si se recibe una cookie de una sesión que ha expirado, se crea una nueva sesión que utiliza la misma cookie de sesión.</span><span class="sxs-lookup"><span data-stu-id="c8204-117">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="c8204-118">El servidor conserva una sesión durante un tiempo limitado después de la última solicitud.</span><span class="sxs-lookup"><span data-stu-id="c8204-118">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="c8204-119">Puede establecer el tiempo de espera de sesión o usar el valor predeterminado de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="c8204-119">You can either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="c8204-120">Estado de sesión es ideal para almacenar datos de usuario que es específico para una sesión determinada, pero no necesitan que se deben conservar de forma permanente.</span><span class="sxs-lookup"><span data-stu-id="c8204-120">Session state is ideal for storing user data that is specific to a particular session but doesn’t need to be persisted permanently.</span></span> <span data-ttu-id="c8204-121">Datos se eliminan de la memoria auxiliar o cuando se llama a `Session.Clear` o cuando expira la sesión en el almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="c8204-121">Data is deleted from the backing store either when you call `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="c8204-122">El servidor no conoce cuando se cierra el explorador o cuando se elimina la cookie de sesión.</span><span class="sxs-lookup"><span data-stu-id="c8204-122">The server does not know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="c8204-123">No almacene datos confidenciales en la sesión.</span><span class="sxs-lookup"><span data-stu-id="c8204-123">Do not store sensitive data in session.</span></span> <span data-ttu-id="c8204-124">El cliente no puede cerrar el explorador y desactive la cookie de sesión (y algunos exploradores mantener las cookies de sesión activa a través de windows).</span><span class="sxs-lookup"><span data-stu-id="c8204-124">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="c8204-125">Además, una sesión podría no estar restringida a un único usuario; el siguiente usuario puede continuar con la misma sesión.</span><span class="sxs-lookup"><span data-stu-id="c8204-125">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="c8204-126">El proveedor de sesión en memoria almacena datos de la sesión en el servidor local.</span><span class="sxs-lookup"><span data-stu-id="c8204-126">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="c8204-127">Si tiene previsto ejecutar la aplicación web en una granja de servidores, debe usar sesiones permanentes para asociar cada sesión en un servidor específico.</span><span class="sxs-lookup"><span data-stu-id="c8204-127">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="c8204-128">El valor predeterminado de la plataforma de sitios Web Windows Azure es sesiones permanentes (enrutamiento de solicitud de aplicación o ARR).</span><span class="sxs-lookup"><span data-stu-id="c8204-128">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="c8204-129">Sin embargo, sesiones permanentes pueden afectar a la escalabilidad y complicar las actualizaciones de aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="c8204-129">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="c8204-130">Una mejor opción consiste en usar Redis o distribuidas de SQL Server almacena en caché, que no requieren sesiones permanentes.</span><span class="sxs-lookup"><span data-stu-id="c8204-130">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="c8204-131">Para obtener más información, consulte [trabajar con una memoria caché distribuida](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="c8204-131">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="c8204-132">Para obtener más información sobre cómo configurar los proveedores de servicios, consulte [configuración de sesión](#configuring-session) más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="c8204-132">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>


<a name="temp"></a>
## <a name="tempdata"></a><span data-ttu-id="c8204-133">TempData</span><span class="sxs-lookup"><span data-stu-id="c8204-133">TempData</span></span>

<span data-ttu-id="c8204-134">MVC de ASP.NET Core expone la [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) propiedad en un [controlador](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="c8204-134">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="c8204-135">Esta propiedad almacena datos hasta que se leen.</span><span class="sxs-lookup"><span data-stu-id="c8204-135">This property stores data until it is read.</span></span> <span data-ttu-id="c8204-136">Los métodos `Keep` y `Peek` se pueden usar para examinar los datos sin que se eliminen.</span><span class="sxs-lookup"><span data-stu-id="c8204-136">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="c8204-137">`TempData`es especialmente útil para la redirección cuando se necesitan datos de más de una única solicitud.</span><span class="sxs-lookup"><span data-stu-id="c8204-137">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="c8204-138">`TempData`se implementa por los proveedores de TempData, por ejemplo, mediante las cookies o estado de sesión.</span><span class="sxs-lookup"><span data-stu-id="c8204-138">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="c8204-139">Proveedores de TempData</span><span class="sxs-lookup"><span data-stu-id="c8204-139">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c8204-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c8204-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c8204-141">En el núcleo de ASP.NET 2.0 y versiones posteriores, el proveedor TempData basado en cookie se utiliza de forma predeterminada para almacenar TempData en cookies.</span><span class="sxs-lookup"><span data-stu-id="c8204-141">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="c8204-142">Los datos de la cookie se codifican con el [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="c8204-142">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span></span> <span data-ttu-id="c8204-143">Dado que la cookie se cifra y fragmentada, se encuentra en ASP.NET Core 1.x no se aplica el límite de tamaño de la cookie única.</span><span class="sxs-lookup"><span data-stu-id="c8204-143">Because the cookie is encrypted and chunked, the single cookie size limit found in ASP.NET Core 1.x does not apply.</span></span> <span data-ttu-id="c8204-144">Los datos de la cookie no se comprimieron porque la compresión de datos de encryped puede provocar problemas de seguridad como la [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) y [infracción](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataques.</span><span class="sxs-lookup"><span data-stu-id="c8204-144">The cookie data is not compressed because compressing encryped data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="c8204-145">Para obtener más información sobre el proveedor TempData basado en cookies, consulte [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="c8204-145">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c8204-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c8204-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c8204-147">En ASP.NET Core 1.0 y 1.1, el proveedor TempData de estado de sesión es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="c8204-147">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

--------------

### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="c8204-148">Elegir un proveedor TempData</span><span class="sxs-lookup"><span data-stu-id="c8204-148">Choosing a TempData provider</span></span>

<span data-ttu-id="c8204-149">Elegir un proveedor TempData implica varias consideraciones, como:</span><span class="sxs-lookup"><span data-stu-id="c8204-149">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="c8204-150">¿La aplicación ya usa el estado de sesión para otros fines?</span><span class="sxs-lookup"><span data-stu-id="c8204-150">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="c8204-151">Si es así, mediante el proveedor TempData de estado de sesión no tiene costo adicional a la aplicación (excepto en el tamaño de los datos).</span><span class="sxs-lookup"><span data-stu-id="c8204-151">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="c8204-152">¿La aplicación usa el solo con moderación de TempData para cantidades relativamente pequeñas de datos (hasta 500 bytes)?</span><span class="sxs-lookup"><span data-stu-id="c8204-152">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="c8204-153">Si por lo tanto, el proveedor TempData de cookie agregará un pequeño costo para cada solicitud que transporta TempData.</span><span class="sxs-lookup"><span data-stu-id="c8204-153">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="c8204-154">De lo contrario, el proveedor TempData de estado de sesión puede ser beneficioso para evitar una gran cantidad de datos en cada solicitud de ida y vuelta hasta que se consume el TempData.</span><span class="sxs-lookup"><span data-stu-id="c8204-154">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="c8204-155">¿La aplicación se ejecuta en una granja de servidores web (varios servidores)?</span><span class="sxs-lookup"><span data-stu-id="c8204-155">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="c8204-156">Si es así, no hay ninguna configuración adicional necesaria para usar el proveedor TempData de cookie.</span><span class="sxs-lookup"><span data-stu-id="c8204-156">If so, there is no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="c8204-157">La mayoría de los clientes de web (por ejemplo, los exploradores web) aplican límites en el tamaño máximo de cada cookie, el número total de cookies o ambos.</span><span class="sxs-lookup"><span data-stu-id="c8204-157">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="c8204-158">Por lo tanto, cuando se usa el proveedor TempData de cookie, compruebe que la aplicación no supera estos límites.</span><span class="sxs-lookup"><span data-stu-id="c8204-158">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="c8204-159">Tenga en cuenta el tamaño total de los datos, teniendo en cuenta las sobrecargas de cifrado y fragmentación.</span><span class="sxs-lookup"><span data-stu-id="c8204-159">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

<span data-ttu-id="c8204-160">Para configurar el proveedor TempData para una aplicación, registrar una implementación de proveedor TempData en `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c8204-160">To configure the TempData provider for an application, register a TempData provider implementation in `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddMvc()
        .AddSessionStateTempDataProvider();

    // The Session State TempData Provider requires adding the session state service
    services.AddSession();
}
```

## <a name="query-strings"></a><span data-ttu-id="c8204-161">Cadenas de consulta</span><span class="sxs-lookup"><span data-stu-id="c8204-161">Query strings</span></span>

<span data-ttu-id="c8204-162">Puede pasar una cantidad limitada de datos de una solicitud a otro, éste se agrega a la cadena de consulta de la solicitud nuevo.</span><span class="sxs-lookup"><span data-stu-id="c8204-162">You can pass a limited amount of data from one request to another by adding it to the new request’s query string.</span></span> <span data-ttu-id="c8204-163">Esto es útil para capturar el estado de forma persistente que permite vínculos de estado incrustada para compartirse a través de correo electrónico o redes sociales.</span><span class="sxs-lookup"><span data-stu-id="c8204-163">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="c8204-164">Sin embargo, por esta razón, nunca debe usar las cadenas de consulta para datos confidenciales.</span><span class="sxs-lookup"><span data-stu-id="c8204-164">However, for this reason,  you should never use query strings for sensitive data.</span></span> <span data-ttu-id="c8204-165">Además de que se va a compartir con facilidad, incluidos los datos de las cadenas de consulta pueden crear oportunidades para [falsificación de solicitud entre sitios (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) ataques, que pueden engañar a los usuarios visitar sitios malintencionados mientras autenticado.</span><span class="sxs-lookup"><span data-stu-id="c8204-165">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="c8204-166">Los atacantes, a continuación, pueden robar datos de usuario de la aplicación o realizar acciones malintencionadas en nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="c8204-166">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="c8204-167">Cualquier estado conservado de application o session debe protegerse de ataques CSRF.</span><span class="sxs-lookup"><span data-stu-id="c8204-167">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="c8204-168">Para obtener más información sobre los ataques CSRF, consulte [evitar Cross-Site falsificación de solicitud (XSRF/CSRF) ataques en ASP.NET Core](../security/anti-request-forgery.md).</span><span class="sxs-lookup"><span data-stu-id="c8204-168">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="c8204-169">Datos POST y los campos ocultos</span><span class="sxs-lookup"><span data-stu-id="c8204-169">Post data and hidden fields</span></span>

<span data-ttu-id="c8204-170">Datos pueden guardarse en campos ocultos de formulario y registrados en la siguiente solicitud.</span><span class="sxs-lookup"><span data-stu-id="c8204-170">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="c8204-171">Esto es habitual en los formularios de varias páginas.</span><span class="sxs-lookup"><span data-stu-id="c8204-171">This is common in multipage forms.</span></span> <span data-ttu-id="c8204-172">Sin embargo, dado que el cliente potencialmente puede alterar los datos, el servidor debe siempre revalidación.</span><span class="sxs-lookup"><span data-stu-id="c8204-172">However, because the  client can potentially tamper with the data, the server must always revalidate it.</span></span> 

## <a name="cookies"></a><span data-ttu-id="c8204-173">Cookies</span><span class="sxs-lookup"><span data-stu-id="c8204-173">Cookies</span></span>

<span data-ttu-id="c8204-174">Las cookies proporcionan una manera de almacenar datos específicos del usuario en aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="c8204-174">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="c8204-175">Dado que las cookies se envían con cada solicitud, su tamaño debe reducirse al mínimo.</span><span class="sxs-lookup"><span data-stu-id="c8204-175">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="c8204-176">Idealmente, solo un identificador debe almacenarse en una cookie con los datos almacenados en el servidor.</span><span class="sxs-lookup"><span data-stu-id="c8204-176">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="c8204-177">Mayoría de los exploradores restringir las cookies de 4096 bytes.</span><span class="sxs-lookup"><span data-stu-id="c8204-177">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="c8204-178">Además, solo un número limitado de las cookies está disponible para cada dominio.</span><span class="sxs-lookup"><span data-stu-id="c8204-178">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="c8204-179">Dado que las cookies están expuestas a alteraciones, deben validarse en el servidor.</span><span class="sxs-lookup"><span data-stu-id="c8204-179">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="c8204-180">Aunque la duración de la cookie en un cliente está sujeto a expiración y la intervención del usuario, generalmente son el formulario más duradero para la persistencia de los datos en el cliente.</span><span class="sxs-lookup"><span data-stu-id="c8204-180">Although the durability of the cookie on a client is subject to user intervention and expiration, they are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="c8204-181">A menudo se usan cookies para personalización, donde el contenido se personaliza para un usuario conocido.</span><span class="sxs-lookup"><span data-stu-id="c8204-181">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="c8204-182">Dado que el usuario se identifica únicamente y no autenticado en la mayoría de los casos, normalmente puede proteger una cookie al almacenar el nombre de usuario, nombre de cuenta o un identificador de usuario único (por ejemplo, un GUID) en la cookie.</span><span class="sxs-lookup"><span data-stu-id="c8204-182">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="c8204-183">A continuación, puede usar la cookie para tener acceso a la infraestructura de personalización de usuario de un sitio.</span><span class="sxs-lookup"><span data-stu-id="c8204-183">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="c8204-184">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="c8204-184">HttpContext.Items</span></span>

<span data-ttu-id="c8204-185">El `Items` colección es un buen lugar para almacenar los datos que es solo necesarios mientras el procesamiento de una solicitud determinada.</span><span class="sxs-lookup"><span data-stu-id="c8204-185">The `Items` collection is a good location to store data that is needed only while processing one particular request.</span></span> <span data-ttu-id="c8204-186">Contenido de la colección se descarta después de cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="c8204-186">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="c8204-187">El `Items` colección mejor se usa como una manera de componentes o de middleware para comunicarse cuando operar en distintos puntos en el tiempo durante una solicitud y no pueden pasar parámetros de forma directa.</span><span class="sxs-lookup"><span data-stu-id="c8204-187">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="c8204-188">Para obtener más información, consulte [trabajar con HttpContext.Items](#working-with-httpcontextitems), más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="c8204-188">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="c8204-189">instancias y claves</span><span class="sxs-lookup"><span data-stu-id="c8204-189">Cache</span></span>

<span data-ttu-id="c8204-190">Almacenamiento en caché es una manera eficaz para almacenar y recuperar datos.</span><span class="sxs-lookup"><span data-stu-id="c8204-190">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="c8204-191">Puede controlar la duración de los elementos almacenados en caché en función de tiempo y otras consideraciones.</span><span class="sxs-lookup"><span data-stu-id="c8204-191">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="c8204-192">Obtenga más información sobre [almacenamiento en caché](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="c8204-192">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name=session></a>
## <a name="working-with-session-state"></a><span data-ttu-id="c8204-193">Trabajar con el estado de sesión</span><span class="sxs-lookup"><span data-stu-id="c8204-193">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="c8204-194">Configuración de sesión</span><span class="sxs-lookup"><span data-stu-id="c8204-194">Configuring Session</span></span>

<span data-ttu-id="c8204-195">El `Microsoft.AspNetCore.Session` paquete proporciona middleware para administrar el estado de sesión.</span><span class="sxs-lookup"><span data-stu-id="c8204-195">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="c8204-196">Para habilitar el middleware de sesión, `Startup`debe contener:</span><span class="sxs-lookup"><span data-stu-id="c8204-196">To enable the session middleware, `Startup`must contain:</span></span>

- <span data-ttu-id="c8204-197">Cualquiera de los [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) cachés de la memoria.</span><span class="sxs-lookup"><span data-stu-id="c8204-197">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="c8204-198">El `IDistributedCache` implementación se usa como una memoria auxiliar para la sesión.</span><span class="sxs-lookup"><span data-stu-id="c8204-198">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="c8204-199">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) llamar a, lo que requiere el paquete NuGet "Microsoft.AspNetCore.Session".</span><span class="sxs-lookup"><span data-stu-id="c8204-199">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="c8204-200">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) llamar.</span><span class="sxs-lookup"><span data-stu-id="c8204-200">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="c8204-201">El código siguiente muestra cómo configurar el proveedor de sesión en memoria.</span><span class="sxs-lookup"><span data-stu-id="c8204-201">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c8204-202">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c8204-202">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c8204-203">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c8204-203">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="c8204-204">Puede hacer referencia a la sesión de `HttpContext` una vez que está instalado y configurado.</span><span class="sxs-lookup"><span data-stu-id="c8204-204">You can reference Session from `HttpContext` once it is installed and configured.</span></span>

<span data-ttu-id="c8204-205">Si se intenta acceder a `Session` antes de `UseSession` se ha llamado, la excepción `InvalidOperationException: Session has not been configured for this application or request` se produce.</span><span class="sxs-lookup"><span data-stu-id="c8204-205">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="c8204-206">Si intenta crear un nuevo `Session` (es decir, no se ha creado ninguna cookie de sesión) después de que ya ha empezado a escribir en el `Response` transmitir por secuencias, la excepción `InvalidOperationException: The session cannot be established after the response has started` se produce.</span><span class="sxs-lookup"><span data-stu-id="c8204-206">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="c8204-207">La excepción puede encontrarse en el registro de servidor web; no se mostrará en el explorador.</span><span class="sxs-lookup"><span data-stu-id="c8204-207">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="c8204-208">Carga de sesión de forma asincrónica</span><span class="sxs-lookup"><span data-stu-id="c8204-208">Loading Session asynchronously</span></span> 

<span data-ttu-id="c8204-209">El proveedor de sesión predeterminado de ASP.NET Core carga el registro de sesión de subyacente [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) almacén asincrónicamente solo si la [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) método se llama explícitamente antes de  el `TryGetValue`, `Set`, o `Remove` métodos.</span><span class="sxs-lookup"><span data-stu-id="c8204-209">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="c8204-210">Si `LoadAsync` no se llama en primer lugar, subyacente registro de sesión se carga de forma sincrónica, que podrían tener consecuencias sobre la capacidad de ampliación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c8204-210">If `LoadAsync` is not called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="c8204-211">Para que las aplicaciones aplicar este patrón, ajustar el [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) y [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementaciones con versiones que producen una excepción si el `LoadAsync` método no es llamado antes de `TryGetValue`, `Set`, o `Remove`.</span><span class="sxs-lookup"><span data-stu-id="c8204-211">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method is not called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="c8204-212">Registrar las versiones ajustadas en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="c8204-212">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="c8204-213">Detalles de implementación</span><span class="sxs-lookup"><span data-stu-id="c8204-213">Implementation Details</span></span>

<span data-ttu-id="c8204-214">Sesión utiliza una cookie para realizar un seguimiento e identificar las solicitudes de un solo explorador.</span><span class="sxs-lookup"><span data-stu-id="c8204-214">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="c8204-215">De manera predeterminada, esta cookie se denomina ". AspNet.Session"y utiliza una ruta de acceso de"/".</span><span class="sxs-lookup"><span data-stu-id="c8204-215">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="c8204-216">Dado que el valor predeterminado de la cookie no especifica un dominio, no estará disponible para el script de cliente en la página (porque `CookieHttpOnly` tiene como valor predeterminado `true`).</span><span class="sxs-lookup"><span data-stu-id="c8204-216">Because the cookie default does not specify a domain, it is not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="c8204-217">Para reemplazar los valores predeterminados de la sesión, utilice `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="c8204-217">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c8204-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c8204-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c8204-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c8204-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="c8204-220">El servidor usa el `IdleTimeout` propiedad para determinar el tiempo que una sesión puede estar inactiva antes de que se abandone su contenido.</span><span class="sxs-lookup"><span data-stu-id="c8204-220">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="c8204-221">Esta propiedad es independiente de la expiración de la cookie.</span><span class="sxs-lookup"><span data-stu-id="c8204-221">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="c8204-222">Todas las solicitudes que se pasan a través del middleware de sesión (de lectura o escritura para) restablece el tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="c8204-222">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="c8204-223">Dado que `Session` es *no realiza el bloqueo*, si dos solicitudes intentan modificar el contenido de la sesión, la última de ellas sustituye a la primera.</span><span class="sxs-lookup"><span data-stu-id="c8204-223">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="c8204-224">`Session`se implementa como un *sesión coherente*, lo que significa que todo el contenido se almacena juntos.</span><span class="sxs-lookup"><span data-stu-id="c8204-224">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="c8204-225">Dos solicitudes que están modificando distintas partes de la sesión (claves diferentes) aún podrían verse afectadas entre sí.</span><span class="sxs-lookup"><span data-stu-id="c8204-225">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="setting-and-getting-session-values"></a><span data-ttu-id="c8204-226">Establecer u obtener valores de la sesión</span><span class="sxs-lookup"><span data-stu-id="c8204-226">Setting and getting Session values</span></span>

<span data-ttu-id="c8204-227">Sesión se accede a través del `Session` propiedad `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="c8204-227">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="c8204-228">Esta propiedad es una [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementación.</span><span class="sxs-lookup"><span data-stu-id="c8204-228">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="c8204-229">En el ejemplo siguiente se muestra cómo establecer y obtener un valor int y una cadena:</span><span class="sxs-lookup"><span data-stu-id="c8204-229">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="c8204-230">Si agrega los siguientes métodos de extensión, puede establecer y obtener objetos serializables en sesión:</span><span class="sxs-lookup"><span data-stu-id="c8204-230">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="c8204-231">El ejemplo siguiente muestra cómo establecer y obtener un objeto serializable:</span><span class="sxs-lookup"><span data-stu-id="c8204-231">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="c8204-232">Trabajar con HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="c8204-232">Working with HttpContext.Items</span></span>

<span data-ttu-id="c8204-233">El `HttpContext` abstracción proporciona compatibilidad para una colección de diccionario de tipo `IDictionary<object, object>`, llamado `Items`.</span><span class="sxs-lookup"><span data-stu-id="c8204-233">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="c8204-234">Esta colección está disponible desde el principio de un *HttpRequest* y se descarta al final de cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="c8204-234">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="c8204-235">Puede tener acceso a asignar un valor a una entrada con clave, o solicitando el valor de una clave determinada.</span><span class="sxs-lookup"><span data-stu-id="c8204-235">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="c8204-236">En el ejemplo siguiente, [Middleware](middleware.md) agrega `isVerified` a la `Items` colección.</span><span class="sxs-lookup"><span data-stu-id="c8204-236">In the sample below, [Middleware](middleware.md) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="c8204-237">Más adelante en la canalización, middleware otro podría acceder a él:</span><span class="sxs-lookup"><span data-stu-id="c8204-237">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="c8204-238">Para el middleware que solo se usará en una única aplicación, `string` claves son aceptables.</span><span class="sxs-lookup"><span data-stu-id="c8204-238">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="c8204-239">Sin embargo, middleware que se compartirá entre las aplicaciones debe usar claves de objeto único para evitar cualquier posibilidad de colisiones de clave.</span><span class="sxs-lookup"><span data-stu-id="c8204-239">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="c8204-240">Si está desarrollando middleware que debe trabajar en varias aplicaciones, utilice una clave de objeto único definida en la clase de middleware tal y como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="c8204-240">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

<span data-ttu-id="c8204-241">Otro código puede tener acceso al valor almacenado en `HttpContext.Items` con la clave que expone la clase de middleware:</span><span class="sxs-lookup"><span data-stu-id="c8204-241">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="c8204-242">Este enfoque también tiene la ventaja de eliminar la repetición de cadenas"mágicas" en varios lugares en el código.</span><span class="sxs-lookup"><span data-stu-id="c8204-242">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name=appstate-errors></a>

## <a name="application-state-data"></a><span data-ttu-id="c8204-243">Datos de estado de aplicación</span><span class="sxs-lookup"><span data-stu-id="c8204-243">Application state data</span></span>

<span data-ttu-id="c8204-244">Use [inyección de dependencia](xref:fundamentals/dependency-injection) para que los datos estén disponibles para todos los usuarios:</span><span class="sxs-lookup"><span data-stu-id="c8204-244">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="c8204-245">Definir un servicio que contiene los datos (por ejemplo, una clase denominada `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="c8204-245">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="c8204-246">Agregue la clase de servicio a `ConfigureServices` (por ejemplo `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="c8204-246">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="c8204-247">Utilizar la clase de servicio de datos en cada controlador:</span><span class="sxs-lookup"><span data-stu-id="c8204-247">Consume the data service class in each controller:</span></span>

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

### <a name="common-errors-when-working-with-session"></a><span data-ttu-id="c8204-248">Errores comunes al trabajar con sesión</span><span class="sxs-lookup"><span data-stu-id="c8204-248">Common errors when working with session</span></span>

* <span data-ttu-id="c8204-249">"No se puede resolver el servicio para el tipo 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' al intentar activar 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span><span class="sxs-lookup"><span data-stu-id="c8204-249">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="c8204-250">Esto puede deberse a que si no se configura al menos una `IDistributedCache` implementación.</span><span class="sxs-lookup"><span data-stu-id="c8204-250">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="c8204-251">Para obtener más información, consulte [trabajar con una memoria caché distribuida](xref:performance/caching/distributed) y [en la memoria caché](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="c8204-251">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

### <a name="additional-resources"></a><span data-ttu-id="c8204-252">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c8204-252">Additional Resources</span></span>


* [<span data-ttu-id="c8204-253">ASP.NET Core 1.x: ejemplo de código que se emplea en este documento</span><span class="sxs-lookup"><span data-stu-id="c8204-253">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="c8204-254">ASP.NET Core 2.x: ejemplo de código que se emplea en este documento</span><span class="sxs-lookup"><span data-stu-id="c8204-254">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
