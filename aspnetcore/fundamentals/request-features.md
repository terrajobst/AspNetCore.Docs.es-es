---
title: "Características de la solicitud de ASP.NET Core"
author: ardalis
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d1fbd23c-2ff9-4216-b908-0201ff3afb7c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: a10aefe3819fb03019575c36274dd164faf7086c
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="34cea-103">Características de la solicitud de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34cea-103">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="34cea-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="34cea-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="34cea-105">Detalles de implementación de servidor Web relacionados con las solicitudes HTTP y las respuestas se definen en las interfaces.</span><span class="sxs-lookup"><span data-stu-id="34cea-105">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="34cea-106">Estas interfaces son usadas por las implementaciones del servidor y middleware para crear y modificar la canalización de hospedaje de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="34cea-106">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="34cea-107">Interfaces de característica</span><span class="sxs-lookup"><span data-stu-id="34cea-107">Feature interfaces</span></span>

<span data-ttu-id="34cea-108">Define el número de interfaces de la característica HTTP en ASP.NET Core `Microsoft.AspNetCore.Http.Features` servidores que se usan para identificar las características que admiten.</span><span class="sxs-lookup"><span data-stu-id="34cea-108">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="34cea-109">Las siguientes interfaces de característica controlen las solicitudes y devuelven respuestas:</span><span class="sxs-lookup"><span data-stu-id="34cea-109">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="34cea-110">`IHttpRequestFeature`Define la estructura de una solicitud HTTP, incluidos el protocolo, ruta de acceso, cadena de consulta, encabezados y el cuerpo.</span><span class="sxs-lookup"><span data-stu-id="34cea-110">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="34cea-111">`IHttpResponseFeature`Define la estructura de una respuesta HTTP, incluido el código de estado, los encabezados y cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="34cea-111">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="34cea-112">`IHttpAuthenticationFeature`Define la compatibilidad para identificar a los usuarios en función de un `ClaimsPrincipal` y la especificación de un controlador de autenticación.</span><span class="sxs-lookup"><span data-stu-id="34cea-112">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="34cea-113">`IHttpUpgradeFeature`Define la compatibilidad con [HTTP actualizaciones](https://tools.ietf.org/html/rfc2616.html#section-14.42), que permiten al cliente para especificar que otros protocolos le gustaría utilizar si el servidor que desea cambiar los protocolos.</span><span class="sxs-lookup"><span data-stu-id="34cea-113">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="34cea-114">`IHttpBufferingFeature`Define métodos para deshabilitar el almacenamiento en búfer de solicitudes o respuestas.</span><span class="sxs-lookup"><span data-stu-id="34cea-114">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="34cea-115">`IHttpConnectionFeature`Define propiedades de puertos y las direcciones locales y remotas.</span><span class="sxs-lookup"><span data-stu-id="34cea-115">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="34cea-116">`IHttpRequestLifetimeFeature`Define la compatibilidad para anular las conexiones o detectar si una solicitud ha finalizado antes de tiempo, tal como una desconexión de cliente.</span><span class="sxs-lookup"><span data-stu-id="34cea-116">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="34cea-117">`IHttpSendFileFeature`Define un método para enviar archivos de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="34cea-117">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="34cea-118">`IHttpWebSocketFeature`Define una API para admitir los sockets web.</span><span class="sxs-lookup"><span data-stu-id="34cea-118">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="34cea-119">`IHttpRequestIdentifierFeature`Agrega una propiedad que se puede implementar para identificar de forma exclusiva las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="34cea-119">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="34cea-120">`ISessionFeature`Define `ISessionFactory` y `ISession` abstracciones para admitir las sesiones de usuario.</span><span class="sxs-lookup"><span data-stu-id="34cea-120">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="34cea-121">`ITlsConnectionFeature`Define una API para recuperar los certificados de cliente.</span><span class="sxs-lookup"><span data-stu-id="34cea-121">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="34cea-122">`ITlsTokenBindingFeature`Define métodos para trabajar con parámetros de enlace del token TLS.</span><span class="sxs-lookup"><span data-stu-id="34cea-122">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="34cea-123">`ISessionFeature`no es una característica de servidor, pero se implementa mediante el `SessionMiddleware` (consulte [administrar el estado de aplicación](app-state.md)).</span><span class="sxs-lookup"><span data-stu-id="34cea-123">`ISessionFeature` is not a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="34cea-124">Colecciones de característica</span><span class="sxs-lookup"><span data-stu-id="34cea-124">Feature collections</span></span>

<span data-ttu-id="34cea-125">El `Features` propiedad de `HttpContext` proporciona una interfaz para obtener y establecer las características HTTP disponibles para la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="34cea-125">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="34cea-126">Puesto que la colección de características es mutable incluso en el contexto de una solicitud, middleware puede usarse para modificar la colección y agregar compatibilidad con características adicionales.</span><span class="sxs-lookup"><span data-stu-id="34cea-126">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="34cea-127">Características de middleware y solicitud</span><span class="sxs-lookup"><span data-stu-id="34cea-127">Middleware and request features</span></span>

<span data-ttu-id="34cea-128">Mientras que los servidores son responsables de crear la colección de características, middleware puede agregar a esta colección y utilizar las características de la colección.</span><span class="sxs-lookup"><span data-stu-id="34cea-128">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="34cea-129">Por ejemplo, el `StaticFileMiddleware` tiene acceso a la `IHttpSendFileFeature` característica.</span><span class="sxs-lookup"><span data-stu-id="34cea-129">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="34cea-130">Si la característica no existe, se utiliza para enviar el archivo estático solicitado desde su ruta de acceso física.</span><span class="sxs-lookup"><span data-stu-id="34cea-130">If the feature exists, it is used to send the requested static file from its physical path.</span></span> <span data-ttu-id="34cea-131">En caso contrario, se usa un método alternativo más lento para enviar el archivo.</span><span class="sxs-lookup"><span data-stu-id="34cea-131">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="34cea-132">Si está disponible, el `IHttpSendFileFeature` permite al sistema operativo abrir el archivo y realizar una copia de modo kernel directa a la tarjeta de red.</span><span class="sxs-lookup"><span data-stu-id="34cea-132">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="34cea-133">Además, puede agregar middleware a la colección de características establecida por el servidor.</span><span class="sxs-lookup"><span data-stu-id="34cea-133">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="34cea-134">Las características existentes incluso pueden reemplazarse por el middleware, lo que permite el middleware para aumentar la funcionalidad del servidor.</span><span class="sxs-lookup"><span data-stu-id="34cea-134">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="34cea-135">Características agregadas a la colección están disponibles inmediatamente para otro middleware o la propia aplicación subyacente más adelante en la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="34cea-135">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="34cea-136">Mediante la combinación de las implementaciones de servidor personalizado y mejoras de middleware específico, se puede construir el conjunto de características que requiere una aplicación.</span><span class="sxs-lookup"><span data-stu-id="34cea-136">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="34cea-137">Esto permite que falta características se agreguen sin necesidad de un cambio en el servidor y se asegura de que se exponen solo la mínima cantidad de características, lo que permite limitar ataque expuesta área y mejorar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="34cea-137">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="34cea-138">Resumen</span><span class="sxs-lookup"><span data-stu-id="34cea-138">Summary</span></span>

<span data-ttu-id="34cea-139">Interfaces de característica definen características específicas de HTTP que puede admitir una solicitud determinada.</span><span class="sxs-lookup"><span data-stu-id="34cea-139">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="34cea-140">Servidores definen colecciones de características y el conjunto inicial de las características admitidas por el servidor, pero middleware puede usarse para mejorar estas características.</span><span class="sxs-lookup"><span data-stu-id="34cea-140">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="34cea-141">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="34cea-141">Additional Resources</span></span>

* [<span data-ttu-id="34cea-142">Servidores</span><span class="sxs-lookup"><span data-stu-id="34cea-142">Servers</span></span>](servers/index.md)

* [<span data-ttu-id="34cea-143">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="34cea-143">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="34cea-144">Abrir la interfaz Web para .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="34cea-144">Open Web Interface for .NET (OWIN)</span></span>](owin.md)
