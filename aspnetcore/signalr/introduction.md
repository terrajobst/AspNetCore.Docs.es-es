---
title: Introducción a ASP.NET Core SignalR
author: bradygaster
description: Obtenga información sobre cómo la biblioteca de ASP.NET Core SignalR simplifica la adición de funcionalidad en tiempo real a las aplicaciones.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 673efafce60dfa46cb99f9537fda2bca42bf9822
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892252"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="91123-103">Introducción a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="91123-103">Introduction to ASP.NET Core SignalR</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="91123-104">¿Qué es SignalR?</span><span class="sxs-lookup"><span data-stu-id="91123-104">What is SignalR?</span></span>

<span data-ttu-id="91123-105">ASP.NET Core SignalR es una biblioteca de código abierto que simplifica la adición de la funcionalidad web en tiempo real a las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="91123-105">ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="91123-106">La funcionalidad web en tiempo real permite que el código del lado servidor inserte contenido en los clientes al momento.</span><span class="sxs-lookup"><span data-stu-id="91123-106">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="91123-107">Buenos candidatos para SignalR:</span><span class="sxs-lookup"><span data-stu-id="91123-107">Good candidates for SignalR:</span></span>

* <span data-ttu-id="91123-108">Aplicaciones que requieren actualizaciones de alta frecuencia desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="91123-108">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="91123-109">Algunos ejemplos son juegos, redes sociales, votación, subastas, mapas y aplicaciones GPS.</span><span class="sxs-lookup"><span data-stu-id="91123-109">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="91123-110">Los paneles y aplicaciones de supervisión.</span><span class="sxs-lookup"><span data-stu-id="91123-110">Dashboards and monitoring apps.</span></span> <span data-ttu-id="91123-111">Los ejemplos incluyen paneles empresariales, actualizaciones de venta instantáneas o alertas de viaje.</span><span class="sxs-lookup"><span data-stu-id="91123-111">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="91123-112">Aplicaciones de colaboración.</span><span class="sxs-lookup"><span data-stu-id="91123-112">Collaborative apps.</span></span> <span data-ttu-id="91123-113">Las aplicaciones de pizarra y software de reuniones de equipo son ejemplos de aplicaciones de colaboración.</span><span class="sxs-lookup"><span data-stu-id="91123-113">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="91123-114">Aplicaciones que requieren las notificaciones.</span><span class="sxs-lookup"><span data-stu-id="91123-114">Apps that require notifications.</span></span> <span data-ttu-id="91123-115">Redes sociales, correo electrónico, chat, juegos, alertas de viaje y muchas otras aplicaciones utilizan notificaciones.</span><span class="sxs-lookup"><span data-stu-id="91123-115">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="91123-116">SignalR proporciona una API para crear el servidor a cliente [llamadas a procedimiento remoto (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="91123-116">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="91123-117">Las RPC llamar a funciones de JavaScript en los clientes desde el código de .NET Core en el servidor.</span><span class="sxs-lookup"><span data-stu-id="91123-117">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="91123-118">Estas son algunas características de SignalR para ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="91123-118">Here are some features of SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="91123-119">Controla automáticamente la administración de conexiones.</span><span class="sxs-lookup"><span data-stu-id="91123-119">Handles connection management automatically.</span></span>
* <span data-ttu-id="91123-120">Envía mensajes a todos los clientes conectados al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="91123-120">Sends messages to all connected clients simultaneously.</span></span> <span data-ttu-id="91123-121">Por ejemplo, un salón de chat.</span><span class="sxs-lookup"><span data-stu-id="91123-121">For example, a chat room.</span></span>
* <span data-ttu-id="91123-122">Envía mensajes a clientes específicos o grupos de clientes.</span><span class="sxs-lookup"><span data-stu-id="91123-122">Sends messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="91123-123">Se puede ampliar para controlar el creciente tráfico.</span><span class="sxs-lookup"><span data-stu-id="91123-123">Scales to handle increasing traffic.</span></span>

<span data-ttu-id="91123-124">El origen se hospeda en un [SignalR repositorio de GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span><span class="sxs-lookup"><span data-stu-id="91123-124">The source is hosted in a [SignalR repository on GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span></span>

## <a name="transports"></a><span data-ttu-id="91123-125">Transportes</span><span class="sxs-lookup"><span data-stu-id="91123-125">Transports</span></span>

<span data-ttu-id="91123-126">SignalR admite varias técnicas para controlar las comunicaciones en tiempo real:</span><span class="sxs-lookup"><span data-stu-id="91123-126">SignalR supports several techniques for handling real-time communications:</span></span>

* [<span data-ttu-id="91123-127">WebSockets</span><span class="sxs-lookup"><span data-stu-id="91123-127">WebSockets</span></span>](https://tools.ietf.org/html/rfc7118)
* <span data-ttu-id="91123-128">Eventos enviados por el servidor</span><span class="sxs-lookup"><span data-stu-id="91123-128">Server-Sent Events</span></span>
* <span data-ttu-id="91123-129">Sondeo largo</span><span class="sxs-lookup"><span data-stu-id="91123-129">Long Polling</span></span>

<span data-ttu-id="91123-130">SignalR elige automáticamente el mejor método de transporte que se encuentra dentro de las capacidades del servidor y cliente.</span><span class="sxs-lookup"><span data-stu-id="91123-130">SignalR automatically chooses the best transport method that is within the capabilities of the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="91123-131">Concentradores</span><span class="sxs-lookup"><span data-stu-id="91123-131">Hubs</span></span>

<span data-ttu-id="91123-132">SignalR usa *hubs* para la comunicación entre clientes y servidores.</span><span class="sxs-lookup"><span data-stu-id="91123-132">SignalR uses *hubs* to communicate between clients and servers.</span></span>

<span data-ttu-id="91123-133">Un concentrador es una canalización de alto nivel que permite que un cliente y servidor llamar a métodos entre sí.</span><span class="sxs-lookup"><span data-stu-id="91123-133">A hub is a high-level pipeline that allows a client and server to call methods on each other.</span></span> <span data-ttu-id="91123-134">SignalR controla el envío a través de los límites del equipo automáticamente, permitir que los clientes llamar a métodos en el servidor y viceversa.</span><span class="sxs-lookup"><span data-stu-id="91123-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa.</span></span> <span data-ttu-id="91123-135">Puede pasar parámetros fuertemente tipados a métodos, lo que permite el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="91123-135">You can pass strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="91123-136">SignalR proporciona dos protocolos de concentrador integrada: un protocolo de texto basado en JSON y un protocolo binario según [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="91123-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="91123-137">MessagePack generalmente crea mensajes más pequeños en comparación con JSON.</span><span class="sxs-lookup"><span data-stu-id="91123-137">MessagePack generally creates smaller messages compared to JSON.</span></span> <span data-ttu-id="91123-138">Deben ser compatible con los exploradores más antiguos [nivel XHR 2](https://caniuse.com/#feat=xhr2) para proporcionar compatibilidad con el protocolo MessagePack.</span><span class="sxs-lookup"><span data-stu-id="91123-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="91123-139">Centros de llamar a código de cliente mediante el envío de mensajes que contienen el nombre y los parámetros del método del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="91123-139">Hubs call client-side code by sending messages that contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="91123-140">Se deserializan los objetos que se envía como parámetros de método mediante el protocolo configurado.</span><span class="sxs-lookup"><span data-stu-id="91123-140">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="91123-141">El cliente intenta hacer coincidir el nombre a un método en el código del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="91123-141">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="91123-142">Cuando el cliente encuentra una coincidencia, llama al método y le pasa los datos deserializados.</span><span class="sxs-lookup"><span data-stu-id="91123-142">When the client finds a match, it calls the method and passes to it the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="91123-143">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="91123-143">Additional resources</span></span>

* [<span data-ttu-id="91123-144">Introducción a SignalR para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="91123-144">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="91123-145">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="91123-145">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="91123-146">Concentradores</span><span class="sxs-lookup"><span data-stu-id="91123-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="91123-147">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="91123-147">JavaScript client</span></span>](xref:signalr/javascript-client)
