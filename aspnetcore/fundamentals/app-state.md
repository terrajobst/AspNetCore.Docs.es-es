---
title: Estado de sesión y aplicación en ASP.NET Core
author: rick-anderson
description: Detecte enfoques para conservar el estado de sesión y aplicación entre las solicitudes.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: 7794b3c10e26720d3e7ef8965f99b204a3c58d5c
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "41870939"
---
# <a name="session-and-app-state-in-aspnet-core"></a><span data-ttu-id="332b1-103">Estado de sesión y aplicación en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="332b1-103">Session and app state in ASP.NET Core</span></span>

<span data-ttu-id="332b1-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="332b1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="332b1-105">HTTP es un protocolo sin estado.</span><span class="sxs-lookup"><span data-stu-id="332b1-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="332b1-106">Sin realizar pasos adicionales, las solicitudes HTTP son mensajes independientes que no conservan los valores de usuario ni el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="332b1-106">Without taking additional steps, HTTP requests are independent messages that don't retain user values or app state.</span></span> <span data-ttu-id="332b1-107">En este artículo se describen varios enfoques para conservar el estado de la aplicación y los datos de usuario entre las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="332b1-107">This article describes several approaches to preserve user data and app state between requests.</span></span>

<span data-ttu-id="332b1-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="332b1-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="state-management"></a><span data-ttu-id="332b1-109">Administración de estado</span><span class="sxs-lookup"><span data-stu-id="332b1-109">State management</span></span>

<span data-ttu-id="332b1-110">El estado se puede almacenar mediante varios enfoques.</span><span class="sxs-lookup"><span data-stu-id="332b1-110">State can be stored using several approaches.</span></span> <span data-ttu-id="332b1-111">Cada enfoque se describe más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="332b1-111">Each approach is described later in this topic.</span></span>

