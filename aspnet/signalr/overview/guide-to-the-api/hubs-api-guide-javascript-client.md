---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: 'Guía de API de ASP.NET SignalR Hubs: cliente JavaScript | Microsoft Docs'
author: pfletcher
description: Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 2 en los clientes de JavaScript, como los exploradores y usos prácticos de Windows Store (WinJS)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/28/2015
ms.topic: article
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 155ad6599f7f790bf52b0bd59053290f3a662a90
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398224"
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="54acf-103">Guía de API de ASP.NET SignalR Hubs: cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="54acf-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="54acf-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="54acf-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="54acf-105">Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 2 en los clientes de JavaScript, como los exploradores y aplicaciones de Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="54acf-105">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="54acf-106">La API de concentradores de SignalR permite realizar llamadas a procedimiento remoto (RPC) de un servidor a los clientes conectados y de clientes en el servidor.</span><span class="sxs-lookup"><span data-stu-id="54acf-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="54acf-107">En el código de servidor, definir los métodos que se pueden llamar a los clientes y llamar a métodos que se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="54acf-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="54acf-108">En el código de cliente, definir los métodos que pueden llamarse desde el servidor y llamar a métodos que se ejecutan en el servidor.</span><span class="sxs-lookup"><span data-stu-id="54acf-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="54acf-109">SignalR se ocupa de todos los mecanismos de cliente a servidor para usted.</span><span class="sxs-lookup"><span data-stu-id="54acf-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="54acf-110">SignalR también ofrece una API de nivel inferior denominada conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="54acf-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="54acf-111">Para obtener una introducción a SignalR, concentradores y conexiones persistentes, consulte [Introducción a SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="54acf-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="54acf-112">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="54acf-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="54acf-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="54acf-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="54acf-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="54acf-114">.NET 4.5</span></span>
> - <span data-ttu-id="54acf-115">Versión 2 de SignalR</span><span class="sxs-lookup"><span data-stu-id="54acf-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="54acf-116">Versiones anteriores de este tema.</span><span class="sxs-lookup"><span data-stu-id="54acf-116">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="54acf-117">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="54acf-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="54acf-118">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="54acf-118">Questions and comments</span></span>
> 
> <span data-ttu-id="54acf-119">Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="54acf-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="54acf-120">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="54acf-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="54acf-121">Información general</span><span class="sxs-lookup"><span data-stu-id="54acf-121">Overview</span></span>

<span data-ttu-id="54acf-122">Este documento contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="54acf-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="54acf-123">El proxy generado y lo que hace por usted</span><span class="sxs-lookup"><span data-stu-id="54acf-123">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="54acf-124">Cuándo se debe usar al proxy generado</span><span class="sxs-lookup"><span data-stu-id="54acf-124">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="54acf-125">Programa de instalación de cliente</span><span class="sxs-lookup"><span data-stu-id="54acf-125">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="54acf-126">Cómo se hacen referencia al proxy generado dinámicamente</span><span class="sxs-lookup"><span data-stu-id="54acf-126">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="54acf-127">Cómo crear un archivo físico para SignalR genera proxy</span><span class="sxs-lookup"><span data-stu-id="54acf-127">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="54acf-128">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="54acf-128">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="54acf-129">$. connection.hub es el mismo objeto crea ese $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="54acf-129">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="54acf-130">Ejecución asincrónica del método de inicio</span><span class="sxs-lookup"><span data-stu-id="54acf-130">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="54acf-131">Cómo establecer una conexión entre dominios</span><span class="sxs-lookup"><span data-stu-id="54acf-131">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="54acf-132">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="54acf-132">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="54acf-133">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="54acf-133">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="54acf-134">Cómo especificar el modo de transporte</span><span class="sxs-lookup"><span data-stu-id="54acf-134">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="54acf-135">Cómo obtener a un proxy para una clase de Hub</span><span class="sxs-lookup"><span data-stu-id="54acf-135">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="54acf-136">Cómo definir métodos en el cliente que el servidor puede llamar a</span><span class="sxs-lookup"><span data-stu-id="54acf-136">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="54acf-137">Cómo llamar a métodos del servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="54acf-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="54acf-138">Cómo controlar eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="54acf-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="54acf-139">Cómo controlar los errores</span><span class="sxs-lookup"><span data-stu-id="54acf-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="54acf-140">Cómo habilitar el registro del lado cliente</span><span class="sxs-lookup"><span data-stu-id="54acf-140">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="54acf-141">Para obtener documentación sobre cómo programar el servidor o los clientes. NET, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="54acf-141">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="54acf-142">Guía de la API SignalR Hubs - servidor</span><span class="sxs-lookup"><span data-stu-id="54acf-142">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="54acf-143">Guía de API de concentradores de SignalR: cliente .NET</span><span class="sxs-lookup"><span data-stu-id="54acf-143">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="54acf-144">El componente de servidor de SignalR 2 solo está disponible en .NET 4.5 (aunque no hay un cliente de .NET para SignalR 2 en .NET 4.0).</span><span class="sxs-lookup"><span data-stu-id="54acf-144">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="54acf-145">El proxy generado y lo que hace por usted</span><span class="sxs-lookup"><span data-stu-id="54acf-145">The generated proxy and what it does for you</span></span>

<span data-ttu-id="54acf-146">Puede programar un cliente de JavaScript para comunicarse con un servicio de SignalR con o sin un proxy que SignalR genera para usted.</span><span class="sxs-lookup"><span data-stu-id="54acf-146">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="54acf-147">Lo que hace el proxy para usted es simplificar la sintaxis del código se usa para conectarse, los métodos de escritura que llama el servidor, y llamar a métodos en el servidor.</span><span class="sxs-lookup"><span data-stu-id="54acf-147">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="54acf-148">Al escribir código para llamar a métodos de servidor, el proxy generado le permite usar la sintaxis que parece ser que se estaban ejecutando una función local: puede escribir `serverMethod(arg1, arg2)` en lugar de `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="54acf-148">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="54acf-149">La sintaxis de proxy generada también permite un error inmediato e inteligible del lado cliente, si se escribe incorrectamente el nombre de un método de servidor.</span><span class="sxs-lookup"><span data-stu-id="54acf-149">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="54acf-150">Y si creas manualmente el archivo que define a los servidores proxy, también puede obtener compatibilidad con IntelliSense para escribir código que llama a métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="54acf-150">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="54acf-151">Por ejemplo, suponga que tiene la siguiente clase de Hub en el servidor:</span><span class="sxs-lookup"><span data-stu-id="54acf-151">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="54acf-152">Los ejemplos de código siguientes muestran JavaScript código aspecto para invocar el `NewContosoChatMessage` método en el servidor y recibir las invocaciones de la `addContosoChatMessageToPage` método desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="54acf-152">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="54acf-153">**Con el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="54acf-153">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="54acf-154">**Sin el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="54acf-154">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="54acf-155">Cuándo se debe usar al proxy generado</span><span class="sxs-lookup"><span data-stu-id="54acf-155">When to use the generated proxy</span></span>

<span data-ttu-id="54acf-156">Si desea registrar varios controladores de eventos para un método de cliente que llama el servidor, no puede usar al proxy generado.</span><span class="sxs-lookup"><span data-stu-id="54acf-156">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="54acf-157">En caso contrario, puede optar por usar al proxy generado o no según sus preferencias de codificación.</span><span class="sxs-lookup"><span data-stu-id="54acf-157">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="54acf-158">Si decide no usarlo, no debe hacer referencia a la dirección URL de "signalr/hubs" en un `script` elemento en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="54acf-158">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="54acf-159">Programa de instalación de cliente</span><span class="sxs-lookup"><span data-stu-id="54acf-159">Client setup</span></span>

<span data-ttu-id="54acf-160">Un cliente de JavaScript requiere referencias a jQuery y el archivo de JavaScript de SignalR core.</span><span class="sxs-lookup"><span data-stu-id="54acf-160">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="54acf-161">Debe ser la versión de jQuery 1.6.4 o versiones posteriores importantes, como 1.9.1, 1.8.2 o 1.7.2.</span><span class="sxs-lookup"><span data-stu-id="54acf-161">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="54acf-162">Si decide usar al proxy generado, también necesita una referencia al proxy de SignalR genera archivos de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="54acf-162">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="54acf-163">El ejemplo siguiente muestra el aspecto que es posible que las referencias en una página HTML que usa al proxy generado.</span><span class="sxs-lookup"><span data-stu-id="54acf-163">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="54acf-164">Estas referencias deben incluirse en este orden: jQuery, SignalR core después de eso y servidores proxy de SignalR apellidos.</span><span class="sxs-lookup"><span data-stu-id="54acf-164">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="54acf-165">Cómo se hacen referencia al proxy generado dinámicamente</span><span class="sxs-lookup"><span data-stu-id="54acf-165">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="54acf-166">En el ejemplo anterior, la referencia al proxy de SignalR generado es código JavaScript generado dinámicamente, no a un archivo físico.</span><span class="sxs-lookup"><span data-stu-id="54acf-166">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="54acf-167">SignalR crea el código de JavaScript para el proxy sobre la marcha y sirve al cliente en respuesta a la dirección URL "/ signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="54acf-167">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="54acf-168">Si especifica una dirección URL base diferente para las conexiones SignalR en el servidor en su `MapSignalR` método, la dirección URL del archivo de proxy generada dinámicamente es la dirección URL personalizada con "/ hubs" anexado a él.</span><span class="sxs-lookup"><span data-stu-id="54acf-168">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="54acf-169">Para los clientes de JavaScript de Windows 8 (Windows Store), use el archivo de proxy físico en lugar del generada dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="54acf-169">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="54acf-170">Para obtener más información, consulte [cómo crear un archivo físico para SignalR genera proxy](#manualproxy) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="54acf-170">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="54acf-171">En ASP.NET MVC 4 o 5 vista de Razor, use la tilde para hacer referencia a la raíz de la aplicación en la referencia del archivo de proxy:</span><span class="sxs-lookup"><span data-stu-id="54acf-171">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="54acf-172">Para obtener más información sobre el uso de SignalR en MVC 5, consulte [Introducción a SignalR y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="54acf-172">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="54acf-173">En una vista de Razor de ASP.NET MVC 3, use `Url.Content` para su referencia de archivo de proxy:</span><span class="sxs-lookup"><span data-stu-id="54acf-173">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="54acf-174">En una aplicación de formularios Web Forms de ASP.NET, utilice `ResolveClientUrl` para servidores proxy de la referencia de archivo o registrarlo mediante ScriptManager mediante una ruta relativa de raíz de aplicación (empezando por una tilde):</span><span class="sxs-lookup"><span data-stu-id="54acf-174">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="54acf-175">Como norma general, utilice el mismo método para especificar la dirección URL "/ signalr/hubs" que usa para los archivos CSS o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="54acf-175">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="54acf-176">Si especifica una dirección URL sin utilizar una tilde, en algunos escenarios de la aplicación funcionará correctamente cuando se prueba en Visual Studio con IIS Express, pero se producirá un error 404 cuando se implementa en IIS completo.</span><span class="sxs-lookup"><span data-stu-id="54acf-176">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="54acf-177">Para obtener más información, consulte **resolver referencias a los recursos de nivel de raíz** en [servidores Web en Visual Studio para proyectos Web de ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) en el sitio MSDN.</span><span class="sxs-lookup"><span data-stu-id="54acf-177">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="54acf-178">Cuando se ejecuta un proyecto web en Visual Studio 2013 en modo de depuración y, si usa Internet Explorer como explorador, puede ver el archivo de proxy en **el Explorador de soluciones** en **documentos de Script**, tal y como se muestra en el ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="54acf-178">When you run a web project in Visual Studio 2013 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Archivo de proxy generada de JavaScript en el Explorador de soluciones](hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="54acf-180">Para ver el contenido del archivo, haga doble clic en **hubs**.</span><span class="sxs-lookup"><span data-stu-id="54acf-180">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="54acf-181">Si no está utilizando Internet Explorer y Visual Studio 2012 o 2013, o si no está en modo de depuración, también puede obtener el contenido del archivo yendo a la dirección URL "/ signalR/hubs".</span><span class="sxs-lookup"><span data-stu-id="54acf-181">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="54acf-182">Por ejemplo, si el sitio se ejecuta en `http://localhost:56699`, vaya a `http://localhost:56699/SignalR/hubs` en el explorador.</span><span class="sxs-lookup"><span data-stu-id="54acf-182">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="54acf-183">Cómo crear un archivo físico para SignalR genera proxy</span><span class="sxs-lookup"><span data-stu-id="54acf-183">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="54acf-184">Como alternativa para el proxy generado de forma dinámica, puede crear un archivo físico que tiene el código proxy y hacer referencia a ese archivo.</span><span class="sxs-lookup"><span data-stu-id="54acf-184">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="54acf-185">Puede hacer un control sobre el almacenamiento en caché o su comportamiento de agrupación u obtener IntelliSense cuando codifique las llamadas a métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="54acf-185">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="54acf-186">Para crear un archivo de proxy, realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="54acf-186">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="54acf-187">Instalar el [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="54acf-187">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="54acf-188">Abra un símbolo del sistema y vaya a la *herramientas* carpeta que contiene el archivo SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="54acf-188">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="54acf-189">La carpeta de herramientas está en la siguiente ubicación:</span><span class="sxs-lookup"><span data-stu-id="54acf-189">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="54acf-190">Escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="54acf-190">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="54acf-191">La ruta de acceso a su *.dll* suele ser el *bin* carpeta en la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="54acf-191">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="54acf-192">Este comando crea un archivo denominado *server.js* en la misma carpeta que *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="54acf-192">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="54acf-193">Coloque el *server.js* de archivos en una carpeta adecuada en el proyecto, cambie su nombre según corresponda para su aplicación y agregue una referencia a ella en lugar de la referencia "signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="54acf-193">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="54acf-194">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="54acf-194">How to establish a connection</span></span>

<span data-ttu-id="54acf-195">Antes de establecer una conexión, tendrá que crear un objeto de conexión, crear a un proxy y registrar los controladores de eventos para los métodos que pueden llamarse desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="54acf-195">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="54acf-196">Cuando se configuran el proxy y controladores de eventos, establecer la conexión mediante una llamada a la `start` método.</span><span class="sxs-lookup"><span data-stu-id="54acf-196">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="54acf-197">Si usa al proxy generado, no tienes que crear el objeto de conexión en su propio código porque el código proxy generado lo hace por usted.</span><span class="sxs-lookup"><span data-stu-id="54acf-197">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="54acf-198">**Establecer una conexión (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-198">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="54acf-199">**Establecer una conexión (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-199">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="54acf-200">El código de ejemplo usa el valor predeterminado "/ signalr" dirección URL para conectarse a su servicio SignalR.</span><span class="sxs-lookup"><span data-stu-id="54acf-200">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="54acf-201">Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [Guía de la API de ASP.NET SignalR Hubs - Server - la dirección URL de /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="54acf-201">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="54acf-202">De forma predeterminada, la ubicación del concentrador es el servidor actual; Si se conecta a un servidor diferente, especifique la dirección URL antes de llamar a la `start` método, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="54acf-202">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="54acf-203">Normalmente registra controladores de eventos antes de llamar a la `start` método para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="54acf-203">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="54acf-204">Si desea registrar algunos controladores de eventos después de establecer la conexión, puede hacerlo, pero debe registrar al menos uno de los controladores de los eventos antes de llamar a la `start` método.</span><span class="sxs-lookup"><span data-stu-id="54acf-204">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="54acf-205">Una razón para esto es que puede haber muchos centros en una aplicación, pero que no desea que activen la `OnConnected` eventos en cada centro si solo va a usar para uno de ellos.</span><span class="sxs-lookup"><span data-stu-id="54acf-205">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="54acf-206">Cuando se establece la conexión, la presencia de un método de cliente de proxy de un concentrador es que le indica a SignalR para desencadenar la `OnConnected` eventos.</span><span class="sxs-lookup"><span data-stu-id="54acf-206">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="54acf-207">Si no registra los controladores de eventos antes de llamar a la `start` método, podrá invocar métodos en el concentrador, pero el concentrador `OnConnected` no se llamará al método y no se invocará ningún método de cliente desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="54acf-207">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="54acf-208">$. connection.hub es el mismo objeto crea ese $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="54acf-208">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="54acf-209">Como puede ver en los ejemplos, cuando se usa el proxy generado, `$.connection.hub` hace referencia al objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="54acf-209">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="54acf-210">Este es el mismo objeto que se obtiene mediante una llamada a `$.hubConnection()` cuando no está usando el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="54acf-210">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="54acf-211">El código proxy generado, crea la conexión automáticamente al ejecutar la instrucción siguiente:</span><span class="sxs-lookup"><span data-stu-id="54acf-211">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Crear una conexión en el archivo de proxy generada](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="54acf-213">Cuando se usa el proxy generado, puede hacer nada con `$.connection.hub` que puede hacer con un objeto de conexión cuando no usa el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="54acf-213">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="54acf-214">Ejecución asincrónica del método de inicio</span><span class="sxs-lookup"><span data-stu-id="54acf-214">Asynchronous execution of the start method</span></span>

<span data-ttu-id="54acf-215">El `start` método se ejecuta de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="54acf-215">The `start` method executes asynchronously.</span></span> <span data-ttu-id="54acf-216">Devuelve un [jQuery aplazado objeto](http://api.jquery.com/category/deferred-object/), lo que significa que puede agregar funciones de devolución de llamada llamando a métodos como `pipe`, `done`, y `fail`.</span><span class="sxs-lookup"><span data-stu-id="54acf-216">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="54acf-217">Si tiene código que desea ejecutar una vez establecida la conexión, como una llamada a un método de servidor, agregue el código en una función de devolución de llamada o llamarlo desde una función de devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="54acf-217">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="54acf-218">El `.done` método de devolución de llamada se ejecuta después de que se ha establecido la conexión y, después de cualquier código que tiene en su `OnConnected` método de controlador de eventos en el servidor termina de ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="54acf-218">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="54acf-219">Si coloca la instrucción "Ahora conectados" del ejemplo anterior, como la siguiente línea de código después la `start` llamada al método (no en un `.done` devolución de llamada), la `console.log` línea se ejecutará antes de establecer la conexión, como se muestra en la siguiente ejemplo:</span><span class="sxs-lookup"><span data-stu-id="54acf-219">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Erróneo para escribir código que se ejecuta una vez establecida la conexión](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="54acf-221">Cómo establecer una conexión entre dominios</span><span class="sxs-lookup"><span data-stu-id="54acf-221">How to establish a cross-domain connection</span></span>

<span data-ttu-id="54acf-222">Normalmente, si el explorador carga una página de `http://contoso.com`, la conexión de SignalR está en el mismo dominio, en `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="54acf-222">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="54acf-223">Si la página de `http://contoso.com` realiza una conexión a `http://fabrikam.com/signalr`, que es una conexión entre dominios.</span><span class="sxs-lookup"><span data-stu-id="54acf-223">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="54acf-224">Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="54acf-224">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="54acf-225">En SignalR 1.x solicitudes entre dominios se controla mediante una única marca EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="54acf-225">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="54acf-226">Este indicador controla las solicitudes de JSONP y CORS.</span><span class="sxs-lookup"><span data-stu-id="54acf-226">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="54acf-227">Para una mayor flexibilidad, compatibilidad con CORS todos los ha quitado el componente de servidor de SignalR (clientes de JavaScript todavía usan CORS normalmente si se detecta que el explorador lo admite), y middleware OWIN nuevo está disponible admitir estos escenarios.</span><span class="sxs-lookup"><span data-stu-id="54acf-227">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="54acf-228">Si se requiere JSONP en el cliente (para admitir las solicitudes entre dominios en los exploradores más antiguos), deberá habilitarse explícitamente estableciendo `EnableJSONP` en el `HubConfiguration` objeto `true`, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="54acf-228">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="54acf-229">JSONP está deshabilitada de forma predeterminada, ya que es menos segura que la CORS.</span><span class="sxs-lookup"><span data-stu-id="54acf-229">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="54acf-230">**Agregar Microsoft.Owin.Cors a su proyecto:** para instalar esta biblioteca, ejecute el siguiente comando en la consola de administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="54acf-230">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="54acf-231">Este comando agregará el 2.1.0 versión del paquete al proyecto.</span><span class="sxs-lookup"><span data-stu-id="54acf-231">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="54acf-232">Llamar a UseCors</span><span class="sxs-lookup"><span data-stu-id="54acf-232">Calling UseCors</span></span>

 <span data-ttu-id="54acf-233">El fragmento de código siguiente muestra cómo implementar las conexiones entre dominios en SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="54acf-233">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span> 

<span data-ttu-id="54acf-234">**Implementación de las solicitudes entre dominios en SignalR 2**</span><span class="sxs-lookup"><span data-stu-id="54acf-234">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="54acf-235">El código siguiente muestra cómo se habilita la CORS o JSONP en un proyecto de SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="54acf-235">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="54acf-236">Este ejemplo de código usa `Map` y `RunSignalR` en lugar de `MapSignalR`, de modo que el middleware de CORS se ejecuta solo para las solicitudes de SignalR que requieren compatibilidad con CORS (en lugar de para todo el tráfico en la ruta de acceso especificada en `MapSignalR`.) Map también puede usarse para cualquier otro middleware que debe ejecutarse para un prefijo de dirección URL específico, en lugar de toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="54acf-236">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - <span data-ttu-id="54acf-237">No establezca `jQuery.support.cors` en true en el código.</span><span class="sxs-lookup"><span data-stu-id="54acf-237">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![No establezca jQuery.support.cors en true](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="54acf-239">SignalR controla el uso de la CORS.</span><span class="sxs-lookup"><span data-stu-id="54acf-239">SignalR handles the use of CORS.</span></span> <span data-ttu-id="54acf-240">Establecer `jQuery.support.cors` a true deshabilita JSONP porque hace que SignalR asumir el explorador admite la CORS.</span><span class="sxs-lookup"><span data-stu-id="54acf-240">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="54acf-241">Cuando se conecta a una dirección URL de localhost, Internet Explorer 10 no considerarlo una conexión entre dominios, por lo que la aplicación funcionará localmente con Internet Explorer 10 incluso si no ha habilitado las conexiones entre dominios en el servidor.</span><span class="sxs-lookup"><span data-stu-id="54acf-241">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="54acf-242">Para obtener información sobre el uso de las conexiones entre dominios con Internet Explorer 9, consulte [este hilo](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="54acf-242">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="54acf-243">Para obtener información sobre el uso de las conexiones entre dominios con Chrome, consulte [este hilo](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="54acf-243">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="54acf-244">El código de ejemplo usa el valor predeterminado "/ signalr" dirección URL para conectarse a su servicio SignalR.</span><span class="sxs-lookup"><span data-stu-id="54acf-244">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="54acf-245">Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [Guía de la API de ASP.NET SignalR Hubs - Server - la dirección URL de /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="54acf-245">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="54acf-246">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="54acf-246">How to configure the connection</span></span>

<span data-ttu-id="54acf-247">Antes de establecer una conexión, puede especificar parámetros de cadena de consulta o especificar el modo de transporte.</span><span class="sxs-lookup"><span data-stu-id="54acf-247">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="54acf-248">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="54acf-248">How to specify query string parameters</span></span>

<span data-ttu-id="54acf-249">Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta para el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="54acf-249">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="54acf-250">Los ejemplos siguientes muestran cómo establecer un parámetro de cadena de consulta en código de cliente.</span><span class="sxs-lookup"><span data-stu-id="54acf-250">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="54acf-251">**Establecer un valor de cadena de consulta antes de llamar al método start (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-251">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="54acf-252">**Establecer un valor de cadena de consulta antes de llamar al método start (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-252">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="54acf-253">El ejemplo siguiente muestra cómo leer un parámetro de cadena de consulta en código del servidor.</span><span class="sxs-lookup"><span data-stu-id="54acf-253">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="54acf-254">Cómo especificar el modo de transporte</span><span class="sxs-lookup"><span data-stu-id="54acf-254">How to specify the transport method</span></span>

<span data-ttu-id="54acf-255">Como parte del proceso de conexión, un cliente de SignalR normalmente negocia con el servidor para determinar el mejor transporte que se admite por servidor y cliente.</span><span class="sxs-lookup"><span data-stu-id="54acf-255">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="54acf-256">Si ya conoce el tipo de transporte que desea usar, puede omitir este proceso de negociación de mediante la especificación del método de transporte cuando se llama a la `start` método.</span><span class="sxs-lookup"><span data-stu-id="54acf-256">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="54acf-257">**Código de cliente que especifica el método de transporte (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-257">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="54acf-258">**Código de cliente que especifica el método de transporte (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-258">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="54acf-259">Como alternativa, puede especificar varios métodos de transporte en el orden en el que desea SignalR para probarlas:</span><span class="sxs-lookup"><span data-stu-id="54acf-259">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="54acf-260">**Código de cliente que especifica un esquema de reserva de transporte personalizado (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-260">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="54acf-261">**Código de cliente que especifica un esquema de reserva de transporte personalizado (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-261">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="54acf-262">Puede usar los siguientes valores para especificar el modo de transporte:</span><span class="sxs-lookup"><span data-stu-id="54acf-262">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="54acf-263">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="54acf-263">"webSockets"</span></span>
- <span data-ttu-id="54acf-264">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="54acf-264">"foreverFrame"</span></span>
- <span data-ttu-id="54acf-265">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="54acf-265">"serverSentEvents"</span></span>
- <span data-ttu-id="54acf-266">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="54acf-266">"longPolling"</span></span>

<span data-ttu-id="54acf-267">Los ejemplos siguientes muestran cómo averiguar qué método de transporte que está usando una conexión.</span><span class="sxs-lookup"><span data-stu-id="54acf-267">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="54acf-268">**Código de cliente que se muestra el método de transporte utilizado por una conexión (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-268">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="54acf-269">**Código de cliente que se muestra el método de transporte utilizado por una conexión (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-269">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="54acf-270">Para obtener información sobre cómo comprobar el modo de transporte en el código de servidor, consulte [Guía de la API de ASP.NET SignalR Hubs - Server - obtener información sobre el cliente de la propiedad de contexto](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="54acf-270">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="54acf-271">Para obtener más información acerca de los transportes y reservas, consulte [Introducción a SignalR - transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="54acf-271">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="54acf-272">Cómo obtener a un proxy para una clase de Hub</span><span class="sxs-lookup"><span data-stu-id="54acf-272">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="54acf-273">Cada objeto de conexión que cree encapsula información sobre una conexión a un servicio de SignalR que contiene una o más clases de concentrador.</span><span class="sxs-lookup"><span data-stu-id="54acf-273">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="54acf-274">Para comunicarse con una clase de Hub, use un objeto proxy que cree usted mismo (si no usa al proxy generado) o que se genera automáticamente.</span><span class="sxs-lookup"><span data-stu-id="54acf-274">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="54acf-275">En el cliente el nombre del proxy es una versión de mayúsculas y minúsculas camel del nombre de clase del concentrador.</span><span class="sxs-lookup"><span data-stu-id="54acf-275">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="54acf-276">SignalR realiza automáticamente este cambio para que el código JavaScript puede cumplir las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="54acf-276">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="54acf-277">**Clase de Hub en servidor**</span><span class="sxs-lookup"><span data-stu-id="54acf-277">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="54acf-278">**Obtener una referencia al proxy de cliente generado para el concentrador**</span><span class="sxs-lookup"><span data-stu-id="54acf-278">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="54acf-279">**Crear el proxy de cliente para la clase Hub (sin proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-279">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="54acf-280">Si decorar la clase de Hub con un `HubName` atributo, use el nombre exacto sin cambiar mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="54acf-280">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="54acf-281">**Clase de Hub en el servidor con el atributo HubName**</span><span class="sxs-lookup"><span data-stu-id="54acf-281">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="54acf-282">**Obtener una referencia al proxy de cliente generado para el concentrador**</span><span class="sxs-lookup"><span data-stu-id="54acf-282">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="54acf-283">**Crear el proxy de cliente para la clase Hub (sin proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-283">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="54acf-284">Cómo definir métodos en el cliente que el servidor puede llamar a</span><span class="sxs-lookup"><span data-stu-id="54acf-284">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="54acf-285">Para definir un método que el servidor puede llamar a un concentrador, agregar un controlador de eventos para el proxy de concentrador mediante el `client` propiedad de la llamada o proxy generado el `on` método si no usa el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="54acf-285">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="54acf-286">Los parámetros pueden ser objetos complejos.</span><span class="sxs-lookup"><span data-stu-id="54acf-286">The parameters can be complex objects.</span></span>

<span data-ttu-id="54acf-287">Agregar el controlador de eventos antes de llamar a la `start` método para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="54acf-287">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="54acf-288">(Si desea agregar controladores de eventos después de llamar a la `start` método, vea la nota de [cómo establecer una conexión](#establishconnection) anteriormente en este documento y utilice la sintaxis mostrada para definir un método sin usar el proxy generado.)</span><span class="sxs-lookup"><span data-stu-id="54acf-288">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="54acf-289">Coincidencia de nombres de método distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="54acf-289">Method name matching is case-insensitive.</span></span> <span data-ttu-id="54acf-290">Por ejemplo, `Clients.All.addContosoChatMessageToPage` se ejecutará en el servidor `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, o `addcontosochatmessagetopage` en el cliente.</span><span class="sxs-lookup"><span data-stu-id="54acf-290">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="54acf-291">**Definir el método en el cliente (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-291">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="54acf-292">**Forma alternativa para definir el método de cliente (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-292">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="54acf-293">**Definir el método en el cliente (sin el proxy generado, o cuando se agrega después de llamar al método start)**</span><span class="sxs-lookup"><span data-stu-id="54acf-293">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="54acf-294">**Código de servidor que llama al método de cliente**</span><span class="sxs-lookup"><span data-stu-id="54acf-294">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="54acf-295">Los ejemplos siguientes incluyen un objeto complejo como un parámetro de método.</span><span class="sxs-lookup"><span data-stu-id="54acf-295">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="54acf-296">**Definir el método en el cliente que toma un objeto complejo (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-296">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="54acf-297">**Definir el método en el cliente que toma un objeto complejo (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-297">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="54acf-298">**Código de servidor que define el objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="54acf-298">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="54acf-299">**Código de servidor que llama al método de cliente mediante un objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="54acf-299">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="54acf-300">Cómo llamar a métodos del servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="54acf-300">How to call server methods from the client</span></span>

<span data-ttu-id="54acf-301">Para llamar a un método de servidor desde el cliente, use el `server` propiedad del proxy generado o `invoke` método en el proxy de concentrador, si no usa el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="54acf-301">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="54acf-302">El valor devuelto o los parámetros pueden ser objetos complejos.</span><span class="sxs-lookup"><span data-stu-id="54acf-302">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="54acf-303">Pasar de una versión de mayúscula y minúscula camel del nombre del método del concentrador.</span><span class="sxs-lookup"><span data-stu-id="54acf-303">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="54acf-304">SignalR realiza automáticamente este cambio para que el código JavaScript puede cumplir las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="54acf-304">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="54acf-305">Los ejemplos siguientes muestran cómo llamar a un método de servidor que no tiene un valor devuelto y cómo llamar a un método de servidor que tiene un valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="54acf-305">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="54acf-306">**Método de servidor con ningún atributo HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="54acf-306">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="54acf-307">**Código de servidor que define el objeto complejo que se pasa en un parámetro**</span><span class="sxs-lookup"><span data-stu-id="54acf-307">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="54acf-308">**Código de cliente que invoca el método de servidor (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-308">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="54acf-309">**Código de cliente que invoca el método de servidor (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-309">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="54acf-310">Si decora el método de concentrador con un `HubMethodName` atributo, use ese nombre sin cambiar mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="54acf-310">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="54acf-311">**Método de servidor** con un atributo HubMethodName</span><span class="sxs-lookup"><span data-stu-id="54acf-311">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="54acf-312">**Código de cliente que invoca el método de servidor (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-312">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="54acf-313">**Código de cliente que invoca el método de servidor (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-313">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="54acf-314">Los ejemplos anteriores muestran cómo llamar a un método de servidor que no tiene ningún valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="54acf-314">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="54acf-315">Los ejemplos siguientes muestran cómo llamar a un método de servidor que tiene un valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="54acf-315">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="54acf-316">**Código de servidor para un método que tiene un valor devuelto**</span><span class="sxs-lookup"><span data-stu-id="54acf-316">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="54acf-317">**La clase de acción utilizada para el** devolver valor</span><span class="sxs-lookup"><span data-stu-id="54acf-317">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="54acf-318">**Código de cliente que invoca el método de servidor (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-318">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="54acf-319">**Código de cliente que invoca el método de servidor (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-319">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="54acf-320">Cómo controlar eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="54acf-320">How to handle connection lifetime events</span></span>

<span data-ttu-id="54acf-321">SignalR proporciona la siguiente conexión de eventos de duración que puede controlar:</span><span class="sxs-lookup"><span data-stu-id="54acf-321">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="54acf-322">`starting`: Se genera antes de que se envían los datos a través de la conexión.</span><span class="sxs-lookup"><span data-stu-id="54acf-322">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="54acf-323">`received`: Se genera cuando se recibe ningún dato en la conexión.</span><span class="sxs-lookup"><span data-stu-id="54acf-323">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="54acf-324">Proporciona los datos recibidos.</span><span class="sxs-lookup"><span data-stu-id="54acf-324">Provides the received data.</span></span>
- <span data-ttu-id="54acf-325">`connectionSlow`: Se genera cuando el cliente detecta una conexión lenta o eliminación con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="54acf-325">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="54acf-326">`reconnecting`: Se genera cuando comienza a volver a conectar el transporte subyacente.</span><span class="sxs-lookup"><span data-stu-id="54acf-326">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="54acf-327">`reconnected`: Se genera cuando se ha vuelto a conectar el transporte subyacente.</span><span class="sxs-lookup"><span data-stu-id="54acf-327">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="54acf-328">`stateChanged`: Se genera cuando cambia el estado de conexión.</span><span class="sxs-lookup"><span data-stu-id="54acf-328">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="54acf-329">Proporciona el estado antiguo y el estado nueva (conexión, conectado, volviendo a conectarse o desconectado).</span><span class="sxs-lookup"><span data-stu-id="54acf-329">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="54acf-330">`disconnected`: Se genera cuando se ha desconectado la conexión.</span><span class="sxs-lookup"><span data-stu-id="54acf-330">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="54acf-331">Por ejemplo, si desea mostrar mensajes de advertencia cuando hay problemas de conexión que pueden provocar retrasos evidentes, controlar el `connectionSlow` eventos.</span><span class="sxs-lookup"><span data-stu-id="54acf-331">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="54acf-332">**Controlar el evento connectionSlow (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-332">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="54acf-333">**Controlar el evento connectionSlow (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-333">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="54acf-334">Para obtener más información, consulte [comprensión y control de eventos de duración de conexión en SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="54acf-334">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="54acf-335">Cómo controlar los errores</span><span class="sxs-lookup"><span data-stu-id="54acf-335">How to handle errors</span></span>

<span data-ttu-id="54acf-336">El cliente de JavaScript de SignalR proporciona una `error` eventos que se puede agregar un controlador para.</span><span class="sxs-lookup"><span data-stu-id="54acf-336">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="54acf-337">También puede usar el método fail para agregar un controlador de errores que resultan de una invocación de método del servidor.</span><span class="sxs-lookup"><span data-stu-id="54acf-337">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="54acf-338">Si no habilita explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que SignalR devuelve después de un error contiene información mínima sobre el error.</span><span class="sxs-lookup"><span data-stu-id="54acf-338">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="54acf-339">Por ejemplo, si una llamada a `newContosoChatMessage` se produce un error, el mensaje de error en el objeto de error contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" enviar mensajes de error detallados a los clientes de producción no se recomienda por motivos de seguridad, pero si desea permitir que los mensajes de error detallados para solucionar problemas, use el código siguiente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="54acf-339">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="54acf-340">El ejemplo siguiente muestra cómo agregar un controlador para el evento de error.</span><span class="sxs-lookup"><span data-stu-id="54acf-340">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="54acf-341">**Agregar un controlador de errores (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-341">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="54acf-342">**Agregar un controlador de errores (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-342">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="54acf-343">El ejemplo siguiente muestra cómo controlar un error de una invocación de método.</span><span class="sxs-lookup"><span data-stu-id="54acf-343">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="54acf-344">**Controla un error de una invocación de método (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-344">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="54acf-345">**Controla un error de una invocación de método (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-345">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="54acf-346">Si se produce un error en una invocación de método, el `error` también se genera el evento, por lo que el código en el `error` controlador de método y en el `.fail` ejecutaría la devolución de llamada de método.</span><span class="sxs-lookup"><span data-stu-id="54acf-346">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="54acf-347">Cómo habilitar el registro del lado cliente</span><span class="sxs-lookup"><span data-stu-id="54acf-347">How to enable client-side logging</span></span>

<span data-ttu-id="54acf-348">Para habilitar el registro de cliente en una conexión, establezca el `logging` propiedad del objeto de conexión antes de llamar a la `start` método para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="54acf-348">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="54acf-349">**Habilitación del registro (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-349">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="54acf-350">**Habilitar el registro (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="54acf-350">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="54acf-351">Para ver los registros, abrir las herramientas de desarrollador de su explorador y vaya a la pestaña de la consola. Para ver un tutorial que muestra instrucciones paso a paso y pantalla capturas que muestran cómo hacerlo, consulte [difusión de servidores con ASP.NET Signalr - habilitar el registro de](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span><span class="sxs-lookup"><span data-stu-id="54acf-351">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span></span>
