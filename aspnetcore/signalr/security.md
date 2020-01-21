---
title: Consideraciones de seguridad en ASP.NET Core SignalR
author: bradygaster
description: Aprenda a usar la autenticación y autorización en ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- SignalR
uid: signalr/security
ms.openlocfilehash: 4b27d9abb36938ed8161ff0d3535204e3fa68765
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294702"
---
# <a name="security-considerations-in-aspnet-core-opno-locsignalr"></a><span data-ttu-id="65f1b-103">Consideraciones de seguridad en ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="65f1b-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="65f1b-104">Por [Andrew Stanton-enfermera](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="65f1b-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="65f1b-105">En este artículo se proporciona información sobre cómo proteger SignalR.</span><span class="sxs-lookup"><span data-stu-id="65f1b-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="65f1b-106">Uso compartido de recursos entre orígenes</span><span class="sxs-lookup"><span data-stu-id="65f1b-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="65f1b-107">[El uso compartido de recursos entre orígenes (CORS)](https://www.w3.org/TR/cors/) se puede usar para permitir conexiones SignalR entre orígenes en el explorador.</span><span class="sxs-lookup"><span data-stu-id="65f1b-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="65f1b-108">Si el código de JavaScript está hospedado en un dominio diferente del SignalR aplicación, el [middleware de CORS](xref:security/cors) debe estar habilitado para permitir que JavaScript se conecte a la aplicación SignalR.</span><span class="sxs-lookup"><span data-stu-id="65f1b-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="65f1b-109">Permita solicitudes entre orígenes únicamente desde dominios en los que confíe o controle.</span><span class="sxs-lookup"><span data-stu-id="65f1b-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="65f1b-110">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="65f1b-110">For example:</span></span>

* <span data-ttu-id="65f1b-111">El sitio se hospeda en `http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="65f1b-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="65f1b-112">La aplicación SignalR se hospeda en `http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="65f1b-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="65f1b-113">CORS debe configurarse en la aplicación SignalR para permitir solo el origen `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="65f1b-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="65f1b-114">Para obtener más información sobre la configuración de CORS, consulte [habilitación de solicitudes entre orígenes (CORS)](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="65f1b-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> SignalR<span data-ttu-id="65f1b-115"> **requiere** las siguientes directivas de CORS:</span><span class="sxs-lookup"><span data-stu-id="65f1b-115"> **requires** the following CORS policies:</span></span>

* <span data-ttu-id="65f1b-116">Permite los orígenes esperados específicos.</span><span class="sxs-lookup"><span data-stu-id="65f1b-116">Allow the specific expected origins.</span></span> <span data-ttu-id="65f1b-117">Permitir cualquier origen es posible, pero **no** es seguro ni recomendado.</span><span class="sxs-lookup"><span data-stu-id="65f1b-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="65f1b-118">Se deben permitir los métodos HTTP `GET` y `POST`.</span><span class="sxs-lookup"><span data-stu-id="65f1b-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="65f1b-119">Se deben permitir las credenciales para que las sesiones permanentes basadas en cookies funcionen correctamente.</span><span class="sxs-lookup"><span data-stu-id="65f1b-119">Credentials must be allowed in order for cookie-based sticky sessions to work correctly.</span></span> <span data-ttu-id="65f1b-120">Deben habilitarse incluso cuando no se utiliza la autenticación.</span><span class="sxs-lookup"><span data-stu-id="65f1b-120">They must be enabled even when authentication isn't used.</span></span>

<!--
::: moniker range=">= aspnetcore-5.0"  // Moniker here just to make sure this doesn't get missed in the 5.0 version update.
However, in 5.0 we have provided an option in the TypeScript client to not use credentials.
The not to use credentials option should only be used when you know 100% that credentials like Cookies are not needed in your app (cookies are used by azure app service when using multiple servers)

For more info, see https://github.com/aspnet/AspNetCore.Docs/issues/16003
.-->

<span data-ttu-id="65f1b-121">Por ejemplo, la siguiente directiva de CORS permite a un cliente de explorador de SignalR hospedado en `https://example.com` acceder a la aplicación de SignalR hospedada en `https://signalr.example.com`:</span><span class="sxs-lookup"><span data-stu-id="65f1b-121">For example, the following CORS policy allows a SignalR browser client hosted on `https://example.com` to access the SignalR app hosted on `https://signalr.example.com`:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder =>
    {
        builder.WithOrigins("https://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chatHub");
    });

    // ... other middleware ...
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

