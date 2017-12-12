---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: "Guía de API de bases de datos centrales de ASP.NET SignalR - cliente .NET (SignalR 1.x) | Documentos de Microsoft"
author: pfletcher
description: "Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 2 en los clientes. NET, como tienda de Windows (WinRT), WPF, Silverlight y los contras..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 9cf99ba7887e7db847097a63c0a964ef5d461a9d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="9266a-103">Guía de API de bases de datos centrales de ASP.NET SignalR - cliente .NET (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="9266a-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>
====================
<span data-ttu-id="9266a-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9266a-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="9266a-105">Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 2 en los clientes. NET, como tienda de Windows (WinRT), WPF, Silverlight y aplicaciones de consola.</span><span class="sxs-lookup"><span data-stu-id="9266a-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="9266a-106">La API de concentradores de SignalR permite realizar llamadas a procedimiento remoto (RPC) desde un servidor a los clientes conectados y desde clientes en el servidor.</span><span class="sxs-lookup"><span data-stu-id="9266a-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="9266a-107">En el código de servidor, definir métodos que puedan llamar los clientes y llamar a métodos que se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="9266a-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="9266a-108">En el código de cliente, definir métodos que se pueda llamar desde el servidor y llamar a métodos que se ejecutan en el servidor.</span><span class="sxs-lookup"><span data-stu-id="9266a-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="9266a-109">SignalR se encarga de todas la mecánica de cliente a servidor para usted.</span><span class="sxs-lookup"><span data-stu-id="9266a-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="9266a-110">SignalR también ofrece una API de nivel inferior denominada conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="9266a-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="9266a-111">Para obtener una introducción a las conexiones persistentes, concentradores y SignalR, o para ver un tutorial que muestra cómo crear una aplicación de SignalR completa, consulte [SignalR - Getting Started](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="9266a-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="9266a-112">Información general</span><span class="sxs-lookup"><span data-stu-id="9266a-112">Overview</span></span>

