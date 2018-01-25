---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: "Guía de API de bases de datos centrales de ASP.NET SignalR - cliente de JavaScript | Documentos de Microsoft"
author: pfletcher
description: "Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 2 en los clientes de JavaScript, como exploradores y usos prácticos de la tienda de Windows (WinJS)..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/28/2015
ms.topic: article
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 794ab576d3c6600911f331bab7c335476e45a0c9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="98e66-103">Guía de API de bases de datos centrales de ASP.NET SignalR - cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="98e66-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="98e66-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="98e66-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="98e66-105">Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 2 en los clientes de JavaScript, como exploradores y aplicaciones de la tienda de Windows (WinJS).</span><span class="sxs-lookup"><span data-stu-id="98e66-105">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="98e66-106">La API de concentradores de SignalR permite realizar llamadas a procedimiento remoto (RPC) desde un servidor a los clientes conectados y desde clientes en el servidor.</span><span class="sxs-lookup"><span data-stu-id="98e66-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="98e66-107">En el código de servidor, definir métodos que puedan llamar los clientes y llamar a métodos que se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="98e66-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="98e66-108">En el código de cliente, definir métodos que se pueda llamar desde el servidor y llamar a métodos que se ejecutan en el servidor.</span><span class="sxs-lookup"><span data-stu-id="98e66-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="98e66-109">SignalR se encarga de todas la mecánica de cliente a servidor para usted.</span><span class="sxs-lookup"><span data-stu-id="98e66-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="98e66-110">SignalR también ofrece una API de nivel inferior denominada conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="98e66-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="98e66-111">Para obtener una introducción a SignalR, concentradores y conexiones persistentes, consulte [Introducción a SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="98e66-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="98e66-112">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="98e66-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="98e66-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="98e66-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="98e66-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="98e66-114">.NET 4.5</span></span>
> - <span data-ttu-id="98e66-115">SignalR versión 2</span><span class="sxs-lookup"><span data-stu-id="98e66-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="98e66-116">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="98e66-116">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="98e66-117">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="98e66-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="98e66-118">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="98e66-118">Questions and comments</span></span>
> 
> <span data-ttu-id="98e66-119">Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="98e66-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="98e66-120">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="98e66-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="98e66-121">Información general</span><span class="sxs-lookup"><span data-stu-id="98e66-121">Overview</span></span>

<span data-ttu-id="98e66-122">Este documento contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="98e66-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="98e66-123">El proxy generado y lo que hace automáticamente</span><span class="sxs-lookup"><span data-stu-id="98e66-123">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="98e66-124">Cuándo se debe utilizar al proxy generado</span><span class="sxs-lookup"><span data-stu-id="98e66-124">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="98e66-125">Programa de instalación de cliente</span><span class="sxs-lookup"><span data-stu-id="98e66-125">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="98e66-126">Cómo hacen referencia al proxy generado dinámicamente</span><span class="sxs-lookup"><span data-stu-id="98e66-126">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="98e66-127">Cómo crear un archivo físico para SignalR genera proxy</span><span class="sxs-lookup"><span data-stu-id="98e66-127">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="98e66-128">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="98e66-128">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="98e66-129">$. connection.hub es el mismo objeto crea ese $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="98e66-129">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="98e66-130">Ejecución asincrónica del método de inicio</span><span class="sxs-lookup"><span data-stu-id="98e66-130">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="98e66-131">Cómo establecer una conexión entre dominios</span><span class="sxs-lookup"><span data-stu-id="98e66-131">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="98e66-132">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="98e66-132">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="98e66-133">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="98e66-133">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="98e66-134">Cómo especificar el método de transporte</span><span class="sxs-lookup"><span data-stu-id="98e66-134">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="98e66-135">Cómo obtener a un proxy para una clase de base de datos central</span><span class="sxs-lookup"><span data-stu-id="98e66-135">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="98e66-136">Cómo definir métodos en el cliente que el servidor puede llamar a</span><span class="sxs-lookup"><span data-stu-id="98e66-136">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="98e66-137">Cómo llamar a métodos de servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="98e66-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="98e66-138">Cómo controlar eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="98e66-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="98e66-139">Cómo controlar errores</span><span class="sxs-lookup"><span data-stu-id="98e66-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="98e66-140">Cómo habilitar el registro de cliente</span><span class="sxs-lookup"><span data-stu-id="98e66-140">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="98e66-141">Para obtener documentación sobre cómo programar el servidor o los clientes de. NET, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="98e66-141">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="98e66-142">Guía de API de concentradores de SignalR - servidor</span><span class="sxs-lookup"><span data-stu-id="98e66-142">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="98e66-143">Guía de API de concentradores de SignalR - cliente .NET</span><span class="sxs-lookup"><span data-stu-id="98e66-143">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="98e66-144">El componente de servidor de SignalR 2 solo está disponible en .NET 4.5 (aunque no hay un cliente de .NET para SignalR 2 en .NET 4.0).</span><span class="sxs-lookup"><span data-stu-id="98e66-144">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="98e66-145">El proxy generado y lo que hace automáticamente</span><span class="sxs-lookup"><span data-stu-id="98e66-145">The generated proxy and what it does for you</span></span>

<span data-ttu-id="98e66-146">Puede programar un cliente de JavaScript para comunicarse con un servicio de SignalR con o sin un proxy que genera SignalR para usted.</span><span class="sxs-lookup"><span data-stu-id="98e66-146">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="98e66-147">Lo que hace el proxy automáticamente es simplificar la sintaxis del código que utiliza para conectar, métodos de escritura que el servidor llama, y llamar a métodos en el servidor.</span><span class="sxs-lookup"><span data-stu-id="98e66-147">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="98e66-148">Cuando se escribe código para llamar a métodos de servidor, el proxy generado le permite utilizar la sintaxis que parece ser que se estaban ejecutando una función local: puede escribir `serverMethod(arg1, arg2)` en lugar de `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="98e66-148">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="98e66-149">La sintaxis de proxy generado también permite un error de lado cliente inmediato e inteligible si se escribe incorrectamente el nombre de un método de servidor.</span><span class="sxs-lookup"><span data-stu-id="98e66-149">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="98e66-150">Y si crea manualmente el archivo que define a los servidores proxy, también puede obtener compatibilidad con IntelliSense para escribir código que llama a métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="98e66-150">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="98e66-151">Por ejemplo, imagine que tiene la siguiente clase de base de datos central en el servidor:</span><span class="sxs-lookup"><span data-stu-id="98e66-151">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="98e66-152">Los siguientes ejemplos de código muestran lo que parece de código de JavaScript que para invocar la `NewContosoChatMessage` método en el servidor y recibir las invocaciones de la `addContosoChatMessageToPage` método desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="98e66-152">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="98e66-153">**Con el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="98e66-153">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="98e66-154">**Sin el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="98e66-154">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="98e66-155">Cuándo se debe utilizar al proxy generado</span><span class="sxs-lookup"><span data-stu-id="98e66-155">When to use the generated proxy</span></span>

<span data-ttu-id="98e66-156">Si desea registrar varios controladores de eventos para un método de cliente que llama el servidor, no puede usar al proxy generado.</span><span class="sxs-lookup"><span data-stu-id="98e66-156">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="98e66-157">En caso contrario, puede optar por usar al proxy generado o no en función de su preferencia de codificación.</span><span class="sxs-lookup"><span data-stu-id="98e66-157">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="98e66-158">Si decide no usarlo, no tienes que hacer referencia a la dirección URL de "/ concentradores de signalr" en un `script` elemento en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="98e66-158">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="98e66-159">Programa de instalación de cliente</span><span class="sxs-lookup"><span data-stu-id="98e66-159">Client setup</span></span>

<span data-ttu-id="98e66-160">Un cliente de JavaScript requiere referencias a jQuery y el archivo de JavaScript de SignalR core.</span><span class="sxs-lookup"><span data-stu-id="98e66-160">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="98e66-161">Debe ser la versión de jQuery 1.6.4 o versiones posteriores importantes, como 1.7.2, 1.8.2 o 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="98e66-161">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="98e66-162">Si decide usar al proxy generado, también se necesita una referencia al proxy de SignalR genera archivos de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="98e66-162">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="98e66-163">En el ejemplo siguiente se muestra el aspecto que podrían las referencias en una página HTML que utiliza al proxy generado.</span><span class="sxs-lookup"><span data-stu-id="98e66-163">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="98e66-164">Estas referencias deben incluirse en este orden: jQuery en primer lugar, la última SignalR core después de eso y los servidores proxy de SignalR.</span><span class="sxs-lookup"><span data-stu-id="98e66-164">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="98e66-165">Cómo hacen referencia al proxy generado dinámicamente</span><span class="sxs-lookup"><span data-stu-id="98e66-165">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="98e66-166">En el ejemplo anterior, la referencia al proxy SignalR generado es código JavaScript generado dinámicamente, no a un archivo físico.</span><span class="sxs-lookup"><span data-stu-id="98e66-166">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="98e66-167">SignalR crea el código de JavaScript para el proxy sobre la marcha y sirve para el cliente en respuesta a la dirección URL "/ signalr/concentradores".</span><span class="sxs-lookup"><span data-stu-id="98e66-167">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="98e66-168">Si especifica una dirección URL base diferente para las conexiones de SignalR en el servidor en su `MapSignalR` método, la dirección URL para el archivo de proxy generados de forma dinámica es la dirección URL personalizada con "/ concentradores" anexada al mismo.</span><span class="sxs-lookup"><span data-stu-id="98e66-168">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="98e66-169">Para los clientes de JavaScript de Windows 8 (tienda Windows), use el archivo de proxy físico en lugar del generado dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="98e66-169">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="98e66-170">Para obtener más información, consulte [cómo crear un archivo físico para SignalR genera proxy](#manualproxy) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="98e66-170">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="98e66-171">En un programa de ASP.NET MVC 4 o 5 Razor, use la tilde para hacer referencia a la raíz de la aplicación en la referencia del archivo de proxy:</span><span class="sxs-lookup"><span data-stu-id="98e66-171">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="98e66-172">Para obtener más información sobre el uso de SignalR en MVC 5, consulte [Getting Started with SignalR y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="98e66-172">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="98e66-173">En una vista de ASP.NET MVC 3 Razor, use `Url.Content` para la referencia del archivo de proxy:</span><span class="sxs-lookup"><span data-stu-id="98e66-173">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="98e66-174">En una aplicación de formularios Web Forms de ASP.NET, utilice `ResolveClientUrl` para el proxy de la referencia de archivo o registrar mediante el objeto ScriptManager mediante una ruta relativa de raíz de aplicación (a partir de una tilde):</span><span class="sxs-lookup"><span data-stu-id="98e66-174">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="98e66-175">Como norma general, utilice el mismo método para especificar la dirección URL "/ signalr/concentradores" que usa para archivos CSS o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="98e66-175">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="98e66-176">Si especifica una dirección URL sin utilizar una tilde, en algunos escenarios de la aplicación funcionará correctamente al probar en Visual Studio mediante IIS Express, pero se producirá un error con un error 404 cuando se implementa en la versión completa de IIS.</span><span class="sxs-lookup"><span data-stu-id="98e66-176">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="98e66-177">Para obtener más información, consulte **resolver referencias a recursos de nivel de raíz** en [servidores Web en Visual Studio para proyectos Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) en el sitio MSDN.</span><span class="sxs-lookup"><span data-stu-id="98e66-177">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="98e66-178">Cuando se ejecuta un proyecto web en Visual Studio 2013 en modo de depuración y, si utiliza Internet Explorer como explorador, puede ver el archivo de proxy en **el Explorador de soluciones** en **documentos de Script**, tal y como se muestra en el ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="98e66-178">When you run a web project in Visual Studio 2013 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Archivo de proxy generado de JavaScript en el Explorador de soluciones](hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="98e66-180">Para ver el contenido del archivo, haga doble clic en **concentradores**.</span><span class="sxs-lookup"><span data-stu-id="98e66-180">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="98e66-181">Si no utiliza Internet Explorer y Visual Studio 2012 o 2013, o si no está en modo de depuración, también puede obtener el contenido del archivo a través de la dirección URL "/ signalR/concentradores".</span><span class="sxs-lookup"><span data-stu-id="98e66-181">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="98e66-182">Por ejemplo, si el sitio se ejecuta en `http://localhost:56699`, vaya a `http://localhost:56699/SignalR/hubs` en el explorador.</span><span class="sxs-lookup"><span data-stu-id="98e66-182">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="98e66-183">Cómo crear un archivo físico para SignalR genera proxy</span><span class="sxs-lookup"><span data-stu-id="98e66-183">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="98e66-184">Como alternativa al proxy generado dinámicamente, puede crear un archivo físico que tenga el código de proxy y hacer referencia a ese archivo.</span><span class="sxs-lookup"><span data-stu-id="98e66-184">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="98e66-185">Puede hacerlo para un control sobre el almacenamiento en caché o el comportamiento de agrupación, u obtener IntelliSense cuando escribe código llamadas a métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="98e66-185">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="98e66-186">Para crear un archivo de proxy, siga los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="98e66-186">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="98e66-187">Instalar el [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="98e66-187">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="98e66-188">Abra un símbolo del sistema y vaya a la *herramientas* carpeta que contiene el archivo SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="98e66-188">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="98e66-189">La carpeta de herramientas está en la siguiente ubicación:</span><span class="sxs-lookup"><span data-stu-id="98e66-189">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="98e66-190">Escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="98e66-190">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="98e66-191">La ruta de acceso a la *.dll* suele ser el *bin* carpeta en la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="98e66-191">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="98e66-192">Este comando crea un archivo denominado *server.js* en la misma carpeta que *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="98e66-192">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="98e66-193">Coloque el *server.js* un archivo en una carpeta correspondiente en el proyecto, cámbiele el nombre según corresponda para su aplicación y agregue una referencia a ella en lugar de la referencia de "/ concentradores de signalr".</span><span class="sxs-lookup"><span data-stu-id="98e66-193">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="98e66-194">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="98e66-194">How to establish a connection</span></span>

<span data-ttu-id="98e66-195">Antes de que puede establecer una conexión, tendrá que crear un objeto de conexión, crear a un proxy y registran controladores de eventos para los métodos que pueden llamarse desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="98e66-195">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="98e66-196">Cuando se configuran el proxy y controladores de eventos, establecer la conexión mediante una llamada a la `start` método.</span><span class="sxs-lookup"><span data-stu-id="98e66-196">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="98e66-197">Si está utilizando al proxy generado, no tienes que crear el objeto de conexión en su propio código porque hace en el código de proxy generado automáticamente.</span><span class="sxs-lookup"><span data-stu-id="98e66-197">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="98e66-198">**Establecer una conexión (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-198">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="98e66-199">**Establecer una conexión (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-199">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="98e66-200">El código de ejemplo usa el valor predeterminado "/ signalr" dirección URL para conectarse a su servicio de SignalR.</span><span class="sxs-lookup"><span data-stu-id="98e66-200">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="98e66-201">Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [Guía de API de concentradores de ASP.NET SignalR - Server - la dirección URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="98e66-201">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="98e66-202">De forma predeterminada, la ubicación de la base de datos central es el servidor actual; Si se conecta a un servidor diferente, especifique la dirección URL antes de llamar a la `start` método, tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="98e66-202">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="98e66-203">Normalmente se registran controladores de eventos antes de llamar a la `start` método para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="98e66-203">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="98e66-204">Si desea registrar algunos controladores de eventos después de establecer la conexión, puede hacerlo, pero debe registrar al menos uno de los handler(s) de evento antes de llamar a la `start` método.</span><span class="sxs-lookup"><span data-stu-id="98e66-204">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="98e66-205">Una razón para esto es que puede haber muchos concentradores en una aplicación, pero que no desea que activen la `OnConnected` eventos en cada concentrador si solo va a usar para uno de ellos.</span><span class="sxs-lookup"><span data-stu-id="98e66-205">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="98e66-206">Cuando se establece la conexión, la presencia de un método de cliente en proxy de una base de datos central es lo que indica SignalR para desencadenar la `OnConnected` eventos.</span><span class="sxs-lookup"><span data-stu-id="98e66-206">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="98e66-207">Si no registra los controladores de eventos antes de llamar a la `start` método, podrá invocar métodos en el concentrador, pero el concentrador `OnConnected` no se llamará al método y no se invocará ningún método de cliente desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="98e66-207">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="98e66-208">$. connection.hub es el mismo objeto crea ese $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="98e66-208">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="98e66-209">Como puede ver en los ejemplos, cuando se usa el proxy generado, `$.connection.hub` hace referencia al objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="98e66-209">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="98e66-210">Este es el mismo objeto que obtiene mediante una llamada a `$.hubConnection()` cuando no está usando el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="98e66-210">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="98e66-211">El código de proxy generado crea la conexión automáticamente mediante la ejecución de la siguiente instrucción:</span><span class="sxs-lookup"><span data-stu-id="98e66-211">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Crear una conexión en el archivo de proxy generado](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="98e66-213">Cuando se usa el proxy generado, puede hacer cualquier cosa con `$.connection.hub` que puede hacer con un objeto de conexión cuando no lo esté usando el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="98e66-213">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="98e66-214">Ejecución asincrónica del método de inicio</span><span class="sxs-lookup"><span data-stu-id="98e66-214">Asynchronous execution of the start method</span></span>

<span data-ttu-id="98e66-215">El `start` método se ejecuta de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="98e66-215">The `start` method executes asynchronously.</span></span> <span data-ttu-id="98e66-216">Devuelve un [jQuery diferida objeto](http://api.jquery.com/category/deferred-object/), lo que significa que puede agregar funciones de devolución de llamada mediante una llamada a métodos como `pipe`, `done`, y `fail`.</span><span class="sxs-lookup"><span data-stu-id="98e66-216">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="98e66-217">Si tiene código que desea ejecutar una vez establecida la conexión, como una llamada a un método de servidor, agregue el código en una función de devolución de llamada o llamarlo desde una función de devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="98e66-217">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="98e66-218">El `.done` método de devolución de llamada se ejecuta después de que se ha establecido la conexión y, después de cualquier código que tiene en su `OnConnected` método de controlador de eventos en el servidor termina de ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="98e66-218">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="98e66-219">Si coloca la instrucción "Ahora conectados" del ejemplo anterior, como la siguiente línea de código después de la `start` llamada al método (no en un `.done` devolución de llamada), la `console.log` línea se ejecutará antes de establece la conexión, tal y como se muestra en el siguiente ejemplo:</span><span class="sxs-lookup"><span data-stu-id="98e66-219">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Medio incorrecto para escribir código que se ejecuta después de establecer conexión](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="98e66-221">Cómo establecer una conexión entre dominios</span><span class="sxs-lookup"><span data-stu-id="98e66-221">How to establish a cross-domain connection</span></span>

<span data-ttu-id="98e66-222">Por lo general si el explorador carga una página de `http://contoso.com`, la conexión de SignalR está en el mismo dominio, en `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="98e66-222">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="98e66-223">Si la página de `http://contoso.com` realiza una conexión a `http://fabrikam.com/signalr`, que es una conexión entre dominios.</span><span class="sxs-lookup"><span data-stu-id="98e66-223">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="98e66-224">Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="98e66-224">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="98e66-225">En SignalR 1.x solicitudes entre dominios se controla mediante una sola marca de EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="98e66-225">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="98e66-226">Este indicador controla las solicitudes de JSONP y CORS.</span><span class="sxs-lookup"><span data-stu-id="98e66-226">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="98e66-227">Para una mayor flexibilidad, compatibilidad con todos los CORS se ha quitado del componente de servidor de SignalR (clientes de JavaScript seguir usan CORS normalmente si se detecta que el explorador admite), y nueva middleware OWIN pone a su disposición admitir estos escenarios.</span><span class="sxs-lookup"><span data-stu-id="98e66-227">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="98e66-228">Si JSONP es obligatorio en el cliente (para admitir las solicitudes entre dominios en exploradores de versiones anteriores), será necesario habilitar de forma explícita mediante el establecimiento `EnableJSONP` en el `HubConfiguration` el objeto a `true`, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="98e66-228">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="98e66-229">JSONP está deshabilitado de forma predeterminada, ya que es menos segura que CORS.</span><span class="sxs-lookup"><span data-stu-id="98e66-229">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="98e66-230">**Agregar Microsoft.Owin.Cors a un proyecto:** para instalar esta biblioteca, ejecute el siguiente comando en la consola de administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="98e66-230">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="98e66-231">Este comando agregará el 2.1.0 versión del paquete a su proyecto.</span><span class="sxs-lookup"><span data-stu-id="98e66-231">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="98e66-232">Al llamar a UseCors</span><span class="sxs-lookup"><span data-stu-id="98e66-232">Calling UseCors</span></span>

 <span data-ttu-id="98e66-233">El siguiente fragmento de código muestra cómo implementar las conexiones entre dominios en el 2 de SignalR.</span><span class="sxs-lookup"><span data-stu-id="98e66-233">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span> 

<span data-ttu-id="98e66-234">**Implementación de las solicitudes entre dominios en SignalR 2**</span><span class="sxs-lookup"><span data-stu-id="98e66-234">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="98e66-235">El código siguiente muestra cómo habilitar CORS o JSONP en un proyecto de SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="98e66-235">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="98e66-236">Este ejemplo de código usa `Map` y `RunSignalR` en lugar de `MapSignalR`, de modo que se ejecuta el middleware CORS solo para las solicitudes de SignalR que requieren compatibilidad con CORS (en lugar de para todo el tráfico en la ruta de acceso especificada en `MapSignalR`.) Asignación también se puede usar para cualquier otro middleware que necesite ejecutar para un prefijo de dirección URL concreto, en lugar de en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="98e66-236">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - <span data-ttu-id="98e66-237">No establece `jQuery.support.cors` en true en el código.</span><span class="sxs-lookup"><span data-stu-id="98e66-237">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![No establece jQuery.support.cors en true](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="98e66-239">SignalR controla el uso de CORS.</span><span class="sxs-lookup"><span data-stu-id="98e66-239">SignalR handles the use of CORS.</span></span> <span data-ttu-id="98e66-240">Establecer `jQuery.support.cors` a true deshabilita JSONP porque hace que SignalR asuma el explorador es compatible con CORS.</span><span class="sxs-lookup"><span data-stu-id="98e66-240">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="98e66-241">Cuando se está conectando a una dirección URL de localhost, Internet Explorer 10 no Considérelo una conexión entre dominios, por lo que la aplicación funcionará localmente con Internet Explorer 10 incluso si no ha habilitado las conexiones entre dominios en el servidor.</span><span class="sxs-lookup"><span data-stu-id="98e66-241">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="98e66-242">Para obtener información sobre el uso de las conexiones entre dominios con Internet Explorer 9, consulte [este subproceso StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="98e66-242">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="98e66-243">Para obtener información sobre el uso de las conexiones entre dominios con Chrome, consulte [este subproceso StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="98e66-243">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="98e66-244">El código de ejemplo usa el valor predeterminado "/ signalr" dirección URL para conectarse a su servicio de SignalR.</span><span class="sxs-lookup"><span data-stu-id="98e66-244">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="98e66-245">Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [Guía de API de concentradores de ASP.NET SignalR - Server - la dirección URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="98e66-245">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="98e66-246">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="98e66-246">How to configure the connection</span></span>

<span data-ttu-id="98e66-247">Antes de establecer una conexión, puede especificar parámetros de cadena de consulta o especifique el método de transporte.</span><span class="sxs-lookup"><span data-stu-id="98e66-247">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="98e66-248">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="98e66-248">How to specify query string parameters</span></span>

<span data-ttu-id="98e66-249">Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta para el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="98e66-249">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="98e66-250">Los ejemplos siguientes muestran cómo establecer un parámetro de cadena de consulta en código de cliente.</span><span class="sxs-lookup"><span data-stu-id="98e66-250">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="98e66-251">**Establecer un valor de cadena de consulta antes de llamar al método start (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-251">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="98e66-252">**Establecer un valor de cadena de consulta antes de llamar al método start (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-252">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="98e66-253">En el ejemplo siguiente se muestra cómo leer un parámetro de cadena de consulta en código del servidor.</span><span class="sxs-lookup"><span data-stu-id="98e66-253">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="98e66-254">Cómo especificar el método de transporte</span><span class="sxs-lookup"><span data-stu-id="98e66-254">How to specify the transport method</span></span>

<span data-ttu-id="98e66-255">Como parte del proceso de conexión, un cliente de SignalR normalmente se negocia con el servidor para determinar el mejor transporte que se admite por servidor y cliente.</span><span class="sxs-lookup"><span data-stu-id="98e66-255">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="98e66-256">Si ya sabe qué transporte que se va a utilizar, puede omitir este proceso de negociación especificando el modo de transporte cuando se llama a la `start` método.</span><span class="sxs-lookup"><span data-stu-id="98e66-256">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="98e66-257">**Código de cliente que especifica el método de transporte (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-257">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="98e66-258">**Código de cliente que especifica el método de transporte (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-258">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="98e66-259">Como alternativa, puede especificar varios métodos de transporte en el orden en el que desea SignalR para intentarlo de ellos:</span><span class="sxs-lookup"><span data-stu-id="98e66-259">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="98e66-260">**Código de cliente que especifica un esquema de reserva de transporte personalizado (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-260">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="98e66-261">**Código de cliente que especifica un esquema de reserva de transporte personalizado (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-261">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="98e66-262">Puede usar los siguientes valores para especificar el método de transporte:</span><span class="sxs-lookup"><span data-stu-id="98e66-262">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="98e66-263">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="98e66-263">"webSockets"</span></span>
- <span data-ttu-id="98e66-264">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="98e66-264">"foreverFrame"</span></span>
- <span data-ttu-id="98e66-265">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="98e66-265">"serverSentEvents"</span></span>
- <span data-ttu-id="98e66-266">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="98e66-266">"longPolling"</span></span>

<span data-ttu-id="98e66-267">Los ejemplos siguientes muestran cómo averiguar qué método de transporte está usándola una conexión.</span><span class="sxs-lookup"><span data-stu-id="98e66-267">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="98e66-268">**Código de cliente que se muestra el método de transporte utilizado por una conexión (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-268">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="98e66-269">**Código de cliente que se muestra el método de transporte utilizado por una conexión (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-269">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="98e66-270">Para obtener información sobre cómo comprobar el método de transporte en el código de servidor, consulte [Guía de API de concentradores de ASP.NET SignalR - Server - cómo obtener información sobre el cliente de la propiedad de contexto](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="98e66-270">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="98e66-271">Para obtener más información acerca de los transportes y reservas, consulte [Introducción a SignalR - transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="98e66-271">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="98e66-272">Cómo obtener a un proxy para una clase de base de datos central</span><span class="sxs-lookup"><span data-stu-id="98e66-272">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="98e66-273">Cada objeto de conexión que cree encapsula información sobre una conexión a un servicio de SignalR que contiene una o más clases de concentrador.</span><span class="sxs-lookup"><span data-stu-id="98e66-273">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="98e66-274">Para comunicarse con una clase de base de datos central, utilice un objeto de proxy que crea usted mismo (si no está utilizando al proxy generado) o que se genera automáticamente.</span><span class="sxs-lookup"><span data-stu-id="98e66-274">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="98e66-275">En el cliente el nombre del proxy es una versión de grafía de camel del nombre de clase del concentrador.</span><span class="sxs-lookup"><span data-stu-id="98e66-275">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="98e66-276">SignalR automáticamente realiza este cambio para que el código de JavaScript puede ajustarse a las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="98e66-276">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="98e66-277">**Clase de base de datos central en servidor**</span><span class="sxs-lookup"><span data-stu-id="98e66-277">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="98e66-278">**Obtener una referencia al proxy de cliente generado para el concentrador**</span><span class="sxs-lookup"><span data-stu-id="98e66-278">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="98e66-279">**Crear el proxy de cliente para la clase de base de datos central (sin proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-279">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="98e66-280">Si decorar la clase de concentrador con un `HubName` de atributo, use el nombre exacto sin cambiar mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="98e66-280">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="98e66-281">**Clase de base de datos central en el servidor con el atributo HubName**</span><span class="sxs-lookup"><span data-stu-id="98e66-281">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="98e66-282">**Obtener una referencia al proxy de cliente generado para el concentrador**</span><span class="sxs-lookup"><span data-stu-id="98e66-282">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="98e66-283">**Crear el proxy de cliente para la clase de base de datos central (sin proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-283">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="98e66-284">Cómo definir métodos en el cliente que el servidor puede llamar a</span><span class="sxs-lookup"><span data-stu-id="98e66-284">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="98e66-285">Para definir un método que el servidor pueda llamar desde un concentrador, agregar un controlador de eventos para el proxy de concentrador mediante el `client` propiedad del proxy generado, o llame a la `on` método si no está usando el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="98e66-285">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="98e66-286">Los parámetros pueden ser objetos complejos.</span><span class="sxs-lookup"><span data-stu-id="98e66-286">The parameters can be complex objects.</span></span>

<span data-ttu-id="98e66-287">Agregue el controlador de eventos antes de llamar a la `start` método para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="98e66-287">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="98e66-288">(Si desea agregar controladores de eventos después de llamar a la `start` método, vea la nota de [cómo establecer una conexión](#establishconnection) anteriormente en este documento y utilizar la sintaxis mostrada para definir un método sin utilizar el proxy generado.)</span><span class="sxs-lookup"><span data-stu-id="98e66-288">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="98e66-289">Coincidencia de nombres de método distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="98e66-289">Method name matching is case-insensitive.</span></span> <span data-ttu-id="98e66-290">Por ejemplo, `Clients.All.addContosoChatMessageToPage` se ejecutará en el servidor `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, o `addcontosochatmessagetopage` en el cliente.</span><span class="sxs-lookup"><span data-stu-id="98e66-290">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="98e66-291">**Definir el método de cliente (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-291">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="98e66-292">**Manera alternativa para definir el método de cliente (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-292">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="98e66-293">**Defina el método de cliente (sin el proxy generado, o cuando se agrega después de llamar al método de inicio)**</span><span class="sxs-lookup"><span data-stu-id="98e66-293">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="98e66-294">**Código de servidor que se llama al método de cliente**</span><span class="sxs-lookup"><span data-stu-id="98e66-294">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="98e66-295">Los ejemplos siguientes incluye un objeto complejo como un parámetro de método.</span><span class="sxs-lookup"><span data-stu-id="98e66-295">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="98e66-296">**Defina el método de cliente que toma un objeto complejo (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-296">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="98e66-297">**Defina el método de cliente que toma un objeto complejo (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-297">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="98e66-298">**Código de servidor que define el objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="98e66-298">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="98e66-299">**Código de servidor que se llama al método de cliente mediante un objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="98e66-299">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="98e66-300">Cómo llamar a métodos de servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="98e66-300">How to call server methods from the client</span></span>

<span data-ttu-id="98e66-301">Para llamar a un método de servidor desde el cliente, use el `server` propiedad del proxy generado o `invoke` método en el proxy de concentrador, si no está usando el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="98e66-301">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="98e66-302">El valor devuelto o los parámetros pueden ser objetos complejos.</span><span class="sxs-lookup"><span data-stu-id="98e66-302">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="98e66-303">Pasar de una versión de mayúscula y minúscula camel del nombre del método en el concentrador.</span><span class="sxs-lookup"><span data-stu-id="98e66-303">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="98e66-304">SignalR automáticamente realiza este cambio para que el código de JavaScript puede ajustarse a las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="98e66-304">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="98e66-305">Los ejemplos siguientes muestran cómo llamar a un método de servidor que no tiene un valor devuelto y cómo llamar a un método de servidor que tiene un valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="98e66-305">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="98e66-306">**Método de servidor con ningún atributo HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="98e66-306">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="98e66-307">**Código de servidor que define el objeto complejo que se pasa en un parámetro**</span><span class="sxs-lookup"><span data-stu-id="98e66-307">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="98e66-308">**Código de cliente que invoca el método de servidor (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-308">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="98e66-309">**Código de cliente que invoca el método de servidor (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-309">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="98e66-310">Si decora el método de concentrador con un `HubMethodName` de atributo, use ese nombre sin cambiar mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="98e66-310">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="98e66-311">**Método de servidor** con un atributo HubMethodName</span><span class="sxs-lookup"><span data-stu-id="98e66-311">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="98e66-312">**Código de cliente que invoca el método de servidor (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-312">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="98e66-313">**Código de cliente que invoca el método de servidor (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-313">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="98e66-314">Los ejemplos anteriores muestran cómo llamar a un método de servidor que no tiene ningún valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="98e66-314">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="98e66-315">Los ejemplos siguientes muestran cómo llamar a un método de servidor que tiene un valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="98e66-315">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="98e66-316">**Código de servidor para un método que tiene un valor devuelto**</span><span class="sxs-lookup"><span data-stu-id="98e66-316">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="98e66-317">**La clase de existencias utilizada para la** valor devuelto</span><span class="sxs-lookup"><span data-stu-id="98e66-317">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="98e66-318">**Código de cliente que invoca el método de servidor (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-318">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="98e66-319">**Código de cliente que invoca el método de servidor (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-319">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="98e66-320">Cómo controlar eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="98e66-320">How to handle connection lifetime events</span></span>

<span data-ttu-id="98e66-321">SignalR proporciona la siguiente conexión de eventos de duración que puede controlar:</span><span class="sxs-lookup"><span data-stu-id="98e66-321">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="98e66-322">`starting`: Genera antes de que se envían los datos a través de la conexión.</span><span class="sxs-lookup"><span data-stu-id="98e66-322">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="98e66-323">`received`: Se genera cuando se reciben los datos en la conexión.</span><span class="sxs-lookup"><span data-stu-id="98e66-323">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="98e66-324">Proporciona los datos recibidos.</span><span class="sxs-lookup"><span data-stu-id="98e66-324">Provides the received data.</span></span>
- <span data-ttu-id="98e66-325">`connectionSlow`: Se genera cuando el cliente detecta una conexión lenta o quitar con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="98e66-325">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="98e66-326">`reconnecting`: Se genera cuando el transporte subyacente comienza conectarse de nuevo.</span><span class="sxs-lookup"><span data-stu-id="98e66-326">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="98e66-327">`reconnected`: Se genera cuando se ha vuelto a conectar el transporte subyacente.</span><span class="sxs-lookup"><span data-stu-id="98e66-327">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="98e66-328">`stateChanged`: Se produce cuando cambia el estado de conexión.</span><span class="sxs-lookup"><span data-stu-id="98e66-328">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="98e66-329">Proporciona el estado anterior y el nuevo estado (conexión, conectado, volviendo a conectarse o Disconnected).</span><span class="sxs-lookup"><span data-stu-id="98e66-329">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="98e66-330">`disconnected`: Se genera cuando se ha desconectado la conexión.</span><span class="sxs-lookup"><span data-stu-id="98e66-330">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="98e66-331">Por ejemplo, si desea mostrar mensajes de advertencia cuando hay problemas de conexión que pueden producir retrasos notables, controlar el `connectionSlow` eventos.</span><span class="sxs-lookup"><span data-stu-id="98e66-331">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="98e66-332">**Controlar el evento connectionSlow (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-332">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="98e66-333">**Controlar el evento connectionSlow (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-333">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="98e66-334">Para obtener más información, consulte [descripción y control de eventos de duración de conexión de SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="98e66-334">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="98e66-335">Cómo controlar errores</span><span class="sxs-lookup"><span data-stu-id="98e66-335">How to handle errors</span></span>

<span data-ttu-id="98e66-336">El cliente de SignalR JavaScript proporciona un `error` eventos que puede agregar un controlador.</span><span class="sxs-lookup"><span data-stu-id="98e66-336">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="98e66-337">También puede usar el método fail para agregar un controlador de errores que son el resultado de una invocación de método de servidor.</span><span class="sxs-lookup"><span data-stu-id="98e66-337">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="98e66-338">Si no habilita explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que SignalR devuelve después de un error contiene información básica sobre el error.</span><span class="sxs-lookup"><span data-stu-id="98e66-338">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="98e66-339">Por ejemplo, si una llamada a `newContosoChatMessage` se produce un error, el mensaje de error en el objeto de error contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" enviar mensajes de error detallados a los clientes de producción no se recomienda por motivos de seguridad, pero si desea habilitar los mensajes de error detallados para solución de problemas, utilice el código siguiente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="98e66-339">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="98e66-340">En el ejemplo siguiente se muestra cómo agregar un controlador para el evento de error.</span><span class="sxs-lookup"><span data-stu-id="98e66-340">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="98e66-341">**Agregar un controlador de errores (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-341">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="98e66-342">**Agregar un controlador de errores (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-342">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="98e66-343">En el ejemplo siguiente se muestra cómo controlar un error de una invocación de método.</span><span class="sxs-lookup"><span data-stu-id="98e66-343">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="98e66-344">**Identificador de error de una invocación de método (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-344">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="98e66-345">**Identificador de error de una invocación de método (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-345">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="98e66-346">Si se produce un error en una invocación de método, el `error` también se genera el evento, por lo que el código en el `error` controlador de método y en el `.fail` ejecutaría la devolución de llamada de método.</span><span class="sxs-lookup"><span data-stu-id="98e66-346">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="98e66-347">Cómo habilitar el registro de cliente</span><span class="sxs-lookup"><span data-stu-id="98e66-347">How to enable client-side logging</span></span>

<span data-ttu-id="98e66-348">Para habilitar el registro de cliente en una conexión, establecer el `logging` propiedad en el objeto de conexión antes de llamar a la `start` método para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="98e66-348">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="98e66-349">**Habilitar el registro (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-349">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="98e66-350">**Habilitar el registro (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="98e66-350">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="98e66-351">Para ver los registros, abrir las herramientas de desarrollador de su explorador y vaya a la pestaña de la consola. Para obtener un tutorial que muestra las instrucciones paso a paso y pantalla capturas que muestran cómo hacerlo, consulte [Server difusión con ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span><span class="sxs-lookup"><span data-stu-id="98e66-351">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span></span>