::: moniker-end

> [!NOTE]
> SignalR<span data-ttu-id="65f1b-122"> no es compatible con la característica de CORS integrada en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="65f1b-122"> is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="65f1b-123">Restricción de origen de WebSocket</span><span class="sxs-lookup"><span data-stu-id="65f1b-123">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="65f1b-124">Las protecciones proporcionadas por CORS no se aplican a WebSockets.</span><span class="sxs-lookup"><span data-stu-id="65f1b-124">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="65f1b-125">En el caso de la restricción de origen en WebSockets, lea [restricción de origen de WebSockets](xref:fundamentals/websockets#websocket-origin-restriction).</span><span class="sxs-lookup"><span data-stu-id="65f1b-125">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="65f1b-126">Las protecciones proporcionadas por CORS no se aplican a WebSockets.</span><span class="sxs-lookup"><span data-stu-id="65f1b-126">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="65f1b-127">Los exploradores **no** hacen lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="65f1b-127">Browsers do **not**:</span></span>

* <span data-ttu-id="65f1b-128">Efectúan solicitudes preparatorias CORS.</span><span class="sxs-lookup"><span data-stu-id="65f1b-128">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="65f1b-129">Respetan las restricciones especificadas en los encabezados `Access-Control` al efectuar solicitudes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="65f1b-129">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="65f1b-130">En cambio, sí que envían el encabezado `Origin` al emitir solicitudes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="65f1b-130">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="65f1b-131">Las aplicaciones deben configurarse para validar estos encabezados a fin de garantizar que solo se permitan los WebSockets procedentes de los orígenes esperados.</span><span class="sxs-lookup"><span data-stu-id="65f1b-131">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="65f1b-132">En ASP.NET Core 2,1 y versiones posteriores, la validación de encabezados se puede lograr mediante un middleware personalizado colocado **antes de `UseSignalR`y el middleware de autenticación** en `Configure`:</span><span class="sxs-lookup"><span data-stu-id="65f1b-132">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="65f1b-133">El encabezado `Origin` está controlado por el cliente y, al igual que el encabezado `Referer`, se puede falsificar.</span><span class="sxs-lookup"><span data-stu-id="65f1b-133">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="65f1b-134">Estos encabezados **no** deben usarse como mecanismo de autenticación.</span><span class="sxs-lookup"><span data-stu-id="65f1b-134">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="connectionid"></a><span data-ttu-id="65f1b-135">ConnectionId</span><span class="sxs-lookup"><span data-stu-id="65f1b-135">ConnectionId</span></span>

<span data-ttu-id="65f1b-136">Exponer `ConnectionId` puede provocar la suplantación malintencionada si la versión del servidor o del cliente de SignalR es ASP.NET Core 2,2 o anterior.</span><span class="sxs-lookup"><span data-stu-id="65f1b-136">Exposing `ConnectionId` can lead to malicious impersonation if the SignalR server or client version is ASP.NET Core 2.2 or earlier.</span></span> <span data-ttu-id="65f1b-137">Si el servidor de SignalR y la versión del cliente son ASP.NET Core 3,0 o posterior, el `ConnectionToken` en lugar del `ConnectionId` debe mantenerse en secreto.</span><span class="sxs-lookup"><span data-stu-id="65f1b-137">If the SignalR server and client version are ASP.NET Core 3.0 or later, the `ConnectionToken` rather than the `ConnectionId` must be kept secret.</span></span> <span data-ttu-id="65f1b-138">El `ConnectionToken` no se expone intencionadamente en ninguna API.</span><span class="sxs-lookup"><span data-stu-id="65f1b-138">The `ConnectionToken` is purposely not exposed in any API.</span></span>  <span data-ttu-id="65f1b-139">Puede ser difícil asegurarse de que los clientes de SignalR más antiguos no se conecten al servidor, por lo que, incluso si la versión del servidor de SignalR es ASP.NET Core 3,0 o posterior, la `ConnectionId` no debe exponerse.</span><span class="sxs-lookup"><span data-stu-id="65f1b-139">It can be difficult to ensure that older SignalR clients aren't connecting to the server, so even if your SignalR server version is ASP.NET Core 3.0 or later, the `ConnectionId` shouldn't be exposed.</span></span>

## <a name="access-token-logging"></a><span data-ttu-id="65f1b-140">Registro de tokens de acceso</span><span class="sxs-lookup"><span data-stu-id="65f1b-140">Access token logging</span></span>

<span data-ttu-id="65f1b-141">Al utilizar WebSockets o eventos enviados por el servidor, el cliente del explorador envía el token de acceso en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="65f1b-141">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="65f1b-142">La recepción del token de acceso a través de la cadena de consulta suele ser segura con el encabezado de `Authorization` estándar.</span><span class="sxs-lookup"><span data-stu-id="65f1b-142">Receiving the access token via query string is generally secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="65f1b-143">Use siempre HTTPS para garantizar una conexión segura de un extremo a otro entre el cliente y el servidor.</span><span class="sxs-lookup"><span data-stu-id="65f1b-143">Always use HTTPS to ensure a secure end-to-end connection between the client and the server.</span></span> <span data-ttu-id="65f1b-144">Muchos servidores web registran la dirección URL para cada solicitud, incluida la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="65f1b-144">Many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="65f1b-145">El registro de las direcciones URL puede registrar el token de acceso.</span><span class="sxs-lookup"><span data-stu-id="65f1b-145">Logging the URLs may log the access token.</span></span> <span data-ttu-id="65f1b-146">ASP.NET Core registra la dirección URL de cada solicitud de forma predeterminada, que incluirá la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="65f1b-146">ASP.NET Core logs the URL for each request by default, which will include the query string.</span></span> <span data-ttu-id="65f1b-147">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="65f1b-147">For example:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

<span data-ttu-id="65f1b-148">Si tiene dudas sobre el registro de estos datos con los registros de servidor, puede deshabilitar este registro por completo configurando el registrador de `Microsoft.AspNetCore.Hosting` en el nivel de `Warning` o superior (estos mensajes se escriben en `Info` nivel).</span><span class="sxs-lookup"><span data-stu-id="65f1b-148">If you have concerns about logging this data with your server logs, you can disable this logging entirely by configuring the `Microsoft.AspNetCore.Hosting` logger to the `Warning` level or above (these messages are written at `Info` level).</span></span> <span data-ttu-id="65f1b-149">Para obtener más información, consulte [filtrado de registros](xref:fundamentals/logging/index#log-filtering) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="65f1b-149">For more information, see [Log Filtering](xref:fundamentals/logging/index#log-filtering) for more information.</span></span> <span data-ttu-id="65f1b-150">Si todavía desea registrar determinada información de la solicitud, puede [escribir un middleware](xref:fundamentals/middleware/write) para registrar los datos que necesita y filtrar el valor de la cadena de consulta `access_token` (si está presente).</span><span class="sxs-lookup"><span data-stu-id="65f1b-150">If you still want to log certain request information, you can [write a middleware](xref:fundamentals/middleware/write) to log the data you require and filter out the `access_token` query string value (if present).</span></span>

## <a name="exceptions"></a><span data-ttu-id="65f1b-151">Excepciones</span><span class="sxs-lookup"><span data-stu-id="65f1b-151">Exceptions</span></span>

<span data-ttu-id="65f1b-152">Los mensajes de excepción generalmente se consideran datos confidenciales que no se deben revelar a un cliente.</span><span class="sxs-lookup"><span data-stu-id="65f1b-152">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="65f1b-153">De forma predeterminada, SignalR no envía al cliente los detalles de una excepción producida por un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="65f1b-153">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="65f1b-154">En su lugar, el cliente recibe un mensaje genérico que indica que se ha producido un error.</span><span class="sxs-lookup"><span data-stu-id="65f1b-154">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="65f1b-155">La entrega de mensajes de excepción al cliente se puede invalidar (por ejemplo, en desarrollo o prueba) con [EnableDetailedErrors](xref:signalr/configuration#configure-server-options).</span><span class="sxs-lookup"><span data-stu-id="65f1b-155">Exception message delivery to the client can be overridden (for example in development or test) with [EnableDetailedErrors](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="65f1b-156">Los mensajes de excepción no deben exponerse al cliente en las aplicaciones de producción.</span><span class="sxs-lookup"><span data-stu-id="65f1b-156">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="65f1b-157">Administración de búfer</span><span class="sxs-lookup"><span data-stu-id="65f1b-157">Buffer management</span></span>

SignalR<span data-ttu-id="65f1b-158"> usa búferes por conexión para administrar los mensajes entrantes y salientes.</span><span class="sxs-lookup"><span data-stu-id="65f1b-158"> uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="65f1b-159">De forma predeterminada, SignalR limita estos búferes a 32 KB.</span><span class="sxs-lookup"><span data-stu-id="65f1b-159">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="65f1b-160">El mensaje más grande que un cliente o servidor puede enviar es 32 KB.</span><span class="sxs-lookup"><span data-stu-id="65f1b-160">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="65f1b-161">La memoria máxima consumida por una conexión para los mensajes es de 32 KB.</span><span class="sxs-lookup"><span data-stu-id="65f1b-161">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="65f1b-162">Si los mensajes son siempre menores que 32 KB, puede reducir el límite, que:</span><span class="sxs-lookup"><span data-stu-id="65f1b-162">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="65f1b-163">Impide que un cliente pueda enviar un mensaje más grande.</span><span class="sxs-lookup"><span data-stu-id="65f1b-163">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="65f1b-164">El servidor nunca tendrá que asignar búferes grandes para aceptar mensajes.</span><span class="sxs-lookup"><span data-stu-id="65f1b-164">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="65f1b-165">Si los mensajes son mayores de 32 KB, puede aumentar el límite.</span><span class="sxs-lookup"><span data-stu-id="65f1b-165">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="65f1b-166">Aumentar este límite significa:</span><span class="sxs-lookup"><span data-stu-id="65f1b-166">Increasing this limit means:</span></span>

* <span data-ttu-id="65f1b-167">El cliente puede hacer que el servidor asigne grandes búferes de memoria.</span><span class="sxs-lookup"><span data-stu-id="65f1b-167">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="65f1b-168">La asignación de servidores de búferes de gran tamaño puede reducir el número de conexiones simultáneas.</span><span class="sxs-lookup"><span data-stu-id="65f1b-168">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="65f1b-169">Hay límites para los mensajes entrantes y salientes, ambos se pueden configurar en el objeto [HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options) configurado en `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="65f1b-169">There are limits for incoming and outgoing messages, both can be configured on the [HttpConnectionDispatcherOptions](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="65f1b-170">`ApplicationMaxBufferSize` representa el número máximo de bytes del cliente que el servidor almacena en búfer.</span><span class="sxs-lookup"><span data-stu-id="65f1b-170">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="65f1b-171">Si el cliente intenta enviar un mensaje mayor que este límite, es posible que se cierre la conexión.</span><span class="sxs-lookup"><span data-stu-id="65f1b-171">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="65f1b-172">`TransportMaxBufferSize` representa el número máximo de bytes que el servidor puede enviar.</span><span class="sxs-lookup"><span data-stu-id="65f1b-172">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="65f1b-173">Si el servidor intenta enviar un mensaje (incluidos los valores devueltos de los métodos de concentrador) más grande que este límite, se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="65f1b-173">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="65f1b-174">Al establecer el límite en `0`, se deshabilita el límite.</span><span class="sxs-lookup"><span data-stu-id="65f1b-174">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="65f1b-175">Al quitar el límite, un cliente puede enviar un mensaje de cualquier tamaño.</span><span class="sxs-lookup"><span data-stu-id="65f1b-175">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="65f1b-176">Los clientes malintencionados que envían mensajes grandes pueden provocar que se asigne exceso de memoria.</span><span class="sxs-lookup"><span data-stu-id="65f1b-176">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="65f1b-177">El uso excesivo de memoria puede reducir significativamente el número de conexiones simultáneas.</span><span class="sxs-lookup"><span data-stu-id="65f1b-177">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
