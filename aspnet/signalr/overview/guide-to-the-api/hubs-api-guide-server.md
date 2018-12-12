---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Guía de la API SignalR de ASP.NET Hubs - servidor (C#) | Microsoft Docs
author: pfletcher
description: Este documento proporciona una introducción a la programación del lado servidor de la API de concentradores de SignalR de ASP.NET para SignalR versión 2, con ejemplos de código que muestran...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: 4730c4d9f601f561cfc884e0a9c2c2d12785ae0f
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53288110"
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a><span data-ttu-id="29cc0-103">Guía de la API SignalR de ASP.NET Hubs - servidor (C#)</span><span class="sxs-lookup"><span data-stu-id="29cc0-103">ASP.NET SignalR Hubs API Guide - Server (C#)</span></span>
====================
<span data-ttu-id="29cc0-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="29cc0-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="29cc0-105">Este documento proporciona una introducción a la programación del lado servidor de la API de concentradores de SignalR de ASP.NET para SignalR versión 2, con ejemplos de código que muestran las opciones comunes.</span><span class="sxs-lookup"><span data-stu-id="29cc0-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 2, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="29cc0-106">La API de concentradores de SignalR permite realizar llamadas a procedimiento remoto (RPC) de un servidor a los clientes conectados y de clientes en el servidor.</span><span class="sxs-lookup"><span data-stu-id="29cc0-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="29cc0-107">En el código de servidor, definir los métodos que se pueden llamar a los clientes y llamar a métodos que se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="29cc0-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="29cc0-108">En el código de cliente, definir los métodos que pueden llamarse desde el servidor y llamar a métodos que se ejecutan en el servidor.</span><span class="sxs-lookup"><span data-stu-id="29cc0-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="29cc0-109">SignalR se ocupa de todos los mecanismos de cliente a servidor para usted.</span><span class="sxs-lookup"><span data-stu-id="29cc0-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="29cc0-110">SignalR también ofrece una API de nivel inferior denominada conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="29cc0-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="29cc0-111">Para obtener una introducción a SignalR, concentradores y conexiones persistentes, consulte [Introducción a SignalR 2](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="29cc0-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR 2](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="29cc0-112">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="29cc0-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="29cc0-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="29cc0-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="29cc0-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="29cc0-114">.NET 4.5</span></span>
> - <span data-ttu-id="29cc0-115">Versión 2 de SignalR</span><span class="sxs-lookup"><span data-stu-id="29cc0-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="topic-versions"></a><span data-ttu-id="29cc0-116">Versiones de tema</span><span class="sxs-lookup"><span data-stu-id="29cc0-116">Topic versions</span></span>
> 
> <span data-ttu-id="29cc0-117">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="29cc0-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="29cc0-118">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="29cc0-118">Questions and comments</span></span>
> 
> <span data-ttu-id="29cc0-119">Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="29cc0-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="29cc0-120">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="29cc0-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="29cc0-121">Información general</span><span class="sxs-lookup"><span data-stu-id="29cc0-121">Overview</span></span>

<span data-ttu-id="29cc0-122">Este documento contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="29cc0-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="29cc0-123">Cómo registrar el middleware de SignalR</span><span class="sxs-lookup"><span data-stu-id="29cc0-123">How to register SignalR middleware</span></span>](#route)

    - [<span data-ttu-id="29cc0-124">La dirección URL de /signalr</span><span class="sxs-lookup"><span data-stu-id="29cc0-124">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="29cc0-125">Configurar las opciones de SignalR</span><span class="sxs-lookup"><span data-stu-id="29cc0-125">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="29cc0-126">Cómo crear y usar clases de centro</span><span class="sxs-lookup"><span data-stu-id="29cc0-126">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="29cc0-127">Duración del objeto hub</span><span class="sxs-lookup"><span data-stu-id="29cc0-127">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="29cc0-128">Grafía Camel de nombres del centro en clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="29cc0-128">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="29cc0-129">Varios centros</span><span class="sxs-lookup"><span data-stu-id="29cc0-129">Multiple Hubs</span></span>](#multiplehubs)
    - [<span data-ttu-id="29cc0-130">Concentradores fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="29cc0-130">Strongly-Typed Hubs</span></span>](#stronglytypedhubs)
- [<span data-ttu-id="29cc0-131">Cómo definir métodos en la clase de concentrador que se pueden llamar los clientes</span><span class="sxs-lookup"><span data-stu-id="29cc0-131">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="29cc0-132">Grafía Camel de nombres de métodos en los clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="29cc0-132">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="29cc0-133">Cuándo se debe ejecutar de forma asincrónica</span><span class="sxs-lookup"><span data-stu-id="29cc0-133">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="29cc0-134">Definir sobrecargas</span><span class="sxs-lookup"><span data-stu-id="29cc0-134">Defining overloads</span></span>](#overloads)
    - [<span data-ttu-id="29cc0-135">Notificar el progreso de las invocaciones de método del concentrador</span><span class="sxs-lookup"><span data-stu-id="29cc0-135">Reporting progress from hub method invocations</span></span>](#progress)
- [<span data-ttu-id="29cc0-136">Cómo llamar a métodos de cliente de la clase Hub</span><span class="sxs-lookup"><span data-stu-id="29cc0-136">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="29cc0-137">Selección de los clientes que recibirán la RPC</span><span class="sxs-lookup"><span data-stu-id="29cc0-137">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="29cc0-138">Ninguna validación en tiempo de compilación para los nombres de método</span><span class="sxs-lookup"><span data-stu-id="29cc0-138">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="29cc0-139">Coincidencia de nombres de método entre mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="29cc0-139">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="29cc0-140">Ejecución asincrónica</span><span class="sxs-lookup"><span data-stu-id="29cc0-140">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="29cc0-141">Cómo administrar la pertenencia a grupos desde la clase Hub</span><span class="sxs-lookup"><span data-stu-id="29cc0-141">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="29cc0-142">Ejecución asincrónica de los métodos Add y Remove</span><span class="sxs-lookup"><span data-stu-id="29cc0-142">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="29cc0-143">Persistencia de pertenencia a grupo</span><span class="sxs-lookup"><span data-stu-id="29cc0-143">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="29cc0-144">Grupos de usuario único</span><span class="sxs-lookup"><span data-stu-id="29cc0-144">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="29cc0-145">Cómo controlar eventos de duración de la conexión en la clase Hub</span><span class="sxs-lookup"><span data-stu-id="29cc0-145">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="29cc0-146">Cuando se llama a OnConnected, OnDisconnected y OnReconnected</span><span class="sxs-lookup"><span data-stu-id="29cc0-146">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="29cc0-147">Estado del llamador no rellenado</span><span class="sxs-lookup"><span data-stu-id="29cc0-147">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="29cc0-148">Cómo obtener información sobre el cliente de la propiedad de contexto</span><span class="sxs-lookup"><span data-stu-id="29cc0-148">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="29cc0-149">Cómo pasar el estado entre los clientes y la clase Hub</span><span class="sxs-lookup"><span data-stu-id="29cc0-149">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="29cc0-150">Cómo controlar los errores en la clase Hub</span><span class="sxs-lookup"><span data-stu-id="29cc0-150">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="29cc0-151">Cómo llamar a los métodos de cliente y administrar grupos de fuera de la clase Hub</span><span class="sxs-lookup"><span data-stu-id="29cc0-151">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="29cc0-152">Llamar a métodos de cliente</span><span class="sxs-lookup"><span data-stu-id="29cc0-152">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="29cc0-153">Administrar la pertenencia a grupos</span><span class="sxs-lookup"><span data-stu-id="29cc0-153">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="29cc0-154">Cómo habilitar el seguimiento</span><span class="sxs-lookup"><span data-stu-id="29cc0-154">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="29cc0-155">Cómo personalizar la canalización de concentradores</span><span class="sxs-lookup"><span data-stu-id="29cc0-155">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="29cc0-156">Para obtener documentación sobre cómo los clientes del programa, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="29cc0-156">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="29cc0-157">Guía de API de concentradores de SignalR: cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="29cc0-157">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)
- [<span data-ttu-id="29cc0-158">Guía de API de concentradores de SignalR: cliente .NET</span><span class="sxs-lookup"><span data-stu-id="29cc0-158">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="29cc0-159">Los componentes de servidor de SignalR 2 solo están disponibles en .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="29cc0-159">The server components for SignalR 2 are only available in .NET 4.5.</span></span> <span data-ttu-id="29cc0-160">Los servidores que ejecutan .NET 4.0 deben usar SignalR v1.x.</span><span class="sxs-lookup"><span data-stu-id="29cc0-160">Servers running .NET 4.0 must use SignalR v1.x.</span></span>

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a><span data-ttu-id="29cc0-161">Cómo registrar el middleware de SignalR</span><span class="sxs-lookup"><span data-stu-id="29cc0-161">How to register SignalR middleware</span></span>

<span data-ttu-id="29cc0-162">Para definir la ruta que los clientes usarán para conectarse a su centro, llame a la `MapSignalR` método cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="29cc0-162">To define the route that clients will use to connect to your Hub, call the `MapSignalR` method when the application starts.</span></span> <span data-ttu-id="29cc0-163">`MapSignalR` es un [método de extensión](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) para el `OwinExtensions` clase.</span><span class="sxs-lookup"><span data-stu-id="29cc0-163">`MapSignalR` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `OwinExtensions` class.</span></span> <span data-ttu-id="29cc0-164">El ejemplo siguiente muestra cómo definir la ruta de concentradores de SignalR utilizando una clase de inicio OWIN.</span><span class="sxs-lookup"><span data-stu-id="29cc0-164">The following example shows how to define the SignalR Hubs route using an OWIN startup class.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

<span data-ttu-id="29cc0-165">Si va a agregar funcionalidad de SignalR a una aplicación ASP.NET MVC, asegúrese de que la ruta de SignalR se agrega delante de las otras rutas.</span><span class="sxs-lookup"><span data-stu-id="29cc0-165">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="29cc0-166">Para obtener más información, consulte [Tutorial: Introducción a SignalR 2 y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="29cc0-166">For more information, see [Tutorial: Getting Started with SignalR 2 and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="29cc0-167">La dirección URL de /signalr</span><span class="sxs-lookup"><span data-stu-id="29cc0-167">The /signalr URL</span></span>

<span data-ttu-id="29cc0-168">De forma predeterminada, es la dirección URL de ruta que los clientes usarán para conectarse a su centro de "/ signalr".</span><span class="sxs-lookup"><span data-stu-id="29cc0-168">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="29cc0-169">(No confunda esta dirección URL con la dirección URL "/ signalr/hubs", que es para el archivo JavaScript generado automáticamente.</span><span class="sxs-lookup"><span data-stu-id="29cc0-169">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="29cc0-170">Para obtener más información sobre el proxy generado, vea [Guía de la API SignalR Hubs: cliente JavaScript: el proxy generado y lo que hace por usted](hubs-api-guide-javascript-client.md#genproxy).)</span><span class="sxs-lookup"><span data-stu-id="29cc0-170">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span></span>

<span data-ttu-id="29cc0-171">Puede haber circunstancias excepcionales que hacen que esta dirección URL base no se puede usar para SignalR; Por ejemplo, tiene una carpeta en el proyecto denominado *signalr* y no desea cambiar el nombre.</span><span class="sxs-lookup"><span data-stu-id="29cc0-171">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="29cc0-172">En ese caso, puede cambiar la dirección URL base, como se muestra en los ejemplos siguientes (reemplace "/ signalr" en el código de ejemplo con la dirección URL deseada).</span><span class="sxs-lookup"><span data-stu-id="29cc0-172">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="29cc0-173">**Código de servidor que especifica la dirección URL**</span><span class="sxs-lookup"><span data-stu-id="29cc0-173">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

<span data-ttu-id="29cc0-174">**Código de cliente de JavaScript que especifica la dirección URL (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="29cc0-174">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

<span data-ttu-id="29cc0-175">**Código de cliente de JavaScript que especifica la dirección URL (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="29cc0-175">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="29cc0-176">**Código de cliente de .NET que especifica la dirección URL**</span><span class="sxs-lookup"><span data-stu-id="29cc0-176">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="29cc0-177">Configurar las opciones de SignalR</span><span class="sxs-lookup"><span data-stu-id="29cc0-177">Configuring SignalR Options</span></span>

<span data-ttu-id="29cc0-178">Las sobrecargas de los `MapSignalR` método permiten especificar una dirección URL personalizada, una resolución de dependencia personalizadas y las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="29cc0-178">Overloads of the `MapSignalR` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="29cc0-179">Permitir llamadas entre dominios mediante la CORS o JSONP desde clientes de explorador.</span><span class="sxs-lookup"><span data-stu-id="29cc0-179">Enable cross-domain calls using CORS or JSONP from browser clients.</span></span>

    <span data-ttu-id="29cc0-180">Normalmente, si el explorador carga una página de `http://contoso.com`, la conexión de SignalR está en el mismo dominio, en `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="29cc0-180">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="29cc0-181">Si la página de `http://contoso.com` realiza una conexión a `http://fabrikam.com/signalr`, que es una conexión entre dominios.</span><span class="sxs-lookup"><span data-stu-id="29cc0-181">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="29cc0-182">Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="29cc0-182">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="29cc0-183">Para obtener más información, consulte [Guía de la API de ASP.NET SignalR Hubs: cliente JavaScript - cómo establecer una conexión entre dominios](hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="29cc0-183">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span></span>
- <span data-ttu-id="29cc0-184">Permitir que los mensajes de error detallado.</span><span class="sxs-lookup"><span data-stu-id="29cc0-184">Enable detailed error messages.</span></span>

    <span data-ttu-id="29cc0-185">Cuando se producen errores, el comportamiento predeterminado de SignalR es enviar a los clientes de un mensaje de notificación sin detalles sobre lo que sucedió.</span><span class="sxs-lookup"><span data-stu-id="29cc0-185">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="29cc0-186">Enviar información detallada del error a los clientes no se recomienda en producción, porque es posible que los usuarios malintencionados pueda utilizar la información de los ataques contra la aplicación.</span><span class="sxs-lookup"><span data-stu-id="29cc0-186">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="29cc0-187">Para solucionar el problema, puede usar esta opción para permitir temporalmente informes de error más informativo.</span><span class="sxs-lookup"><span data-stu-id="29cc0-187">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="29cc0-188">Deshabilitar los archivos del proxy de JavaScript generados automáticamente.</span><span class="sxs-lookup"><span data-stu-id="29cc0-188">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="29cc0-189">De forma predeterminada, se genera un archivo de JavaScript con servidores proxy para las clases de Hub en respuesta a la dirección URL "/ signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="29cc0-189">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="29cc0-190">Si no desea usar a los servidores proxy de JavaScript, o si desea generar manualmente este archivo y hacer referencia a un archivo físico en los clientes, puede usar esta opción para deshabilitar la generación de proxy.</span><span class="sxs-lookup"><span data-stu-id="29cc0-190">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="29cc0-191">Para obtener más información, consulte [Guía de la API SignalR Hubs: cliente JavaScript - creación de un archivo físico para SignalR genera proxy](hubs-api-guide-javascript-client.md#manualproxy).</span><span class="sxs-lookup"><span data-stu-id="29cc0-191">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span></span>

<span data-ttu-id="29cc0-192">El ejemplo siguiente muestra cómo especificar la dirección URL de conexión de SignalR y estas opciones en una llamada a la `MapSignalR` método.</span><span class="sxs-lookup"><span data-stu-id="29cc0-192">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapSignalR` method.</span></span> <span data-ttu-id="29cc0-193">Para especificar una dirección URL personalizada, reemplace "/ signalr" en el ejemplo con la dirección URL que desea usar.</span><span class="sxs-lookup"><span data-stu-id="29cc0-193">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="29cc0-194">Cómo crear y usar clases de centro</span><span class="sxs-lookup"><span data-stu-id="29cc0-194">How to create and use Hub classes</span></span>

<span data-ttu-id="29cc0-195">Para crear un concentrador, cree una clase que derive de [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="29cc0-195">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="29cc0-196">El ejemplo siguiente muestra una clase de Hub simple para una aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="29cc0-196">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

<span data-ttu-id="29cc0-197">En este ejemplo, puede llamar un cliente conectado la `NewContosoChatMessage` método, y al mismo tiempo, los datos recibidos se difunden a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="29cc0-197">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="29cc0-198">Duración del objeto hub</span><span class="sxs-lookup"><span data-stu-id="29cc0-198">Hub object lifetime</span></span>

<span data-ttu-id="29cc0-199">No crear una instancia de la clase Hub o llamar a sus métodos desde su propio código en el servidor. todo lo que se hace automáticamente por la canalización de concentradores de SignalR.</span><span class="sxs-lookup"><span data-stu-id="29cc0-199">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="29cc0-200">SignalR crea una nueva instancia de la clase Hub cada vez que necesita para controlar una operación de concentrador, como cuando un cliente se conecta, desconecta o realiza un llamada de método en el servidor.</span><span class="sxs-lookup"><span data-stu-id="29cc0-200">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="29cc0-201">Dado que las instancias de la clase Hub son transitorias, no se pueden utilizar para mantener el estado de una llamada al método a la siguiente.</span><span class="sxs-lookup"><span data-stu-id="29cc0-201">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="29cc0-202">Cada vez que el servidor recibe una llamada al método desde un cliente, una nueva instancia de los procesos de la clase Hub el mensaje.</span><span class="sxs-lookup"><span data-stu-id="29cc0-202">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="29cc0-203">Para mantener el estado a través de varias conexiones y las llamadas a métodos, utilizar algún otro método, como una base de datos o una variable estática en la clase Hub o una clase diferente que no se deriva de `Hub`.</span><span class="sxs-lookup"><span data-stu-id="29cc0-203">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="29cc0-204">Si conservar datos en memoria, utilizando un método como una variable estática en la clase de Hub, los datos se perderán cuando se recicla el dominio de aplicación.</span><span class="sxs-lookup"><span data-stu-id="29cc0-204">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="29cc0-205">Si desea enviar mensajes a los clientes desde su propio código que se ejecuta fuera de la clase de Hub, no podrá hacerlo creando una instancia de la clase de concentrador, pero puede hacerlo mediante la obtención de una referencia al objeto de contexto de SignalR para la clase de concentrador.</span><span class="sxs-lookup"><span data-stu-id="29cc0-205">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="29cc0-206">Para obtener más información, consulte [cómo llamar a los métodos de cliente y administrar grupos de fuera de la clase Hub](#callfromoutsidehub) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="29cc0-206">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="29cc0-207">Grafía Camel de nombres del centro en clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="29cc0-207">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="29cc0-208">De forma predeterminada, los clientes de JavaScript consulte Hubs mediante el uso de una versión de mayúsculas y minúsculas camel del nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="29cc0-208">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="29cc0-209">SignalR realiza automáticamente este cambio para que el código JavaScript puede cumplir las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="29cc0-209">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="29cc0-210">El ejemplo anterior podría denominarse `contosoChatHub` en código de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="29cc0-210">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="29cc0-211">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="29cc0-211">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

<span data-ttu-id="29cc0-212">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="29cc0-212">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

<span data-ttu-id="29cc0-213">Si desea especificar un nombre diferente para los clientes usar, agregue el `HubName` atributo.</span><span class="sxs-lookup"><span data-stu-id="29cc0-213">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="29cc0-214">Cuando se usa un `HubName` atributo, no hay ningún cambio de nombre en mayúsculas y minúsculas en los clientes de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="29cc0-214">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="29cc0-215">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="29cc0-215">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

<span data-ttu-id="29cc0-216">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="29cc0-216">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="29cc0-217">Varios centros</span><span class="sxs-lookup"><span data-stu-id="29cc0-217">Multiple Hubs</span></span>

<span data-ttu-id="29cc0-218">Puede definir varias clases de Hub en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="29cc0-218">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="29cc0-219">Al hacerlo, se comparte la conexión pero grupos son independientes:</span><span class="sxs-lookup"><span data-stu-id="29cc0-219">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="29cc0-220">Todos los clientes usarán la misma dirección URL para establecer una conexión de SignalR con el servicio ("/ signalr" o la dirección URL personalizada si ha especificado uno), y que la conexión se utiliza para todos los concentradores definen por el servicio.</span><span class="sxs-lookup"><span data-stu-id="29cc0-220">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="29cc0-221">No hay ninguna diferencia de rendimiento de varios centros en comparación con la definición de toda la funcionalidad de Hub en una sola clase.</span><span class="sxs-lookup"><span data-stu-id="29cc0-221">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="29cc0-222">Todos los centros de obtención la misma información de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="29cc0-222">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="29cc0-223">Puesto que todos los concentradores comparten la misma conexión, la única información de la solicitud HTTP que obtiene el servidor es lo que viene en la solicitud HTTP original que establece la conexión de SignalR.</span><span class="sxs-lookup"><span data-stu-id="29cc0-223">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="29cc0-224">Si la solicitud de conexión se usa para pasar información del cliente al servidor mediante la especificación de una cadena de consulta, no puede proporcionar las cadenas de consulta diferentes en centros diferentes.</span><span class="sxs-lookup"><span data-stu-id="29cc0-224">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="29cc0-225">Todos los concentradores recibirán la misma información.</span><span class="sxs-lookup"><span data-stu-id="29cc0-225">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="29cc0-226">El archivo de los servidores proxy de JavaScript generado contendrá a los servidores proxy para todos los centros de un archivo.</span><span class="sxs-lookup"><span data-stu-id="29cc0-226">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="29cc0-227">Para obtener información sobre servidores proxy de JavaScript, consulte [Guía de la API SignalR Hubs: cliente JavaScript: el proxy generado y lo que hace por usted](hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="29cc0-227">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span></span>
- <span data-ttu-id="29cc0-228">Se definen grupos de concentradores.</span><span class="sxs-lookup"><span data-stu-id="29cc0-228">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="29cc0-229">En SignalR, que puede definir grupos para difundir a los subconjuntos de clientes conectados con nombre.</span><span class="sxs-lookup"><span data-stu-id="29cc0-229">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="29cc0-230">Los grupos se mantienen por separado para cada concentrador.</span><span class="sxs-lookup"><span data-stu-id="29cc0-230">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="29cc0-231">Por ejemplo, un grupo llamado "Administradores" incluye un conjunto de clientes para su `ContosoChatHub` clase y el mismo nombre de grupo haría referencia a un conjunto diferente de los clientes para su `StockTickerHub` clase.</span><span class="sxs-lookup"><span data-stu-id="29cc0-231">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a><span data-ttu-id="29cc0-232">Concentradores fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="29cc0-232">Strongly-Typed Hubs</span></span>

<span data-ttu-id="29cc0-233">Para definir una interfaz para los métodos de concentrador puede su cliente de referencia (y habilitar Intellisense en los métodos de concentrador), derive su centro de `Hub<T>` (introducida en SignalR 2.1) en lugar de `Hub`:</span><span class="sxs-lookup"><span data-stu-id="29cc0-233">To define an interface for your hub methods that your client can reference (and enable Intellisense on your hub methods), derive your hub from `Hub<T>` (introduced in SignalR 2.1) rather than `Hub`:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="29cc0-234">Cómo definir métodos en la clase de concentrador que se pueden llamar los clientes</span><span class="sxs-lookup"><span data-stu-id="29cc0-234">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="29cc0-235">Para exponer un método en el centro que desea que se pueda llamar desde el cliente, declare un método público, tal como se muestra en los ejemplos siguientes.</span><span class="sxs-lookup"><span data-stu-id="29cc0-235">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="29cc0-236">Puede especificar un tipo de valor devuelto y parámetros, incluidos los tipos complejos y matrices, como lo haría en cualquier método de C#.</span><span class="sxs-lookup"><span data-stu-id="29cc0-236">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="29cc0-237">Los datos que recibe en parámetros o devolver al llamador se comunican entre el cliente y el servidor mediante el uso de JSON, y SignalR controla el enlace de objetos complejos y matrices de objetos automáticamente.</span><span class="sxs-lookup"><span data-stu-id="29cc0-237">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="29cc0-238">Grafía Camel de nombres de métodos en los clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="29cc0-238">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="29cc0-239">De forma predeterminada, los clientes de JavaScript hacen referencia a los métodos de concentrador mediante el uso de una versión de mayúsculas y minúsculas camel del nombre del método.</span><span class="sxs-lookup"><span data-stu-id="29cc0-239">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="29cc0-240">SignalR realiza automáticamente este cambio para que el código JavaScript puede cumplir las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="29cc0-240">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="29cc0-241">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="29cc0-241">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="29cc0-242">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="29cc0-242">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="29cc0-243">Si desea especificar un nombre diferente para los clientes usar, agregue el `HubMethodName` atributo.</span><span class="sxs-lookup"><span data-stu-id="29cc0-243">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="29cc0-244">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="29cc0-244">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="29cc0-245">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="29cc0-245">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="29cc0-246">Cuándo se debe ejecutar de forma asincrónica</span><span class="sxs-lookup"><span data-stu-id="29cc0-246">When to execute asynchronously</span></span>

<span data-ttu-id="29cc0-247">Si el método se se larga o tiene que realizar el trabajo que implican la espera, por ejemplo, una búsqueda de la base de datos o una llamada de servicio web, convertir el método de concentrador a asincrónico devolviendo un [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (en lugar de `void` devolver) o [ Tarea&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) objeto (en lugar de `T` tipo de valor devuelto).</span><span class="sxs-lookup"><span data-stu-id="29cc0-247">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="29cc0-248">Cuando vuelva un `Task` espera a que el objeto desde el método de SignalR el `Task` en completarse, y, a continuación, envía el resultado sin ajustar al cliente, por lo que no hay ninguna diferencia en la manera en que codifica la llamada al método en el cliente.</span><span class="sxs-lookup"><span data-stu-id="29cc0-248">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="29cc0-249">Hacer que un método de concentrador asincrónicas evita el bloqueo de la conexión cuando utiliza el transporte de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="29cc0-249">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="29cc0-250">Cuando se ejecuta sincrónicamente un método de concentrador y el transporte es WebSocket, las siguientes invocaciones de métodos en el centro del mismo cliente se bloquean hasta que se complete el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="29cc0-250">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="29cc0-251">El ejemplo siguiente se muestra el mismo método en el código para ejecutar de forma sincrónica o asincrónicamente, seguido por el código de cliente de JavaScript que funciona para llamar a cualquiera de las versiones.</span><span class="sxs-lookup"><span data-stu-id="29cc0-251">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="29cc0-252">**Sincrónica**</span><span class="sxs-lookup"><span data-stu-id="29cc0-252">**Synchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="29cc0-253">**Asincrónica**</span><span class="sxs-lookup"><span data-stu-id="29cc0-253">**Asynchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="29cc0-254">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="29cc0-254">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="29cc0-255">Para obtener más información sobre cómo usar los métodos asincrónicos en ASP.NET 4.5, vea [utilizando los métodos asincrónicos en ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="29cc0-255">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="29cc0-256">Definir sobrecargas</span><span class="sxs-lookup"><span data-stu-id="29cc0-256">Defining Overloads</span></span>

<span data-ttu-id="29cc0-257">Si desea definir sobrecargas para un método, el número de parámetros en cada sobrecarga debe ser diferente.</span><span class="sxs-lookup"><span data-stu-id="29cc0-257">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="29cc0-258">Si diferenciar una sobrecarga simplemente mediante la especificación de distintos tipos de parámetro, la clase de concentrador se compilarán, pero el servicio SignalR iniciará una excepción en tiempo de ejecución cuando los clientes intentan para llamar a una de las sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="29cc0-258">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a><span data-ttu-id="29cc0-259">Notificar el progreso de las invocaciones de método del concentrador</span><span class="sxs-lookup"><span data-stu-id="29cc0-259">Reporting progress from hub method invocations</span></span>

<span data-ttu-id="29cc0-260">2.1 SignalR agrega compatibilidad para la [patrón para informar del progreso](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introducidas en .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="29cc0-260">SignalR 2.1 adds support for the [progress reporting pattern](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduced in .NET 4.5.</span></span> <span data-ttu-id="29cc0-261">Para implementar informes de progreso, definir un `IProgress<T>` parámetro para el método de concentrador que pueda acceder el cliente:</span><span class="sxs-lookup"><span data-stu-id="29cc0-261">To implement progress reporting, define an `IProgress<T>` parameter for your hub method that your client can access:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

<span data-ttu-id="29cc0-262">Al escribir un método de servidor de ejecución prolongada, es importante usar un modelo de programación asincrónico como Async / Await en lugar de bloquear el subproceso de concentrador.</span><span class="sxs-lookup"><span data-stu-id="29cc0-262">When writing a long-running server method, it is important to use an asynchronous programming pattern like Async/ Await rather than blocking the hub thread.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="29cc0-263">Cómo llamar a métodos de cliente de la clase Hub</span><span class="sxs-lookup"><span data-stu-id="29cc0-263">How to call client methods from the Hub class</span></span>

<span data-ttu-id="29cc0-264">Para llamar métodos de cliente desde el servidor, use el `Clients` propiedad en un método en la clase de concentrador.</span><span class="sxs-lookup"><span data-stu-id="29cc0-264">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="29cc0-265">El ejemplo siguiente muestra código de servidor que llama a `addNewMessageToPage` en los clientes conectados todo y código de cliente que define el método en un cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="29cc0-265">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="29cc0-266">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="29cc0-266">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

<span data-ttu-id="29cc0-267">Invocar un método de cliente es una operación asincrónica y devuelve un `Task`.</span><span class="sxs-lookup"><span data-stu-id="29cc0-267">Invoking a client method is an asynchronous operation and returns a `Task`.</span></span> <span data-ttu-id="29cc0-268">Use `await`:</span><span class="sxs-lookup"><span data-stu-id="29cc0-268">Use `await`:</span></span>

* <span data-ttu-id="29cc0-269">Para asegurarse de que el mensaje se envió sin errores.</span><span class="sxs-lookup"><span data-stu-id="29cc0-269">To ensure the message is sent without error.</span></span> 
* <span data-ttu-id="29cc0-270">Para habilitar la detección y control de errores en un bloque try-catch.</span><span class="sxs-lookup"><span data-stu-id="29cc0-270">To enable catching and handling errors in a try-catch block.</span></span>

<span data-ttu-id="29cc0-271">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="29cc0-271">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

<span data-ttu-id="29cc0-272">No se puede obtener un valor devuelto de un método de cliente; sintaxis como `int x = Clients.All.add(1,1)` no funciona.</span><span class="sxs-lookup"><span data-stu-id="29cc0-272">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="29cc0-273">Puede especificar los tipos complejos y matrices para los parámetros.</span><span class="sxs-lookup"><span data-stu-id="29cc0-273">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="29cc0-274">El siguiente ejemplo pasa un tipo complejo al cliente en un parámetro de método.</span><span class="sxs-lookup"><span data-stu-id="29cc0-274">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="29cc0-275">**Código de servidor que llama a un método de cliente mediante un objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="29cc0-275">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

<span data-ttu-id="29cc0-276">**Código de servidor que define el objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="29cc0-276">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

<span data-ttu-id="29cc0-277">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="29cc0-277">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="29cc0-278">Selección de los clientes que recibirán la RPC</span><span class="sxs-lookup"><span data-stu-id="29cc0-278">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="29cc0-279">La propiedad no devuelve los clientes un [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) objeto que proporciona varias opciones para especificar que los clientes recibirán la RPC:</span><span class="sxs-lookup"><span data-stu-id="29cc0-279">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="29cc0-280">Todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="29cc0-280">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="29cc0-281">Solo el cliente que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="29cc0-281">Only the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="29cc0-282">Todos los clientes, excepto el cliente que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="29cc0-282">All clients except the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- <span data-ttu-id="29cc0-283">Un cliente específico identificado por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="29cc0-283">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    <span data-ttu-id="29cc0-284">Este ejemplo llama a `addContosoChatMessageToPage` en el cliente que realiza la llamada y tiene el mismo efecto que usar `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="29cc0-284">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="29cc0-285">Todos los clientes conectados, excepto los clientes especificados, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="29cc0-285">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- <span data-ttu-id="29cc0-286">Todos los clientes en un grupo especificado conectados.</span><span class="sxs-lookup"><span data-stu-id="29cc0-286">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- <span data-ttu-id="29cc0-287">Todos los clientes conectados en un grupo especificado excepción los clientes especificados, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="29cc0-287">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- <span data-ttu-id="29cc0-288">Todos los clientes conectados en un grupo especificado, excepto el cliente que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="29cc0-288">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- <span data-ttu-id="29cc0-289">Un usuario específico, identificado por el identificador de usuario.</span><span class="sxs-lookup"><span data-stu-id="29cc0-289">A specific user, identified by userId.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    <span data-ttu-id="29cc0-290">De forma predeterminada, se trata de `IPrincipal.Identity.Name`, pero puede cambiarse por [registrarse en el host global de una implementación de IUserIdProvider](mapping-users-to-connections.md#IUserIdProvider).</span><span class="sxs-lookup"><span data-stu-id="29cc0-290">By default, this is `IPrincipal.Identity.Name`, but this can be changed by [registering an implementation of IUserIdProvider with the global host](mapping-users-to-connections.md#IUserIdProvider).</span></span>
- <span data-ttu-id="29cc0-291">Todos los clientes y grupos de una lista de identificadores de conexión.</span><span class="sxs-lookup"><span data-stu-id="29cc0-291">All clients and groups in a list of connection IDs.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- <span data-ttu-id="29cc0-292">Una lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="29cc0-292">A list of groups.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- <span data-ttu-id="29cc0-293">Un usuario por nombre.</span><span class="sxs-lookup"><span data-stu-id="29cc0-293">A user by name.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- <span data-ttu-id="29cc0-294">Una lista de nombres de usuario (introducida en SignalR 2.1).</span><span class="sxs-lookup"><span data-stu-id="29cc0-294">A list of user names (introduced in SignalR 2.1).</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="29cc0-295">Ninguna validación en tiempo de compilación para los nombres de método</span><span class="sxs-lookup"><span data-stu-id="29cc0-295">No compile-time validation for method names</span></span>

<span data-ttu-id="29cc0-296">El nombre del método que especifique se interpreta como un objeto dinámico, lo que significa que hay ningún IntelliSense ni la validación en tiempo de compilación para él.</span><span class="sxs-lookup"><span data-stu-id="29cc0-296">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="29cc0-297">La expresión se evalúa en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="29cc0-297">The expression is evaluated at run time.</span></span> <span data-ttu-id="29cc0-298">Cuando se ejecuta la llamada al método, SignalR envía el nombre del método y los valores de parámetro al cliente y, si el cliente tiene un método que coincide con el nombre, que se llama al método y los valores de parámetros se pasan a él.</span><span class="sxs-lookup"><span data-stu-id="29cc0-298">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="29cc0-299">Si no se encuentra ningún método coincidente en el cliente, se produce ningún error.</span><span class="sxs-lookup"><span data-stu-id="29cc0-299">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="29cc0-300">Para obtener información sobre el formato de los datos que SignalR se transmite al cliente en segundo plano cuando se llama a un método de cliente, consulte [Introducción a SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="29cc0-300">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="29cc0-301">Coincidencia de nombres de método entre mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="29cc0-301">Case-insensitive method name matching</span></span>

<span data-ttu-id="29cc0-302">Coincidencia de nombres de método distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="29cc0-302">Method name matching is case-insensitive.</span></span> <span data-ttu-id="29cc0-303">Por ejemplo, `Clients.All.addContosoChatMessageToPage` se ejecutará en el servidor `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, o `addContosoChatMessageToPage` en el cliente.</span><span class="sxs-lookup"><span data-stu-id="29cc0-303">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="29cc0-304">Ejecución asincrónica</span><span class="sxs-lookup"><span data-stu-id="29cc0-304">Asynchronous execution</span></span>

<span data-ttu-id="29cc0-305">El método al que llama se ejecuta de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="29cc0-305">The method that you call executes asynchronously.</span></span> <span data-ttu-id="29cc0-306">Cualquier código que viene después de una llamada de método a un cliente se ejecutará inmediatamente sin esperar a SignalR transmitir datos a los clientes a menos que especifique que las siguientes líneas de código deben esperar la finalización del método de fin.</span><span class="sxs-lookup"><span data-stu-id="29cc0-306">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="29cc0-307">En el ejemplo de código siguiente se muestra cómo dos métodos de cliente se ejecutan secuencialmente.</span><span class="sxs-lookup"><span data-stu-id="29cc0-307">The following code example shows how to execute two client methods sequentially.</span></span>

<span data-ttu-id="29cc0-308">**Uso de Await (.NET 4.5)**</span><span class="sxs-lookup"><span data-stu-id="29cc0-308">**Using Await (.NET 4.5)**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="29cc0-309">Si usa `await` para esperar hasta que un método de cliente finalice antes de que ejecute la siguiente línea de código, eso no significa que los clientes recibirán realmente el mensaje antes de que ejecute la siguiente línea de código.</span><span class="sxs-lookup"><span data-stu-id="29cc0-309">If you use `await` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="29cc0-310">"Conclusión" de una llamada al método de cliente sólo significa que SignalR ha hecho todo lo necesario enviar el mensaje.</span><span class="sxs-lookup"><span data-stu-id="29cc0-310">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="29cc0-311">Si necesita que los clientes reciben el mensaje de comprobación, deberá programar ese mecanismo usted mismo.</span><span class="sxs-lookup"><span data-stu-id="29cc0-311">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="29cc0-312">Por ejemplo, podríamos codificar un `MessageReceived` método del concentrador y, en el `addContosoChatMessageToPage` método en el cliente puede llamar a `MessageReceived` después de hacer el trabajo que se debe hacer en el cliente.</span><span class="sxs-lookup"><span data-stu-id="29cc0-312">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="29cc0-313">En `MessageReceived` en el concentrador puede hacer cualquier trabajo depende de la recepción de cliente real y el procesamiento de la llamada al método original.</span><span class="sxs-lookup"><span data-stu-id="29cc0-313">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="29cc0-314">Cómo usar una variable de cadena como el nombre del método</span><span class="sxs-lookup"><span data-stu-id="29cc0-314">How to use a string variable as the method name</span></span>

<span data-ttu-id="29cc0-315">Si desea invocar un método de cliente mediante el uso de una variable de cadena como el nombre del método, convertir `Clients.All` (o `Clients.Others`, `Clients.Caller`, etc.) a `IClientProxy` y, a continuación, llame a [Invoke (methodName, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="29cc0-315">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="29cc0-316">Cómo administrar la pertenencia a grupos desde la clase Hub</span><span class="sxs-lookup"><span data-stu-id="29cc0-316">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="29cc0-317">Los grupos en SignalR proporcionan un método para difundir mensajes a subconjuntos especificados de los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="29cc0-317">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="29cc0-318">Un grupo puede tener cualquier número de clientes y un cliente puede ser un miembro de cualquier número de grupos.</span><span class="sxs-lookup"><span data-stu-id="29cc0-318">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="29cc0-319">Para administrar la pertenencia a grupos, use el [agregar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) y [quitar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) métodos proporcionados por el `Groups` propiedad de la clase de concentrador.</span><span class="sxs-lookup"><span data-stu-id="29cc0-319">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="29cc0-320">El ejemplo siguiente se muestra el `Groups.Add` y `Groups.Remove` métodos utilizados en los métodos de concentrador que son llamados por código de cliente, seguido por el código de cliente de JavaScript que llama a ellos.</span><span class="sxs-lookup"><span data-stu-id="29cc0-320">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="29cc0-321">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="29cc0-321">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

<span data-ttu-id="29cc0-322">**Cliente de JavaScript mediante el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="29cc0-322">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

<span data-ttu-id="29cc0-323">No tiene que crear explícitamente grupos.</span><span class="sxs-lookup"><span data-stu-id="29cc0-323">You don't have to explicitly create groups.</span></span> <span data-ttu-id="29cc0-324">En efecto un grupo se crea automáticamente la primera vez que especifique su nombre en una llamada a `Groups.Add`, y se elimina cuando se quita la última conexión de la pertenencia a él.</span><span class="sxs-lookup"><span data-stu-id="29cc0-324">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="29cc0-325">No hay ninguna API para obtener una lista de miembros de grupo o una lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="29cc0-325">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="29cc0-326">SignalR envía mensajes a los clientes y grupos según un [modelo pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), y el servidor no mantiene las listas de grupos o pertenencias a grupos.</span><span class="sxs-lookup"><span data-stu-id="29cc0-326">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="29cc0-327">Esto ayuda a maximizar la escalabilidad, ya que cada vez que agrega un nodo a una granja de servidores web, cualquier estado que mantiene SignalR se propaguen al nuevo nodo.</span><span class="sxs-lookup"><span data-stu-id="29cc0-327">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="29cc0-328">Ejecución asincrónica de los métodos Add y Remove</span><span class="sxs-lookup"><span data-stu-id="29cc0-328">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="29cc0-329">El `Groups.Add` y `Groups.Remove` métodos se ejecutan de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="29cc0-329">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="29cc0-330">Si desea agregar un cliente a un grupo y enviar inmediatamente un mensaje al cliente mediante el grupo, deberá asegurarse de que el `Groups.Add` método finaliza primero.</span><span class="sxs-lookup"><span data-stu-id="29cc0-330">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="29cc0-331">El ejemplo de código siguiente muestra cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="29cc0-331">The following code example shows how to do that.</span></span>

<span data-ttu-id="29cc0-332">**Adición de un cliente a un grupo y, a continuación, ese cliente de mensajería**</span><span class="sxs-lookup"><span data-stu-id="29cc0-332">**Adding a client to a group and then messaging that client**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="29cc0-333">Persistencia de pertenencia a grupo</span><span class="sxs-lookup"><span data-stu-id="29cc0-333">Group membership persistence</span></span>

<span data-ttu-id="29cc0-334">SignalR realiza un seguimiento de las conexiones, no a los usuarios, por lo que si desea que un usuario esté en el mismo grupo cada vez que el usuario establece una conexión, se debe llamar a `Groups.Add` cada vez que el usuario establezca una conexión nueva.</span><span class="sxs-lookup"><span data-stu-id="29cc0-334">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="29cc0-335">Después de una pérdida temporal de conectividad, en ocasiones, SignalR puede restaurar la conexión automáticamente.</span><span class="sxs-lookup"><span data-stu-id="29cc0-335">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="29cc0-336">En ese caso, SignalR está restaurando la misma conexión, sin establecer una conexión nueva y, por lo que se restaura automáticamente pertenencia a grupos de clientes.</span><span class="sxs-lookup"><span data-stu-id="29cc0-336">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="29cc0-337">Esto es posible incluso cuando la interrupción temporal es el resultado de un reinicio del servidor o un error, porque el estado de conexión para cada cliente, incluida la pertenencia a grupos, es de ida y vuelta al cliente.</span><span class="sxs-lookup"><span data-stu-id="29cc0-337">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="29cc0-338">Si un servidor deja de funcionar y se reemplaza por un nuevo servidor antes de la conexión agota el tiempo, un cliente puede volver a conectar automáticamente al nuevo servidor y volver a inscribirte en es un miembro de los grupos.</span><span class="sxs-lookup"><span data-stu-id="29cc0-338">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="29cc0-339">Cuando una conexión no se puede restaurar automáticamente después de una pérdida de conectividad, o cuando se agota el tiempo de espera de la conexión, o cuando se desconecta el cliente (por ejemplo, cuando un explorador se desplaza a una nueva página), se pierden las pertenencias a grupos.</span><span class="sxs-lookup"><span data-stu-id="29cc0-339">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="29cc0-340">La próxima vez que se conecta el usuario será una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="29cc0-340">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="29cc0-341">Para mantener las pertenencias a grupos cuando el mismo usuario establezca una conexión nueva, la aplicación tiene que realizar un seguimiento de las asociaciones entre usuarios y grupos y restaurar las pertenencias a grupos cada vez que un usuario establece una conexión nueva.</span><span class="sxs-lookup"><span data-stu-id="29cc0-341">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="29cc0-342">Para obtener más información sobre las conexiones y reconexiones, consulte [cómo controlar eventos de duración de la conexión en la clase Hub](#connectionlifetime) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="29cc0-342">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="29cc0-343">Grupos de usuario único</span><span class="sxs-lookup"><span data-stu-id="29cc0-343">Single-user groups</span></span>

<span data-ttu-id="29cc0-344">Las aplicaciones que usan SignalR normalmente tienen que realizar un seguimiento de las asociaciones entre usuarios y conexiones con el fin de saber qué usuario ha enviado un mensaje y qué usuarios deben recibir un mensaje.</span><span class="sxs-lookup"><span data-stu-id="29cc0-344">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="29cc0-345">Los grupos se usan en uno de los dos patrones usados para hacerlo.</span><span class="sxs-lookup"><span data-stu-id="29cc0-345">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="29cc0-346">Grupos de usuario único.</span><span class="sxs-lookup"><span data-stu-id="29cc0-346">Single-user groups.</span></span>

    <span data-ttu-id="29cc0-347">Puede especificar el nombre de usuario como el nombre del grupo y agregar el identificador de conexión actual al grupo cada vez que el usuario se conecta o se vuelve a conectar.</span><span class="sxs-lookup"><span data-stu-id="29cc0-347">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="29cc0-348">Para enviar mensajes al usuario enviar al grupo.</span><span class="sxs-lookup"><span data-stu-id="29cc0-348">To send messages to the user you send to the group.</span></span> <span data-ttu-id="29cc0-349">Una desventaja de este método es que el grupo no le proporciona una manera de averiguar si el usuario está conectado o desconectado.</span><span class="sxs-lookup"><span data-stu-id="29cc0-349">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="29cc0-350">Realizar un seguimiento de las asociaciones entre los nombres de usuario e identificadores de conexión.</span><span class="sxs-lookup"><span data-stu-id="29cc0-350">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="29cc0-351">Puede almacenar una asociación entre cada nombre de usuario y una o más identificadores de conexión en un diccionario o una base de datos y actualizar los datos almacenados cada vez que el usuario se conecta o desconecta.</span><span class="sxs-lookup"><span data-stu-id="29cc0-351">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="29cc0-352">Para enviar mensajes al usuario especificar los identificadores de conexión.</span><span class="sxs-lookup"><span data-stu-id="29cc0-352">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="29cc0-353">Una desventaja de este método es que consume más memoria.</span><span class="sxs-lookup"><span data-stu-id="29cc0-353">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="29cc0-354">Cómo controlar eventos de duración de la conexión en la clase Hub</span><span class="sxs-lookup"><span data-stu-id="29cc0-354">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="29cc0-355">Algunos motivos habituales para controlar los eventos de duración de conexión son para realizar un seguimiento de si un usuario está conectado o no y realizar un seguimiento de la asociación entre los nombres de usuario e identificadores de conexión.</span><span class="sxs-lookup"><span data-stu-id="29cc0-355">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="29cc0-356">Para ejecutar su propio código cuando los clientes conexión o desconexión, invalidar el `OnConnected`, `OnDisconnected`, y `OnReconnected` métodos virtuales del centro de la clase, como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="29cc0-356">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="29cc0-357">Cuando se llama a OnConnected, OnDisconnected y OnReconnected</span><span class="sxs-lookup"><span data-stu-id="29cc0-357">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="29cc0-358">Cada vez que un explorador se desplaza a una página nueva, una nueva conexión tiene que establecerse, lo que significa que se ejecutará SignalR el `OnDisconnected` método seguido por el `OnConnected` método.</span><span class="sxs-lookup"><span data-stu-id="29cc0-358">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="29cc0-359">SignalR crea siempre un nuevo identificador de conexión cuando se establece una conexión nueva.</span><span class="sxs-lookup"><span data-stu-id="29cc0-359">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="29cc0-360">El `OnReconnected` se llama al método cuando se ha producido una interrupción temporal de conectividad que SignalR puede recuperarse automáticamente, por ejemplo, cuando un cable está temporalmente desconectado y vuelto a conectar antes de la conexión agota el tiempo. El `OnDisconnected` se llama al método cuando el cliente se desconecta y SignalR no se puede volver a conectar automáticamente, por ejemplo, cuando un explorador navega a una página nueva.</span><span class="sxs-lookup"><span data-stu-id="29cc0-360">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="29cc0-361">Por lo tanto, es una secuencia de eventos de un cliente determinado posible `OnConnected`, `OnReconnected`, `OnDisconnected`; o `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="29cc0-361">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="29cc0-362">No verá la secuencia `OnConnected`, `OnDisconnected`, `OnReconnected` para una conexión determinada.</span><span class="sxs-lookup"><span data-stu-id="29cc0-362">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="29cc0-363">El `OnDisconnected` método que no se llama en algunos escenarios, como cuando un servidor deja de funcionar u obtiene Reciclando el dominio de aplicación.</span><span class="sxs-lookup"><span data-stu-id="29cc0-363">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="29cc0-364">Cuando otro servidor que se incluye en la línea o el dominio de aplicación se completa su reciclaje, algunos clientes pueden que pueda volver a conectar y activar el `OnReconnected` eventos.</span><span class="sxs-lookup"><span data-stu-id="29cc0-364">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="29cc0-365">Para obtener más información, consulte [comprensión y control de eventos de duración de conexión en SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="29cc0-365">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="29cc0-366">Estado del llamador no rellenado</span><span class="sxs-lookup"><span data-stu-id="29cc0-366">Caller state not populated</span></span>

<span data-ttu-id="29cc0-367">Los métodos de controlador de eventos de duración de conexión se llaman desde el servidor, lo que significa que cualquier estado que se coloca en el `state` objeto en el cliente no se rellenarán en la `Caller` propiedad en el servidor.</span><span class="sxs-lookup"><span data-stu-id="29cc0-367">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="29cc0-368">Para obtener información sobre la `state` objeto y el `Caller` propiedad, vea [cómo pasar el estado entre los clientes y la clase Hub](#passstate) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="29cc0-368">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="29cc0-369">Cómo obtener información sobre el cliente de la propiedad de contexto</span><span class="sxs-lookup"><span data-stu-id="29cc0-369">How to get information about the client from the Context property</span></span>

<span data-ttu-id="29cc0-370">Para obtener información sobre el cliente, use el `Context` propiedad de la clase de concentrador.</span><span class="sxs-lookup"><span data-stu-id="29cc0-370">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="29cc0-371">El `Context` propiedad devuelve un [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) objeto que proporciona acceso a la siguiente información:</span><span class="sxs-lookup"><span data-stu-id="29cc0-371">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="29cc0-372">El identificador de conexión del cliente que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="29cc0-372">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    <span data-ttu-id="29cc0-373">El identificador de conexión es un GUID asignado por SignalR (no se puede especificar el valor en su propio código).</span><span class="sxs-lookup"><span data-stu-id="29cc0-373">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="29cc0-374">Hay un identificador de conexión para cada conexión y la misma conexión de que identificador es usado por todos los concentradores si tiene varios centros en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="29cc0-374">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="29cc0-375">Datos de encabezado HTTP.</span><span class="sxs-lookup"><span data-stu-id="29cc0-375">HTTP header data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="29cc0-376">También puede obtener los encabezados HTTP desde `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="29cc0-376">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="29cc0-377">El motivo para varias referencias a la misma cosa es que `Context.Headers` se crean en primer lugar, el `Context.Request` propiedad la agregó más tarde, y `Context.Headers` se conserva por compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="29cc0-377">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="29cc0-378">Datos de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="29cc0-378">Query string data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    <span data-ttu-id="29cc0-379">También puede obtener datos de cadena de consulta de `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="29cc0-379">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="29cc0-380">La cadena de consulta que se obtiene en esta propiedad es el que se usó con la solicitud HTTP que se estableció la conexión de SignalR.</span><span class="sxs-lookup"><span data-stu-id="29cc0-380">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="29cc0-381">Puede agregar parámetros de cadena de consulta en el cliente mediante la configuración de la conexión, que es una manera cómoda para pasar datos sobre el cliente desde el cliente al servidor.</span><span class="sxs-lookup"><span data-stu-id="29cc0-381">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="29cc0-382">El ejemplo siguiente muestra una forma de agregar una cadena de consulta en un cliente de JavaScript cuando se usa el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="29cc0-382">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    <span data-ttu-id="29cc0-383">Para obtener más información sobre cómo establecer parámetros de cadena de consulta, consulte las guías de API para el [JavaScript](hubs-api-guide-javascript-client.md) y [.NET](hubs-api-guide-net-client.md) los clientes.</span><span class="sxs-lookup"><span data-stu-id="29cc0-383">For more information about setting query string parameters, see the API guides for the [JavaScript](hubs-api-guide-javascript-client.md) and [.NET](hubs-api-guide-net-client.md) clients.</span></span>

    <span data-ttu-id="29cc0-384">Puede encontrar el método de transporte utilizado para la conexión en los datos de cadena de consulta, junto con algunos otros valores que se usa internamente SignalR:</span><span class="sxs-lookup"><span data-stu-id="29cc0-384">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    <span data-ttu-id="29cc0-385">El valor de `transportMethod` será "webSockets", "serverSentEvents", "foreverFrame" o "longPolling".</span><span class="sxs-lookup"><span data-stu-id="29cc0-385">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="29cc0-386">Tenga en cuenta que si comprobar este valor el `OnConnected` método de controlador de eventos, en algunos escenarios inicialmente podría obtener un valor de transporte que no es el método de transporte negociada final para la conexión.</span><span class="sxs-lookup"><span data-stu-id="29cc0-386">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="29cc0-387">En ese caso, el método producirá una excepción y se llamará de nuevo más tarde cuando se establece el modo de transporte final.</span><span class="sxs-lookup"><span data-stu-id="29cc0-387">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="29cc0-388">Cookies.</span><span class="sxs-lookup"><span data-stu-id="29cc0-388">Cookies.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    <span data-ttu-id="29cc0-389">También puede obtener las cookies de `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="29cc0-389">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="29cc0-390">Información del usuario.</span><span class="sxs-lookup"><span data-stu-id="29cc0-390">User information.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- <span data-ttu-id="29cc0-391">El objeto HttpContext para la solicitud:</span><span class="sxs-lookup"><span data-stu-id="29cc0-391">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    <span data-ttu-id="29cc0-392">Use este método en lugar de obtener `HttpContext.Current` para obtener el `HttpContext` objeto para la conexión de SignalR.</span><span class="sxs-lookup"><span data-stu-id="29cc0-392">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="29cc0-393">Cómo pasar el estado entre los clientes y la clase Hub</span><span class="sxs-lookup"><span data-stu-id="29cc0-393">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="29cc0-394">El proxy de cliente proporciona un `state` objeto en el que puede almacenar datos que desea que se transmite al servidor con cada llamada al método.</span><span class="sxs-lookup"><span data-stu-id="29cc0-394">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="29cc0-395">En el servidor puede tener acceso a estos datos en el `Clients.Caller` propiedad en los métodos de concentrador que lo llaman los clientes.</span><span class="sxs-lookup"><span data-stu-id="29cc0-395">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="29cc0-396">El `Clients.Caller` propiedad no se rellena para los métodos de controlador de eventos de duración de conexión `OnConnected`, `OnDisconnected`, y `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="29cc0-396">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="29cc0-397">Crear o actualizar datos en el `state` objeto y el `Clients.Caller` propiedad funciona en ambas direcciones.</span><span class="sxs-lookup"><span data-stu-id="29cc0-397">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="29cc0-398">Puede actualizar valores en el servidor y se pasan al cliente.</span><span class="sxs-lookup"><span data-stu-id="29cc0-398">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="29cc0-399">El ejemplo siguiente muestra código de cliente de JavaScript que almacena el estado para la transmisión al servidor con cada llamada al método.</span><span class="sxs-lookup"><span data-stu-id="29cc0-399">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

<span data-ttu-id="29cc0-400">El ejemplo siguiente muestra el código equivalente en un cliente. NET.</span><span class="sxs-lookup"><span data-stu-id="29cc0-400">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

<span data-ttu-id="29cc0-401">En la clase de Hub, puede tener acceso a estos datos en el `Clients.Caller` propiedad.</span><span class="sxs-lookup"><span data-stu-id="29cc0-401">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="29cc0-402">El ejemplo siguiente muestra código que recupera el estado mencionado en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="29cc0-402">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="29cc0-403">Este mecanismo para conservar el estado no está diseñada para grandes cantidades de datos, desde todo lo coloca en el `state` o `Clients.Caller` propiedad es de ida y vuelta con cada invocación del método.</span><span class="sxs-lookup"><span data-stu-id="29cc0-403">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="29cc0-404">Es útil para los elementos más pequeños, como los nombres de usuario o los contadores.</span><span class="sxs-lookup"><span data-stu-id="29cc0-404">It's useful for smaller items such as user names or counters.</span></span>


<span data-ttu-id="29cc0-405">En VB.NET o en un concentrador fuertemente tipada, el objeto de estado de autor de llamada no puede obtenerse a través `Clients.Caller`; en su lugar, utilice `Clients.CallerState` (introducida en SignalR 2.1):</span><span class="sxs-lookup"><span data-stu-id="29cc0-405">In VB.NET or in a strongly-typed hub, the caller state object can't be accessed through `Clients.Caller`; instead, use `Clients.CallerState` (introduced in SignalR 2.1):</span></span>

<span data-ttu-id="29cc0-406">**Uso de CallerState en C#**</span><span class="sxs-lookup"><span data-stu-id="29cc0-406">**Using CallerState in C#**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

<span data-ttu-id="29cc0-407">**Uso de CallerState en Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="29cc0-407">**Using CallerState in Visual Basic**</span></span>

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="29cc0-408">Cómo controlar los errores en la clase Hub</span><span class="sxs-lookup"><span data-stu-id="29cc0-408">How to handle errors in the Hub class</span></span>

<span data-ttu-id="29cc0-409">Para controlar los errores que se producen en los métodos de clase de concentrador, asegúrese en primer lugar "observar" las excepciones de operaciones asincrónicas (como la invocación de métodos de cliente) mediante el uso de `await`.</span><span class="sxs-lookup"><span data-stu-id="29cc0-409">To handle errors that occur in your Hub class methods, first ensure you "observe" any exceptions from async operations (such as invoking client methods) by using `await`.</span></span> <span data-ttu-id="29cc0-410">A continuación, use uno o varios de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="29cc0-410">Then use one or more of the following methods:</span></span>

- <span data-ttu-id="29cc0-411">Ajustar el código del método en bloques try-catch y registrar el objeto de excepción.</span><span class="sxs-lookup"><span data-stu-id="29cc0-411">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="29cc0-412">Con fines de depuración puede enviar la excepción al cliente, pero para la seguridad no se recomienda motivos enviar información detallada a los clientes en producción.</span><span class="sxs-lookup"><span data-stu-id="29cc0-412">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="29cc0-413">Crear un módulo de canalización de concentradores controla la [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) método.</span><span class="sxs-lookup"><span data-stu-id="29cc0-413">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="29cc0-414">El ejemplo siguiente muestra un módulo de la canalización que registra los errores, seguidos por el código en Startup.cs que inserta el módulo en la canalización de concentradores.</span><span class="sxs-lookup"><span data-stu-id="29cc0-414">The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- <span data-ttu-id="29cc0-415">Use la `HubException` clase (introducida en SignalR 2).</span><span class="sxs-lookup"><span data-stu-id="29cc0-415">Use the `HubException` class (introduced in SignalR 2).</span></span> <span data-ttu-id="29cc0-416">Este error se puede iniciar desde cualquier invocación de concentrador.</span><span class="sxs-lookup"><span data-stu-id="29cc0-416">This error can be thrown from any hub invocation.</span></span> <span data-ttu-id="29cc0-417">El `HubError` constructor toma un mensaje de cadena y un objeto para almacenar datos de error adicionales.</span><span class="sxs-lookup"><span data-stu-id="29cc0-417">The `HubError` constructor takes a string message, and an object for storing extra error data.</span></span> <span data-ttu-id="29cc0-418">SignalR se auto-serializar la excepción y enviarlo al cliente, donde se usará para rechazar o se producirá un error en la invocación del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="29cc0-418">SignalR will auto-serialize the exception and send it to the client, where it will be used to reject or fail the hub method invocation.</span></span>

    <span data-ttu-id="29cc0-419">Los ejemplos de código siguientes muestran cómo iniciar un `HubException` durante una invocación de concentrador y cómo controlar la excepción en los clientes de JavaScript y. NET.</span><span class="sxs-lookup"><span data-stu-id="29cc0-419">The following code samples demonstrate how to throw a `HubException` during a Hub invocation, and how to handle the exception on JavaScript and .NET clients.</span></span>

    <span data-ttu-id="29cc0-420">**Código de servidor que muestra la clase HubException**</span><span class="sxs-lookup"><span data-stu-id="29cc0-420">**Server code demonstrating the HubException class**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    <span data-ttu-id="29cc0-421">**Código de cliente de JavaScript que muestra la respuesta para producir una HubException en un concentrador**</span><span class="sxs-lookup"><span data-stu-id="29cc0-421">**JavaScript client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    <span data-ttu-id="29cc0-422">**Código de cliente de .NET que muestra la respuesta para producir una HubException en un concentrador**</span><span class="sxs-lookup"><span data-stu-id="29cc0-422">**.NET client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

<span data-ttu-id="29cc0-423">Para obtener más información acerca de los módulos de la canalización de concentrador, consulte [cómo personalizar la canalización de concentradores](#hubpipeline) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="29cc0-423">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="29cc0-424">Cómo habilitar el seguimiento</span><span class="sxs-lookup"><span data-stu-id="29cc0-424">How to enable tracing</span></span>

<span data-ttu-id="29cc0-425">Para habilitar el seguimiento de servidor, agregue un elemento de system.diagnostics al archivo Web.config, tal como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="29cc0-425">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

<span data-ttu-id="29cc0-426">Al ejecutar la aplicación en Visual Studio, puede ver los registros en el **salida** ventana.</span><span class="sxs-lookup"><span data-stu-id="29cc0-426">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="29cc0-427">Cómo llamar a los métodos de cliente y administrar grupos de fuera de la clase Hub</span><span class="sxs-lookup"><span data-stu-id="29cc0-427">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="29cc0-428">Para llamar al cliente métodos de una clase diferente que la clase de Hub, obtener una referencia al objeto de contexto de SignalR para el concentrador y usarlo para llamar a métodos en el cliente o administración de grupos de.</span><span class="sxs-lookup"><span data-stu-id="29cc0-428">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="29cc0-429">El ejemplo siguiente `StockTicker` clase obtiene el objeto de contexto, lo almacena en una instancia de la clase, almacena la instancia de clase en una propiedad estática y usa el contexto de la instancia de la clase singleton para llamar a la `updateStockPrice` método en los clientes que están conectado a un concentrador denominado `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="29cc0-429">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

<span data-ttu-id="29cc0-430">Si tiene que usar el contexto varias veces en un objeto de larga duración, obtener la referencia una vez y guardar, en lugar de obtenerlo nuevo cada vez.</span><span class="sxs-lookup"><span data-stu-id="29cc0-430">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="29cc0-431">Obtener el contexto de una vez garantiza que SignalR envía mensajes a los clientes en la misma secuencia en la que los métodos de concentrador realizan a cliente las invocaciones de método.</span><span class="sxs-lookup"><span data-stu-id="29cc0-431">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="29cc0-432">Para ver un tutorial que muestra cómo usar el contexto de SignalR para un concentrador, consulte [difusión de servidores con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="29cc0-432">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="29cc0-433">Llamar a métodos de cliente</span><span class="sxs-lookup"><span data-stu-id="29cc0-433">Calling client methods</span></span>

<span data-ttu-id="29cc0-434">Puede especificar que los clientes recibirán la RPC, pero tiene menos opciones que cuando se llama a partir de una clase de concentrador.</span><span class="sxs-lookup"><span data-stu-id="29cc0-434">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="29cc0-435">La razón de esto es que el contexto no está asociado con una determinada llamada desde un cliente, por lo que todos los métodos que requieren conocer el identificador de conexión actual, como `Clients.Others`, o `Clients.Caller`, o `Clients.OthersInGroup`, no están disponibles.</span><span class="sxs-lookup"><span data-stu-id="29cc0-435">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="29cc0-436">Están disponibles las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="29cc0-436">The following options are available:</span></span>

- <span data-ttu-id="29cc0-437">Todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="29cc0-437">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- <span data-ttu-id="29cc0-438">Un cliente específico identificado por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="29cc0-438">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- <span data-ttu-id="29cc0-439">Todos los clientes conectados, excepto los clientes especificados, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="29cc0-439">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- <span data-ttu-id="29cc0-440">Todos los clientes en un grupo especificado conectados.</span><span class="sxs-lookup"><span data-stu-id="29cc0-440">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- <span data-ttu-id="29cc0-441">Clientes conectados todo en un grupo especificado, excepto los clientes especificados, identificado por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="29cc0-441">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

<span data-ttu-id="29cc0-442">Si se llama en la clase que no sea centro de métodos en la clase de Hub, puede pasar el identificador de conexión actual y úsela con `Clients.Client`, `Clients.AllExcept`, o `Clients.Group` para simular `Clients.Caller`, `Clients.Others`, o `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="29cc0-442">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="29cc0-443">En el ejemplo siguiente, la `MoveShapeHub` clase pasa el identificador de conexión para el `Broadcaster` clase para que la `Broadcaster` puede simular la clase `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="29cc0-443">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="29cc0-444">Administrar la pertenencia a grupos</span><span class="sxs-lookup"><span data-stu-id="29cc0-444">Managing group membership</span></span>

<span data-ttu-id="29cc0-445">Para administrar grupos tienen las mismas opciones como se hace en una clase de Hub.</span><span class="sxs-lookup"><span data-stu-id="29cc0-445">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="29cc0-446">Agregar a un cliente a un grupo</span><span class="sxs-lookup"><span data-stu-id="29cc0-446">Add a client to a group</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- <span data-ttu-id="29cc0-447">Quitar a un cliente de un grupo</span><span class="sxs-lookup"><span data-stu-id="29cc0-447">Remove a client from a group</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="29cc0-448">Cómo personalizar la canalización de concentradores</span><span class="sxs-lookup"><span data-stu-id="29cc0-448">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="29cc0-449">SignalR le permite insertar su propio código en la canalización de concentrador.</span><span class="sxs-lookup"><span data-stu-id="29cc0-449">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="29cc0-450">El ejemplo siguiente muestra un módulo personalizado de canalización de concentrador que registra cada llamada de método entrante recibido desde el cliente y la llamada al método salientes que se invoca en el cliente:</span><span class="sxs-lookup"><span data-stu-id="29cc0-450">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

<span data-ttu-id="29cc0-451">El siguiente código en el *Startup.cs* archivo registra el módulo se ejecuten en la canalización de concentrador:</span><span class="sxs-lookup"><span data-stu-id="29cc0-451">The following code in the *Startup.cs* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

<span data-ttu-id="29cc0-452">Hay muchos métodos diferentes que puede invalidar.</span><span class="sxs-lookup"><span data-stu-id="29cc0-452">There are many different methods that you can override.</span></span> <span data-ttu-id="29cc0-453">Para obtener una lista completa, consulte [HubPipelineModule métodos](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="29cc0-453">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
