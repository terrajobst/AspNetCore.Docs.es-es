---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: 'Guía de API de ASP.NET SignalR Hubs: cliente .NET (SignalR 1.x) | Microsoft Docs'
author: bradygaster
description: Este documento proporciona una introducción al uso de la API de concentradores de SignalR en clientes. NET, como Windows Store (WinRT), WPF, Silverlight y los contras de la versión 2...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: dbf62b2f9851e3612885aa5375cd2c3432570643
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836208"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a>Guía de API de ASP.NET SignalR Hubs: cliente .NET (SignalR 1.x)
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 2 en los clientes. NET, como aplicaciones de consola, WPF, Silverlight y Windows Store (WinRT).
> 
> La API de concentradores de SignalR permite realizar llamadas a procedimiento remoto (RPC) de un servidor a los clientes conectados y de clientes en el servidor. En el código de servidor, definir los métodos que se pueden llamar a los clientes y llamar a métodos que se ejecutan en el cliente. En el código de cliente, definir los métodos que pueden llamarse desde el servidor y llamar a métodos que se ejecutan en el servidor. SignalR se ocupa de todos los mecanismos de cliente a servidor para usted.
> 
> SignalR también ofrece una API de nivel inferior denominada conexiones persistentes. Para obtener una introducción a SignalR, concentradores y conexiones persistentes, o para ver un tutorial que muestra cómo crear una aplicación de SignalR completa, consulte [SignalR - Introducción a](../getting-started/index.md).


## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [Programa de instalación de cliente](#clientsetup)
- [Cómo establecer una conexión](#establishconnection)

    - [Conexiones entre dominios desde clientes de Silverlight](#slcrossdomain)
- [Cómo configurar la conexión](#configureconnection)

    - [Cómo establecer el número máximo de conexiones simultáneas en los clientes WPF](#maxconnections)
    - [Cómo especificar parámetros de cadena de consulta](#querystring)
    - [Cómo especificar el modo de transporte](#transport)
    - [Cómo especificar los encabezados HTTP](#httpheaders)
    - [Cómo especificar los certificados de cliente](#clientcertificate)
- [Cómo crear al proxy de concentrador](#proxy)
- [Cómo definir métodos en el cliente que el servidor puede llamar a](#callclient)

    - [Métodos sin parámetros](#clientmethodswithoutparms)
    - [Métodos con parámetros, que se especifiquen tipos de parámetros](#clientmethodswithparmtypes)
    - [Métodos con parámetros, que se especifiquen objetos dinámicos para los parámetros](#clientmethodswithdynamparms)
    - [Cómo quitar un controlador](#removehandler)
- [Cómo llamar a métodos del servidor desde el cliente](#callserver)
- [Cómo controlar eventos de duración de la conexión](#connectionlifetime)
- [Cómo controlar los errores](#handleerrors)
- [Cómo habilitar el registro del lado cliente](#logging)
- [Ejemplos de código de aplicación de consola para que el servidor puede llamar a métodos del cliente, Silverlight y WPF](#wpfsl)

Para un proyecto de cliente .NET de ejemplo, consulte los siguientes recursos:

- [Gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) en GitHub.com (ejemplos de aplicación de WinRT, Silverlight, consola).
- [DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) en GitHub.com (ejemplo de WPF).
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) en GitHub.com (ejemplo de aplicación de consola).

Para obtener documentación sobre cómo programar el servidor o los clientes de JavaScript, consulte los siguientes recursos:

- [Guía de la API SignalR Hubs - servidor](../guide-to-the-api/hubs-api-guide-server.md)
- [Guía de API de concentradores de SignalR: cliente JavaScript](../guide-to-the-api/hubs-api-guide-javascript-client.md)

Son vínculos a temas de referencia de API a la versión 4.5 de .NET de la API. Si usa .NET 4, consulte [la versión 4 de .NET de los temas de la API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Programa de instalación de cliente

Instalar el [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) paquete NuGet (no el [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) paquete). Este paquete es compatible con WinRT, WPF, Silverlight, aplicación de consola y los clientes de Windows Phone, para .NET 4 y .NET 4.5.

Si la versión de SignalR que existen en el cliente es diferente de la versión que tiene en el servidor, SignalR a menudo es capaz de adaptarse a la diferencia. Por ejemplo, cuando se lanza la versión 2.0 de SignalR e instala en el servidor, el servidor será compatible con los clientes que tienen instalado, así como los clientes que tienen instalado 2.0 de 1.1. Si la diferencia entre la versión en el servidor y la versión en el cliente es demasiado grande, SignalR genera un `InvalidOperationException` excepción cuando el cliente intenta establecer una conexión. El mensaje de error es "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Cómo establecer una conexión

Antes de establecer una conexión, debe crear un `HubConnection` de objetos y crear un proxy. Para establecer la conexión, llame a la `Start` método en el `HubConnection` objeto.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> Para los clientes de JavaScript tiene que registrar al menos un controlador de eventos antes de llamar a la `Start` método para establecer la conexión. Esto no es necesario para los clientes. NET. Para los clientes de JavaScript, el código proxy generado automáticamente crea objetos proxy para todos los centros que existen en el servidor y registrar un controlador es el modo de indicar qué Hubs el cliente desea usar. Pero para que un cliente .NET crear a servidores proxy de concentrador manualmente, por lo que SignalR se da por supuesto que va a usar cualquier Hub que ha creado a un proxy para.


El código de ejemplo usa el valor predeterminado "/ signalr" dirección URL para conectarse a su servicio SignalR. Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [Guía de la API de ASP.NET SignalR Hubs - Server - la dirección URL de /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

El `Start` método se ejecuta de forma asincrónica. Para asegurarse de que no se ejecutan las siguientes líneas de código hasta una vez establecida la conexión, use `await` en un método asincrónico de ASP.NET 4.5 o `.Wait()` en un método sincrónico. No use `.Wait()` en un cliente de WinRT.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

La clase `HubConnection` es segura para la ejecución de subprocesos.

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

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Para obtener más información, consulte [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Cómo especificar parámetros de cadena de consulta

Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta para el objeto de conexión. El ejemplo siguiente muestra cómo establecer un parámetro de cadena de consulta en código de cliente.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

El ejemplo siguiente muestra cómo leer un parámetro de cadena de consulta en código del servidor.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Cómo especificar el modo de transporte

Como parte del proceso de conexión, un cliente de SignalR normalmente negocia con el servidor para determinar el mejor transporte que se admite por servidor y cliente. Si ya conoce el tipo de transporte que desea usar, puede omitir este proceso de negociación. Para especificar el método de transporte, pasar un objeto de transporte al método Start. El ejemplo siguiente muestra cómo especificar el modo de transporte en el código de cliente.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

El [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) espacio de nombres incluye las siguientes clases que se pueden utilizar para especificar el transporte.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponible solo cuando el servidor y cliente usan .NET 4.5).
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (elige automáticamente el mejor transporte es compatible con el cliente y el servidor. Se trata el transporte predeterminado. Esto en para pasar el `Start` método tiene el mismo efecto que no pasa nada.)

El transporte ForeverFrame no se incluye en esta lista, porque se utiliza únicamente por los exploradores.

Para obtener información sobre cómo comprobar el modo de transporte en el código de servidor, consulte [Guía de la API de ASP.NET SignalR Hubs - Server - obtener información sobre el cliente de la propiedad de contexto](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Para obtener más información acerca de los transportes y reservas, consulte [Introducción a SignalR - transportes y reservas](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Cómo especificar los encabezados HTTP

Para establecer encabezados HTTP, utilice el `Headers` propiedad del objeto de conexión. El ejemplo siguiente muestra cómo agregar un encabezado HTTP.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Cómo especificar los certificados de cliente

Para agregar certificados de cliente, use el `AddClientCertificate` método en el objeto de conexión.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Cómo crear al proxy de concentrador

Con el fin de definir métodos en el cliente que se puede llamar un concentrador del servidor y para invocar métodos en un centro en el servidor, crear un proxy para el concentrador mediante una llamada a `CreateHubProxy` en el objeto de conexión. La cadena se pasa a `CreateHubProxy` es el nombre de la clase Hub o el nombre especificado por el `HubName` atributo si se usó en el servidor. Nombre de la coincidencia distingue mayúsculas de minúsculas.

**Clase de Hub en servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Crear el proxy de cliente para la clase Hub**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Si decorar la clase de Hub con un `HubName` atributo, use ese nombre.

**Clase de Hub en servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

**Crear el proxy de cliente para la clase Hub**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

El objeto proxy es segura para subprocesos. De hecho, si se llama a `HubConnection.CreateHubProxy` varias veces con el mismo `hubName`, obtendrá la misma caché `IHubProxy` objeto.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Cómo definir métodos en el cliente que el servidor puede llamar a

Para definir un método que puede llamar el servidor, utilice el proxy `On` método para registrar un controlador de eventos.

Coincidencia de nombres de método distingue mayúsculas de minúsculas. Por ejemplo, `Clients.All.UpdateStockPrice` se ejecutará en el servidor `updateStockPrice`, `updatestockprice`, o `UpdateStockPrice` en el cliente.

Plataformas de cliente diferentes tienen requisitos diferentes para cómo escribir código del método para actualizar la interfaz de usuario. Los ejemplos mostrados son para los clientes de WinRT (Windows Store,. NET). Se proporcionan ejemplos de aplicación de consola, WPF y Silverlight en [una sección independiente más adelante en este tema](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Métodos sin parámetros

Si el método que se está administrando no tiene parámetros, use la sobrecarga no genérica de la `On` método:

**Código de servidor, llamar al método de cliente sin parámetros**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Código de cliente de WinRT para el método se llama desde el servidor sin parámetros ([ver ejemplos WPF y Silverlight más adelante en este tema](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Métodos con parámetros, especificando los tipos de parámetro

Si el método que está administrando tiene parámetros, especifique los tipos de los parámetros como los tipos genéricos de la `On` método. Hay sobrecargas genéricas de los `On` método para permitirle especificar parámetros de hasta 8 (4 en Windows Phone 7). En el ejemplo siguiente, se envía un parámetro a la `UpdateStockPrice` método.

**Llamar al método de cliente con un parámetro de código de servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**La clase de acción utilizada para el parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

**Código de cliente de WinRT para un método se llama desde el servidor con un parámetro ([ver ejemplos WPF y Silverlight más adelante en este tema](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Métodos con parámetros, que se especifiquen objetos dinámicos para los parámetros

Como alternativa a especificar parámetros como tipos genéricos de la `On` método, puede especificar parámetros como objetos dinámicos:

**Llamar al método de cliente con un parámetro de código de servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**La clase de acción utilizada para el parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

**Código de cliente de WinRT para un método se llama desde el servidor con un parámetro, con un objeto dinámico para el parámetro ([ver ejemplos WPF y Silverlight más adelante en este tema](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Cómo quitar un controlador

Para quitar un controlador, llame a su `Dispose` método.

**Código de cliente para un método llamado desde servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Al quitar el controlador de código de cliente**

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Cómo llamar a métodos del servidor desde el cliente

Para llamar a un método en el servidor, use el `Invoke` método en el proxy de concentrador.

Si el método de servidor no tiene ningún valor devuelto, use la sobrecarga no genérica de la `Invoke` método.

**Código de servidor para un método que no tiene ningún valor devuelto**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Código de cliente llama a un método que no tiene ningún valor devuelto**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Si el método de servidor tiene un valor devuelto, especifique el tipo de valor devuelto como el tipo genérico de la `Invoke` método.

**Código de servidor para un método que tiene un valor devuelto y toma un parámetro de tipo complejo**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**La clase de Stock utilizada para el parámetro y valor devuelto**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

**Código de cliente llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método asincrónico de ASP.NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Código de cliente llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método sincrónico**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

El `Invoke` método se ejecuta de forma asincrónica y devuelve un `Task` objeto. Si no se especifica `await` o `.Wait()`, la siguiente línea de código se ejecutará antes de que el método que invoca ha terminado de ejecutarse.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Cómo controlar eventos de duración de la conexión

SignalR proporciona la siguiente conexión de eventos de duración que puede controlar:

- `Received`: Se genera cuando se recibe ningún dato en la conexión. Proporciona los datos recibidos.
- `ConnectionSlow`: Se genera cuando el cliente detecta una conexión lenta o eliminación con frecuencia.
- `Reconnecting`: Se genera cuando comienza a volver a conectar el transporte subyacente.
- `Reconnected`: Se genera cuando se ha vuelto a conectar el transporte subyacente.
- `StateChanged`: Se genera cuando cambia el estado de conexión. Proporciona el estado antiguo y el estado nueva. Para obtener información acerca de la conexión que vea los valores de estado [enumeración ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Se genera cuando se ha desconectado la conexión.

Por ejemplo, si desea mostrar los mensajes de advertencia de errores que no son irrecuperables pero causará problemas de conexión intermitentes, como lentitud o frecuentes quitar de la conexión, controlar el `ConnectionSlow` eventos.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

Para obtener más información, consulte [comprensión y control de eventos de duración de conexión en SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Cómo controlar los errores

Si no habilita explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que SignalR devuelve después de un error contiene información mínima sobre el error. Por ejemplo, si una llamada a `newContosoChatMessage` se produce un error, el mensaje de error en el objeto de error contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" enviar mensajes de error detallados a los clientes de producción no se recomienda por motivos de seguridad, pero si desea permitir que los mensajes de error detallados para solucionar problemas, use el código siguiente en el servidor.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Para controlar los errores que genera SignalR, puede agregar un controlador para el `Error` evento en el objeto de conexión.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

Para controlar los errores de las invocaciones de método, encapsule el código en un bloque try-catch.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Cómo habilitar el registro del lado cliente

Para habilitar el registro del lado cliente, establezca el `TraceLevel` y `TraceWriter` las propiedades del objeto de conexión.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>Ejemplos de código de aplicación de consola para que el servidor puede llamar a métodos del cliente, Silverlight y WPF

Los ejemplos de código mostrados anteriormente para definir métodos de cliente que el servidor puede llamar a aplican a los clientes de WinRT. Los ejemplos siguientes muestran el código equivalente para WPF, Silverlight y los clientes de aplicación de consola.

### <a name="methods-without-parameters"></a>Métodos sin parámetros

**Código de cliente WPF para el método que se llama desde el servidor sin parámetros**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Código de cliente de Silverlight para el método que se llama desde el servidor sin parámetros**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Código de cliente de aplicación de consola para el método se llama desde el servidor sin parámetros**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Métodos con parámetros, especificando los tipos de parámetro

**Código de cliente WPF para un método llamado desde el servidor con un parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Código de cliente de Silverlight para un método llamado desde el servidor con un parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Código de cliente de aplicación de consola para un método se llama desde el servidor con un parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Métodos con parámetros, que se especifiquen objetos dinámicos para los parámetros

**Código de cliente WPF para un método llamado desde el servidor con un parámetro, con un objeto dinámico para el parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Código de cliente de Silverlight para un método llamado desde el servidor con un parámetro, con un objeto dinámico para el parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Código de cliente de aplicación de consola para un método se llama desde el servidor con un parámetro, con un objeto dinámico para el parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
