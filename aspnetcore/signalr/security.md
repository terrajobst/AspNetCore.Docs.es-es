---
title: Consideraciones de seguridad en ASP.NET Core SignalR
author: rachelappel
description: Obtenga información sobre cómo utilizar la autenticación y autorización en ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: eff4542b88f24dd6c1c0675f56874e368d441fdd
ms.sourcegitcommit: 32626efaa7316c9b283c96be6516e637d548c5e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39028487"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="0e827-103">Consideraciones de seguridad en ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="0e827-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="0e827-104">Por [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="0e827-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

## <a name="overview"></a><span data-ttu-id="0e827-105">Información general</span><span class="sxs-lookup"><span data-stu-id="0e827-105">Overview</span></span>

<span data-ttu-id="0e827-106">SignalR proporciona una serie de protecciones de seguridad de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0e827-106">SignalR provides a number of security protections by default.</span></span> <span data-ttu-id="0e827-107">Es importante entender cómo configurar estas protecciones.</span><span class="sxs-lookup"><span data-stu-id="0e827-107">It's important to understand how to configure these protections.</span></span>

### <a name="cross-origin-resource-sharing"></a><span data-ttu-id="0e827-108">Uso compartido de recursos entre orígenes</span><span class="sxs-lookup"><span data-stu-id="0e827-108">Cross-origin resource sharing</span></span>

<span data-ttu-id="0e827-109">[Los recursos entre orígenes (CORS) de uso compartido](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) puede utilizarse para permitir que las conexiones de origen cruzado SignalR en el explorador.</span><span class="sxs-lookup"><span data-stu-id="0e827-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="0e827-110">Si el código de JavaScript está hospedado en otro nombre de dominio de la aplicación de SignalR, tendrá que habilitar el [middleware de ASP.NET Core CORS](xref:security/cors) con el fin de permitir la conexión.</span><span class="sxs-lookup"><span data-stu-id="0e827-110">If your JavaScript code is hosted on a different domain name from your SignalR app, you have to enable the [ASP.NET Core CORS middleware](xref:security/cors) in order to allow the connection.</span></span> <span data-ttu-id="0e827-111">En general, permitir solicitudes entre orígenes solo desde los dominios que controla.</span><span class="sxs-lookup"><span data-stu-id="0e827-111">In general, allow cross-origin requests only from domains you control.</span></span> <span data-ttu-id="0e827-112">Por ejemplo, si su sitio está hospedado en `http://www.example.com` y la aplicación de SignalR se hospeda en `http://signalr.example.com`, debe configurar CORS en la aplicación de SignalR para permitir únicamente el origen `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="0e827-112">For example, if your site is hosted on `http://www.example.com` and your SignalR app is hosted on `http://signalr.example.com`, you should configure CORS in your SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="0e827-113">Para obtener más información sobre cómo configurar la CORS, vea [la documentación sobre ASP.NET Core CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="0e827-113">For more information on configuring CORS, see [the documentation on ASP.NET Core CORS](xref:security/cors).</span></span> <span data-ttu-id="0e827-114">SignalR requiere las siguientes directivas CORS para poder funcionar correctamente:</span><span class="sxs-lookup"><span data-stu-id="0e827-114">SignalR requires the following CORS policies in order to operate correctly:</span></span>

* <span data-ttu-id="0e827-115">La directiva debe permitir los orígenes específicos previsto o permitir que cualquier origen (no recomendado).</span><span class="sxs-lookup"><span data-stu-id="0e827-115">The policy must allow the specific origins you expect, or allow any origin (not recommended).</span></span>
* <span data-ttu-id="0e827-116">Métodos HTTP `GET` y `POST` debe estar permitido.</span><span class="sxs-lookup"><span data-stu-id="0e827-116">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="0e827-117">Deben habilitarse las credenciales, incluso cuando no está usando la autenticación.</span><span class="sxs-lookup"><span data-stu-id="0e827-117">Credentials must be enabled, even when you aren't using authentication.</span></span>

<span data-ttu-id="0e827-118">Por ejemplo, la siguiente directiva CORS permite hospedado en un cliente del explorador SignalR `http://example.com` para tener acceso a la aplicación de SignalR:</span><span class="sxs-lookup"><span data-stu-id="0e827-118">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access your SignalR app:</span></span>

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
> <span data-ttu-id="0e827-119">SignalR no es compatible con la característica CORS integrada en Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="0e827-119">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

### <a name="access-token-logging"></a><span data-ttu-id="0e827-120">Registro de token de acceso</span><span class="sxs-lookup"><span data-stu-id="0e827-120">Access token logging</span></span>

<span data-ttu-id="0e827-121">Al usar WebSockets o los eventos, el explorador del cliente envía el token de acceso en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="0e827-121">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="0e827-122">Esto es generalmente tan seguro como usar el estándar `Authorization` encabezado, sin embargo, la dirección URL para cada solicitud de registro de muchos servidores web, incluida la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="0e827-122">This is generally as secure as using the standard `Authorization` header, however many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="0e827-123">Esto significa que el token de acceso que puede incluirse en los registros.</span><span class="sxs-lookup"><span data-stu-id="0e827-123">This means the access token may be included in logs.</span></span> <span data-ttu-id="0e827-124">Considere la posibilidad de revisar la configuración de registro del servidor web para evitar esta información de registro.</span><span class="sxs-lookup"><span data-stu-id="0e827-124">Consider reviewing the web server's logging settings to avoid logging this information.</span></span>

### <a name="exceptions"></a><span data-ttu-id="0e827-125">Excepciones</span><span class="sxs-lookup"><span data-stu-id="0e827-125">Exceptions</span></span>

<span data-ttu-id="0e827-126">Los mensajes de excepción normalmente se consideran información confidencial que no debe mostrarse a un cliente.</span><span class="sxs-lookup"><span data-stu-id="0e827-126">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="0e827-127">De forma predeterminada, SignalR no envía los detalles de una excepción producida por un método de concentrador al cliente.</span><span class="sxs-lookup"><span data-stu-id="0e827-127">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="0e827-128">En su lugar, el cliente recibe un mensaje genérico que indica que un error.</span><span class="sxs-lookup"><span data-stu-id="0e827-128">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="0e827-129">Puede invalidar este comportamiento estableciendo el [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) configuración.</span><span class="sxs-lookup"><span data-stu-id="0e827-129">You can override this behavior by setting the [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options) setting.</span></span>

### <a name="buffer-management"></a><span data-ttu-id="0e827-130">Administración de búfer</span><span class="sxs-lookup"><span data-stu-id="0e827-130">Buffer management</span></span>

<span data-ttu-id="0e827-131">SignalR usa búferes por conexión con el fin de administrar los mensajes entrantes y salientes.</span><span class="sxs-lookup"><span data-stu-id="0e827-131">SignalR uses per-connection buffers in order to manage incoming and outgoing messages.</span></span> <span data-ttu-id="0e827-132">De forma predeterminada, SignalR limita estos búferes a 32KB.</span><span class="sxs-lookup"><span data-stu-id="0e827-132">By default, SignalR limits these buffers to 32KB.</span></span> <span data-ttu-id="0e827-133">Esto significa que el mensaje posible más grande que puede enviar un cliente o servidor es 32KB.</span><span class="sxs-lookup"><span data-stu-id="0e827-133">This means the largest possible message a client or server can send is 32KB.</span></span> <span data-ttu-id="0e827-134">Esto también significa que la cantidad máxima de memoria consumida por una conexión para los mensajes es 32KB.</span><span class="sxs-lookup"><span data-stu-id="0e827-134">This also means the maximum amount of memory consumed by a connection for messages is 32KB.</span></span> <span data-ttu-id="0e827-135">Si conoce que los mensajes siempre son menores que este límite, puede reducir este tamaño para impedir que un cliente puede enviar un mensaje mayor y forzar al servidor para asignar memoria para aceptarla.</span><span class="sxs-lookup"><span data-stu-id="0e827-135">If you know your messages are always smaller than this limit, you can reduce this size to prevent a client from being able to send a larger message and force the server to allocate memory to accept it.</span></span> <span data-ttu-id="0e827-136">De forma similar, si sabe que los mensajes son mayores que este límite, se puede aumentar.</span><span class="sxs-lookup"><span data-stu-id="0e827-136">Similarly, if you know your messages are larger than this limit, you can increase it.</span></span> <span data-ttu-id="0e827-137">Sin embargo, tenga en cuenta que al aumentar este límite significa que el cliente es capaz de hacer que el servidor asignar memoria adicional y puede reducir el número de conexiones simultáneas que puede controlar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0e827-137">However, be aware that increasing this limit means that the client is able to cause the server to allocate additional memory and may reduce the number of concurrent connections your app can handle.</span></span>

<span data-ttu-id="0e827-138">Hay límites independientes para mensajes entrantes y salientes, ambos se pueden configurar en el [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) configurado en el objeto `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="0e827-138">There are separate limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="0e827-139">`ApplicationMaxBufferSize` representa el número máximo de bytes desde el cliente que los búferes del servidor.</span><span class="sxs-lookup"><span data-stu-id="0e827-139">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="0e827-140">Si el cliente intenta enviar un mensaje supere ese límite, se puede cerrar la conexión.</span><span class="sxs-lookup"><span data-stu-id="0e827-140">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="0e827-141">`TransportMaxBufferSize` representa el número máximo de bytes que el servidor puede enviar.</span><span class="sxs-lookup"><span data-stu-id="0e827-141">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="0e827-142">Si el servidor intenta enviar un mensaje (incluye los valores devueltos de métodos de concentrador) supera este límite, se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="0e827-142">If the server attempts to send a message (includes return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="0e827-143">Establecer el límite en `0` deshabilita totalmente el límite.</span><span class="sxs-lookup"><span data-stu-id="0e827-143">Setting the limit to `0` disables the limit entirely.</span></span> <span data-ttu-id="0e827-144">Sin embargo, esto debe hacerse con sumo cuidado.</span><span class="sxs-lookup"><span data-stu-id="0e827-144">However, this should be done with extreme caution.</span></span> <span data-ttu-id="0e827-145">Quitar el límite permite que un cliente enviar un mensaje de cualquier tamaño.</span><span class="sxs-lookup"><span data-stu-id="0e827-145">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="0e827-146">Esto se puede utilizar un cliente malintencionado para provocar un exceso de memoria que se asignen, lo que podría disminuir considerablemente el número de conexiones simultáneas que puede admitir la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0e827-146">This could be used by a malicious client to cause excess memory to be allocated, which could dramatically reduce the number of concurrent connections your app can support.</span></span>
