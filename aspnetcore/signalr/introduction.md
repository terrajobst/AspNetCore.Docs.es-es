---
title: Introducción a ASP.NET Core SignalR
author: rachelappel
description: Obtenga información acerca de cómo la biblioteca ASP.NET Core SignalR simplifica la adición de funcionalidad en tiempo real a las aplicaciones.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction
ms.openlocfilehash: f05b7cbf05372dc5d5cdadaf5a534d7a9d9bfecc
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2018
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="6cf16-103">Introducción a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="6cf16-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="6cf16-104">Por [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="6cf16-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="6cf16-105">¿Qué es SignalR?</span><span class="sxs-lookup"><span data-stu-id="6cf16-105">What is SignalR?</span></span>

<span data-ttu-id="6cf16-106">Núcleo de ASP.NET SignalR es una biblioteca que simplifica la agregación de funcionalidades de web en tiempo real a las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="6cf16-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="6cf16-107">La funcionalidad web en tiempo real permite que el código de servidor para el contenido de inserción a los clientes al instante.</span><span class="sxs-lookup"><span data-stu-id="6cf16-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="6cf16-108">Buenos candidatos para SignalR:</span><span class="sxs-lookup"><span data-stu-id="6cf16-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="6cf16-109">Aplicaciones que requieren alta frecuencia actualizaciones desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="6cf16-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="6cf16-110">Se trata de juegos, redes sociales, votar, subasta, mapas y aplicaciones GPS.</span><span class="sxs-lookup"><span data-stu-id="6cf16-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="6cf16-111">Los paneles y supervisión de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="6cf16-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="6cf16-112">Los ejemplos incluyen paneles de la empresa, las actualizaciones de ventas instantáneas, o alertas de viaje.</span><span class="sxs-lookup"><span data-stu-id="6cf16-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="6cf16-113">Aplicaciones de colaboración.</span><span class="sxs-lookup"><span data-stu-id="6cf16-113">Collaborative apps.</span></span> <span data-ttu-id="6cf16-114">Aplicaciones de pizarra y reunión software de equipo son ejemplos de aplicaciones de colaboración.</span><span class="sxs-lookup"><span data-stu-id="6cf16-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="6cf16-115">Aplicaciones que requieren las notificaciones.</span><span class="sxs-lookup"><span data-stu-id="6cf16-115">Apps that require notifications.</span></span> <span data-ttu-id="6cf16-116">Las redes sociales, correo electrónico, chat, juegos, alertas de viaje y muchas otras aplicaciones utilizan notificaciones.</span><span class="sxs-lookup"><span data-stu-id="6cf16-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="6cf16-117">SignalR proporciona una API para la creación de servidor a cliente [llamadas a procedimiento remoto (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="6cf16-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="6cf16-118">Las RPC llamar a funciones de JavaScript en los clientes desde el código de .NET Core en el servidor de.</span><span class="sxs-lookup"><span data-stu-id="6cf16-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="6cf16-119">SignalR principales de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="6cf16-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="6cf16-120">Controla automáticamente la administración de conexiones.</span><span class="sxs-lookup"><span data-stu-id="6cf16-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="6cf16-121">Permite difundir mensajes a todos los clientes conectados al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="6cf16-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="6cf16-122">Por ejemplo, un salón de chat.</span><span class="sxs-lookup"><span data-stu-id="6cf16-122">For example, a chat room.</span></span>
* <span data-ttu-id="6cf16-123">Habilita el envío de mensajes a clientes específicos o grupos de clientes.</span><span class="sxs-lookup"><span data-stu-id="6cf16-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="6cf16-124">Es código abierto en [GitHub](https://github.com/aspnet/signalr).</span><span class="sxs-lookup"><span data-stu-id="6cf16-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="6cf16-125">Escalable.</span><span class="sxs-lookup"><span data-stu-id="6cf16-125">Scalable.</span></span>

