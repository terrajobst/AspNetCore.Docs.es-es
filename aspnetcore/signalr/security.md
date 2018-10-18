---
title: Consideraciones de seguridad en ASP.NET Core SignalR
author: tdykstra
description: Obtenga información sobre cómo utilizar la autenticación y autorización en ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: 98b5eb7be87920aacf7a941f76ff652ae7905303
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391263"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="0ee0f-103">Consideraciones de seguridad en ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="0ee0f-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="0ee0f-104">Por [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="0ee0f-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

## <a name="overview"></a><span data-ttu-id="0ee0f-105">Información general</span><span class="sxs-lookup"><span data-stu-id="0ee0f-105">Overview</span></span>

<span data-ttu-id="0ee0f-106">SignalR proporciona una serie de protecciones de seguridad de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-106">SignalR provides a number of security protections by default.</span></span> <span data-ttu-id="0ee0f-107">Es importante entender cómo configurar estas protecciones.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-107">It's important to understand how to configure these protections.</span></span>

### <a name="cross-origin-resource-sharing"></a><span data-ttu-id="0ee0f-108">Uso compartido de recursos entre orígenes</span><span class="sxs-lookup"><span data-stu-id="0ee0f-108">Cross-origin resource sharing</span></span>

<span data-ttu-id="0ee0f-109">[Los recursos entre orígenes (CORS) de uso compartido](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) puede utilizarse para permitir que las conexiones de origen cruzado SignalR en el explorador.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="0ee0f-110">Si el código de JavaScript está hospedado en otro nombre de dominio de la aplicación de SignalR, tendrá que habilitar el [middleware de ASP.NET Core CORS](xref:security/cors) con el fin de permitir la conexión.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-110">If your JavaScript code is hosted on a different domain name from your SignalR app, you have to enable the [ASP.NET Core CORS middleware](xref:security/cors) in order to allow the connection.</span></span> <span data-ttu-id="0ee0f-111">En general, permitir solicitudes entre orígenes solo desde los dominios que controla.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-111">In general, allow cross-origin requests only from domains you control.</span></span> <span data-ttu-id="0ee0f-112">Por ejemplo, si su sitio está hospedado en `http://www.example.com` y la aplicación de SignalR se hospeda en `http://signalr.example.com`, debe configurar CORS en la aplicación de SignalR para permitir únicamente el origen `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-112">For example, if your site is hosted on `http://www.example.com` and your SignalR app is hosted on `http://signalr.example.com`, you should configure CORS in your SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="0ee0f-113">Para obtener más información sobre cómo configurar la CORS, vea [la documentación sobre ASP.NET Core CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="0ee0f-113">For more information on configuring CORS, see [the documentation on ASP.NET Core CORS](xref:security/cors).</span></span> <span data-ttu-id="0ee0f-114">SignalR requiere las siguientes directivas CORS para poder funcionar correctamente:</span><span class="sxs-lookup"><span data-stu-id="0ee0f-114">SignalR requires the following CORS policies in order to operate correctly:</span></span>

* <span data-ttu-id="0ee0f-115">La directiva debe permitir los orígenes específicos previsto o permitir que cualquier origen (no recomendado).</span><span class="sxs-lookup"><span data-stu-id="0ee0f-115">The policy must allow the specific origins you expect, or allow any origin (not recommended).</span></span>
* <span data-ttu-id="0ee0f-116">Métodos HTTP `GET` y `POST` debe estar permitido.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-116">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="0ee0f-117">Deben habilitarse las credenciales, incluso cuando no está usando la autenticación.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-117">Credentials must be enabled, even when you aren't using authentication.</span></span>

<span data-ttu-id="0ee0f-118">Por ejemplo, la siguiente directiva CORS permite hospedado en un cliente del explorador SignalR `http://example.com` para tener acceso a la aplicación de SignalR:</span><span class="sxs-lookup"><span data-stu-id="0ee0f-118">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access your SignalR app:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="0ee0f-119">SignalR no es compatible con la característica CORS integrada en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-119">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="0ee0f-120">Restricción de origen de WebSocket</span><span class="sxs-lookup"><span data-stu-id="0ee0f-120">WebSocket Origin Restriction</span></span>

<span data-ttu-id="0ee0f-121">Las protecciones proporcionadas por la CORS no se aplican a WebSockets.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-121">The protections provided by CORS do not apply to WebSockets.</span></span> <span data-ttu-id="0ee0f-122">Los exploradores no realizan solicitudes preparatorias CORS, ni tampoco se respetan las restricciones especificadas en `Access-Control` encabezados al realizar solicitudes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-122">Browsers do not perform CORS pre-flight requests, nor do they respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span> <span data-ttu-id="0ee0f-123">Sin embargo, los exploradores envían el `Origin` encabezado al emitir solicitudes de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-123">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="0ee0f-124">Debe configurar la aplicación para validar estos encabezados para asegurarse de que solo WebSockets procedentes de los orígenes que se espera que se permiten.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-124">You should configure your application to validate these headers in order to ensure that only WebSockets coming from the origins you expect are allowed.</span></span>

<span data-ttu-id="0ee0f-125">En ASP.NET Core 2.1, esto puede lograrse mediante un software intermedio personalizado puede colocar **anteriormente `UseSignalR`y cualquier middleware de autenticación** en su `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="0ee0f-125">In ASP.NET Core 2.1, this can be achieved using a custom middleware you can place **above `UseSignalR`, and any authentication middleware** in your `Configure` method:</span></span>

```csharp
// In your Startup class, add a static field listing the allowed Origin values:
private static readonly HashSet<string> _allowedOrigins = new HashSet<string>()
{
    // Add allowed origins here. For example:
    "http://www.mysite.com",
    "http://mysite.com",
};

// In your Configure method:
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Validate Origin header on WebSocket requests to prevent unexpected cross-site WebSocket requests
    app.Use((context, next) =>
    {
        // Check for a WebSocket request.
        if(string.Equals(context.Request.Headers["Upgrade"], "websocket"))
        {
            var origin = context.Request.Headers["Origin"];

            // If there is no origin header, or if the origin header doesn't match an allowed value:
            if(string.IsNullOrEmpty(origin) && !_allowedOrigins.Contains(origin))
            {
                // The origin is not allowed, reject the request
                context.Response.StatusCode = StatusCodes.Status400BadRequest;
                return Task.CompletedTask;
            }
        }

        // The request is not a WebSocket request or is a valid Origin, so let it continue
        return next();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="0ee0f-126">El `Origin` encabezado completamente está controlado por el cliente y, al igual que el `Referer` encabezado, se pueden falsificar.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-126">The `Origin` header is completely controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="0ee0f-127">Estos encabezados nunca deben utilizarse como mecanismo de autenticación.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-127">These headers should never be used as an authentication mechanism.</span></span>

### <a name="access-token-logging"></a><span data-ttu-id="0ee0f-128">Registro de token de acceso</span><span class="sxs-lookup"><span data-stu-id="0ee0f-128">Access token logging</span></span>

<span data-ttu-id="0ee0f-129">Al usar WebSockets o los eventos, el explorador del cliente envía el token de acceso en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-129">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="0ee0f-130">Esto es generalmente tan seguro como usar el estándar `Authorization` encabezado, sin embargo, la dirección URL para cada solicitud de registro de muchos servidores web, incluida la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-130">This is generally as secure as using the standard `Authorization` header, however many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="0ee0f-131">Esto significa que el token de acceso que puede incluirse en los registros.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-131">This means the access token may be included in logs.</span></span> <span data-ttu-id="0ee0f-132">Considere la posibilidad de revisar la configuración de registro del servidor web para evitar esta información de registro.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-132">Consider reviewing the web server's logging settings to avoid logging this information.</span></span>

### <a name="exceptions"></a><span data-ttu-id="0ee0f-133">Excepciones</span><span class="sxs-lookup"><span data-stu-id="0ee0f-133">Exceptions</span></span>

<span data-ttu-id="0ee0f-134">Los mensajes de excepción normalmente se consideran información confidencial que no debe mostrarse a un cliente.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-134">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="0ee0f-135">De forma predeterminada, SignalR no envía los detalles de una excepción producida por un método de concentrador al cliente.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-135">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="0ee0f-136">En su lugar, el cliente recibe un mensaje genérico que indica que un error.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-136">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="0ee0f-137">Puede invalidar este comportamiento estableciendo el [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) configuración.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-137">You can override this behavior by setting the [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options) setting.</span></span>

### <a name="buffer-management"></a><span data-ttu-id="0ee0f-138">Administración de búfer</span><span class="sxs-lookup"><span data-stu-id="0ee0f-138">Buffer management</span></span>

<span data-ttu-id="0ee0f-139">SignalR usa búferes por conexión con el fin de administrar los mensajes entrantes y salientes.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-139">SignalR uses per-connection buffers in order to manage incoming and outgoing messages.</span></span> <span data-ttu-id="0ee0f-140">De forma predeterminada, SignalR limita estos búferes a 32KB.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-140">By default, SignalR limits these buffers to 32KB.</span></span> <span data-ttu-id="0ee0f-141">Esto significa que el mensaje posible más grande que puede enviar un cliente o servidor es 32KB.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-141">This means the largest possible message a client or server can send is 32KB.</span></span> <span data-ttu-id="0ee0f-142">Esto también significa que la cantidad máxima de memoria consumida por una conexión para los mensajes es 32KB.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-142">This also means the maximum amount of memory consumed by a connection for messages is 32KB.</span></span> <span data-ttu-id="0ee0f-143">Si conoce que los mensajes siempre son menores que este límite, puede reducir este tamaño para impedir que un cliente puede enviar un mensaje mayor y forzar al servidor para asignar memoria para aceptarla.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-143">If you know your messages are always smaller than this limit, you can reduce this size to prevent a client from being able to send a larger message and force the server to allocate memory to accept it.</span></span> <span data-ttu-id="0ee0f-144">De forma similar, si sabe que los mensajes son mayores que este límite, se puede aumentar.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-144">Similarly, if you know your messages are larger than this limit, you can increase it.</span></span> <span data-ttu-id="0ee0f-145">Sin embargo, tenga en cuenta que al aumentar este límite significa que el cliente es capaz de hacer que el servidor asignar memoria adicional y puede reducir el número de conexiones simultáneas que puede controlar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-145">However, be aware that increasing this limit means that the client is able to cause the server to allocate additional memory and may reduce the number of concurrent connections your app can handle.</span></span>

<span data-ttu-id="0ee0f-146">Hay límites independientes para mensajes entrantes y salientes, ambos se pueden configurar en el [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) configurado en el objeto `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="0ee0f-146">There are separate limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="0ee0f-147">`ApplicationMaxBufferSize` representa el número máximo de bytes desde el cliente que los búferes del servidor.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-147">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="0ee0f-148">Si el cliente intenta enviar un mensaje supere ese límite, se puede cerrar la conexión.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-148">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="0ee0f-149">`TransportMaxBufferSize` representa el número máximo de bytes que el servidor puede enviar.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-149">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="0ee0f-150">Si el servidor intenta enviar un mensaje (incluye los valores devueltos de métodos de concentrador) supera este límite, se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-150">If the server attempts to send a message (includes return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="0ee0f-151">Establecer el límite en `0` deshabilita totalmente el límite.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-151">Setting the limit to `0` disables the limit entirely.</span></span> <span data-ttu-id="0ee0f-152">Sin embargo, esto debe hacerse con sumo cuidado.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-152">However, this should be done with extreme caution.</span></span> <span data-ttu-id="0ee0f-153">Quitar el límite permite que un cliente enviar un mensaje de cualquier tamaño.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-153">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="0ee0f-154">Esto se puede utilizar un cliente malintencionado para provocar un exceso de memoria que se asignen, lo que podría disminuir considerablemente el número de conexiones simultáneas que puede admitir la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0ee0f-154">This could be used by a malicious client to cause excess memory to be allocated, which could dramatically reduce the number of concurrent connections your app can support.</span></span>