<span data-ttu-id="9266a-113">Este documento contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="9266a-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="9266a-114">Programa de instalación de cliente</span><span class="sxs-lookup"><span data-stu-id="9266a-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="9266a-115">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="9266a-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="9266a-116">Conexiones entre dominios desde clientes de Silverlight</span><span class="sxs-lookup"><span data-stu-id="9266a-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="9266a-117">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="9266a-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="9266a-118">Cómo establecer el número máximo de conexiones simultáneas en los clientes WPF</span><span class="sxs-lookup"><span data-stu-id="9266a-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="9266a-119">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="9266a-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="9266a-120">Cómo especificar el método de transporte</span><span class="sxs-lookup"><span data-stu-id="9266a-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="9266a-121">Cómo especificar encabezados HTTP</span><span class="sxs-lookup"><span data-stu-id="9266a-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="9266a-122">Cómo especificar los certificados de cliente</span><span class="sxs-lookup"><span data-stu-id="9266a-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="9266a-123">Cómo crear al proxy de concentrador</span><span class="sxs-lookup"><span data-stu-id="9266a-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="9266a-124">Cómo definir métodos en el cliente que el servidor puede llamar a</span><span class="sxs-lookup"><span data-stu-id="9266a-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="9266a-125">Métodos sin parámetros</span><span class="sxs-lookup"><span data-stu-id="9266a-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="9266a-126">Métodos con parámetros, especificar los tipos de parámetro</span><span class="sxs-lookup"><span data-stu-id="9266a-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="9266a-127">Métodos con parámetros, que se especifiquen objetos dinámicos para los parámetros</span><span class="sxs-lookup"><span data-stu-id="9266a-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="9266a-128">Cómo quitar un controlador</span><span class="sxs-lookup"><span data-stu-id="9266a-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="9266a-129">Cómo llamar a métodos de servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="9266a-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="9266a-130">Cómo controlar eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="9266a-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="9266a-131">Cómo controlar errores</span><span class="sxs-lookup"><span data-stu-id="9266a-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="9266a-132">Cómo habilitar el registro de cliente</span><span class="sxs-lookup"><span data-stu-id="9266a-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="9266a-133">Ejemplos de código de aplicación de consola para los métodos de cliente que el servidor puede llamar, WPF y Silverlight</span><span class="sxs-lookup"><span data-stu-id="9266a-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="9266a-134">Para un proyectos de cliente de .NET de ejemplo, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="9266a-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="9266a-135">[Gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) en GitHub.com (ejemplos de aplicación de consola WinRT, Silverlight,).</span><span class="sxs-lookup"><span data-stu-id="9266a-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="9266a-136">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) en GitHub.com (ejemplo WPF).</span><span class="sxs-lookup"><span data-stu-id="9266a-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="9266a-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) en GitHub.com (ejemplo de aplicación de consola).</span><span class="sxs-lookup"><span data-stu-id="9266a-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="9266a-138">Para obtener documentación sobre cómo programar el servidor o los clientes de JavaScript, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="9266a-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="9266a-139">Guía de API de concentradores de SignalR - servidor</span><span class="sxs-lookup"><span data-stu-id="9266a-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="9266a-140">Guía de API de concentradores de SignalR - cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="9266a-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="9266a-141">Vínculos a temas de referencia de la API están a la versión de .NET 4.5 de la API.</span><span class="sxs-lookup"><span data-stu-id="9266a-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="9266a-142">Si usa .NET 4, consulte [la versión 4 de .NET de los temas de la API](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="9266a-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="9266a-143">Programa de instalación de cliente</span><span class="sxs-lookup"><span data-stu-id="9266a-143">Client setup</span></span>

<span data-ttu-id="9266a-144">Instalar el [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) paquete NuGet (no el [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) paquete).</span><span class="sxs-lookup"><span data-stu-id="9266a-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="9266a-145">Este paquete admite WinRT, Silverlight, WPF, aplicación de consola y los clientes de Windows Phone, para .NET 4 y .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="9266a-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="9266a-146">Si la versión de SignalR que existen en el cliente es diferente de la versión que haya en el servidor, SignalR es a menudo pueda adaptar a la diferencia.</span><span class="sxs-lookup"><span data-stu-id="9266a-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="9266a-147">Por ejemplo, cuando se suelta SignalR versión 2.0 y se instale en el servidor, el servidor va a admitir a clientes que tienen 1.1.x instalado, así como los clientes que tienen instalado 2.0.</span><span class="sxs-lookup"><span data-stu-id="9266a-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="9266a-148">Si la diferencia entre la versión en el servidor y la versión del cliente es demasiado grande, SignalR produce una `InvalidOperationException` excepción cuando el cliente intenta establecer una conexión.</span><span class="sxs-lookup"><span data-stu-id="9266a-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="9266a-149">El mensaje de error es "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="9266a-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="9266a-150">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="9266a-150">How to establish a connection</span></span>

<span data-ttu-id="9266a-151">Antes de que puede establecer una conexión, tendrá que crear un `HubConnection` de objetos y crear un proxy.</span><span class="sxs-lookup"><span data-stu-id="9266a-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="9266a-152">Para establecer la conexión, llame a la `Start` método en la `HubConnection` objeto.</span><span class="sxs-lookup"><span data-stu-id="9266a-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="9266a-153">Para los clientes de JavaScript tiene que registrar al menos un controlador de eventos antes de llamar a la `Start` método para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="9266a-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="9266a-154">Esto no es necesario para clientes de. NET.</span><span class="sxs-lookup"><span data-stu-id="9266a-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="9266a-155">Para los clientes de JavaScript, el código de proxy generado automáticamente crea servidores proxy para todos los concentradores que existen en el servidor y registrar un controlador es cómo indicar qué centros de su cliente se va a utilizar.</span><span class="sxs-lookup"><span data-stu-id="9266a-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="9266a-156">Pero para un cliente .NET crear a servidores proxy de concentrador manualmente, por lo que SignalR se da por supuesto que va a usar cualquier concentrador que cree a un proxy para.</span><span class="sxs-lookup"><span data-stu-id="9266a-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="9266a-157">El código de ejemplo usa el valor predeterminado "/ signalr" dirección URL para conectarse a su servicio de SignalR.</span><span class="sxs-lookup"><span data-stu-id="9266a-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="9266a-158">Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [Guía de API de concentradores de ASP.NET SignalR - Server - la dirección URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="9266a-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="9266a-159">El `Start` método se ejecuta de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="9266a-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="9266a-160">Para asegurarse de que las siguientes líneas de código no se ejecutan hasta que una vez establecida la conexión, use `await` en un método asincrónico de ASP.NET 4.5 o `.Wait()` en un método sincrónico.</span><span class="sxs-lookup"><span data-stu-id="9266a-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="9266a-161">No utilice `.Wait()` en un cliente de WinRT.</span><span class="sxs-lookup"><span data-stu-id="9266a-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="9266a-162">La clase `HubConnection` es segura para la ejecución de subprocesos.</span><span class="sxs-lookup"><span data-stu-id="9266a-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="9266a-163">Conexiones entre dominios desde clientes de Silverlight</span><span class="sxs-lookup"><span data-stu-id="9266a-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="9266a-164">Para obtener información acerca de cómo habilitar las conexiones entre dominios desde clientes de Silverlight, vea [hacer que un servicio disponible a través de los límites del dominio](https://msdn.microsoft.com/en-us/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="9266a-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/en-us/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="9266a-165">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="9266a-165">How to configure the connection</span></span>

<span data-ttu-id="9266a-166">Antes de establecer una conexión, puede especificar cualquiera de las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="9266a-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="9266a-167">Límite de conexiones simultáneas.</span><span class="sxs-lookup"><span data-stu-id="9266a-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="9266a-168">Parámetros de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="9266a-168">Query string parameters.</span></span>
- <span data-ttu-id="9266a-169">El método de transporte.</span><span class="sxs-lookup"><span data-stu-id="9266a-169">The transport method.</span></span>
- <span data-ttu-id="9266a-170">Encabezados HTTP.</span><span class="sxs-lookup"><span data-stu-id="9266a-170">HTTP headers.</span></span>
- <span data-ttu-id="9266a-171">Certificados de cliente.</span><span class="sxs-lookup"><span data-stu-id="9266a-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="9266a-172">Cómo establecer el número máximo de conexiones simultáneas en los clientes WPF</span><span class="sxs-lookup"><span data-stu-id="9266a-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="9266a-173">En los clientes WPF, es posible que deba aumentar el número máximo de conexiones simultáneas de su valor predeterminado de 2.</span><span class="sxs-lookup"><span data-stu-id="9266a-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="9266a-174">El valor recomendado es 10.</span><span class="sxs-lookup"><span data-stu-id="9266a-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="9266a-175">Para obtener más información, consulte [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="9266a-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="9266a-176">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="9266a-176">How to specify query string parameters</span></span>

<span data-ttu-id="9266a-177">Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta para el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="9266a-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="9266a-178">En el ejemplo siguiente se muestra cómo establecer un parámetro de cadena de consulta en código de cliente.</span><span class="sxs-lookup"><span data-stu-id="9266a-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="9266a-179">En el ejemplo siguiente se muestra cómo leer un parámetro de cadena de consulta en código del servidor.</span><span class="sxs-lookup"><span data-stu-id="9266a-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="9266a-180">Cómo especificar el método de transporte</span><span class="sxs-lookup"><span data-stu-id="9266a-180">How to specify the transport method</span></span>

<span data-ttu-id="9266a-181">Como parte del proceso de conexión, un cliente de SignalR normalmente se negocia con el servidor para determinar el mejor transporte que se admite por servidor y cliente.</span><span class="sxs-lookup"><span data-stu-id="9266a-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="9266a-182">Si ya sabe qué transporte que se va a utilizar, puede omitir este proceso de negociación.</span><span class="sxs-lookup"><span data-stu-id="9266a-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="9266a-183">Para especificar el método de transporte, pasar un objeto de transporte para el método de inicio.</span><span class="sxs-lookup"><span data-stu-id="9266a-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="9266a-184">En el ejemplo siguiente se muestra cómo especificar el método de transporte en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="9266a-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="9266a-185">El [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/en-us/library/jj918090(v=vs.111).aspx) espacio de nombres incluye las siguientes clases que se pueden utilizar para especificar el transporte.</span><span class="sxs-lookup"><span data-stu-id="9266a-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/en-us/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="9266a-186">[LongPollingTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="9266a-186">[LongPollingTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="9266a-187">[ServerSentEventsTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="9266a-187">[ServerSentEventsTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="9266a-188">[WebSocketTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponible solo cuando el servidor y cliente usan .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="9266a-188">[WebSocketTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="9266a-189">[AutoTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (elige automáticamente el transporte mejor que es compatible con el cliente y el servidor.</span><span class="sxs-lookup"><span data-stu-id="9266a-189">[AutoTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="9266a-190">Esto es el transporte predeterminado.</span><span class="sxs-lookup"><span data-stu-id="9266a-190">This is the default transport.</span></span> <span data-ttu-id="9266a-191">Esto en para pasar el `Start` método tiene el mismo efecto que no pasa nada.)</span><span class="sxs-lookup"><span data-stu-id="9266a-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="9266a-192">El transporte de ForeverFrame no está incluido en esta lista porque se utiliza únicamente por los exploradores.</span><span class="sxs-lookup"><span data-stu-id="9266a-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="9266a-193">Para obtener información sobre cómo comprobar el método de transporte en el código de servidor, consulte [Guía de API de concentradores de ASP.NET SignalR - Server - cómo obtener información sobre el cliente de la propiedad de contexto](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="9266a-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="9266a-194">Para obtener más información acerca de los transportes y reservas, consulte [Introducción a SignalR - transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="9266a-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="9266a-195">Cómo especificar encabezados HTTP</span><span class="sxs-lookup"><span data-stu-id="9266a-195">How to specify HTTP headers</span></span>

<span data-ttu-id="9266a-196">Para establecer encabezados HTTP, utilice el `Headers` propiedad en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="9266a-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="9266a-197">En el ejemplo siguiente se muestra cómo agregar un encabezado HTTP.</span><span class="sxs-lookup"><span data-stu-id="9266a-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="9266a-198">Cómo especificar los certificados de cliente</span><span class="sxs-lookup"><span data-stu-id="9266a-198">How to specify client certificates</span></span>

<span data-ttu-id="9266a-199">Para agregar certificados de cliente, use el `AddClientCertificate` método en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="9266a-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="9266a-200">Cómo crear al proxy de concentrador</span><span class="sxs-lookup"><span data-stu-id="9266a-200">How to create the Hub proxy</span></span>

<span data-ttu-id="9266a-201">Para definir métodos en el cliente que se puede llamar un concentrador desde el servidor y para invocar métodos en un concentrador en el servidor, crear un proxy para el concentrador mediante una llamada a `CreateHubProxy` en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="9266a-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="9266a-202">La cadena se pasa a `CreateHubProxy` es el nombre de la clase de base de datos central o el nombre especificado por el `HubName` atributo si se usó en el servidor.</span><span class="sxs-lookup"><span data-stu-id="9266a-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="9266a-203">Nombre la coincidencia distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="9266a-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="9266a-204">**Clase de base de datos central en servidor**</span><span class="sxs-lookup"><span data-stu-id="9266a-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="9266a-205">**Crear el proxy de cliente para la clase de base de datos central**</span><span class="sxs-lookup"><span data-stu-id="9266a-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="9266a-206">Si decorar la clase de concentrador con un `HubName` de atributo, use ese nombre.</span><span class="sxs-lookup"><span data-stu-id="9266a-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="9266a-207">**Clase de base de datos central en servidor**</span><span class="sxs-lookup"><span data-stu-id="9266a-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="9266a-208">**Crear el proxy de cliente para la clase de base de datos central**</span><span class="sxs-lookup"><span data-stu-id="9266a-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="9266a-209">El objeto proxy es segura para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="9266a-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="9266a-210">De hecho, si se llama a `HubConnection.CreateHubProxy` varias veces con el mismo `hubName`, obtendrá la misma caché `IHubProxy` objeto.</span><span class="sxs-lookup"><span data-stu-id="9266a-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="9266a-211">Cómo definir métodos en el cliente que el servidor puede llamar a</span><span class="sxs-lookup"><span data-stu-id="9266a-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="9266a-212">Para definir un método que el servidor puede llamar, use el proxy `On` método para registrar un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="9266a-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="9266a-213">Coincidencia de nombres de método distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="9266a-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="9266a-214">Por ejemplo, `Clients.All.UpdateStockPrice` se ejecutará en el servidor `updateStockPrice`, `updatestockprice`, o `UpdateStockPrice` en el cliente.</span><span class="sxs-lookup"><span data-stu-id="9266a-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="9266a-215">Las plataformas de clientes diferentes tienen requisitos diferentes para cómo escribir código del método para actualizar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="9266a-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="9266a-216">Los ejemplos mostrados son para los clientes de WinRT (tienda de Windows. NET).</span><span class="sxs-lookup"><span data-stu-id="9266a-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="9266a-217">Se proporcionan ejemplos de aplicación de consola, WPF y Silverlight en [una sección independiente más adelante en este tema](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="9266a-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="9266a-218">Métodos sin parámetros</span><span class="sxs-lookup"><span data-stu-id="9266a-218">Methods without parameters</span></span>

<span data-ttu-id="9266a-219">Si el método se está administrando no tiene parámetros, use la sobrecarga no genérica de la `On` método:</span><span class="sxs-lookup"><span data-stu-id="9266a-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="9266a-220">**Código de servidor llamar al método de cliente sin parámetros**</span><span class="sxs-lookup"><span data-stu-id="9266a-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="9266a-221">**Código de cliente de WinRT para el método se llama desde servidor sin parámetros ([ver ejemplos WPF y Silverlight más adelante en este tema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="9266a-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="9266a-222">Métodos con parámetros, especificando los tipos de parámetros</span><span class="sxs-lookup"><span data-stu-id="9266a-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="9266a-223">Si el método se está administrando tiene parámetros, especifique los tipos de los parámetros como los tipos genéricos de la `On` método.</span><span class="sxs-lookup"><span data-stu-id="9266a-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="9266a-224">Hay sobrecargas genéricas de los `On` método que le permite especificar los parámetros de hasta 8 (4 en Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="9266a-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="9266a-225">En el ejemplo siguiente, se envía un parámetro a la `UpdateStockPrice` método.</span><span class="sxs-lookup"><span data-stu-id="9266a-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="9266a-226">**Llamar al método de cliente con un parámetro de código de servidor**</span><span class="sxs-lookup"><span data-stu-id="9266a-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="9266a-227">**La clase de existencias utilizada para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="9266a-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="9266a-228">**Llama al código de cliente de WinRT para un método de servidor con un parámetro ([ver ejemplos WPF y Silverlight más adelante en este tema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="9266a-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="9266a-229">Métodos con parámetros, que se especifiquen objetos dinámicos para los parámetros</span><span class="sxs-lookup"><span data-stu-id="9266a-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="9266a-230">Como alternativa a especificar parámetros como tipos genéricos de la `On` método, puede especificar parámetros como objetos dinámicos:</span><span class="sxs-lookup"><span data-stu-id="9266a-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="9266a-231">**Llamar al método de cliente con un parámetro de código de servidor**</span><span class="sxs-lookup"><span data-stu-id="9266a-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="9266a-232">**La clase de existencias utilizada para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="9266a-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="9266a-233">**Llama al código de cliente de WinRT para un método de servidor con un parámetro, con un objeto dinámico para el parámetro ([ver ejemplos WPF y Silverlight más adelante en este tema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="9266a-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="9266a-234">Cómo quitar un controlador</span><span class="sxs-lookup"><span data-stu-id="9266a-234">How to remove a handler</span></span>

<span data-ttu-id="9266a-235">Para quitar un controlador, llame a su `Dispose` método.</span><span class="sxs-lookup"><span data-stu-id="9266a-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="9266a-236">**Código de cliente para un método invocado desde el servidor**</span><span class="sxs-lookup"><span data-stu-id="9266a-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="9266a-237">**Al quitar el controlador de código de cliente**</span><span class="sxs-lookup"><span data-stu-id="9266a-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="9266a-238">Cómo llamar a métodos de servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="9266a-238">How to call server methods from the client</span></span>

<span data-ttu-id="9266a-239">Para llamar a un método en el servidor, use la `Invoke` método en el proxy de concentrador.</span><span class="sxs-lookup"><span data-stu-id="9266a-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="9266a-240">Si el método de servidor no tiene ningún valor devuelto, use la sobrecarga no genérica de la `Invoke` método.</span><span class="sxs-lookup"><span data-stu-id="9266a-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="9266a-241">**Código de servidor para un método que no tiene ningún valor devuelto**</span><span class="sxs-lookup"><span data-stu-id="9266a-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="9266a-242">**Código de cliente llama a un método que no tiene ningún valor devuelto**</span><span class="sxs-lookup"><span data-stu-id="9266a-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="9266a-243">Si el método de servidor tiene un valor devuelto, especificar el tipo de valor devuelto como el tipo genérico de la `Invoke` método.</span><span class="sxs-lookup"><span data-stu-id="9266a-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="9266a-244">**Código de servidor para un método que tiene un valor devuelto y toma un parámetro de tipo complejo**</span><span class="sxs-lookup"><span data-stu-id="9266a-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="9266a-245">**La clase de existencias utilizada para el parámetro y valor devuelto**</span><span class="sxs-lookup"><span data-stu-id="9266a-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="9266a-246">**Código de cliente llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método asincrónico de ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="9266a-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="9266a-247">**Código de cliente llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método sincrónico**</span><span class="sxs-lookup"><span data-stu-id="9266a-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="9266a-248">El `Invoke` método ejecuta de forma asincrónica y devuelve un `Task` objeto.</span><span class="sxs-lookup"><span data-stu-id="9266a-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="9266a-249">Si no se especifica `await` o `.Wait()`, la siguiente línea de código se ejecutará antes de que el método que se invoca ha terminado de ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="9266a-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="9266a-250">Cómo controlar eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="9266a-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="9266a-251">SignalR proporciona la siguiente conexión de eventos de duración que puede controlar:</span><span class="sxs-lookup"><span data-stu-id="9266a-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="9266a-252">`Received`: Se genera cuando se reciben los datos en la conexión.</span><span class="sxs-lookup"><span data-stu-id="9266a-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="9266a-253">Proporciona los datos recibidos.</span><span class="sxs-lookup"><span data-stu-id="9266a-253">Provides the received data.</span></span>
- <span data-ttu-id="9266a-254">`ConnectionSlow`: Se genera cuando el cliente detecta una conexión lenta o quitar con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="9266a-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="9266a-255">`Reconnecting`: Se genera cuando el transporte subyacente comienza conectarse de nuevo.</span><span class="sxs-lookup"><span data-stu-id="9266a-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="9266a-256">`Reconnected`: Se genera cuando se ha vuelto a conectar el transporte subyacente.</span><span class="sxs-lookup"><span data-stu-id="9266a-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="9266a-257">`StateChanged`: Se produce cuando cambia el estado de conexión.</span><span class="sxs-lookup"><span data-stu-id="9266a-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="9266a-258">Proporciona el estado anterior y el nuevo estado.</span><span class="sxs-lookup"><span data-stu-id="9266a-258">Provides the old state and the new state.</span></span> <span data-ttu-id="9266a-259">Para obtener información acerca de la conexión de los valores de estado, vea [enumeración ConnectionState](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="9266a-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="9266a-260">`Closed`: Se genera cuando se ha desconectado la conexión.</span><span class="sxs-lookup"><span data-stu-id="9266a-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="9266a-261">Por ejemplo, si desea mostrar mensajes de advertencia para los errores que no son graves pero producir problemas de conexión intermitentes, por ejemplo, como lentitud o demasiado frecuente quitar de la conexión, controlar el `ConnectionSlow` eventos.</span><span class="sxs-lookup"><span data-stu-id="9266a-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="9266a-262">Para obtener más información, consulte [descripción y control de eventos de duración de conexión de SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="9266a-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="9266a-263">Cómo controlar errores</span><span class="sxs-lookup"><span data-stu-id="9266a-263">How to handle errors</span></span>

<span data-ttu-id="9266a-264">Si no habilita explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que SignalR devuelve después de un error contiene información básica sobre el error.</span><span class="sxs-lookup"><span data-stu-id="9266a-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="9266a-265">Por ejemplo, si una llamada a `newContosoChatMessage` se produce un error, el mensaje de error en el objeto de error contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" enviar mensajes de error detallados a los clientes de producción no se recomienda por motivos de seguridad, pero si desea habilitar los mensajes de error detallados para solución de problemas, utilice el código siguiente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="9266a-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="9266a-266">Para controlar los errores que genera SignalR, puede agregar un controlador para el `Error` evento en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="9266a-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="9266a-267">Para controlar los errores de las invocaciones de método, encapsule el código en un bloque try-catch.</span><span class="sxs-lookup"><span data-stu-id="9266a-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="9266a-268">Cómo habilitar el registro de cliente</span><span class="sxs-lookup"><span data-stu-id="9266a-268">How to enable client-side logging</span></span>

<span data-ttu-id="9266a-269">Para habilitar el registro de cliente, establezca el `TraceLevel` y `TraceWriter` propiedades en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="9266a-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="9266a-270">Ejemplos de código de aplicación de consola para los métodos de cliente que el servidor puede llamar, WPF y Silverlight</span><span class="sxs-lookup"><span data-stu-id="9266a-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="9266a-271">Los ejemplos de código se muestra anteriormente para definir métodos de cliente que el servidor puede llamar a aplican a los clientes de WinRT.</span><span class="sxs-lookup"><span data-stu-id="9266a-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="9266a-272">Los ejemplos siguientes muestran el código equivalente en WPF, Silverlight y los clientes de aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="9266a-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="9266a-273">Métodos sin parámetros</span><span class="sxs-lookup"><span data-stu-id="9266a-273">Methods without parameters</span></span>

<span data-ttu-id="9266a-274">**Código de cliente WPF para el método invocado desde el servidor sin parámetros**</span><span class="sxs-lookup"><span data-stu-id="9266a-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="9266a-275">**Código de cliente de Silverlight para el método invocado desde el servidor sin parámetros**</span><span class="sxs-lookup"><span data-stu-id="9266a-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="9266a-276">**Código de cliente de aplicación de consola para el método se llama desde servidor sin parámetros**</span><span class="sxs-lookup"><span data-stu-id="9266a-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="9266a-277">Métodos con parámetros, especificando los tipos de parámetros</span><span class="sxs-lookup"><span data-stu-id="9266a-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="9266a-278">**Código de cliente WPF para un método invocado desde el servidor con un parámetro**</span><span class="sxs-lookup"><span data-stu-id="9266a-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="9266a-279">**Código de cliente de Silverlight para un método invocado desde el servidor con un parámetro**</span><span class="sxs-lookup"><span data-stu-id="9266a-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="9266a-280">**Llama a código de cliente de aplicación de consola para un método de servidor con un parámetro**</span><span class="sxs-lookup"><span data-stu-id="9266a-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="9266a-281">Métodos con parámetros, que se especifiquen objetos dinámicos para los parámetros</span><span class="sxs-lookup"><span data-stu-id="9266a-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="9266a-282">**Código de cliente WPF para un método invocado desde el servidor con un parámetro, con un objeto dinámico para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="9266a-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="9266a-283">**Código de cliente de Silverlight para un método invocado desde el servidor con un parámetro, con un objeto dinámico para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="9266a-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="9266a-284">**Llama a código de cliente de aplicación de consola para un método de servidor con un parámetro, con un objeto dinámico para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="9266a-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