<span data-ttu-id="6cf16-126">La conexión entre el cliente y el servidor es persistente, a diferencia de una conexión HTTP.</span><span class="sxs-lookup"><span data-stu-id="6cf16-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="6cf16-127">Transportes</span><span class="sxs-lookup"><span data-stu-id="6cf16-127">Transports</span></span>

<span data-ttu-id="6cf16-128">Resúmenes de SignalR a través de una serie de técnicas para crear aplicaciones web en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="6cf16-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="6cf16-129">[WebSockets](https://tools.ietf.org/html/rfc7118) es el transporte óptimo, pero se pueden usar otras técnicas como eventos de Server-Sent y sondeo largo cuando los que no están disponibles.</span><span class="sxs-lookup"><span data-stu-id="6cf16-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="6cf16-130">SignalR detectará automáticamente e inicializar el transporte adecuado en función de las características admitidas en el servidor y el cliente.</span><span class="sxs-lookup"><span data-stu-id="6cf16-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="6cf16-131">Concentradores</span><span class="sxs-lookup"><span data-stu-id="6cf16-131">Hubs</span></span>

<span data-ttu-id="6cf16-132">SignalR usa centros para la comunicación entre clientes y servidores.</span><span class="sxs-lookup"><span data-stu-id="6cf16-132">SignalR uses hubs to communicate between clients and servers.</span></span>

<span data-ttu-id="6cf16-133">Un concentrador es una canalización de alto nivel que permite que el cliente y servidor para llamar a métodos en entre sí.</span><span class="sxs-lookup"><span data-stu-id="6cf16-133">A hub is a high-level pipeline that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="6cf16-134">SignalR administra el envío a través de los límites del equipo automáticamente, permitiendo a los clientes llamar a métodos en el servidor como fácilmente como métodos locales y viceversa.</span><span class="sxs-lookup"><span data-stu-id="6cf16-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="6cf16-135">Los concentradores permiten pasar parámetros fuertemente tipados a métodos, lo que permite el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="6cf16-135">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="6cf16-136">SignalR proporciona dos protocolos de concentrador integrada: un protocolo de texto en función de JSON y un protocolo binario basado en [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="6cf16-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="6cf16-137">Por lo general, MessagePack crea mensajes más pequeños que cuando se usan JSON.</span><span class="sxs-lookup"><span data-stu-id="6cf16-137">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="6cf16-138">Deben ser compatible con los exploradores más antiguos [nivel XHR 2](https://caniuse.com/#feat=xhr2) para proporcionar compatibilidad con el protocolo MessagePack.</span><span class="sxs-lookup"><span data-stu-id="6cf16-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="6cf16-139">Los centros de llamar a código de cliente mediante el envío de mensajes mediante el transporte activo.</span><span class="sxs-lookup"><span data-stu-id="6cf16-139">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="6cf16-140">Los mensajes contienen el nombre y los parámetros del método de cliente.</span><span class="sxs-lookup"><span data-stu-id="6cf16-140">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="6cf16-141">Objetos que se envíen como parámetros de método se deserializan mediante el protocolo configurado.</span><span class="sxs-lookup"><span data-stu-id="6cf16-141">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="6cf16-142">El cliente intenta hacer coincidir el nombre a un método en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="6cf16-142">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="6cf16-143">Cuando se produce una coincidencia, el método de cliente se ejecuta con los datos del parámetro deserializado.</span><span class="sxs-lookup"><span data-stu-id="6cf16-143">When a match happens, the client method runs using the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6cf16-144">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="6cf16-144">Additional resources</span></span>

* [<span data-ttu-id="6cf16-145">Empezar a trabajar con SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6cf16-145">Get started with SignalR for ASP.NET Core</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="6cf16-146">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="6cf16-146">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="6cf16-147">Concentradores</span><span class="sxs-lookup"><span data-stu-id="6cf16-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="6cf16-148">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="6cf16-148">JavaScript client</span></span>](xref:signalr/javascript-client)