| <span data-ttu-id="332b1-112">Enfoque de almacenamiento</span><span class="sxs-lookup"><span data-stu-id="332b1-112">Storage approach</span></span> | <span data-ttu-id="332b1-113">Mecanismo de almacenamiento</span><span class="sxs-lookup"><span data-stu-id="332b1-113">Storage mechanism</span></span> |
| ---------------- | ----------------- |
| [<span data-ttu-id="332b1-114">Cookies</span><span class="sxs-lookup"><span data-stu-id="332b1-114">Cookies</span></span>](#cookies) | <span data-ttu-id="332b1-115">Cookies HTTP (pueden incluir los datos almacenados mediante el código de aplicación del lado servidor)</span><span class="sxs-lookup"><span data-stu-id="332b1-115">HTTP cookies (may include data stored using server-side app code)</span></span> |
| [<span data-ttu-id="332b1-116">Estado de sesión</span><span class="sxs-lookup"><span data-stu-id="332b1-116">Session state</span></span>](#session-state) | <span data-ttu-id="332b1-117">Cookies HTTP y código de aplicación del lado servidor</span><span class="sxs-lookup"><span data-stu-id="332b1-117">HTTP cookies and server-side app code</span></span> |
| [<span data-ttu-id="332b1-118">TempData</span><span class="sxs-lookup"><span data-stu-id="332b1-118">TempData</span></span>](#tempdata) | <span data-ttu-id="332b1-119">Cookies HTTP o estado de sesión</span><span class="sxs-lookup"><span data-stu-id="332b1-119">HTTP cookies or session state</span></span> |
| [<span data-ttu-id="332b1-120">Cadenas de consulta</span><span class="sxs-lookup"><span data-stu-id="332b1-120">Query strings</span></span>](#query-strings) | <span data-ttu-id="332b1-121">Cadenas de consulta HTTP</span><span class="sxs-lookup"><span data-stu-id="332b1-121">HTTP query strings</span></span> |
| [<span data-ttu-id="332b1-122">Campos ocultos</span><span class="sxs-lookup"><span data-stu-id="332b1-122">Hidden fields</span></span>](#hidden-fields) | <span data-ttu-id="332b1-123">Campos de formularios HTTP</span><span class="sxs-lookup"><span data-stu-id="332b1-123">HTTP form fields</span></span> |
| [<span data-ttu-id="332b1-124">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="332b1-124">HttpContext.Items</span></span>](#httpcontextitems) | <span data-ttu-id="332b1-125">Código de aplicación del lado servidor</span><span class="sxs-lookup"><span data-stu-id="332b1-125">Server-side app code</span></span> |
| [<span data-ttu-id="332b1-126">Caché</span><span class="sxs-lookup"><span data-stu-id="332b1-126">Cache</span></span>](#cache) | <span data-ttu-id="332b1-127">Código de aplicación del lado servidor</span><span class="sxs-lookup"><span data-stu-id="332b1-127">Server-side app code</span></span> |
| [<span data-ttu-id="332b1-128">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="332b1-128">Dependency Injection</span></span>](#dependency-injection) | <span data-ttu-id="332b1-129">Código de aplicación del lado servidor</span><span class="sxs-lookup"><span data-stu-id="332b1-129">Server-side app code</span></span> |

## <a name="cookies"></a><span data-ttu-id="332b1-130">Cookies</span><span class="sxs-lookup"><span data-stu-id="332b1-130">Cookies</span></span>

<span data-ttu-id="332b1-131">Las cookies almacenan datos de todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="332b1-131">Cookies store data across requests.</span></span> <span data-ttu-id="332b1-132">Dado que las cookies se envían con cada solicitud, su tamaño debe reducirse al mínimo.</span><span class="sxs-lookup"><span data-stu-id="332b1-132">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="332b1-133">Lo ideal es que en cada cookie se almacene un solo identificador con los datos almacenados por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="332b1-133">Ideally, only an identifier should be stored in a cookie with the data stored by the app.</span></span> <span data-ttu-id="332b1-134">La mayoría de los exploradores restringen el tamaño de las cookies a 4096 bytes.</span><span class="sxs-lookup"><span data-stu-id="332b1-134">Most browsers restrict cookie size to 4096 bytes.</span></span> <span data-ttu-id="332b1-135">Solo hay disponible un número limitado de cookies para cada dominio.</span><span class="sxs-lookup"><span data-stu-id="332b1-135">Only a limited number of cookies are available for each domain.</span></span>

<span data-ttu-id="332b1-136">Como las cookies están expuestas a alteraciones, deben validarse por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="332b1-136">Because cookies are subject to tampering, they must be validated by the app.</span></span> <span data-ttu-id="332b1-137">Los usuarios pueden eliminar las cookies y estas pueden caducar en los clientes.</span><span class="sxs-lookup"><span data-stu-id="332b1-137">Cookies can be deleted by users and expire on clients.</span></span> <span data-ttu-id="332b1-138">Pero las cookies generalmente son la forma más duradera de persistencia de datos en el cliente.</span><span class="sxs-lookup"><span data-stu-id="332b1-138">However, cookies are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="332b1-139">Las cookies suelen utilizarse para personalizar el contenido ofrecido a un usuario conocido.</span><span class="sxs-lookup"><span data-stu-id="332b1-139">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="332b1-140">En la mayoría de los casos, el usuario solo se identifica y no se autentica.</span><span class="sxs-lookup"><span data-stu-id="332b1-140">The user is only identified and not authenticated in most cases.</span></span> <span data-ttu-id="332b1-141">La cookie puede almacenar el nombre de usuario, el nombre de cuenta o el id. de usuario único (por ejemplo, un GUID).</span><span class="sxs-lookup"><span data-stu-id="332b1-141">The cookie can store the user's name, account name, or unique user ID (such as a GUID).</span></span> <span data-ttu-id="332b1-142">Después, puede usar la cookie para tener acceso a la configuración personalizada del usuario, como su color de fondo del sitio web preferido.</span><span class="sxs-lookup"><span data-stu-id="332b1-142">You can then use the cookie to access the user's personalized settings, such as their preferred website background color.</span></span>

<span data-ttu-id="332b1-143">Preste atención al [reglamento general de protección de datos (GDPR) de la Unión Europea](https://ec.europa.eu/info/law/law-topic/data-protection) cuando emita cookies y trate con casos de privacidad.</span><span class="sxs-lookup"><span data-stu-id="332b1-143">Be mindful of the [European Union General Data Protection Regulations (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) when issuing cookies and dealing with privacy concerns.</span></span> <span data-ttu-id="332b1-144">Para obtener más información, vea [Compatibilidad con el Reglamento general de protección de datos (RGPD) en ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="332b1-144">For more information, see [General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr).</span></span>

## <a name="session-state"></a><span data-ttu-id="332b1-145">Estado de sesión</span><span class="sxs-lookup"><span data-stu-id="332b1-145">Session state</span></span>

<span data-ttu-id="332b1-146">El estado de sesión es un escenario de ASP.NET Core para el almacenamiento de datos de usuario mientras el usuario examina una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="332b1-146">Session state is an ASP.NET Core scenario for storage of user data while the user browses a web app.</span></span> <span data-ttu-id="332b1-147">El estado de sesión usa un almacén mantenido por la aplicación para conservar los datos en las solicitudes de un cliente.</span><span class="sxs-lookup"><span data-stu-id="332b1-147">Session state uses a store maintained by the app to persist data across requests from a client.</span></span> <span data-ttu-id="332b1-148">Los datos de sesión están respaldados por una memoria caché y se consideran datos efímeros y el sitio debería continuar funcionando correctamente sin los datos de sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-148">The session data is backed by a cache and considered ephemeral data&mdash;the site should continue to function without the session data.</span></span>

> [!NOTE]
> <span data-ttu-id="332b1-149">La sesión no es compatible con aplicaciones [SignalR](xref:signalr/index) porque un [concentrador SignalR](xref:signalr/hubs) podría ejecutarse independientemente de un contexto HTTP.</span><span class="sxs-lookup"><span data-stu-id="332b1-149">Session isn't supported in [SignalR](xref:signalr/index) apps because a [SignalR Hub](xref:signalr/hubs) may execute independent of an HTTP context.</span></span> <span data-ttu-id="332b1-150">Por ejemplo, esto puede ocurrir cuando un concentrador mantiene abierta una solicitud de sondeo larga más allá de la duración del contexto HTTP de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="332b1-150">For example, this can occur when a long polling request is held open by a hub beyond the lifetime of the request's HTTP context.</span></span>

<span data-ttu-id="332b1-151">Para mantener el estado de sesión, ASP.NET Core proporciona una cookie al cliente que contiene un identificador de sesión, que se envía a la aplicación con cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="332b1-151">ASP.NET Core maintains session state by providing a cookie to the client that contains a session ID, which is sent to the app with each request.</span></span> <span data-ttu-id="332b1-152">La aplicación usa el identificador de sesión para capturar los datos de sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-152">The app uses the session ID to fetch the session data.</span></span>

<span data-ttu-id="332b1-153">El estado de sesión muestra los siguientes comportamientos:</span><span class="sxs-lookup"><span data-stu-id="332b1-153">Session state exhibits the following behaviors:</span></span>

* <span data-ttu-id="332b1-154">Dado que la cookie de sesión es específica del explorador, las sesiones no se comparten entre los exploradores.</span><span class="sxs-lookup"><span data-stu-id="332b1-154">Because the session cookie is specific to the browser, sessions aren't shared across browsers.</span></span>
* <span data-ttu-id="332b1-155">Las cookies de sesión se eliminan cuando finaliza la sesión del explorador.</span><span class="sxs-lookup"><span data-stu-id="332b1-155">Session cookies are deleted when the browser session ends.</span></span>
* <span data-ttu-id="332b1-156">Si se recibe una cookie de una sesión que ha expirado, se crea una nueva sesión que usa la misma cookie de sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-156">If a cookie is received for an expired session, a new session is created that uses the same session cookie.</span></span>
* <span data-ttu-id="332b1-157">Las sesiones vacías no se mantienen y la sesión debe tener, al menos un conjunto de valores en ella para que se conserve entre solicitudes.</span><span class="sxs-lookup"><span data-stu-id="332b1-157">Empty sessions aren't retained&mdash;the session must have at least one value set into it to persist the session across requests.</span></span> <span data-ttu-id="332b1-158">Cuando una sesión no se conserva, se genera un nuevo identificador de sesión para cada nueva solicitud.</span><span class="sxs-lookup"><span data-stu-id="332b1-158">When a session isn't retained, a new session ID is generated for each new request.</span></span>
* <span data-ttu-id="332b1-159">La aplicación conserva una sesión durante un tiempo limitado después de la última solicitud.</span><span class="sxs-lookup"><span data-stu-id="332b1-159">The app retains a session for a limited time after the last request.</span></span> <span data-ttu-id="332b1-160">La aplicación especifica un tiempo de espera de sesión o usa el valor predeterminado de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="332b1-160">The app either sets the session timeout or uses the default value of 20 minutes.</span></span> <span data-ttu-id="332b1-161">El estado de sesión es ideal para almacenar datos de usuario que son específicos de una sesión determinada, pero que no necesitan conservarse de forma permanente entre las sesiones.</span><span class="sxs-lookup"><span data-stu-id="332b1-161">Session state is ideal for storing user data that's specific to a particular session but where the data doesn't require permanent storage across sessions.</span></span>
* <span data-ttu-id="332b1-162">Los datos de sesión se eliminan cuando se llama a la implementación [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) o cuando expira la sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-162">Session data is deleted either when the [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementation is called or when the session expires.</span></span>
* <span data-ttu-id="332b1-163">No hay ningún mecanismo predeterminado para informar al código de aplicación que se ha cerrado un explorador del cliente o cuando la cookie de sesión se elimina o caduca en el cliente.</span><span class="sxs-lookup"><span data-stu-id="332b1-163">There's no default mechanism to inform app code that a client browser has been closed or when the session cookie is deleted or expired on the client.</span></span>

> [!WARNING]
> <span data-ttu-id="332b1-164">No almacene datos confidenciales en un estado de sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-164">Don't store sensitive data in session state.</span></span> <span data-ttu-id="332b1-165">El usuario podría no cerrar el explorador y borrar la cookie de sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-165">The user might not close the browser and clear the session cookie.</span></span> <span data-ttu-id="332b1-166">Algunos exploradores mantienen las cookies de sesión válidas en las ventanas del explorador.</span><span class="sxs-lookup"><span data-stu-id="332b1-166">Some browsers maintain valid session cookies across browser windows.</span></span> <span data-ttu-id="332b1-167">Es posible que una sesión no esté restringida a un único usuario y que el siguiente usuario continúe examinando la aplicación con la misma cookie de sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-167">A session might not be restricted to a single user&mdash;the next user might continue to browse the app with the same session cookie.</span></span>

<span data-ttu-id="332b1-168">El proveedor de caché en memoria almacena datos de sesión en la memoria del servidor donde reside la aplicación.</span><span class="sxs-lookup"><span data-stu-id="332b1-168">The in-memory cache provider stores session data in the memory of the server where the app resides.</span></span> <span data-ttu-id="332b1-169">En un escenario de granja de servidores:</span><span class="sxs-lookup"><span data-stu-id="332b1-169">In a server farm scenario:</span></span>

* <span data-ttu-id="332b1-170">Use *sesiones permanentes* para asociar cada sesión a una instancia de aplicación específica en un servidor individual.</span><span class="sxs-lookup"><span data-stu-id="332b1-170">Use *sticky sessions* to tie each session to a specific app instance on an individual server.</span></span> <span data-ttu-id="332b1-171">[Azure App Service](https://azure.microsoft.com/services/app-service/) usa [enrutamiento de solicitud de aplicaciones (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) para exigir sesiones permanentes de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="332b1-171">[Azure App Service](https://azure.microsoft.com/services/app-service/) uses [Application Request Routing (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) to enforce sticky sessions by default.</span></span> <span data-ttu-id="332b1-172">Pero las sesiones permanentes pueden afectar a la escalabilidad y complicar la actualización de las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="332b1-172">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="332b1-173">Un enfoque mejor consiste en usar una memoria caché distribuida de Redis o SQL Server, que no requiere sesiones permanentes.</span><span class="sxs-lookup"><span data-stu-id="332b1-173">A better approach is to use a Redis or SQL Server distributed cache, which doesn't require sticky sessions.</span></span> <span data-ttu-id="332b1-174">Para obtener más información, vea [Working with a Distributed Cache](xref:performance/caching/distributed) (Trabajar con una memoria caché distribuida).</span><span class="sxs-lookup"><span data-stu-id="332b1-174">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>
* <span data-ttu-id="332b1-175">La cookie de sesión se cifra mediante [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span><span class="sxs-lookup"><span data-stu-id="332b1-175">The session cookie is encrypted via [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span></span> <span data-ttu-id="332b1-176">La protección de datos debe configurarse correctamente para que lea las cookies de sesión en cada equipo.</span><span class="sxs-lookup"><span data-stu-id="332b1-176">Data Protection must be properly configured to read session cookies on each machine.</span></span> <span data-ttu-id="332b1-177">Para obtener más información, vea [Protección de datos en ASP.NET Core](xref:security/data-protection/index) y [Key storage providers](xref:security/data-protection/implementation/key-storage-providers) (Proveedores de almacenamiento de claves).</span><span class="sxs-lookup"><span data-stu-id="332b1-177">For more information, see [Data Protection in ASP.NET Core](xref:security/data-protection/index) and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

### <a name="configure-session-state"></a><span data-ttu-id="332b1-178">Configurar el estado de sesión</span><span class="sxs-lookup"><span data-stu-id="332b1-178">Configure session state</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="332b1-179">El paquete [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/), que se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), proporciona middleware para administrar el estado de sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-179">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), provides middleware for managing session state.</span></span> <span data-ttu-id="332b1-180">Para habilitar el middleware de sesión, `Startup` debe contener:</span><span class="sxs-lookup"><span data-stu-id="332b1-180">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="332b1-181">El paquete [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) proporciona middleware para administrar el estado de sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-181">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package provides middleware for managing session state.</span></span> <span data-ttu-id="332b1-182">Para habilitar el middleware de sesión, `Startup` debe contener:</span><span class="sxs-lookup"><span data-stu-id="332b1-182">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

* <span data-ttu-id="332b1-183">Cualquiera de las cachés de memoria [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache).</span><span class="sxs-lookup"><span data-stu-id="332b1-183">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="332b1-184">La implementación de `IDistributedCache` se usa como una memoria auxiliar para la sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-184">The `IDistributedCache` implementation is used as a backing store for session.</span></span> <span data-ttu-id="332b1-185">Para obtener más información, vea [Working with a Distributed Cache](xref:performance/caching/distributed) (Trabajar con una memoria caché distribuida).</span><span class="sxs-lookup"><span data-stu-id="332b1-185">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>
* <span data-ttu-id="332b1-186">Una llamada a [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="332b1-186">A call to [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) in `ConfigureServices`.</span></span>
* <span data-ttu-id="332b1-187">Una llamada a [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) en `Configure`.</span><span class="sxs-lookup"><span data-stu-id="332b1-187">A call to [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) in `Configure`.</span></span>

<span data-ttu-id="332b1-188">El código siguiente muestra cómo configurar el proveedor de sesión en memoria con una implementación en memoria de `IDistributedCache`:</span><span class="sxs-lookup"><span data-stu-id="332b1-188">The following code shows how to set up the in-memory session provider with a default in-memory implementation of `IDistributedCache`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

<span data-ttu-id="332b1-189">El orden del middleware es importante.</span><span class="sxs-lookup"><span data-stu-id="332b1-189">The order of middleware is important.</span></span> <span data-ttu-id="332b1-190">En el ejemplo anterior, se produce una excepción `InvalidOperationException` cuando `UseSession` se invoca después de `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="332b1-190">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="332b1-191">Para obtener más información vea [Ordenación de Middleware](xref:fundamentals/middleware/index#order).</span><span class="sxs-lookup"><span data-stu-id="332b1-191">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

<span data-ttu-id="332b1-192">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) está disponible después de configurar el estado de sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-192">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) is available after session state is configured.</span></span>

<span data-ttu-id="332b1-193">No se puede acceder a `HttpContext.Session` antes de llamar a `UseSession`.</span><span class="sxs-lookup"><span data-stu-id="332b1-193">`HttpContext.Session` can't be accessed before `UseSession` has been called.</span></span>

<span data-ttu-id="332b1-194">No se puede crear una nueva sesión con una cookie de sesión nueva después de que la aplicación haya empezado a escribir en la secuencia de respuesta.</span><span class="sxs-lookup"><span data-stu-id="332b1-194">A new session with a new session cookie can't be created after the app has begun writing to the response stream.</span></span> <span data-ttu-id="332b1-195">La excepción se registra en el registro del servidor web y no se muestra en el explorador.</span><span class="sxs-lookup"><span data-stu-id="332b1-195">The exception is recorded in the web server log and not displayed in the browser.</span></span>

### <a name="load-session-state-asynchronously"></a><span data-ttu-id="332b1-196">Cargar de forma asincrónica el estado de sesión</span><span class="sxs-lookup"><span data-stu-id="332b1-196">Load session state asynchronously</span></span>

<span data-ttu-id="332b1-197">El proveedor de sesión predeterminado de ASP.NET Core carga asincrónicamente los registros de sesión desde la memoria auxiliar subyacente [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) solo si se llama explícitamente al método [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) antes que a los métodos [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set) o [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove).</span><span class="sxs-lookup"><span data-stu-id="332b1-197">The default session provider in ASP.NET Core loads session records from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) backing store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) method is explicitly called before the [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set), or [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) methods.</span></span> <span data-ttu-id="332b1-198">Si primero no se llama a `LoadAsync`, el registro de sesión subyacente se carga de forma sincrónica, lo que podría conllevar una disminución del rendimiento al escalar.</span><span class="sxs-lookup"><span data-stu-id="332b1-198">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which can incur a performance penalty at scale.</span></span>

<span data-ttu-id="332b1-199">Para que las aplicaciones impongan este patrón, ajuste las implementaciones de [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) y [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) con versiones que produzcan una excepción si el método `LoadAsync` no se llama antes que `TryGetValue`, `Set` o `Remove`.</span><span class="sxs-lookup"><span data-stu-id="332b1-199">To have apps enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="332b1-200">Registre las versiones ajustadas en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="332b1-200">Register the wrapped versions in the services container.</span></span>

### <a name="session-options"></a><span data-ttu-id="332b1-201">Opciones de sesión</span><span class="sxs-lookup"><span data-stu-id="332b1-201">Session options</span></span>

<span data-ttu-id="332b1-202">Para reemplazar los valores predeterminados de la sesión, use [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span><span class="sxs-lookup"><span data-stu-id="332b1-202">To override session defaults, use [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="332b1-203">Opción</span><span class="sxs-lookup"><span data-stu-id="332b1-203">Option</span></span> | <span data-ttu-id="332b1-204">Descripción</span><span class="sxs-lookup"><span data-stu-id="332b1-204">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="332b1-205">Cookie</span><span class="sxs-lookup"><span data-stu-id="332b1-205">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | <span data-ttu-id="332b1-206">Determina la configuración usada para crear la cookie.</span><span class="sxs-lookup"><span data-stu-id="332b1-206">Determines the settings used to create the cookie.</span></span> <span data-ttu-id="332b1-207">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) tiene como valor predeterminado [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="332b1-207">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) defaults to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> <span data-ttu-id="332b1-208">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) tiene como valor predeterminado [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="332b1-208">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> <span data-ttu-id="332b1-209">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) tiene como valor predeterminado [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span><span class="sxs-lookup"><span data-stu-id="332b1-209">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) defaults to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span></span> <span data-ttu-id="332b1-210">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) tiene como valor predeterminado `true`.</span><span class="sxs-lookup"><span data-stu-id="332b1-210">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`.</span></span> <span data-ttu-id="332b1-211">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="332b1-211">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) defaults to `false`.</span></span> |
| [<span data-ttu-id="332b1-212">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="332b1-212">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="332b1-213">`IdleTimeout` indica cuánto tiempo puede estar inactiva la sesión antes de que se abandone su contenido.</span><span class="sxs-lookup"><span data-stu-id="332b1-213">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="332b1-214">Cada acceso a la sesión restablece el tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="332b1-214">Each session access resets the timeout.</span></span> <span data-ttu-id="332b1-215">Tenga en cuenta que esto solo es aplicable al contenido de la sesión, no a la cookie.</span><span class="sxs-lookup"><span data-stu-id="332b1-215">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="332b1-216">El valor predeterminado es de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="332b1-216">The default is 20 minutes.</span></span> |
| [<span data-ttu-id="332b1-217">IOTimeout</span><span class="sxs-lookup"><span data-stu-id="332b1-217">IOTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | <span data-ttu-id="332b1-218">El período máximo de tiempo permitido para cargar una sesión del almacén o devolverla a él.</span><span class="sxs-lookup"><span data-stu-id="332b1-218">The maximim amount of time allowed to load a session from the store or to commit it back to the store.</span></span> <span data-ttu-id="332b1-219">Tenga en cuenta que esto solo es aplicable a las operaciones asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="332b1-219">Note this may only apply to asynchronous operations.</span></span> <span data-ttu-id="332b1-220">Puede deshabilitar este tiempo de espera mediante [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span><span class="sxs-lookup"><span data-stu-id="332b1-220">This timeout can be disabled using [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span></span> <span data-ttu-id="332b1-221">El valor predeterminado es 1 minuto.</span><span class="sxs-lookup"><span data-stu-id="332b1-221">The default is 1 minute.</span></span> |

<span data-ttu-id="332b1-222">La sesión utiliza una cookie para realizar el seguimiento de las solicitudes emitidas por un solo explorador e identificarlas.</span><span class="sxs-lookup"><span data-stu-id="332b1-222">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="332b1-223">De manera predeterminada, esta cookie se denomina `.AspNetCore.Session` y usa una ruta de acceso de `/`.</span><span class="sxs-lookup"><span data-stu-id="332b1-223">By default, this cookie is named `.AspNetCore.Session`, and it uses a path of `/`.</span></span> <span data-ttu-id="332b1-224">Dado que el valor predeterminado de la cookie no especifica un dominio, no estará disponible para el script de cliente en la página (porque [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) tiene como valor predeterminado `true`).</span><span class="sxs-lookup"><span data-stu-id="332b1-224">Because the cookie default doesn't specify a domain, it isn't made available to the client-side script on the page (because [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="332b1-225">Opción</span><span class="sxs-lookup"><span data-stu-id="332b1-225">Option</span></span> | <span data-ttu-id="332b1-226">Descripción</span><span class="sxs-lookup"><span data-stu-id="332b1-226">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="332b1-227">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="332b1-227">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | <span data-ttu-id="332b1-228">Determina el dominio usado para crear la cookie.</span><span class="sxs-lookup"><span data-stu-id="332b1-228">Determines the domain used to create the cookie.</span></span> <span data-ttu-id="332b1-229">`CookieDomain` no se encuentra configurado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="332b1-229">`CookieDomain` isn't set by default.</span></span> |
| [<span data-ttu-id="332b1-230">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="332b1-230">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | <span data-ttu-id="332b1-231">Determina si el explorador debe permitir que la cookie tenga acceso a código JavaScript del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="332b1-231">Determines if the browser should allow the cookie to be accessed by client-side JavaScript.</span></span> <span data-ttu-id="332b1-232">El valor predeterminado es `true`, lo que significa que la cookie solo se pasa a las solicitudes HTTP y no está disponible para script en la página.</span><span class="sxs-lookup"><span data-stu-id="332b1-232">The default is `true`, which means the cookie is only passed to HTTP requests and isn't made available to script on the page.</span></span> |
| [<span data-ttu-id="332b1-233">CookieName</span><span class="sxs-lookup"><span data-stu-id="332b1-233">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | <span data-ttu-id="332b1-234">Determina el nombre de cookie que se usa para conservar el identificador de sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-234">Determines the cookie name used to persist the session ID.</span></span> <span data-ttu-id="332b1-235">El valor predeterminado es [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="332b1-235">The default is [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> |
| [<span data-ttu-id="332b1-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="332b1-236">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | <span data-ttu-id="332b1-237">Determina la ruta de acceso usada para crear la cookie.</span><span class="sxs-lookup"><span data-stu-id="332b1-237">Determines the path used to create the cookie.</span></span> <span data-ttu-id="332b1-238">Tiene como valor predeterminado [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="332b1-238">Defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> |
| [<span data-ttu-id="332b1-239">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="332b1-239">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | <span data-ttu-id="332b1-240">Determina si la cookie solo se debe transmitir en las solicitudes HTTPS.</span><span class="sxs-lookup"><span data-stu-id="332b1-240">Determines if the cookie should only be transmitted on HTTPS requests.</span></span> <span data-ttu-id="332b1-241">El valor predeterminado es [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span><span class="sxs-lookup"><span data-stu-id="332b1-241">The default is [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span></span> |
| [<span data-ttu-id="332b1-242">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="332b1-242">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="332b1-243">`IdleTimeout` indica cuánto tiempo puede estar inactiva la sesión antes de que se abandone su contenido.</span><span class="sxs-lookup"><span data-stu-id="332b1-243">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="332b1-244">Cada acceso a la sesión restablece el tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="332b1-244">Each session access resets the timeout.</span></span> <span data-ttu-id="332b1-245">Tenga en cuenta que esto solo es aplicable al contenido de la sesión, no a la cookie.</span><span class="sxs-lookup"><span data-stu-id="332b1-245">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="332b1-246">El valor predeterminado es de 20 minutos.</span><span class="sxs-lookup"><span data-stu-id="332b1-246">The default is 20 minutes.</span></span> |

<span data-ttu-id="332b1-247">La sesión utiliza una cookie para realizar el seguimiento de las solicitudes emitidas por un solo explorador e identificarlas.</span><span class="sxs-lookup"><span data-stu-id="332b1-247">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="332b1-248">De manera predeterminada, esta cookie se denomina `.AspNet.Session` y usa una ruta de acceso de `/`.</span><span class="sxs-lookup"><span data-stu-id="332b1-248">By default, this cookie is named `.AspNet.Session`, and it uses a path of `/`.</span></span>

::: moniker-end

<span data-ttu-id="332b1-249">Para reemplazar los valores predeterminados de la sesión de cookies, use `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="332b1-249">To override cookie session defaults, use `SessionOptions`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

<span data-ttu-id="332b1-250">La aplicación usa la propiedad [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) para determinar el tiempo que una sesión puede estar inactiva antes de que se abandone el contenido de la caché de servidor.</span><span class="sxs-lookup"><span data-stu-id="332b1-250">The app uses the [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) property to determine how long a session can be idle before its contents in the server's cache are abandoned.</span></span> <span data-ttu-id="332b1-251">Esta propiedad es independiente de la expiración de la cookie.</span><span class="sxs-lookup"><span data-stu-id="332b1-251">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="332b1-252">Cada solicitud que se pasa a través del [middleware de sesión](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) restablece el tiempo de espera.</span><span class="sxs-lookup"><span data-stu-id="332b1-252">Each request that passes through the [Session Middleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resets the timeout.</span></span>

<span data-ttu-id="332b1-253">El estado de sesión es *no realiza bloqueo*.</span><span class="sxs-lookup"><span data-stu-id="332b1-253">Session state is *non-locking*.</span></span> <span data-ttu-id="332b1-254">Si dos solicitudes intentan modificar el contenido de una sesión simultáneamente, la última solicitud reemplaza a la primera.</span><span class="sxs-lookup"><span data-stu-id="332b1-254">If two requests simultaneously attempt to modify the contents of a session, the last request overrides the first.</span></span> <span data-ttu-id="332b1-255">`Session` se implementa como una *sesión coherente*, lo que significa que todo el contenido se almacena junto.</span><span class="sxs-lookup"><span data-stu-id="332b1-255">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="332b1-256">Cuando dos solicitudes buscan modificar diferentes valores de sesión, la última solicitud podría reemplazar los cambios de sesión realizados por la primera.</span><span class="sxs-lookup"><span data-stu-id="332b1-256">When two requests seek to modify different session values, the last request may override session changes made by the first.</span></span>

### <a name="set-and-get-session-values"></a><span data-ttu-id="332b1-257">Establecer y obtener valores de Session</span><span class="sxs-lookup"><span data-stu-id="332b1-257">Set and get Session values</span></span>

<span data-ttu-id="332b1-258">Se accede al estado de sesión desde una clase [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) de Razor Pages o una clase [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) de MVC con [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span><span class="sxs-lookup"><span data-stu-id="332b1-258">Session state is accessed from a Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) class or MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) class with [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span></span> <span data-ttu-id="332b1-259">Esta propiedad es una implementación de [ISession](/dotnet/api/microsoft.aspnetcore.http.isession).</span><span class="sxs-lookup"><span data-stu-id="332b1-259">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="332b1-260">La implementación `ISession` proporciona varios métodos de extensión para establecer y recuperar valores de cadena y enteros.</span><span class="sxs-lookup"><span data-stu-id="332b1-260">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="332b1-261">Los métodos de extensión están en el espacio de nombres [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) (agregue una instrucción `using Microsoft.AspNetCore.Http;` para obtener acceso a los métodos de extensión) cuando el proyecto hace referencia al paquete [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/).</span><span class="sxs-lookup"><span data-stu-id="332b1-261">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span> <span data-ttu-id="332b1-262">Ambos paquetes están incluidos en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="332b1-262">Both packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="332b1-263">La implementación `ISession` proporciona varios métodos de extensión para establecer y recuperar valores de cadena y enteros.</span><span class="sxs-lookup"><span data-stu-id="332b1-263">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="332b1-264">Los métodos de extensión están en el espacio de nombres [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) (agregue una instrucción `using Microsoft.AspNetCore.Http;` para obtener acceso a los métodos de extensión) cuando el proyecto hace referencia al paquete [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/).</span><span class="sxs-lookup"><span data-stu-id="332b1-264">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span>

::: moniker-end

<span data-ttu-id="332b1-265">Métodos de extensión `ISession`:</span><span class="sxs-lookup"><span data-stu-id="332b1-265">`ISession` extension methods:</span></span>

* [<span data-ttu-id="332b1-266">Get(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="332b1-266">Get(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [<span data-ttu-id="332b1-267">GetInt32(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="332b1-267">GetInt32(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [<span data-ttu-id="332b1-268">GetString(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="332b1-268">GetString(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [<span data-ttu-id="332b1-269">SetInt32(ISession, String, Int32)</span><span class="sxs-lookup"><span data-stu-id="332b1-269">SetInt32(ISession, String, Int32)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [<span data-ttu-id="332b1-270">SetString(ISession, String, String)</span><span class="sxs-lookup"><span data-stu-id="332b1-270">SetString(ISession, String, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

<span data-ttu-id="332b1-271">En el ejemplo siguiente se recupera el valor de sesión para la clave `IndexModel.SessionKeyName` (`_Name` en la aplicación de ejemplo) en una página de Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="332b1-271">The following example retrieves the session value for the `IndexModel.SessionKeyName` key (`_Name` in the sample app) in a Razor Pages page:</span></span>

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

<span data-ttu-id="332b1-272">En el ejemplo siguiente se muestra cómo establecer y obtener un entero y una cadena:</span><span class="sxs-lookup"><span data-stu-id="332b1-272">The following example shows how to set and get an integer and a string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

<span data-ttu-id="332b1-273">Todos los datos de sesión se deben serializar para habilitar un escenario de caché distribuida, incluso cuando se usa la caché en memoria.</span><span class="sxs-lookup"><span data-stu-id="332b1-273">All session data must be serialized to enable a distributed cache scenario, even when using the in-memory cache.</span></span> <span data-ttu-id="332b1-274">Se proporcionan una cadena mínima y serializadores de número (vea los métodos y los métodos de extensión de [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span><span class="sxs-lookup"><span data-stu-id="332b1-274">Minimal string and number serializers are provided (see the methods and extension methods of [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span></span> <span data-ttu-id="332b1-275">El usuario debe serializar los tipos complejos mediante otro mecanismo, como JSON.</span><span class="sxs-lookup"><span data-stu-id="332b1-275">Complex types must be serialized by the user using another mechanism, such as JSON.</span></span>

<span data-ttu-id="332b1-276">Agregue los siguientes métodos de extensión, para establecer y obtener objetos serializables:</span><span class="sxs-lookup"><span data-stu-id="332b1-276">Add the following extension methods to set and get serializable objects:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="332b1-277">En el ejemplo siguiente se muestra cómo establecer y obtener un objeto serializable con los método de extensión:</span><span class="sxs-lookup"><span data-stu-id="332b1-277">The following example shows how to set and get a serializable object with the extension methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="332b1-278">TempData</span><span class="sxs-lookup"><span data-stu-id="332b1-278">TempData</span></span>

<span data-ttu-id="332b1-279">ASP.NET Core expone la [propiedad TempData de un modelo de página de Razor Pages](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) o [TempData de un controlador de MVC](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span><span class="sxs-lookup"><span data-stu-id="332b1-279">ASP.NET Core exposes the [TempData property of a Razor Pages page model](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) or [TempData of an MVC controller](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span></span> <span data-ttu-id="332b1-280">Esta propiedad almacena datos hasta que se leen.</span><span class="sxs-lookup"><span data-stu-id="332b1-280">This property stores data until it's read.</span></span> <span data-ttu-id="332b1-281">Los métodos [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) y [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) se pueden usar para examinar los datos sin que se eliminen.</span><span class="sxs-lookup"><span data-stu-id="332b1-281">The [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) and [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="332b1-282">TempData es particularmente útil para el redireccionamiento cuando se necesitan los datos para más de una única solicitud.</span><span class="sxs-lookup"><span data-stu-id="332b1-282">TempData is particularly useful for redirection when data is required for more than a single request.</span></span> <span data-ttu-id="332b1-283">Los proveedores de TempData implementan TempData mediante cookies o estado de sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-283">TempData is implemented by TempData providers using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="332b1-284">Proveedores de TempData</span><span class="sxs-lookup"><span data-stu-id="332b1-284">TempData providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="332b1-285">En ASP.NET Core 2.0 y versiones posteriores, el proveedor TempData basado en cookies se usa de forma predeterminada para almacenar TempData en cookies.</span><span class="sxs-lookup"><span data-stu-id="332b1-285">In ASP.NET Core 2.0 or later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="332b1-286">Los datos de cookie se cifran mediante [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), codificado con [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), y después se fragmentan.</span><span class="sxs-lookup"><span data-stu-id="332b1-286">The cookie data is encrypted using [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encoded with [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), then chunked.</span></span> <span data-ttu-id="332b1-287">Como la cookie está fragmentada, no se aplica el límite de tamaño único de cookie que se encuentra en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="332b1-287">Because the cookie is chunked, the single cookie size limit found in ASP.NET Core 1.x doesn't apply.</span></span> <span data-ttu-id="332b1-288">Los datos de cookie no se comprimen porque la compresión de datos cifrados puede provocar problemas de seguridad como los ataques [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) y [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)).</span><span class="sxs-lookup"><span data-stu-id="332b1-288">The cookie data isn't compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="332b1-289">Para obtener más información sobre el proveedor TempData basado en cookies, consulte [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span><span class="sxs-lookup"><span data-stu-id="332b1-289">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="332b1-290">En ASP.NET Core 1.0 y 1.1, el proveedor TempData de estado de sesión es el proveedor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="332b1-290">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default provider.</span></span>

::: moniker-end

### <a name="choose-a-tempdata-provider"></a><span data-ttu-id="332b1-291">Elegir un proveedor TempData</span><span class="sxs-lookup"><span data-stu-id="332b1-291">Choose a TempData provider</span></span>

<span data-ttu-id="332b1-292">Elegir un proveedor TempData implica una serie de consideraciones:</span><span class="sxs-lookup"><span data-stu-id="332b1-292">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="332b1-293">¿La aplicación ya usa el estado de sesión?</span><span class="sxs-lookup"><span data-stu-id="332b1-293">Does the app already use session state?</span></span> <span data-ttu-id="332b1-294">Si es así, el uso del proveedor TempData de estado de sesión no tiene costo adicional para la aplicación (excepto en el tamaño de los datos).</span><span class="sxs-lookup"><span data-stu-id="332b1-294">If so, using the session state TempData provider has no additional cost to the app (aside from the size of the data).</span></span>
2. <span data-ttu-id="332b1-295">¿La aplicación usa TempData con moderación, solo para cantidades relativamente pequeñas de datos (hasta 500 bytes)?</span><span class="sxs-lookup"><span data-stu-id="332b1-295">Does the app use TempData only sparingly for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="332b1-296">Si es así, el proveedor TempData de cookies agrega un pequeño costo a cada solicitud que transporta TempData.</span><span class="sxs-lookup"><span data-stu-id="332b1-296">If so, the cookie TempData provider adds a small cost to each request that carries TempData.</span></span> <span data-ttu-id="332b1-297">De lo contrario, el proveedor TempData de estado de sesión puede ser beneficioso para evitar que una gran cantidad de datos hagan un recorrido de ida y vuelta en cada solicitud hasta que se consuma TempData.</span><span class="sxs-lookup"><span data-stu-id="332b1-297">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="332b1-298">¿La aplicación se ejecuta en una granja de servidores en varios servidores?</span><span class="sxs-lookup"><span data-stu-id="332b1-298">Does the app run in a server farm on multiple servers?</span></span> <span data-ttu-id="332b1-299">Si es así, no es necesaria ninguna configuración adicional para usar el proveedor de TempData de cookies además de la protección de datos (vea [Protección de datos ](xref:security/data-protection/index) y [Key storage providers](xref:security/data-protection/implementation/key-storage-providers) (Proveedores de almacenamiento de claves)).</span><span class="sxs-lookup"><span data-stu-id="332b1-299">If so, there's no additional configuration required to use the cookie TempData provider outside of Data Protection (see [Data Protection](xref:security/data-protection/index) and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers)).</span></span>

> [!NOTE]
> <span data-ttu-id="332b1-300">La mayoría de los clientes de web (por ejemplo, los exploradores web) aplican límites en el tamaño máximo de cada cookie, el número total de cookies o ambos.</span><span class="sxs-lookup"><span data-stu-id="332b1-300">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="332b1-301">Cuando use el proveedor TempData de cookies, compruebe que la aplicación no supera esos límites.</span><span class="sxs-lookup"><span data-stu-id="332b1-301">When using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="332b1-302">Tenga en cuenta el tamaño total de los datos.</span><span class="sxs-lookup"><span data-stu-id="332b1-302">Consider the total size of the data.</span></span> <span data-ttu-id="332b1-303">Cuenta para los aumentos de tamaño de cookie debidos a la fragmentación y el cifrado.</span><span class="sxs-lookup"><span data-stu-id="332b1-303">Account for increases in cookie size due to encryption and chunking.</span></span>

### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="332b1-304">Configurar el proveedor TempData</span><span class="sxs-lookup"><span data-stu-id="332b1-304">Configure the TempData provider</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="332b1-305">El proveedor TempData basado en cookies está habilitado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="332b1-305">The cookie-based TempData provider is enabled by default.</span></span>

<span data-ttu-id="332b1-306">Para habilitar el proveedor TempData basado en sesión, use el método de extensión [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider):</span><span class="sxs-lookup"><span data-stu-id="332b1-306">To enable the session-based TempData provider, use the [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) extension method:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="332b1-307">El siguiente código de clase `Startup` configura el proveedor TempData basado en sesión:</span><span class="sxs-lookup"><span data-stu-id="332b1-307">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

<span data-ttu-id="332b1-308">El orden del middleware es importante.</span><span class="sxs-lookup"><span data-stu-id="332b1-308">The order of middleware is important.</span></span> <span data-ttu-id="332b1-309">En el ejemplo anterior, se produce una excepción `InvalidOperationException` cuando `UseSession` se invoca después de `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="332b1-309">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="332b1-310">Para obtener más información vea [Ordenación de Middleware](xref:fundamentals/middleware/index#order).</span><span class="sxs-lookup"><span data-stu-id="332b1-310">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="332b1-311">Si el destino es .NET Framework y usa el proveedor TempData basado en sesión, agregue el paquete [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) al proyecto.</span><span class="sxs-lookup"><span data-stu-id="332b1-311">If targeting .NET Framework and using the session-based TempData provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package to the project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="332b1-312">Cadenas de consulta</span><span class="sxs-lookup"><span data-stu-id="332b1-312">Query strings</span></span>

<span data-ttu-id="332b1-313">Se puede pasar una cantidad limitada de datos de una solicitud a otra si agrega los datos a la cadena de consulta de la solicitud nueva.</span><span class="sxs-lookup"><span data-stu-id="332b1-313">A limited amount of data can be passed from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="332b1-314">Esto es útil para capturar el estado de una forma persistente que permita que los vínculos con estado insertado se compartan a través del correo electrónico o las redes sociales.</span><span class="sxs-lookup"><span data-stu-id="332b1-314">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="332b1-315">Dado que las cadenas de consulta de direcciones URL son públicas, nunca use las cadenas de consulta para datos confidenciales.</span><span class="sxs-lookup"><span data-stu-id="332b1-315">Because URL query strings are public, never use query strings for sensitive data.</span></span>

<span data-ttu-id="332b1-316">Además del uso compartido no intencionado, la inclusión de datos en las cadenas de consulta puede propiciar ataques de [falsificación de solicitud entre sitios (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)), cuya intención es engañar a los usuarios para que visiten sitios malintencionados mientras están autenticados.</span><span class="sxs-lookup"><span data-stu-id="332b1-316">In addition to unintended sharing, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="332b1-317">Después, los atacantes pueden robar los datos de usuario de la aplicación o realizar acciones malintencionadas en nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="332b1-317">Attackers can then steal user data from the app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="332b1-318">Cualquier estado de sesión o aplicación conservado debe protegerse contra los ataques CSRF.</span><span class="sxs-lookup"><span data-stu-id="332b1-318">Any preserved app or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="332b1-319">Para más información, vea [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks](xref:security/anti-request-forgery) (Evitar los ataques de falsificación de solicitud entre sitios [XSRF/CSRF]).</span><span class="sxs-lookup"><span data-stu-id="332b1-319">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="hidden-fields"></a><span data-ttu-id="332b1-320">Campos ocultos</span><span class="sxs-lookup"><span data-stu-id="332b1-320">Hidden fields</span></span>

<span data-ttu-id="332b1-321">Los datos pueden guardarse en campos ocultos de formulario e incluirse de nuevo en la siguiente solicitud.</span><span class="sxs-lookup"><span data-stu-id="332b1-321">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="332b1-322">Esto es habitual en los formularios de varias páginas.</span><span class="sxs-lookup"><span data-stu-id="332b1-322">This is common in multi-page forms.</span></span> <span data-ttu-id="332b1-323">Dado que el cliente puede llegar a alterar los datos, la aplicación siempre debe revalidar los datos almacenados en campos ocultos.</span><span class="sxs-lookup"><span data-stu-id="332b1-323">Because the client can potentially tamper with the data, the app must always revalidate the data stored in hidden fields.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="332b1-324">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="332b1-324">HttpContext.Items</span></span>

<span data-ttu-id="332b1-325">La colección [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) se usa para almacenar los datos al procesar una única solicitud.</span><span class="sxs-lookup"><span data-stu-id="332b1-325">The [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) collection is used to store data while processing a single request.</span></span> <span data-ttu-id="332b1-326">El contenido de la colección se descarta después de procesar una solicitud.</span><span class="sxs-lookup"><span data-stu-id="332b1-326">The collection's contents are discarded after a request is processed.</span></span> <span data-ttu-id="332b1-327">A menudo se usa la colección `Items` para permitir que los componentes o el middleware se comuniquen cuando operan en distintos puntos en el tiempo durante una solicitud y no pueden pasarse parámetros de forma directa.</span><span class="sxs-lookup"><span data-stu-id="332b1-327">The `Items` collection is often used to allow components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span>

<span data-ttu-id="332b1-328">En el ejemplo siguiente, [Middleware](xref:fundamentals/middleware/index) agrega `isVerified` a la colección `Items`.</span><span class="sxs-lookup"><span data-stu-id="332b1-328">In the following example, [middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="332b1-329">Más adelante en la canalización, otro middleware puede acceder al valor de `isVerified`:</span><span class="sxs-lookup"><span data-stu-id="332b1-329">Later in the pipeline, another middleware can access the value of `isVerified`:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

<span data-ttu-id="332b1-330">Si el middleware solo se usa en una única aplicación, se aceptan claves `string`.</span><span class="sxs-lookup"><span data-stu-id="332b1-330">For middleware that's only used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="332b1-331">El middleware compartido entre instancias de aplicación debería usar claves de objeto únicas para evitar conflictos de clave.</span><span class="sxs-lookup"><span data-stu-id="332b1-331">Middleware shared between app instances should use unique object keys to avoid key collisions.</span></span> <span data-ttu-id="332b1-332">En el ejemplo siguiente se muestra cómo usar una clave de objeto única definida en una clase de middleware:</span><span class="sxs-lookup"><span data-stu-id="332b1-332">The following example shows how to use a unique object key defined in a middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

<span data-ttu-id="332b1-333">Otro código puede tener acceso al valor almacenado en `HttpContext.Items` con la clave que expone la clase de middleware:</span><span class="sxs-lookup"><span data-stu-id="332b1-333">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="332b1-334">Este enfoque también tiene la ventaja de eliminar el uso de cadenas de claves en el código.</span><span class="sxs-lookup"><span data-stu-id="332b1-334">This approach also has the advantage of eliminating the use of key strings in the code.</span></span>

## <a name="cache"></a><span data-ttu-id="332b1-335">instancias y claves</span><span class="sxs-lookup"><span data-stu-id="332b1-335">Cache</span></span>

<span data-ttu-id="332b1-336">El almacenamiento en caché es una manera eficaz de almacenar y recuperar datos.</span><span class="sxs-lookup"><span data-stu-id="332b1-336">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="332b1-337">La aplicación puede controlar la duración de los elementos almacenados en caché.</span><span class="sxs-lookup"><span data-stu-id="332b1-337">The app can control the lifetime of cached items.</span></span>

<span data-ttu-id="332b1-338">Los datos almacenados en caché no están asociados a una solicitud, usuario o sesión específicos.</span><span class="sxs-lookup"><span data-stu-id="332b1-338">Cached data isn't associated with a specific request, user, or session.</span></span> <span data-ttu-id="332b1-339">**Procure no almacenar en caché datos específicos de usuario que podrían recuperar las solicitudes de otros usuarios.**</span><span class="sxs-lookup"><span data-stu-id="332b1-339">**Be careful not to cache user-specific data that may be retrieved by other users' requests.**</span></span>

<span data-ttu-id="332b1-340">Para obtener más información, vea el tema [Almacenamiento en caché de respuestas](xref:performance/caching/index).</span><span class="sxs-lookup"><span data-stu-id="332b1-340">For more information, see the [Cache responses](xref:performance/caching/index) topic.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="332b1-341">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="332b1-341">Dependency Injection</span></span>

<span data-ttu-id="332b1-342">Use [inserción de dependencias](xref:fundamentals/dependency-injection) para que los datos estén disponibles para todos los usuarios:</span><span class="sxs-lookup"><span data-stu-id="332b1-342">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="332b1-343">Definir un servicio que contiene los datos.</span><span class="sxs-lookup"><span data-stu-id="332b1-343">Define a service containing the data.</span></span> <span data-ttu-id="332b1-344">Por ejemplo, una clase denominada `MyAppData` se define:</span><span class="sxs-lookup"><span data-stu-id="332b1-344">For example, a class named `MyAppData` is defined:</span></span>

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. <span data-ttu-id="332b1-345">Agregue la clase de servicio a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="332b1-345">Add the service class to `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. <span data-ttu-id="332b1-346">Use la clase de servicio de datos:</span><span class="sxs-lookup"><span data-stu-id="332b1-346">Consume the data service class:</span></span>

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a><span data-ttu-id="332b1-347">Errores comunes</span><span class="sxs-lookup"><span data-stu-id="332b1-347">Common errors</span></span>

* <span data-ttu-id="332b1-348">"No se puede resolver el servicio para el tipo 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' al intentar activar 'Microsoft.AspNetCore.Session.DistributedSessionStore'".</span><span class="sxs-lookup"><span data-stu-id="332b1-348">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="332b1-349">Esto puede deberse a que no se ha configurado al menos una implementación `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="332b1-349">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="332b1-350">Para obtener más información, vea [Working with a Distributed Cache](xref:performance/caching/distributed) (Trabajar con una memoria caché distribuida) e [In memory caching](xref:performance/caching/memory) (Almacenamiento en memoria caché).</span><span class="sxs-lookup"><span data-stu-id="332b1-350">For more information, see [Work with a distributed cache](xref:performance/caching/distributed) and [Cache in-memory](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="332b1-351">En caso de que el middleware de sesión no logre conservar una sesión (por ejemplo, si la memoria auxiliar no está disponible), el middleware registra la excepción y la solicitud continúa con normalidad.</span><span class="sxs-lookup"><span data-stu-id="332b1-351">In the event that the session middleware fails to persist a session (for example, if the backing store isn't available), the middleware logs the exception and the request continues normally.</span></span> <span data-ttu-id="332b1-352">Esto provoca un comportamiento imprevisible.</span><span class="sxs-lookup"><span data-stu-id="332b1-352">This leads to unpredictable behavior.</span></span>

  <span data-ttu-id="332b1-353">Por ejemplo, un usuario almacena un carro de la compra en la sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-353">For example, a user stores a shopping cart in session.</span></span> <span data-ttu-id="332b1-354">El usuario agrega un elemento al carro, pero se produce un error en la confirmación.</span><span class="sxs-lookup"><span data-stu-id="332b1-354">The user adds an item to the cart but the commit fails.</span></span> <span data-ttu-id="332b1-355">La aplicación no se percata del error y notifica al usuario que el elemento se ha agregado al carro, lo cual no es cierto.</span><span class="sxs-lookup"><span data-stu-id="332b1-355">The app doesn't know about the failure so it reports to the user that the item was added to their cart, which isn't true.</span></span>

  <span data-ttu-id="332b1-356">El enfoque recomendado para comprobar los errores es llamar a `await feature.Session.CommitAsync();` desde el código de la aplicación cuando esta haya terminado de escribir en la sesión.</span><span class="sxs-lookup"><span data-stu-id="332b1-356">The recommended approach to check for errors is to call `await feature.Session.CommitAsync();` from app code when the app is done writing to the session.</span></span> <span data-ttu-id="332b1-357">`CommitAsync` produce una excepción si la memoria auxiliar no está disponible.</span><span class="sxs-lookup"><span data-stu-id="332b1-357">`CommitAsync` throws an exception if the backing store is unavailable.</span></span> <span data-ttu-id="332b1-358">Si `CommitAsync` produce un error, la aplicación puede procesar la excepción.</span><span class="sxs-lookup"><span data-stu-id="332b1-358">If `CommitAsync` fails, the app can process the exception.</span></span> <span data-ttu-id="332b1-359">`LoadAsync` se produce en las mismas condiciones donde el almacén de datos no está disponible.</span><span class="sxs-lookup"><span data-stu-id="332b1-359">`LoadAsync` throws under the same conditions where the data store is unavailable.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="332b1-360">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="332b1-360">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
