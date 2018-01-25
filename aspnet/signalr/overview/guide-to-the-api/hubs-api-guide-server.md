---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: "Guía de API de bases de datos centrales de ASP.NET SignalR - Server (C#) | Documentos de Microsoft"
author: pfletcher
description: "Este documento proporciona una introducción a la programación del lado del servidor de la API de concentradores de SignalR de ASP.NET para SignalR versión 2, con ejemplos de código que muestran..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c2567d4d39a494daf77a23db5dff83c8fae4925d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a><span data-ttu-id="5deba-103">Guía de API de bases de datos centrales de ASP.NET SignalR - Server (C#)</span><span class="sxs-lookup"><span data-stu-id="5deba-103">ASP.NET SignalR Hubs API Guide - Server (C#)</span></span>
====================
<span data-ttu-id="5deba-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5deba-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="5deba-105">Este documento proporciona una introducción a la programación del lado del servidor de la API de concentradores de SignalR de ASP.NET para SignalR versión 2, con ejemplos de código que muestran las opciones habituales.</span><span class="sxs-lookup"><span data-stu-id="5deba-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 2, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="5deba-106">La API de concentradores de SignalR permite realizar llamadas a procedimiento remoto (RPC) desde un servidor a los clientes conectados y desde clientes en el servidor.</span><span class="sxs-lookup"><span data-stu-id="5deba-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="5deba-107">En el código de servidor, definir métodos que puedan llamar los clientes y llamar a métodos que se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="5deba-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="5deba-108">En el código de cliente, definir métodos que se pueda llamar desde el servidor y llamar a métodos que se ejecutan en el servidor.</span><span class="sxs-lookup"><span data-stu-id="5deba-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="5deba-109">SignalR se encarga de todas la mecánica de cliente a servidor para usted.</span><span class="sxs-lookup"><span data-stu-id="5deba-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="5deba-110">SignalR también ofrece una API de nivel inferior denominada conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="5deba-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="5deba-111">Para obtener una introducción a SignalR, concentradores y conexiones persistentes, consulte [Introducción a 2 SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="5deba-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR 2](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5deba-112">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="5deba-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="5deba-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5deba-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="5deba-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5deba-114">.NET 4.5</span></span>
> - <span data-ttu-id="5deba-115">SignalR versión 2</span><span class="sxs-lookup"><span data-stu-id="5deba-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="topic-versions"></a><span data-ttu-id="5deba-116">Versiones de tema</span><span class="sxs-lookup"><span data-stu-id="5deba-116">Topic versions</span></span>
> 
> <span data-ttu-id="5deba-117">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="5deba-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="5deba-118">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="5deba-118">Questions and comments</span></span>
> 
> <span data-ttu-id="5deba-119">Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="5deba-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5deba-120">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="5deba-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="5deba-121">Información general</span><span class="sxs-lookup"><span data-stu-id="5deba-121">Overview</span></span>

<span data-ttu-id="5deba-122">Este documento contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="5deba-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="5deba-123">Cómo registrar el middleware de SignalR</span><span class="sxs-lookup"><span data-stu-id="5deba-123">How to register SignalR middleware</span></span>](#route)

    - [<span data-ttu-id="5deba-124">La dirección URL /signalr</span><span class="sxs-lookup"><span data-stu-id="5deba-124">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="5deba-125">Configurar las opciones de SignalR</span><span class="sxs-lookup"><span data-stu-id="5deba-125">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="5deba-126">Cómo crear y utilizar clases de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-126">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="5deba-127">Duración de objetos de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-127">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="5deba-128">Grafía Camel de nombres de base de datos central en clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="5deba-128">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="5deba-129">Varios centros</span><span class="sxs-lookup"><span data-stu-id="5deba-129">Multiple Hubs</span></span>](#multiplehubs)
    - [<span data-ttu-id="5deba-130">Concentradores fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="5deba-130">Strongly-Typed Hubs</span></span>](#stronglytypedhubs)
- [<span data-ttu-id="5deba-131">Cómo definir métodos en la clase de base de datos central que pueden llamar los clientes</span><span class="sxs-lookup"><span data-stu-id="5deba-131">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="5deba-132">Grafía Camel de nombres de método en clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="5deba-132">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="5deba-133">Cuándo se debe ejecutar de forma asincrónica</span><span class="sxs-lookup"><span data-stu-id="5deba-133">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="5deba-134">Definir sobrecargas</span><span class="sxs-lookup"><span data-stu-id="5deba-134">Defining overloads</span></span>](#overloads)
    - [<span data-ttu-id="5deba-135">Notificar el progreso de las invocaciones de método de concentrador</span><span class="sxs-lookup"><span data-stu-id="5deba-135">Reporting progress from hub method invocations</span></span>](#progress)
- [<span data-ttu-id="5deba-136">Cómo llamar a métodos de cliente de la clase de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-136">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="5deba-137">Al seleccionar a los clientes que recibirán la RPC</span><span class="sxs-lookup"><span data-stu-id="5deba-137">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="5deba-138">Ninguna validación en tiempo de compilación para los nombres de método</span><span class="sxs-lookup"><span data-stu-id="5deba-138">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="5deba-139">Coincidencia de nombres de método entre mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="5deba-139">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="5deba-140">Ejecución asincrónica</span><span class="sxs-lookup"><span data-stu-id="5deba-140">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="5deba-141">Cómo administrar la pertenencia a grupos de la clase de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-141">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="5deba-142">Ejecución asincrónica de métodos Add y Remove</span><span class="sxs-lookup"><span data-stu-id="5deba-142">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="5deba-143">Persistencia de pertenencia a grupo</span><span class="sxs-lookup"><span data-stu-id="5deba-143">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="5deba-144">Grupos de usuario único</span><span class="sxs-lookup"><span data-stu-id="5deba-144">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="5deba-145">Cómo controlar los eventos de duración de conexión en la clase de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-145">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="5deba-146">Cuando se llama a OnConnected, OnDisconnected y OnReconnected</span><span class="sxs-lookup"><span data-stu-id="5deba-146">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="5deba-147">Estado del llamador no rellenado</span><span class="sxs-lookup"><span data-stu-id="5deba-147">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="5deba-148">Cómo obtener información sobre el cliente de la propiedad de contexto</span><span class="sxs-lookup"><span data-stu-id="5deba-148">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="5deba-149">Cómo pasar información de estado entre los clientes y la clase de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-149">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="5deba-150">Cómo controlar los errores en la clase de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-150">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="5deba-151">Cómo llamar a los métodos de cliente y administrar grupos desde fuera de la clase de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-151">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="5deba-152">Llamar a métodos de cliente</span><span class="sxs-lookup"><span data-stu-id="5deba-152">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="5deba-153">Administrar la pertenencia a grupos</span><span class="sxs-lookup"><span data-stu-id="5deba-153">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="5deba-154">Cómo habilitar el seguimiento</span><span class="sxs-lookup"><span data-stu-id="5deba-154">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="5deba-155">Cómo personalizar la canalización de concentradores</span><span class="sxs-lookup"><span data-stu-id="5deba-155">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="5deba-156">Para obtener documentación sobre cómo los clientes del programa, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="5deba-156">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="5deba-157">Guía de API de concentradores de SignalR - cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="5deba-157">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)
- [<span data-ttu-id="5deba-158">Guía de API de concentradores de SignalR - cliente .NET</span><span class="sxs-lookup"><span data-stu-id="5deba-158">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="5deba-159">Los componentes de servidor de SignalR 2 solo están disponibles en .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="5deba-159">The server components for SignalR 2 are only available in .NET 4.5.</span></span> <span data-ttu-id="5deba-160">Servidores que ejecutan .NET 4.0 deben usar v1.x de SignalR.</span><span class="sxs-lookup"><span data-stu-id="5deba-160">Servers running .NET 4.0 must use SignalR v1.x.</span></span>

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a><span data-ttu-id="5deba-161">Cómo registrar el middleware de SignalR</span><span class="sxs-lookup"><span data-stu-id="5deba-161">How to register SignalR middleware</span></span>

<span data-ttu-id="5deba-162">Para definir la ruta que los clientes usarán para conectar con el centro, llame a la `MapSignalR` método cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5deba-162">To define the route that clients will use to connect to your Hub, call the `MapSignalR` method when the application starts.</span></span> <span data-ttu-id="5deba-163">`MapSignalR`es un [método de extensión](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) para el `OwinExtensions` clase.</span><span class="sxs-lookup"><span data-stu-id="5deba-163">`MapSignalR` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `OwinExtensions` class.</span></span> <span data-ttu-id="5deba-164">En el ejemplo siguiente se muestra cómo definir la ruta de los concentradores SignalR utilizando una clase de inicio OWIN.</span><span class="sxs-lookup"><span data-stu-id="5deba-164">The following example shows how to define the SignalR Hubs route using an OWIN startup class.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

<span data-ttu-id="5deba-165">Si va a agregar funcionalidad de SignalR a una aplicación de ASP.NET MVC, asegúrese de que se ha agregado la ruta de SignalR antes de las otras rutas.</span><span class="sxs-lookup"><span data-stu-id="5deba-165">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="5deba-166">Para obtener más información, consulte [Tutorial: Introducción a SignalR 2 y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="5deba-166">For more information, see [Tutorial: Getting Started with SignalR 2 and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="5deba-167">La dirección URL /signalr</span><span class="sxs-lookup"><span data-stu-id="5deba-167">The /signalr URL</span></span>

<span data-ttu-id="5deba-168">De forma predeterminada, es la dirección URL de la ruta que los clientes usarán para conectar con el centro "/ signalr".</span><span class="sxs-lookup"><span data-stu-id="5deba-168">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="5deba-169">(No confunda esta dirección URL con la dirección URL "/ signalr/concentradores", que es para el archivo JavaScript generado automáticamente.</span><span class="sxs-lookup"><span data-stu-id="5deba-169">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="5deba-170">Para obtener más información sobre el proxy generado, consulte [Guía de API de concentradores de SignalR - cliente JavaScript - el proxy generado y lo que hace que](hubs-api-guide-javascript-client.md#genproxy).)</span><span class="sxs-lookup"><span data-stu-id="5deba-170">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span></span>

<span data-ttu-id="5deba-171">Puede haber circunstancias extraordinarias que hacen que esta dirección URL base no se puede usar para SignalR; Por ejemplo, tiene una carpeta en el proyecto denominado *signalr* y no desea cambiar el nombre.</span><span class="sxs-lookup"><span data-stu-id="5deba-171">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="5deba-172">En ese caso, puede cambiar la dirección URL base, como se muestra en los ejemplos siguientes (reemplazar "/ signalr" en el código de ejemplo con su dirección URL deseado).</span><span class="sxs-lookup"><span data-stu-id="5deba-172">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="5deba-173">**Código de servidor que especifica la dirección URL**</span><span class="sxs-lookup"><span data-stu-id="5deba-173">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

<span data-ttu-id="5deba-174">**Código de cliente de JavaScript que especifica la dirección URL (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="5deba-174">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

<span data-ttu-id="5deba-175">**Código de cliente de JavaScript que especifica la dirección URL (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="5deba-175">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="5deba-176">**Código de cliente de .NET que especifica la dirección URL**</span><span class="sxs-lookup"><span data-stu-id="5deba-176">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="5deba-177">Configurar las opciones de SignalR</span><span class="sxs-lookup"><span data-stu-id="5deba-177">Configuring SignalR Options</span></span>

<span data-ttu-id="5deba-178">Las sobrecargas de los `MapSignalR` método permiten especificar una dirección URL personalizada, una resolución de dependencia personalizados y las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="5deba-178">Overloads of the `MapSignalR` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="5deba-179">Habilitar las llamadas entre dominios mediante CORS o JSONP los clientes de explorador.</span><span class="sxs-lookup"><span data-stu-id="5deba-179">Enable cross-domain calls using CORS or JSONP from browser clients.</span></span>

    <span data-ttu-id="5deba-180">Por lo general si el explorador carga una página de `http://contoso.com`, la conexión de SignalR está en el mismo dominio, en `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="5deba-180">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="5deba-181">Si la página de `http://contoso.com` realiza una conexión a `http://fabrikam.com/signalr`, que es una conexión entre dominios.</span><span class="sxs-lookup"><span data-stu-id="5deba-181">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="5deba-182">Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="5deba-182">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="5deba-183">Para obtener más información, consulte [Guía de API de concentradores ASP.NET SignalR - cliente JavaScript - cómo establecer una conexión entre dominios](hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="5deba-183">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span></span>
- <span data-ttu-id="5deba-184">Habilitar mensajes de error detallados.</span><span class="sxs-lookup"><span data-stu-id="5deba-184">Enable detailed error messages.</span></span>

    <span data-ttu-id="5deba-185">Cuando se producen errores, el comportamiento predeterminado de SignalR es enviar a los clientes un mensaje de notificación sin detalles sobre lo que ocurrió.</span><span class="sxs-lookup"><span data-stu-id="5deba-185">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="5deba-186">Enviar información detallada del error a los clientes no se recomienda en producción, porque los usuarios malintencionados podrían llevar a la utilice la información de los ataques contra la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5deba-186">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="5deba-187">Para solucionar problemas, puede usar esta opción para habilitar temporalmente informes de error más informativo.</span><span class="sxs-lookup"><span data-stu-id="5deba-187">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="5deba-188">Deshabilite generados automáticamente archivos de proxy de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5deba-188">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="5deba-189">De forma predeterminada, se genera un archivo de JavaScript con servidores proxy para las clases de base de datos central en respuesta a la dirección URL "/ signalr/concentradores".</span><span class="sxs-lookup"><span data-stu-id="5deba-189">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="5deba-190">Si no desea usar a los servidores proxy de JavaScript, o si desea generar este archivo manualmente y hacer referencia a un archivo físico en los clientes, puede usar esta opción para deshabilitar la generación de proxy.</span><span class="sxs-lookup"><span data-stu-id="5deba-190">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="5deba-191">Para obtener más información, consulte [Guía de API de concentradores de SignalR - cliente JavaScript - cómo crear un archivo físico para SignalR genera proxy](hubs-api-guide-javascript-client.md#manualproxy).</span><span class="sxs-lookup"><span data-stu-id="5deba-191">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span></span>

<span data-ttu-id="5deba-192">En el ejemplo siguiente se muestra cómo especificar la dirección URL de conexión de SignalR y estas opciones en una llamada a la `MapSignalR` método.</span><span class="sxs-lookup"><span data-stu-id="5deba-192">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapSignalR` method.</span></span> <span data-ttu-id="5deba-193">Para especificar una dirección URL personalizada, reemplace "/ signalr" en el ejemplo con la dirección URL que se va a utilizar.</span><span class="sxs-lookup"><span data-stu-id="5deba-193">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="5deba-194">Cómo crear y utilizar clases de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-194">How to create and use Hub classes</span></span>

<span data-ttu-id="5deba-195">Para crear un concentrador, cree una clase que deriva de [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="5deba-195">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="5deba-196">En el ejemplo siguiente se muestra una clase simple de concentrador para una aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="5deba-196">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

<span data-ttu-id="5deba-197">En este ejemplo, puede llamar un cliente conectado el `NewContosoChatMessage` método, y cuando lo hace, los datos recibidos se difunden a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="5deba-197">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="5deba-198">Duración de objetos de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-198">Hub object lifetime</span></span>

<span data-ttu-id="5deba-199">No crear una instancia de la clase de concentrador o llamar a sus métodos desde su propio código en el servidor; todo lo que es necesario encargarse de la canalización de concentradores de SignalR.</span><span class="sxs-lookup"><span data-stu-id="5deba-199">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="5deba-200">SignalR crea una nueva instancia de la clase de base de datos central cada vez que necesita para administrar una operación de base de datos central, como cuando un cliente se conecta, desconecta o realiza un método de llamada al servidor.</span><span class="sxs-lookup"><span data-stu-id="5deba-200">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="5deba-201">Dado que las instancias de la clase de base de datos central son transitorias, no puede usarlos para mantener el estado de una llamada al método a la siguiente.</span><span class="sxs-lookup"><span data-stu-id="5deba-201">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="5deba-202">Cada vez que el servidor recibe una llamada al método desde un cliente, una nueva instancia de los procesos de clase de base de datos central el mensaje.</span><span class="sxs-lookup"><span data-stu-id="5deba-202">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="5deba-203">Para mantener el estado a través de varias conexiones y llamadas a métodos, utilizar algún otro método, como una base de datos o una variable estática en la clase de base de datos central, o en una clase diferente que no se deriva de `Hub`.</span><span class="sxs-lookup"><span data-stu-id="5deba-203">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="5deba-204">Si conservar los datos en memoria, utilizando un método como una variable estática en la clase base de datos central, los datos se perderán cuando se recicla el dominio de aplicación.</span><span class="sxs-lookup"><span data-stu-id="5deba-204">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="5deba-205">Si desea enviar mensajes a los clientes desde su propio código que se ejecuta fuera de la clase de base de datos central, no puede hacerlo creando una instancia de la clase de base de datos central, pero puede hacerlo mediante la obtención de una referencia al objeto de contexto de SignalR para la clase de base de datos central.</span><span class="sxs-lookup"><span data-stu-id="5deba-205">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="5deba-206">Para obtener más información, consulte [cómo llamar a los métodos de cliente y administrar grupos desde fuera de la clase de base de datos central](#callfromoutsidehub) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="5deba-206">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="5deba-207">Grafía Camel de nombres de base de datos central en clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="5deba-207">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="5deba-208">De forma predeterminada, los clientes de JavaScript hacen referencia a los concentradores mediante una versión de grafía de camel del nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="5deba-208">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="5deba-209">SignalR automáticamente realiza este cambio para que el código de JavaScript puede ajustarse a las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5deba-209">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="5deba-210">El ejemplo anterior se denominará `contosoChatHub` en código de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5deba-210">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="5deba-211">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="5deba-211">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

<span data-ttu-id="5deba-212">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="5deba-212">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

<span data-ttu-id="5deba-213">Si desea especificar un nombre diferente para que los clientes, agregue el `HubName` atributo.</span><span class="sxs-lookup"><span data-stu-id="5deba-213">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="5deba-214">Cuando se usa un `HubName` atributo, no hay ningún cambio de nombre en mayúsculas y minúsculas camel en los clientes de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5deba-214">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="5deba-215">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="5deba-215">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

<span data-ttu-id="5deba-216">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="5deba-216">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="5deba-217">Varios centros</span><span class="sxs-lookup"><span data-stu-id="5deba-217">Multiple Hubs</span></span>

<span data-ttu-id="5deba-218">Puede definir varias clases de base de datos central en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="5deba-218">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="5deba-219">Al hacerlo, se comparte la conexión, pero los grupos son independientes:</span><span class="sxs-lookup"><span data-stu-id="5deba-219">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="5deba-220">Todos los clientes usarán la misma dirección URL para establecer una conexión SignalR con su servicio ("/ signalr" o la dirección URL personalizada si ha especificado uno), y que se utiliza la conexión para todos los centros definen por el servicio.</span><span class="sxs-lookup"><span data-stu-id="5deba-220">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="5deba-221">No hay ninguna diferencia de rendimiento para varios centros en comparación a la definición de toda la funcionalidad de base de datos central en una sola clase.</span><span class="sxs-lookup"><span data-stu-id="5deba-221">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="5deba-222">Todos los concentradores de obtener la misma información de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="5deba-222">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="5deba-223">Puesto que todos los concentradores comparten la misma conexión, la única información de la solicitud HTTP que el servidor obtiene es qué se incluye en la solicitud HTTP original que establece la conexión de SignalR.</span><span class="sxs-lookup"><span data-stu-id="5deba-223">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="5deba-224">Si usas la solicitud de conexión para pasar información del cliente al servidor mediante la especificación de una cadena de consulta, no puede proporcionar las cadenas de consulta diferentes a los concentradores diferentes.</span><span class="sxs-lookup"><span data-stu-id="5deba-224">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="5deba-225">Todos los concentradores recibirá la misma información.</span><span class="sxs-lookup"><span data-stu-id="5deba-225">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="5deba-226">El archivo de proxy de JavaScript generado contendrá a todos los centros de un archivo de los servidores proxy.</span><span class="sxs-lookup"><span data-stu-id="5deba-226">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="5deba-227">Para obtener información sobre servidores proxy de JavaScript, consulte [Guía de API de concentradores de SignalR - cliente JavaScript - el proxy generado y lo que hace que](hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="5deba-227">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span></span>
- <span data-ttu-id="5deba-228">Se definen grupos de concentradores.</span><span class="sxs-lookup"><span data-stu-id="5deba-228">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="5deba-229">En SignalR que puede definir grupos para difundirlo a los subconjuntos de clientes conectados con nombre.</span><span class="sxs-lookup"><span data-stu-id="5deba-229">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="5deba-230">Los grupos se mantienen por separado para cada concentrador.</span><span class="sxs-lookup"><span data-stu-id="5deba-230">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="5deba-231">Por ejemplo, un grupo llamado "Administradores" incluye un conjunto de clientes para su `ContosoChatHub` clase y el mismo nombre de grupo haría referencia a un conjunto diferente de los clientes de su `StockTickerHub` clase.</span><span class="sxs-lookup"><span data-stu-id="5deba-231">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a><span data-ttu-id="5deba-232">Concentradores fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="5deba-232">Strongly-Typed Hubs</span></span>

<span data-ttu-id="5deba-233">Para definir una interfaz para los métodos de concentrador que el cliente puede referencia (y habilitar Intellisense en los métodos de concentrador), derivar el centro de `Hub<T>` (introducida en SignalR 2.1) en lugar de `Hub`:</span><span class="sxs-lookup"><span data-stu-id="5deba-233">To define an interface for your hub methods that your client can reference (and enable Intellisense on your hub methods), derive your hub from `Hub<T>` (introduced in SignalR 2.1) rather than `Hub`:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="5deba-234">Cómo definir métodos en la clase de base de datos central que pueden llamar los clientes</span><span class="sxs-lookup"><span data-stu-id="5deba-234">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="5deba-235">Para exponer un método en el concentrador al que desea que se pueda llamar desde el cliente, declarar un método público, tal y como se muestra en los ejemplos siguientes.</span><span class="sxs-lookup"><span data-stu-id="5deba-235">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="5deba-236">Puede especificar un tipo de valor devuelto y parámetros, incluidos los tipos complejos y matrices, como lo haría en cualquier método de C#.</span><span class="sxs-lookup"><span data-stu-id="5deba-236">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="5deba-237">Los datos que recibe en parámetros o devolver al llamador se comunican entre el cliente y el servidor mediante el uso de JSON y SignalR controla el enlace de objetos complejos y matrices de objetos automáticamente.</span><span class="sxs-lookup"><span data-stu-id="5deba-237">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="5deba-238">Grafía Camel de nombres de método en clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="5deba-238">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="5deba-239">De forma predeterminada, los clientes de JavaScript hacen referencia a los métodos de concentrador mediante una versión de grafía de camel del nombre del método.</span><span class="sxs-lookup"><span data-stu-id="5deba-239">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="5deba-240">SignalR automáticamente realiza este cambio para que el código de JavaScript puede ajustarse a las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5deba-240">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="5deba-241">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="5deba-241">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="5deba-242">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="5deba-242">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="5deba-243">Si desea especificar un nombre diferente para que los clientes, agregue el `HubMethodName` atributo.</span><span class="sxs-lookup"><span data-stu-id="5deba-243">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="5deba-244">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="5deba-244">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="5deba-245">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="5deba-245">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="5deba-246">Cuándo se debe ejecutar de forma asincrónica</span><span class="sxs-lookup"><span data-stu-id="5deba-246">When to execute asynchronously</span></span>

<span data-ttu-id="5deba-247">Si el método se puede o de larga ejecución tiene que trabajar que implican espera, por ejemplo, una búsqueda de la base de datos o una llamada de servicio web, hacer que el método de concentrador asincrónica devolviendo un [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (en lugar de `void` devolver) o [ Tarea&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) objeto (en lugar de `T` tipo de valor devuelto).</span><span class="sxs-lookup"><span data-stu-id="5deba-247">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="5deba-248">Cuando vuelva un `Task` objeto desde el método de SignalR espera el `Task` en completarse, y, a continuación, envía el resultado sin ajustar al cliente, así que no hay ninguna diferencia en cómo código la llamada al método en el cliente.</span><span class="sxs-lookup"><span data-stu-id="5deba-248">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="5deba-249">Hacer que un método de concentrador asincrónica evita bloqueando la conexión cuando se utiliza el transporte de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5deba-249">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="5deba-250">Cuando un método de concentrador se ejecuta de forma sincrónica y el transporte es WebSocket, las siguientes llamadas a métodos en el concentrador del mismo cliente se bloquean hasta que se complete el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="5deba-250">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="5deba-251">El ejemplo siguiente se muestra el mismo método en el código para ejecutar de forma sincrónica o asincrónica, seguido por el código de cliente de JavaScript que funciona para llamar a cualquiera de las versiones.</span><span class="sxs-lookup"><span data-stu-id="5deba-251">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="5deba-252">**Sincrónico**</span><span class="sxs-lookup"><span data-stu-id="5deba-252">**Synchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="5deba-253">**Asincrónica**</span><span class="sxs-lookup"><span data-stu-id="5deba-253">**Asynchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="5deba-254">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="5deba-254">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="5deba-255">Para obtener más información sobre cómo usar métodos asincrónicos en 4.5 de ASP.NET, vea [mediante los métodos asincrónicos en ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="5deba-255">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="5deba-256">Definir sobrecargas</span><span class="sxs-lookup"><span data-stu-id="5deba-256">Defining Overloads</span></span>

<span data-ttu-id="5deba-257">Si desea definir sobrecargas para un método, el número de parámetros en cada sobrecarga debe ser diferente.</span><span class="sxs-lookup"><span data-stu-id="5deba-257">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="5deba-258">Si es posible diferenciar una sobrecarga simplemente mediante la especificación de distintos tipos de parámetro, la clase de base de datos central se compilará, pero el servicio de SignalR iniciará una excepción en tiempo de ejecución cuando los clientes intentan para llamar a una de las sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="5deba-258">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a><span data-ttu-id="5deba-259">Notificar el progreso de las invocaciones de método de concentrador</span><span class="sxs-lookup"><span data-stu-id="5deba-259">Reporting progress from hub method invocations</span></span>

<span data-ttu-id="5deba-260">SignalR 2.1 agrega compatibilidad para la [patrón para informar del progreso](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introducidas en .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="5deba-260">SignalR 2.1 adds support for the [progress reporting pattern](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduced in .NET 4.5.</span></span> <span data-ttu-id="5deba-261">Para implementar informes de progreso, defina un `IProgress<T>` parámetro para el método de concentrador que pueda acceder el cliente:</span><span class="sxs-lookup"><span data-stu-id="5deba-261">To implement progress reporting, define an `IProgress<T>` parameter for your hub method that your client can access:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

<span data-ttu-id="5deba-262">Al escribir un método de servidor de ejecución prolongada, es importante que use un modelo de programación asincrónico como asincrónicas / Await en lugar de bloquear el subproceso de la base de datos central.</span><span class="sxs-lookup"><span data-stu-id="5deba-262">When writing a long-running server method, it is important to use an asynchronous programming pattern like Async/ Await rather than blocking the hub thread.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="5deba-263">Cómo llamar a métodos de cliente de la clase de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-263">How to call client methods from the Hub class</span></span>

<span data-ttu-id="5deba-264">Para llamar métodos de cliente desde el servidor, utilice el `Clients` propiedad en un método en la clase de base de datos central.</span><span class="sxs-lookup"><span data-stu-id="5deba-264">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="5deba-265">En el ejemplo siguiente se muestra el código de servidor que llame `addNewMessageToPage` en todos los clientes y el código de cliente que define el método en un cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5deba-265">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="5deba-266">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="5deba-266">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

<span data-ttu-id="5deba-267">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="5deba-267">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

<span data-ttu-id="5deba-268">No se puede obtener un valor devuelto de un método de cliente; sintaxis como `int x = Clients.All.add(1,1)` no funciona.</span><span class="sxs-lookup"><span data-stu-id="5deba-268">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="5deba-269">Puede especificar los tipos complejos y matrices para los parámetros.</span><span class="sxs-lookup"><span data-stu-id="5deba-269">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="5deba-270">En el ejemplo siguiente se pasa un tipo complejo al cliente en un parámetro de método.</span><span class="sxs-lookup"><span data-stu-id="5deba-270">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="5deba-271">**Código de servidor que llama a un método de cliente mediante un objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="5deba-271">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

<span data-ttu-id="5deba-272">**Código de servidor que define el objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="5deba-272">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

<span data-ttu-id="5deba-273">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="5deba-273">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="5deba-274">Al seleccionar a los clientes que recibirán la RPC</span><span class="sxs-lookup"><span data-stu-id="5deba-274">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="5deba-275">La propiedad no devuelve los clientes un [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) objeto que proporciona varias opciones para especificar que los clientes recibirán la RPC:</span><span class="sxs-lookup"><span data-stu-id="5deba-275">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="5deba-276">Todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="5deba-276">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="5deba-277">Solo el cliente que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="5deba-277">Only the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="5deba-278">Todos los clientes, excepto el cliente que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="5deba-278">All clients except the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- <span data-ttu-id="5deba-279">Un cliente específico identificado por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-279">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    <span data-ttu-id="5deba-280">Este ejemplo llama `addContosoChatMessageToPage` en el cliente que realiza la llamada y tiene el mismo efecto que usar `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="5deba-280">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="5deba-281">Todos los clientes conectados excepción los clientes especificados, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-281">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- <span data-ttu-id="5deba-282">Todos los clientes en un grupo especificado conectados.</span><span class="sxs-lookup"><span data-stu-id="5deba-282">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- <span data-ttu-id="5deba-283">Todos los clientes conectados en un grupo especificado, excepto los clientes especificados, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-283">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- <span data-ttu-id="5deba-284">Todos los clientes conectados en un grupo especificado, excepto el cliente que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="5deba-284">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- <span data-ttu-id="5deba-285">Un usuario específico, identificado por el identificador de usuario.</span><span class="sxs-lookup"><span data-stu-id="5deba-285">A specific user, identified by userId.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    <span data-ttu-id="5deba-286">De forma predeterminada, es `IPrincipal.Identity.Name`, pero se puede cambiar [registrar una implementación de IUserIdProvider con el host global](mapping-users-to-connections.md#IUserIdProvider).</span><span class="sxs-lookup"><span data-stu-id="5deba-286">By default, this is `IPrincipal.Identity.Name`, but this can be changed by [registering an implementation of IUserIdProvider with the global host](mapping-users-to-connections.md#IUserIdProvider).</span></span>
- <span data-ttu-id="5deba-287">Todos los clientes y grupos de una lista de identificadores de conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-287">All clients and groups in a list of connection IDs.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- <span data-ttu-id="5deba-288">Una lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="5deba-288">A list of groups.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- <span data-ttu-id="5deba-289">Un usuario por su nombre.</span><span class="sxs-lookup"><span data-stu-id="5deba-289">A user by name.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- <span data-ttu-id="5deba-290">Una lista de nombres de usuario (introducida en SignalR 2.1).</span><span class="sxs-lookup"><span data-stu-id="5deba-290">A list of user names (introduced in SignalR 2.1).</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="5deba-291">Ninguna validación en tiempo de compilación para los nombres de método</span><span class="sxs-lookup"><span data-stu-id="5deba-291">No compile-time validation for method names</span></span>

<span data-ttu-id="5deba-292">El nombre del método que especifique se interpreta como un objeto dinámico, lo que significa que no hay ningún IntelliSense o validación en tiempo de compilación para él.</span><span class="sxs-lookup"><span data-stu-id="5deba-292">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="5deba-293">La expresión se evalúa en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="5deba-293">The expression is evaluated at run time.</span></span> <span data-ttu-id="5deba-294">Cuando se ejecuta la llamada al método, SignalR envía el nombre del método y los valores de parámetro para el cliente, y si el cliente tiene un método que coincide con el nombre, que se llama el método y los valores de parámetro se pasan a él.</span><span class="sxs-lookup"><span data-stu-id="5deba-294">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="5deba-295">Si no se encuentra ningún método coincidente en el cliente, se produce ningún error.</span><span class="sxs-lookup"><span data-stu-id="5deba-295">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="5deba-296">Para obtener información sobre el formato de los datos que SignalR se transmite al cliente en segundo plano cuando se llama a un método de cliente, consulte [Introducción a SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="5deba-296">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="5deba-297">Coincidencia de nombres de método entre mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="5deba-297">Case-insensitive method name matching</span></span>

<span data-ttu-id="5deba-298">Coincidencia de nombres de método distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="5deba-298">Method name matching is case-insensitive.</span></span> <span data-ttu-id="5deba-299">Por ejemplo, `Clients.All.addContosoChatMessageToPage` se ejecutará en el servidor `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, o `addContosoChatMessageToPage` en el cliente.</span><span class="sxs-lookup"><span data-stu-id="5deba-299">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="5deba-300">Ejecución asincrónica</span><span class="sxs-lookup"><span data-stu-id="5deba-300">Asynchronous execution</span></span>

<span data-ttu-id="5deba-301">El método que se llama a ejecuta de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="5deba-301">The method that you call executes asynchronously.</span></span> <span data-ttu-id="5deba-302">Cualquier código que viene después de una llamada de método a un cliente se ejecutará inmediatamente sin tener que esperar para que SignalR finalizar la transmisión de datos a los clientes a menos que especifique que las siguientes líneas de código deben esperar la finalización del método.</span><span class="sxs-lookup"><span data-stu-id="5deba-302">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="5deba-303">En el ejemplo de código siguiente se muestra cómo dos métodos de cliente se ejecutan secuencialmente.</span><span class="sxs-lookup"><span data-stu-id="5deba-303">The following code example shows how to execute two client methods sequentially.</span></span>

<span data-ttu-id="5deba-304">**Usar Await (.NET 4.5)**</span><span class="sxs-lookup"><span data-stu-id="5deba-304">**Using Await (.NET 4.5)**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="5deba-305">Si usa `await` para esperar hasta que un método de cliente finalice antes de que se ejecuta la siguiente línea de código, esto no significa que los clientes realmente recibirá el mensaje antes de que se ejecuta la siguiente línea de código.</span><span class="sxs-lookup"><span data-stu-id="5deba-305">If you use `await` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="5deba-306">"Completar" de una llamada al método de cliente sólo significa que ha hecho SignalR todo lo necesario para enviar el mensaje.</span><span class="sxs-lookup"><span data-stu-id="5deba-306">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="5deba-307">Si necesita que los clientes reciben el mensaje de comprobación, tendrá que programar ese mecanismo usted mismo.</span><span class="sxs-lookup"><span data-stu-id="5deba-307">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="5deba-308">Por ejemplo, puede codificar un `MessageReceived` método del concentrador y en el `addContosoChatMessageToPage` método en el cliente puede llamar a `MessageReceived` después de trabajo necesita hacer en el cliente.</span><span class="sxs-lookup"><span data-stu-id="5deba-308">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="5deba-309">En `MessageReceived` en el concentrador se puede realizar cualquier trabajo depende de la recepción de cliente real y el procesamiento de la llamada al método original.</span><span class="sxs-lookup"><span data-stu-id="5deba-309">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="5deba-310">Cómo utilizar una variable de cadena como el nombre del método</span><span class="sxs-lookup"><span data-stu-id="5deba-310">How to use a string variable as the method name</span></span>

<span data-ttu-id="5deba-311">Si desea invocar un método de cliente mediante el uso de una variable de cadena como el nombre del método, convierte `Clients.All` (o `Clients.Others`, `Clients.Caller`, etc.) a `IClientProxy` y, a continuación, llame a [Invoke (methodName, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="5deba-311">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="5deba-312">Cómo administrar la pertenencia a grupos de la clase de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-312">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="5deba-313">Grupos de SignalR proporcionan un método para difundir mensajes a subconjuntos especificados de los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="5deba-313">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="5deba-314">Un grupo puede tener cualquier número de clientes y un cliente puede ser un miembro de cualquier número de grupos.</span><span class="sxs-lookup"><span data-stu-id="5deba-314">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="5deba-315">Para administrar la pertenencia a grupos, use la [agregar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) y [quitar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) métodos proporcionados por el `Groups` propiedad de la clase de base de datos central.</span><span class="sxs-lookup"><span data-stu-id="5deba-315">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="5deba-316">El siguiente ejemplo se muestra la `Groups.Add` y `Groups.Remove` métodos utilizados en los métodos de concentrador que se llaman al código de cliente, seguido por el código de cliente de JavaScript que llama.</span><span class="sxs-lookup"><span data-stu-id="5deba-316">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="5deba-317">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="5deba-317">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

<span data-ttu-id="5deba-318">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="5deba-318">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

<span data-ttu-id="5deba-319">No tienes que crear explícitamente grupos.</span><span class="sxs-lookup"><span data-stu-id="5deba-319">You don't have to explicitly create groups.</span></span> <span data-ttu-id="5deba-320">En vigor un grupo se crea automáticamente la primera vez que especifique su nombre en una llamada a `Groups.Add`, y se elimina cuando se quita la última conexión de pertenencia en ella.</span><span class="sxs-lookup"><span data-stu-id="5deba-320">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="5deba-321">No hay ninguna API para obtener una lista de pertenencia a grupos o una lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="5deba-321">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="5deba-322">SignalR envía mensajes a los clientes y grupos basados en un [modelo pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), y el servidor no mantiene las listas de grupos o las pertenencias a grupos.</span><span class="sxs-lookup"><span data-stu-id="5deba-322">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="5deba-323">Esto ayuda a maximizar la escalabilidad, ya que cada vez que agregue un nodo a una granja de servidores web, cualquier estado que mantiene SignalR debe se propaguen en el nuevo nodo.</span><span class="sxs-lookup"><span data-stu-id="5deba-323">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="5deba-324">Ejecución asincrónica de métodos Add y Remove</span><span class="sxs-lookup"><span data-stu-id="5deba-324">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="5deba-325">El `Groups.Add` y `Groups.Remove` métodos ejecutan de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="5deba-325">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="5deba-326">Si desea agregar un cliente a un grupo y enviar inmediatamente un mensaje al cliente mediante el grupo, se tiene que asegurarse de que el `Groups.Add` método finaliza primero.</span><span class="sxs-lookup"><span data-stu-id="5deba-326">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="5deba-327">En el ejemplo de código siguiente se muestra cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="5deba-327">The following code example shows how to do that.</span></span>

<span data-ttu-id="5deba-328">**Agregar a un cliente a un grupo y, a continuación, ese cliente de mensajería**</span><span class="sxs-lookup"><span data-stu-id="5deba-328">**Adding a client to a group and then messaging that client**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="5deba-329">Persistencia de pertenencia a grupo</span><span class="sxs-lookup"><span data-stu-id="5deba-329">Group membership persistence</span></span>

<span data-ttu-id="5deba-330">SignalR realiza un seguimiento de las conexiones, no a los usuarios, por lo que si desea que un usuario esté en el mismo grupo cada vez que el usuario establece una conexión, tiene que llamar a `Groups.Add` cada vez que el usuario establece una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-330">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="5deba-331">Después de una pérdida temporal de conectividad, en ocasiones SignalR puede restaurar la conexión automáticamente.</span><span class="sxs-lookup"><span data-stu-id="5deba-331">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="5deba-332">En ese caso, SignalR está restaurando la misma conexión, sin establecer una nueva conexión y, por lo que se restaura automáticamente pertenencia de grupo del cliente.</span><span class="sxs-lookup"><span data-stu-id="5deba-332">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="5deba-333">Esto es posible incluso cuando la interrupción temporal es el resultado de un reinicio del servidor o un error, porque el estado de la conexión para cada cliente, incluida la pertenencia a grupos, es de ida y vuelta al cliente.</span><span class="sxs-lookup"><span data-stu-id="5deba-333">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="5deba-334">Si un servidor deja de funcionar y se reemplaza por un nuevo servidor antes agote el tiempo de espera de la conexión, un cliente puede volver a conectarse automáticamente al nuevo servidor y volver a inscribir en es un miembro de los grupos.</span><span class="sxs-lookup"><span data-stu-id="5deba-334">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="5deba-335">Cuando una conexión no se puede restaurar automáticamente después de una pérdida de conectividad, o cuando se agota el tiempo de espera de la conexión, o cuando el cliente se desconecta (por ejemplo, cuando un explorador se desplaza a una nueva página), se pierden las pertenencias a grupos.</span><span class="sxs-lookup"><span data-stu-id="5deba-335">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="5deba-336">La próxima vez que el usuario se conecta será una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-336">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="5deba-337">Para mantener las pertenencias a grupos cuando el mismo usuario establece una nueva conexión, la aplicación tiene que realizar el seguimiento de las asociaciones entre usuarios y grupos y restaurar las pertenencias a grupos cada vez que un usuario establece una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-337">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="5deba-338">Para obtener más información sobre las conexiones y reconexiones, consulte [cómo controlar los eventos de duración de conexión en la clase de base de datos central](#connectionlifetime) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="5deba-338">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="5deba-339">Grupos de usuario único</span><span class="sxs-lookup"><span data-stu-id="5deba-339">Single-user groups</span></span>

<span data-ttu-id="5deba-340">Las aplicaciones que utilizan SignalR normalmente tienen que realizar un seguimiento de las asociaciones entre usuarios y las conexiones para saber qué usuario ha enviado un mensaje y qué usuarios deberían recibir un mensaje.</span><span class="sxs-lookup"><span data-stu-id="5deba-340">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="5deba-341">Grupos se utilizan en uno de los dos patrones de uso frecuente para hacerlo.</span><span class="sxs-lookup"><span data-stu-id="5deba-341">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="5deba-342">Grupos de usuario único.</span><span class="sxs-lookup"><span data-stu-id="5deba-342">Single-user groups.</span></span>

    <span data-ttu-id="5deba-343">Puede especificar el nombre de usuario como el nombre del grupo y agregue el identificador de conexión actual al grupo cada vez que el usuario se conecta o se vuelve a conectar.</span><span class="sxs-lookup"><span data-stu-id="5deba-343">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="5deba-344">Para enviar mensajes al usuario que enviar al grupo.</span><span class="sxs-lookup"><span data-stu-id="5deba-344">To send messages to the user you send to the group.</span></span> <span data-ttu-id="5deba-345">Una desventaja de este método es que el grupo no le proporciona una manera de averiguar si el usuario está conectado o desconectado.</span><span class="sxs-lookup"><span data-stu-id="5deba-345">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="5deba-346">Realizar un seguimiento de las asociaciones entre los nombres de usuario y los identificadores de conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-346">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="5deba-347">Puede almacenar una asociación entre cada nombre de usuario y los identificadores de uno o más conexiones en un diccionario o una base de datos y actualizar los datos almacenados cada vez que el usuario se conecta o desconecta.</span><span class="sxs-lookup"><span data-stu-id="5deba-347">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="5deba-348">Para enviar mensajes al usuario especificar el Id. de conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-348">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="5deba-349">Una desventaja de este método es que se tarda más memoria.</span><span class="sxs-lookup"><span data-stu-id="5deba-349">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="5deba-350">Cómo controlar los eventos de duración de conexión en la clase de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-350">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="5deba-351">Motivos habituales para el control de eventos de duración de conexión son para realizar un seguimiento de si un usuario está conectado o no y realizar un seguimiento de la asociación entre los nombres de usuario y los identificadores de conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-351">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="5deba-352">Para ejecutar su propio código cuando los clientes conectan o desconexión, invalidar la `OnConnected`, `OnDisconnected`, y `OnReconnected` métodos virtuales del centro de la clase, como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="5deba-352">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="5deba-353">Cuando se llama a OnConnected, OnDisconnected y OnReconnected</span><span class="sxs-lookup"><span data-stu-id="5deba-353">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="5deba-354">Cada vez que un explorador se desplaza a una nueva página, una nueva conexión tiene que establecerse, lo que significa que se ejecutará SignalR el `OnDisconnected` método seguido por el `OnConnected` método.</span><span class="sxs-lookup"><span data-stu-id="5deba-354">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="5deba-355">SignalR siempre crea un nuevo identificador de conexión cuando se establece una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-355">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="5deba-356">El `OnReconnected` método se llama cuando se ha producido una interrupción temporal de conectividad que SignalR puede recuperarse automáticamente de, por ejemplo, cuando un cable está desconectado y vuelto a conectar antes agote el tiempo de espera de la conexión temporalmente. El `OnDisconnected` método se llama cuando el cliente se desconecta y SignalR no se puede volver a conectar automáticamente, por ejemplo, cuando un explorador se desplaza a una nueva página.</span><span class="sxs-lookup"><span data-stu-id="5deba-356">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="5deba-357">Por lo tanto, es una secuencia de eventos para un determinado cliente posibles `OnConnected`, `OnReconnected`, `OnDisconnected`; o `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="5deba-357">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="5deba-358">No verá la secuencia `OnConnected`, `OnDisconnected`, `OnReconnected` para una conexión dada.</span><span class="sxs-lookup"><span data-stu-id="5deba-358">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="5deba-359">El `OnDisconnected` método que no se llama en algunos escenarios, como cuando un servidor deja de funcionar o se encienda y el dominio de aplicación.</span><span class="sxs-lookup"><span data-stu-id="5deba-359">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="5deba-360">Cuando otro servidor se pone en línea o el dominio de aplicación completa su reciclaje, algunos clientes podrán que pueda volver a conectar y activar el `OnReconnected` eventos.</span><span class="sxs-lookup"><span data-stu-id="5deba-360">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="5deba-361">Para obtener más información, consulte [descripción y control de eventos de duración de conexión de SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="5deba-361">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="5deba-362">Estado del llamador no rellenado</span><span class="sxs-lookup"><span data-stu-id="5deba-362">Caller state not populated</span></span>

<span data-ttu-id="5deba-363">Se llaman a los métodos de controlador de eventos de duración de conexión desde el servidor, lo que significa que cualquier estado que coloca en el `state` objeto en el cliente no incluirá ningún dato en el `Caller` propiedad en el servidor.</span><span class="sxs-lookup"><span data-stu-id="5deba-363">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="5deba-364">Para obtener información sobre la `state` objeto y el `Caller` propiedad, vea [cómo pasar información de estado entre los clientes y la clase de base de datos central](#passstate) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="5deba-364">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="5deba-365">Cómo obtener información sobre el cliente de la propiedad de contexto</span><span class="sxs-lookup"><span data-stu-id="5deba-365">How to get information about the client from the Context property</span></span>

<span data-ttu-id="5deba-366">Para obtener información acerca del cliente, use el `Context` propiedad de la clase de base de datos central.</span><span class="sxs-lookup"><span data-stu-id="5deba-366">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="5deba-367">El `Context` propiedad devuelve un [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) objeto que proporciona acceso a la siguiente información:</span><span class="sxs-lookup"><span data-stu-id="5deba-367">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="5deba-368">El identificador de conexión del cliente que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="5deba-368">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    <span data-ttu-id="5deba-369">El identificador de conexión es un GUID asignado por SignalR (no se puede especificar el valor en su propio código).</span><span class="sxs-lookup"><span data-stu-id="5deba-369">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="5deba-370">Hay un identificador de conexión para cada conexión y la misma conexión que identificador es usado por todos los concentradores, si tiene varios centros en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5deba-370">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="5deba-371">Datos del encabezado HTTP.</span><span class="sxs-lookup"><span data-stu-id="5deba-371">HTTP header data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="5deba-372">También puede obtener encabezados HTTP desde `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="5deba-372">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="5deba-373">La razón para varias referencias a la misma acción que es `Context.Headers` se creó en primer lugar, el `Context.Request` propiedad se agregó más tarde, y `Context.Headers` se conserva por compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="5deba-373">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="5deba-374">Consultar datos de cadena.</span><span class="sxs-lookup"><span data-stu-id="5deba-374">Query string data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    <span data-ttu-id="5deba-375">También puede obtener datos de cadena de consulta de `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="5deba-375">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="5deba-376">La cadena de consulta que se obtiene en esta propiedad es el que se usó con la solicitud HTTP que se estableció la conexión de SignalR.</span><span class="sxs-lookup"><span data-stu-id="5deba-376">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="5deba-377">Puede agregar parámetros de cadena de consulta en el cliente mediante la configuración de la conexión, que es una manera cómoda de pasar datos sobre el cliente desde el cliente al servidor.</span><span class="sxs-lookup"><span data-stu-id="5deba-377">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="5deba-378">En el ejemplo siguiente se muestra una manera de agregar una cadena de consulta en un cliente de JavaScript cuando se utiliza el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="5deba-378">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    <span data-ttu-id="5deba-379">Para obtener más información acerca de cómo establecer parámetros de cadena de consulta, consulte las guías de API para el [JavaScript](hubs-api-guide-javascript-client.md) y [.NET](hubs-api-guide-net-client.md) los clientes.</span><span class="sxs-lookup"><span data-stu-id="5deba-379">For more information about setting query string parameters, see the API guides for the [JavaScript](hubs-api-guide-javascript-client.md) and [.NET](hubs-api-guide-net-client.md) clients.</span></span>

    <span data-ttu-id="5deba-380">Puede encontrar el método de transporte utilizado para la conexión en los datos de cadena de consulta, junto con algunos otros valores utilizadas internamente por SignalR:</span><span class="sxs-lookup"><span data-stu-id="5deba-380">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    <span data-ttu-id="5deba-381">El valor de `transportMethod` será "webSockets", "serverSentEvents", "foreverFrame" o "longPolling".</span><span class="sxs-lookup"><span data-stu-id="5deba-381">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="5deba-382">Tenga en cuenta que si selecciona este valor el `OnConnected` método de controlador de eventos, en algunos escenarios inicialmente podría obtener un valor de transporte que no es el método de transporte negociada final para la conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-382">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="5deba-383">En ese caso, el método producirá una excepción y se llamará de nuevo más tarde cuando se establece el modo de transporte final.</span><span class="sxs-lookup"><span data-stu-id="5deba-383">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="5deba-384">Cookies.</span><span class="sxs-lookup"><span data-stu-id="5deba-384">Cookies.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    <span data-ttu-id="5deba-385">También puede obtener las cookies de `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="5deba-385">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="5deba-386">Información del usuario.</span><span class="sxs-lookup"><span data-stu-id="5deba-386">User information.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- <span data-ttu-id="5deba-387">El objeto HttpContext para la solicitud:</span><span class="sxs-lookup"><span data-stu-id="5deba-387">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    <span data-ttu-id="5deba-388">Utilice este método en lugar de ver `HttpContext.Current` para obtener la `HttpContext` objeto para la conexión de SignalR.</span><span class="sxs-lookup"><span data-stu-id="5deba-388">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="5deba-389">Cómo pasar información de estado entre los clientes y la clase de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-389">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="5deba-390">El proxy de cliente proporciona un `state` objeto en el que puede almacenar datos que se van a transmitir al servidor con cada llamada al método.</span><span class="sxs-lookup"><span data-stu-id="5deba-390">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="5deba-391">En el servidor puede tener acceso a estos datos en el `Clients.Caller` propiedad en los métodos de concentrador que se llama a los clientes.</span><span class="sxs-lookup"><span data-stu-id="5deba-391">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="5deba-392">El `Clients.Caller` propiedad no se rellena para los métodos de controlador de eventos de duración de conexión `OnConnected`, `OnDisconnected`, y `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="5deba-392">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="5deba-393">Crear o actualizar datos en el `state` objeto y el `Clients.Caller` propiedad funciona en ambas direcciones.</span><span class="sxs-lookup"><span data-stu-id="5deba-393">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="5deba-394">También puede actualizar los valores en el servidor y se pasan al cliente.</span><span class="sxs-lookup"><span data-stu-id="5deba-394">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="5deba-395">En el ejemplo siguiente se muestra código de cliente de JavaScript que almacena el estado de transmisión en el servidor con cada llamada al método.</span><span class="sxs-lookup"><span data-stu-id="5deba-395">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

<span data-ttu-id="5deba-396">En el ejemplo siguiente se muestra el código equivalente en un cliente. NET.</span><span class="sxs-lookup"><span data-stu-id="5deba-396">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

<span data-ttu-id="5deba-397">En la clase base de datos central, puede tener acceso a estos datos en el `Clients.Caller` propiedad.</span><span class="sxs-lookup"><span data-stu-id="5deba-397">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="5deba-398">En el ejemplo siguiente se muestra código que recupera el estado que se hace referencia en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="5deba-398">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="5deba-399">Este mecanismo para mantener el estado no está diseñado para grandes cantidades de datos, porque todo lo que coloque en la `state` o `Clients.Caller` propiedad es de ida y vuelta con cada invocación del método.</span><span class="sxs-lookup"><span data-stu-id="5deba-399">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="5deba-400">Es útil para los elementos más pequeños como nombres de usuario o los contadores.</span><span class="sxs-lookup"><span data-stu-id="5deba-400">It's useful for smaller items such as user names or counters.</span></span>


<span data-ttu-id="5deba-401">En VB.NET o en un concentrador fuertemente tipada, el objeto de estado del autor de la llamada no puede obtenerse a través `Clients.Caller`; en su lugar, utilice `Clients.CallerState` (introducida en SignalR 2.1):</span><span class="sxs-lookup"><span data-stu-id="5deba-401">In VB.NET or in a strongly-typed hub, the caller state object can't be accessed through `Clients.Caller`; instead, use `Clients.CallerState` (introduced in SignalR 2.1):</span></span>

<span data-ttu-id="5deba-402">**Usar CallerState en C#**</span><span class="sxs-lookup"><span data-stu-id="5deba-402">**Using CallerState in C#**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

<span data-ttu-id="5deba-403">**Usar CallerState en Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="5deba-403">**Using CallerState in Visual Basic**</span></span>

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="5deba-404">Cómo controlar los errores en la clase de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-404">How to handle errors in the Hub class</span></span>

<span data-ttu-id="5deba-405">Para controlar los errores que se producen en los métodos de clase de base de datos central, utilice uno o varios de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5deba-405">To handle errors that occur in your Hub class methods, use one or more of the following methods:</span></span>

- <span data-ttu-id="5deba-406">Ajustar el código del método en bloques try-catch y registrar el objeto de excepción.</span><span class="sxs-lookup"><span data-stu-id="5deba-406">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="5deba-407">Para fines de depuración puede enviar la excepción al cliente, pero para la seguridad no se recomienda motivos enviar información detallada para los clientes de producción.</span><span class="sxs-lookup"><span data-stu-id="5deba-407">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="5deba-408">Crear un módulo de la canalización de concentradores que administra el [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) método.</span><span class="sxs-lookup"><span data-stu-id="5deba-408">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="5deba-409">En el ejemplo siguiente se muestra un módulo de canalización que registra los errores, seguidos por el código de Startup.cs que inserta el módulo en la canalización de concentradores.</span><span class="sxs-lookup"><span data-stu-id="5deba-409">The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- <span data-ttu-id="5deba-410">Use la `HubException` clase (introducida en SignalR 2).</span><span class="sxs-lookup"><span data-stu-id="5deba-410">Use the `HubException` class (introduced in SignalR 2).</span></span> <span data-ttu-id="5deba-411">Este error se puede producir de cualquier invocación de concentrador.</span><span class="sxs-lookup"><span data-stu-id="5deba-411">This error can be thrown from any hub invocation.</span></span> <span data-ttu-id="5deba-412">El `HubError` constructor toma un mensaje de cadena y un objeto para almacenar datos de error adicionales.</span><span class="sxs-lookup"><span data-stu-id="5deba-412">The `HubError` constructor takes a string message, and an object for storing extra error data.</span></span> <span data-ttu-id="5deba-413">SignalR se auto-serializar la excepción y enviarlo al cliente, donde se utilizará para rechazar o producirá un error en la invocación del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="5deba-413">SignalR will auto-serialize the exception and send it to the client, where it will be used to reject or fail the hub method invocation.</span></span>

    <span data-ttu-id="5deba-414">Los ejemplos de código siguiente muestran cómo producir una `HubException` durante una invocación de concentrador y cómo controlar la excepción en los clientes de JavaScript y. NET.</span><span class="sxs-lookup"><span data-stu-id="5deba-414">The following code samples demonstrate how to throw a `HubException` during a Hub invocation, and how to handle the exception on JavaScript and .NET clients.</span></span>

    <span data-ttu-id="5deba-415">**Código de servidor que muestra la clase HubException**</span><span class="sxs-lookup"><span data-stu-id="5deba-415">**Server code demonstrating the HubException class**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    <span data-ttu-id="5deba-416">**Código de cliente de JavaScript que muestra la respuesta para producir una HubException en un centro de**</span><span class="sxs-lookup"><span data-stu-id="5deba-416">**JavaScript client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    <span data-ttu-id="5deba-417">**Código de cliente de .NET que muestra la respuesta para producir una HubException en un centro de**</span><span class="sxs-lookup"><span data-stu-id="5deba-417">**.NET client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

<span data-ttu-id="5deba-418">Para obtener más información acerca de los módulos de la canalización de concentrador, consulte [cómo personalizar la canalización de concentradores](#hubpipeline) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="5deba-418">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="5deba-419">Cómo habilitar el seguimiento</span><span class="sxs-lookup"><span data-stu-id="5deba-419">How to enable tracing</span></span>

<span data-ttu-id="5deba-420">Para habilitar el seguimiento de servidor, agregue un elemento de system.diagnostics al archivo Web.config, tal y como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5deba-420">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

<span data-ttu-id="5deba-421">Al ejecutar la aplicación en Visual Studio, puede ver los registros en la **salida** ventana.</span><span class="sxs-lookup"><span data-stu-id="5deba-421">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="5deba-422">Cómo llamar a los métodos de cliente y administrar grupos desde fuera de la clase de base de datos central</span><span class="sxs-lookup"><span data-stu-id="5deba-422">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="5deba-423">Para llamar a cliente métodos de una clase diferente que la clase de base de datos central, obtener una referencia al objeto de contexto de SignalR para el concentrador y usarlo para llamar a métodos en el cliente o administrar grupos.</span><span class="sxs-lookup"><span data-stu-id="5deba-423">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="5deba-424">El ejemplo siguiente `StockTicker` clase obtiene el objeto de contexto, lo almacena en una instancia de la clase, almacena la instancia de clase en una propiedad estática y usa el contexto de la instancia de clase singleton para llamar a la `updateStockPrice` método en los clientes que están conectado a un concentrador denominado `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="5deba-424">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

<span data-ttu-id="5deba-425">Si tiene que utilizar los tiempos de varios de contexto en un objeto de larga duración, obtener la referencia de una vez y guardar en lugar de obtenerlo nuevo cada vez.</span><span class="sxs-lookup"><span data-stu-id="5deba-425">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="5deba-426">Obtener el contexto de una vez, se garantiza que SignalR envía mensajes a los clientes en la misma secuencia en la que los métodos de concentrador realizan a cliente las invocaciones de método.</span><span class="sxs-lookup"><span data-stu-id="5deba-426">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="5deba-427">Para obtener un tutorial que muestra cómo utilizar el contexto de SignalR para un concentrador, consulte [Server difusión con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="5deba-427">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="5deba-428">Llamar a métodos de cliente</span><span class="sxs-lookup"><span data-stu-id="5deba-428">Calling client methods</span></span>

<span data-ttu-id="5deba-429">Puede especificar que los clientes recibirán la RPC, pero tiene menos opciones que cuando se llama a partir de una clase de base de datos central.</span><span class="sxs-lookup"><span data-stu-id="5deba-429">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="5deba-430">El motivo es que el contexto no está asociado a una determinada llamada de un cliente, por lo que los métodos que requieren conocimientos del identificador de conexión actual, como `Clients.Others`, o `Clients.Caller`, o `Clients.OthersInGroup`, no están disponibles.</span><span class="sxs-lookup"><span data-stu-id="5deba-430">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="5deba-431">Están disponibles las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="5deba-431">The following options are available:</span></span>

- <span data-ttu-id="5deba-432">Todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="5deba-432">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- <span data-ttu-id="5deba-433">Un cliente específico identificado por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-433">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- <span data-ttu-id="5deba-434">Todos los clientes conectados excepción los clientes especificados, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-434">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- <span data-ttu-id="5deba-435">Todos los clientes en un grupo especificado conectados.</span><span class="sxs-lookup"><span data-stu-id="5deba-435">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- <span data-ttu-id="5deba-436">Todos los clientes en un grupo especificado, excepto los clientes especificados, identificado por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="5deba-436">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

<span data-ttu-id="5deba-437">Si está llamando a la clase de concentrador no de métodos en la clase de base de datos central, puede pasar el identificador de conexión actual y usarlo con `Clients.Client`, `Clients.AllExcept`, o `Clients.Group` para simular `Clients.Caller`, `Clients.Others`, o `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="5deba-437">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="5deba-438">En el ejemplo siguiente, la `MoveShapeHub` clase pasa el identificador de conexión para el `Broadcaster` clase para que la `Broadcaster` puede simular la clase `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="5deba-438">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="5deba-439">Administrar la pertenencia a grupos</span><span class="sxs-lookup"><span data-stu-id="5deba-439">Managing group membership</span></span>

<span data-ttu-id="5deba-440">Para administrar grupos de tener las mismas opciones tal y como haría en una clase de base de datos central.</span><span class="sxs-lookup"><span data-stu-id="5deba-440">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="5deba-441">Agregar a un cliente a un grupo</span><span class="sxs-lookup"><span data-stu-id="5deba-441">Add a client to a group</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- <span data-ttu-id="5deba-442">Quitar a un cliente de un grupo</span><span class="sxs-lookup"><span data-stu-id="5deba-442">Remove a client from a group</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="5deba-443">Cómo personalizar la canalización de concentradores</span><span class="sxs-lookup"><span data-stu-id="5deba-443">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="5deba-444">SignalR le permite insertar su propio código en la canalización de concentrador.</span><span class="sxs-lookup"><span data-stu-id="5deba-444">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="5deba-445">En el ejemplo siguiente se muestra un módulo personalizado de canalización de concentrador que registra cada llamada de método entrante recibido desde el cliente y la llamada de método de salida se invoca en el cliente:</span><span class="sxs-lookup"><span data-stu-id="5deba-445">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

<span data-ttu-id="5deba-446">El siguiente código en el *Startup.cs* archivo registra el módulo se ejecuten en la canalización de concentrador:</span><span class="sxs-lookup"><span data-stu-id="5deba-446">The following code in the *Startup.cs* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

<span data-ttu-id="5deba-447">Hay muchos métodos diferentes que se pueden reemplazar.</span><span class="sxs-lookup"><span data-stu-id="5deba-447">There are many different methods that you can override.</span></span> <span data-ttu-id="5deba-448">Para obtener una lista completa, consulte [HubPipelineModule métodos](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="5deba-448">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
