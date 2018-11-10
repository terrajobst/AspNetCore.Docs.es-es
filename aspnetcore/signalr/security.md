---
title: Consideraciones de seguridad en ASP.NET Core SignalR
author: tdykstra
description: Obtenga información sobre cómo utilizar la autenticación y autorización en ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 10/17/2018
uid: signalr/security
ms.openlocfilehash: be1dd24c40327d9a0d8f91bf75300128d3d52725
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225374"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="66616-103">Consideraciones de seguridad en ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="66616-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="66616-104">Por [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="66616-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="66616-105">En este artículo se proporciona información sobre la protección de SignalR.</span><span class="sxs-lookup"><span data-stu-id="66616-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="66616-106">Uso compartido de recursos entre orígenes</span><span class="sxs-lookup"><span data-stu-id="66616-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="66616-107">[Los recursos entre orígenes (CORS) de uso compartido](https://www.w3.org/TR/cors/) puede utilizarse para permitir que las conexiones de origen cruzado SignalR en el explorador.</span><span class="sxs-lookup"><span data-stu-id="66616-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="66616-108">Si el código de JavaScript está hospedado en un dominio distinto de la aplicación de SignalR, [middleware CORS](xref:security/cors) debe habilitarse para permitir que el código JavaScript para conectarse a la aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="66616-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="66616-109">Permitir solicitudes entre orígenes solo desde el control o dominios que confía.</span><span class="sxs-lookup"><span data-stu-id="66616-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="66616-110">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="66616-110">For example:</span></span>

* <span data-ttu-id="66616-111">El sitio se hospeda en `http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="66616-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="66616-112">La aplicación de SignalR se hospeda en `http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="66616-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="66616-113">Debe configurarse la CORS en la aplicación de SignalR para permitir únicamente el origen `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="66616-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="66616-114">Para obtener más información sobre cómo configurar la CORS, vea [solicitudes habilitar de origen cruzado (CORS)](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="66616-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> <span data-ttu-id="66616-115">SignalR **requiere** las siguientes directivas CORS:</span><span class="sxs-lookup"><span data-stu-id="66616-115">SignalR **requires** the following CORS policies:</span></span>

* <span data-ttu-id="66616-116">Permitir los orígenes específicos esperados.</span><span class="sxs-lookup"><span data-stu-id="66616-116">Allow the specific expected origins.</span></span> <span data-ttu-id="66616-117">Permitir cualquier origen, es posible pero es **no** seguro o recomendada.</span><span class="sxs-lookup"><span data-stu-id="66616-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="66616-118">Métodos HTTP `GET` y `POST` debe estar permitido.</span><span class="sxs-lookup"><span data-stu-id="66616-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="66616-119">Deben habilitarse las credenciales, incluso cuando no se usa la autenticación.</span><span class="sxs-lookup"><span data-stu-id="66616-119">Credentials must be enabled, even when authentication is not used.</span></span>

<span data-ttu-id="66616-120">Por ejemplo, la siguiente directiva CORS permite hospedado en un cliente del explorador SignalR `http://example.com` para tener acceso a la aplicación de SignalR hospedada en `http://signalr.example.com`:</span><span class="sxs-lookup"><span data-stu-id="66616-120">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access the SignalR app hosted on `http://signalr.example.com`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="66616-121">SignalR no es compatible con la característica CORS integrada en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="66616-121">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="66616-122">Restricción de origen de WebSocket</span><span class="sxs-lookup"><span data-stu-id="66616-122">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="66616-123">Las protecciones proporcionadas por la CORS no se aplican a WebSockets.</span><span class="sxs-lookup"><span data-stu-id="66616-123">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="66616-124">Para la restricción de origen de WebSockets, lea [restricción de origen de WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).</span><span class="sxs-lookup"><span data-stu-id="66616-124">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="66616-125">Las protecciones proporcionadas por la CORS no se aplican a WebSockets.</span><span class="sxs-lookup"><span data-stu-id="66616-125">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="66616-126">Los exploradores lo hacen **no**:</span><span class="sxs-lookup"><span data-stu-id="66616-126">Browsers do **not**:</span></span>

* <span data-ttu-id="66616-127">Realizar solicitudes preparatorias CORS.</span><span class="sxs-lookup"><span data-stu-id="66616-127">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="66616-128">Respeta las restricciones especificadas en `Access-Control` encabezados al realizar solicitudes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="66616-128">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="66616-129">Sin embargo, los exploradores envían el `Origin` encabezado al emitir solicitudes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="66616-129">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="66616-130">Las aplicaciones deben configurarse para validar estos encabezados para asegurarse de que se permiten solo WebSockets procedentes de los orígenes esperados.</span><span class="sxs-lookup"><span data-stu-id="66616-130">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="66616-131">En ASP.NET Core 2.1 y versiones posteriores, la validación del encabezado puede lograrse mediante un middleware personalizado colocarlo **antes `UseSignalR`y el middleware de autenticación** en `Configure`:</span><span class="sxs-lookup"><span data-stu-id="66616-131">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="66616-132">El `Origin` encabezado está controlado por el cliente y, al igual que el `Referer` encabezado, se pueden falsificar.</span><span class="sxs-lookup"><span data-stu-id="66616-132">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="66616-133">Estos encabezados deben **no** utilizarse como mecanismo de autenticación.</span><span class="sxs-lookup"><span data-stu-id="66616-133">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="access-token-logging"></a><span data-ttu-id="66616-134">Registro de token de acceso</span><span class="sxs-lookup"><span data-stu-id="66616-134">Access token logging</span></span>

<span data-ttu-id="66616-135">Al usar WebSockets o los eventos, el explorador del cliente envía el token de acceso en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="66616-135">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="66616-136">Recibir el token de acceso a través de la cadena de consulta es normalmente tan seguro como usar el estándar `Authorization` encabezado.</span><span class="sxs-lookup"><span data-stu-id="66616-136">Receiving the access token via query string is generally as secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="66616-137">Sin embargo, muchos servidores web de registro la dirección URL para cada solicitud, incluida la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="66616-137">However, many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="66616-138">Las direcciones URL de registro, es posible que registre el token de acceso.</span><span class="sxs-lookup"><span data-stu-id="66616-138">Logging the URLs may log the access token.</span></span> <span data-ttu-id="66616-139">Una práctica recomendada consiste en establecer configuración de registro del servidor para evitar que los tokens de acceso de registro de la web.</span><span class="sxs-lookup"><span data-stu-id="66616-139">A best practice is to set the web server's logging settings to prevent logging access tokens.</span></span>

## <a name="exceptions"></a><span data-ttu-id="66616-140">Excepciones</span><span class="sxs-lookup"><span data-stu-id="66616-140">Exceptions</span></span>

<span data-ttu-id="66616-141">Los mensajes de excepción normalmente se consideran información confidencial que no debe mostrarse a un cliente.</span><span class="sxs-lookup"><span data-stu-id="66616-141">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="66616-142">De forma predeterminada, SignalR no envía los detalles de una excepción producida por un método de concentrador al cliente.</span><span class="sxs-lookup"><span data-stu-id="66616-142">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="66616-143">En su lugar, el cliente recibe un mensaje genérico que indica que un error.</span><span class="sxs-lookup"><span data-stu-id="66616-143">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="66616-144">Entrega de mensajes de excepción al cliente se puede invalidar (por ejemplo, en el desarrollo o pruebas) con [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options).</span><span class="sxs-lookup"><span data-stu-id="66616-144">Exception message delivery to the client can be overridden (for example in development or test) with [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="66616-145">Los mensajes de excepción no se deben exponer al cliente en aplicaciones de producción.</span><span class="sxs-lookup"><span data-stu-id="66616-145">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="66616-146">Administración de búfer</span><span class="sxs-lookup"><span data-stu-id="66616-146">Buffer management</span></span>

<span data-ttu-id="66616-147">SignalR usa búferes por conexión para administrar los mensajes entrantes y salientes.</span><span class="sxs-lookup"><span data-stu-id="66616-147">SignalR uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="66616-148">De forma predeterminada, SignalR limita estos búferes a 32 KB.</span><span class="sxs-lookup"><span data-stu-id="66616-148">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="66616-149">El mensaje más grande que puede enviar un cliente o servidor es 32 KB.</span><span class="sxs-lookup"><span data-stu-id="66616-149">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="66616-150">La memoria máxima utilizada por una conexión para los mensajes es 32 KB.</span><span class="sxs-lookup"><span data-stu-id="66616-150">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="66616-151">Si los mensajes siempre son menores que 32 KB, puede reducir el límite, que:</span><span class="sxs-lookup"><span data-stu-id="66616-151">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="66616-152">Impide que un cliente puede enviar un mensaje mayor.</span><span class="sxs-lookup"><span data-stu-id="66616-152">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="66616-153">El servidor nunca tendrá que asignar los búferes grandes para aceptar mensajes.</span><span class="sxs-lookup"><span data-stu-id="66616-153">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="66616-154">Si los mensajes son superiores a 32 KB, puede aumentar el límite.</span><span class="sxs-lookup"><span data-stu-id="66616-154">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="66616-155">Aumentar este límite significa:</span><span class="sxs-lookup"><span data-stu-id="66616-155">Increasing this limit means:</span></span>

* <span data-ttu-id="66616-156">El cliente puede hacer que el servidor asignar búferes de memoria de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="66616-156">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="66616-157">Asignación de servidor de los búferes grandes puede reducir el número de conexiones simultáneas.</span><span class="sxs-lookup"><span data-stu-id="66616-157">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="66616-158">Hay límites para los mensajes entrantes y salientes, ambos se pueden configurar en el [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) configurado en el objeto `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="66616-158">There are limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="66616-159">`ApplicationMaxBufferSize` representa el número máximo de bytes desde el cliente que los búferes del servidor.</span><span class="sxs-lookup"><span data-stu-id="66616-159">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="66616-160">Si el cliente intenta enviar un mensaje supere ese límite, se puede cerrar la conexión.</span><span class="sxs-lookup"><span data-stu-id="66616-160">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="66616-161">`TransportMaxBufferSize` representa el número máximo de bytes que el servidor puede enviar.</span><span class="sxs-lookup"><span data-stu-id="66616-161">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="66616-162">Si el servidor intenta enviar un mensaje (incluidos los valores devueltos de métodos de concentrador) supere ese límite, se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="66616-162">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="66616-163">Establecer el límite en `0` desactivar el límite.</span><span class="sxs-lookup"><span data-stu-id="66616-163">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="66616-164">Quitar el límite permite que un cliente enviar un mensaje de cualquier tamaño.</span><span class="sxs-lookup"><span data-stu-id="66616-164">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="66616-165">Los clientes malintencionados enviar mensajes de gran tamaño pueden producir un exceso de memoria que se va a asignar.</span><span class="sxs-lookup"><span data-stu-id="66616-165">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="66616-166">Uso de memoria excesivo puede reducir significativamente el número de conexiones simultáneas.</span><span class="sxs-lookup"><span data-stu-id="66616-166">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
