---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Guía de API de bases de datos centrales de ASP.NET SignalR - cliente .NET (C#) | Documentos de Microsoft
author: pfletcher
description: Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 2 en los clientes. NET, como tienda de Windows (WinRT), WPF, Silverlight y los contras...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: c52a02291e18b1dd8a9d95b33fe466d17aae835f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043943"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>Guía de API de bases de datos centrales de ASP.NET SignalR - cliente .NET (C#)
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 2 en los clientes. NET, como tienda de Windows (WinRT), WPF, Silverlight y aplicaciones de consola.
> 
> La API de concentradores de SignalR permite realizar llamadas a procedimiento remoto (RPC) desde un servidor a los clientes conectados y desde clientes en el servidor. En el código de servidor, definir métodos que puedan llamar los clientes y llamar a métodos que se ejecutan en el cliente. En el código de cliente, definir métodos que se pueda llamar desde el servidor y llamar a métodos que se ejecutan en el servidor. SignalR se encarga de todas la mecánica de cliente a servidor para usted.
> 
> SignalR también ofrece una API de nivel inferior denominada conexiones persistentes. Para obtener una introducción a las conexiones persistentes, concentradores y SignalR, o para ver un tutorial que muestra cómo crear una aplicación de SignalR completa, consulte [SignalR - Getting Started](../getting-started/index.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software que se usa en este tema
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versión 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema
> 
> Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [Programa de instalación de cliente](#clientsetup)
- [Cómo establecer una conexión](#establishconnection)

    - [Conexiones entre dominios desde clientes de Silverlight](#slcrossdomain)
- [Cómo configurar la conexión](#configureconnection)

    - [Cómo establecer el número máximo de conexiones simultáneas en los clientes WPF](#maxconnections)
    - [Cómo especificar parámetros de cadena de consulta](#querystring)
    - [Cómo especificar el método de transporte](#transport)
    - [Cómo especificar encabezados HTTP](#httpheaders)
    - [Cómo especificar los certificados de cliente](#clientcertificate)
- [Cómo crear al proxy de concentrador](#proxy)
- [Cómo definir métodos en el cliente que el servidor puede llamar a](#callclient)

    - [Métodos sin parámetros](#clientmethodswithoutparms)
    - [Métodos con parámetros, especificar los tipos de parámetro](#clientmethodswithparmtypes)
    - [Métodos con parámetros, que se especifiquen objetos dinámicos para los parámetros](#clientmethodswithdynamparms)
    - [Cómo quitar un controlador](#removehandler)
- [Cómo llamar a métodos de servidor desde el cliente](#callserver)
- [Cómo controlar eventos de duración de la conexión](#connectionlifetime)
- [Cómo controlar errores](#handleerrors)
- [Cómo habilitar el registro de cliente](#logging)
- [Ejemplos de código de aplicación de consola para los métodos de cliente que el servidor puede llamar, WPF y Silverlight](#wpfsl)

Para un proyectos de cliente de .NET de ejemplo, vea los siguientes recursos:

- [Gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) en GitHub.com (ejemplos de aplicación de consola WinRT, Silverlight,).
- [DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) en GitHub.com (ejemplo WPF).
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) en GitHub.com (ejemplo de aplicación de consola).

Para obtener documentación sobre cómo programar el servidor o los clientes de JavaScript, consulte los siguientes recursos:

- [Guía de API de concentradores de SignalR - servidor](hubs-api-guide-server.md)
- [Guía de API de concentradores de SignalR - cliente de JavaScript](hubs-api-guide-javascript-client.md)

Vínculos a temas de referencia de la API están a la versión de .NET 4.5 de la API. Si usa .NET 4, consulte [la versión 4 de .NET de los temas de la API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Programa de instalación de cliente

Instalar el [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) paquete NuGet (no el [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) paquete). Este paquete admite WinRT, Silverlight, WPF, aplicación de consola y los clientes de Windows Phone, para .NET 4 y .NET 4.5.

Si la versión de SignalR que existen en el cliente es diferente de la versión que haya en el servidor, SignalR es a menudo pueda adaptar a la diferencia. Por ejemplo, un servidor que ejecuta la versión 2 de SignalR admitirá los clientes que tienen 1.1.x instalado, así como los clientes que tienen la versión 2 instalado. Si la diferencia entre la versión en el servidor y la versión del cliente es demasiado grande, o si el cliente es más reciente que el servidor, inicia SignalR un `InvalidOperationException` excepción cuando el cliente intenta establecer una conexión. El mensaje de error es "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Cómo establecer una conexión

Antes de que puede establecer una conexión, tendrá que crear un `HubConnection` de objetos y crear un proxy. Para establecer la conexión, llame a la `Start` método en la `HubConnection` objeto.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> Para los clientes de JavaScript tiene que registrar al menos un controlador de eventos antes de llamar a la `Start` método para establecer la conexión. Esto no es necesario para clientes de. NET. Para los clientes de JavaScript, el código de proxy generado automáticamente crea servidores proxy para todos los concentradores que existen en el servidor y registrar un controlador es cómo indicar qué centros de su cliente se va a utilizar. Pero para un cliente .NET crear a servidores proxy de concentrador manualmente, por lo que SignalR se da por supuesto que va a usar cualquier concentrador que cree a un proxy para.


El código de ejemplo usa el valor predeterminado "/ signalr" dirección URL para conectarse a su servicio de SignalR. Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [Guía de API de concentradores de ASP.NET SignalR - Server - la dirección URL /signalr](hubs-api-guide-server.md#signalrurl).

El `Start` método se ejecuta de forma asincrónica. Para asegurarse de que las siguientes líneas de código no se ejecutan hasta que una vez establecida la conexión, use `await` en un método asincrónico de ASP.NET 4.5 o `.Wait()` en un método sincrónico. No utilice `.Wait()` en un cliente de WinRT.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Conexiones entre dominios desde clientes de Silverlight

Para obtener información acerca de cómo habilitar las conexiones entre dominios desde clientes de Silverlight, vea [hacer que un servicio disponible a través de los límites del dominio](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Cómo configurar la conexión

Antes de establecer una conexión, puede especificar cualquiera de las siguientes opciones:

- Límite de conexiones simultáneas.
- Parámetros de cadena de consulta.
- El método de transporte.
- Encabezados HTTP.
- Certificados de cliente.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Cómo establecer el número máximo de conexiones simultáneas en los clientes WPF

En los clientes WPF, es posible que deba aumentar el número máximo de conexiones simultáneas de su valor predeterminado de 2. El valor recomendado es 10.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Para obtener más información, consulte [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Cómo especificar parámetros de cadena de consulta

Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta para el objeto de conexión. En el ejemplo siguiente se muestra cómo establecer un parámetro de cadena de consulta en código de cliente.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

En el ejemplo siguiente se muestra cómo leer un parámetro de cadena de consulta en código del servidor.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Cómo especificar el método de transporte

Como parte del proceso de conexión, un cliente de SignalR normalmente se negocia con el servidor para determinar el mejor transporte que se admite por servidor y cliente. Si ya sabe qué transporte que se va a utilizar, puede omitir este proceso de negociación. Para especificar el método de transporte, pasar un objeto de transporte para el método de inicio. En el ejemplo siguiente se muestra cómo especificar el método de transporte en el código de cliente.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

El [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) espacio de nombres incluye las siguientes clases que se pueden utilizar para especificar el transporte.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponible solo cuando el servidor y cliente usan .NET 4.5).
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (elige automáticamente el transporte mejor que es compatible con el cliente y el servidor. Esto es el transporte predeterminado. Esto en para pasar el `Start` método tiene el mismo efecto que no pasa nada.)

El transporte de ForeverFrame no está incluido en esta lista porque se utiliza únicamente por los exploradores.

Para obtener información sobre cómo comprobar el método de transporte en el código de servidor, consulte [Guía de API de concentradores de ASP.NET SignalR - Server - cómo obtener información sobre el cliente de la propiedad de contexto](hubs-api-guide-server.md#contextproperty). Para obtener más información acerca de los transportes y reservas, consulte [Introducción a SignalR - transportes y reservas](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Cómo especificar encabezados HTTP

Para establecer encabezados HTTP, utilice el `Headers` propiedad en el objeto de conexión. En el ejemplo siguiente se muestra cómo agregar un encabezado HTTP.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Cómo especificar los certificados de cliente

Para agregar certificados de cliente, use el `AddClientCertificate` método en el objeto de conexión.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Cómo crear al proxy de concentrador

Para definir métodos en el cliente que se puede llamar un concentrador desde el servidor y para invocar métodos en un concentrador en el servidor, crear un proxy para el concentrador mediante una llamada a `CreateHubProxy` en el objeto de conexión. La cadena se pasa a `CreateHubProxy` es el nombre de la clase de base de datos central o el nombre especificado por el `HubName` atributo si se usó en el servidor. Nombre la coincidencia distingue entre mayúsculas y minúsculas.

**Clase de base de datos central en servidor**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Crear el proxy de cliente para la clase de base de datos central**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Si decorar la clase de concentrador con un `HubName` de atributo, use ese nombre.

**Clase de base de datos central en servidor**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**Crear el proxy de cliente para la clase de base de datos central**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

Si se llama a `HubConnection.CreateHubProxy` varias veces con el mismo `hubName`, obtendrá la misma caché `IHubProxy` objeto.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Cómo definir métodos en el cliente que el servidor puede llamar a

Para definir un método que el servidor puede llamar, use el proxy `On` método para registrar un controlador de eventos.

Coincidencia de nombres de método distingue mayúsculas de minúsculas. Por ejemplo, `Clients.All.UpdateStockPrice` se ejecutará en el servidor `updateStockPrice`, `updatestockprice`, o `UpdateStockPrice` en el cliente.

Las plataformas de clientes diferentes tienen requisitos diferentes para cómo escribir código del método para actualizar la interfaz de usuario. Los ejemplos mostrados son para los clientes de WinRT (tienda de Windows. NET). Se proporcionan ejemplos de aplicación de consola, WPF y Silverlight en [una sección independiente más adelante en este tema](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Métodos sin parámetros

Si el método se está administrando no tiene parámetros, use la sobrecarga no genérica de la `On` método:

**Código de servidor llamar al método de cliente sin parámetros**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Código de cliente de WinRT para el método se llama desde servidor sin parámetros ([ver ejemplos WPF y Silverlight más adelante en este tema](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Métodos con parámetros, especificando los tipos de parámetros

Si el método se está administrando tiene parámetros, especifique los tipos de los parámetros como los tipos genéricos de la `On` método. Hay sobrecargas genéricas de los `On` método que le permite especificar los parámetros de hasta 8 (4 en Windows Phone 7). En el ejemplo siguiente, se envía un parámetro a la `UpdateStockPrice` método.

**Llamar al método de cliente con un parámetro de código de servidor**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**La clase de existencias utilizada para el parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**Llama al código de cliente de WinRT para un método de servidor con un parámetro ([ver ejemplos WPF y Silverlight más adelante en este tema](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Métodos con parámetros, que se especifiquen objetos dinámicos para los parámetros

Como alternativa a especificar parámetros como tipos genéricos de la `On` método, puede especificar parámetros como objetos dinámicos:

**Llamar al método de cliente con un parámetro de código de servidor**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**La clase de existencias utilizada para el parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**Llama al código de cliente de WinRT para un método de servidor con un parámetro, con un objeto dinámico para el parámetro ([ver ejemplos WPF y Silverlight más adelante en este tema](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Cómo quitar un controlador

Para quitar un controlador, llame a su `Dispose` método.

**Código de cliente para un método invocado desde el servidor**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Al quitar el controlador de código de cliente**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Cómo llamar a métodos de servidor desde el cliente

Para llamar a un método en el servidor, use la `Invoke` método en el proxy de concentrador.

Si el método de servidor no tiene ningún valor devuelto, use la sobrecarga no genérica de la `Invoke` método.

**Código de servidor para un método que no tiene ningún valor devuelto**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Código de cliente llama a un método que no tiene ningún valor devuelto**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Si el método de servidor tiene un valor devuelto, especificar el tipo de valor devuelto como el tipo genérico de la `Invoke` método.

**Código de servidor para un método que tiene un valor devuelto y toma un parámetro de tipo complejo**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**La clase de existencias utilizada para el parámetro y valor devuelto**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**Código de cliente llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método asincrónico de ASP.NET 4.5**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Código de cliente llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método sincrónico**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

El `Invoke` método ejecuta de forma asincrónica y devuelve un `Task` objeto. Si no se especifica `await` o `.Wait()`, la siguiente línea de código se ejecutará antes de que el método que se invoca ha terminado de ejecutarse.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Cómo controlar eventos de duración de la conexión

SignalR proporciona la siguiente conexión de eventos de duración que puede controlar:

- `Received`: Se genera cuando se reciben los datos en la conexión. Proporciona los datos recibidos.
- `ConnectionSlow`: Se genera cuando el cliente detecta una conexión lenta o quitar con frecuencia.
- `Reconnecting`: Se genera cuando el transporte subyacente comienza conectarse de nuevo.
- `Reconnected`: Se genera cuando se ha vuelto a conectar el transporte subyacente.
- `StateChanged`: Se produce cuando cambia el estado de conexión. Proporciona el estado anterior y el nuevo estado. Para obtener información acerca de la conexión de los valores de estado, vea [enumeración ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Se genera cuando se ha desconectado la conexión.

Por ejemplo, si desea mostrar mensajes de advertencia para los errores que no son graves pero producir problemas de conexión intermitentes, por ejemplo, como lentitud o demasiado frecuente quitar de la conexión, controlar el `ConnectionSlow` eventos.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

Para obtener más información, consulte [descripción y control de eventos de duración de conexión de SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Cómo controlar errores

Si no habilita explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que SignalR devuelve después de un error contiene información básica sobre el error. Por ejemplo, si una llamada a `newContosoChatMessage` se produce un error, el mensaje de error en el objeto de error contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" enviar mensajes de error detallados a los clientes de producción no se recomienda por motivos de seguridad, pero si desea habilitar los mensajes de error detallados para solución de problemas, utilice el código siguiente en el servidor.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Para controlar los errores que genera SignalR, puede agregar un controlador para el `Error` evento en el objeto de conexión.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

Para controlar los errores de las invocaciones de método, encapsule el código en un bloque try-catch.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Cómo habilitar el registro de cliente

Para habilitar el registro de cliente, establezca el `TraceLevel` y `TraceWriter` propiedades en el objeto de conexión.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>Ejemplos de código de aplicación de consola para los métodos de cliente que el servidor puede llamar, WPF y Silverlight

Los ejemplos de código se muestra anteriormente para definir métodos de cliente que el servidor puede llamar a aplican a los clientes de WinRT. Los ejemplos siguientes muestran el código equivalente en WPF, Silverlight y los clientes de aplicación de consola.

### <a name="methods-without-parameters"></a>Métodos sin parámetros

**Código de cliente WPF para el método invocado desde el servidor sin parámetros**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Código de cliente de Silverlight para el método invocado desde el servidor sin parámetros**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Código de cliente de aplicación de consola para el método se llama desde servidor sin parámetros**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Métodos con parámetros, especificando los tipos de parámetros

**Código de cliente WPF para un método invocado desde el servidor con un parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Código de cliente de Silverlight para un método invocado desde el servidor con un parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Llama a código de cliente de aplicación de consola para un método de servidor con un parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Métodos con parámetros, que se especifiquen objetos dinámicos para los parámetros

**Código de cliente WPF para un método invocado desde el servidor con un parámetro, con un objeto dinámico para el parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Código de cliente de Silverlight para un método invocado desde el servidor con un parámetro, con un objeto dinámico para el parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Llama a código de cliente de aplicación de consola para un método de servidor con un parámetro, con un objeto dinámico para el parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
