---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: Guía de API de ASP.NET SignalR Hubs - servidor (SignalR 1.x) | Microsoft Docs
author: bradygaster
description: Este documento proporciona una introducción a la programación del lado servidor de la API de concentradores de SignalR de ASP.NET para SignalR versión 1.1, con demonstratin de ejemplos de código...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: 8d544e81f87998581afb2a1228233b4d374ad70a
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837395"
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a>Guía de API de ASP.NET SignalR Hubs - servidor (SignalR 1.x)
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este documento proporciona una introducción a la programación del lado servidor de la API de concentradores de SignalR de ASP.NET para SignalR versión 1.1, con ejemplos de código que muestran las opciones comunes.
> 
> La API de concentradores de SignalR permite realizar llamadas a procedimiento remoto (RPC) de un servidor a los clientes conectados y de clientes en el servidor. En el código de servidor, definir los métodos que se pueden llamar a los clientes y llamar a métodos que se ejecutan en el cliente. En el código de cliente, definir los métodos que pueden llamarse desde el servidor y llamar a métodos que se ejecutan en el servidor. SignalR se ocupa de todos los mecanismos de cliente a servidor para usted.
> 
> SignalR también ofrece una API de nivel inferior denominada conexiones persistentes. Para obtener una introducción a SignalR, concentradores y conexiones persistentes, o para ver un tutorial que muestra cómo crear una aplicación de SignalR completa, consulte [SignalR - Introducción a](index.md).


## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [Cómo registrar la ruta de SignalR y configurar las opciones de SignalR](#route)

    - [La dirección URL de /signalr](#signalrurl)
    - [Configurar las opciones de SignalR](#options)
- [Cómo crear y usar clases de centro](#hubclass)

    - [Duración del objeto hub](#transience)
    - [Grafía Camel de nombres del centro en clientes de JavaScript](#hubnames)
    - [Varios centros](#multiplehubs)
- [Cómo definir métodos en la clase de concentrador que se pueden llamar los clientes](#hubmethods)

    - [Grafía Camel de nombres de métodos en los clientes de JavaScript](#methodnames)
    - [Cuándo se debe ejecutar de forma asincrónica](#asyncmethods)
    - [Definir sobrecargas](#overloads)
- [Cómo llamar a métodos de cliente de la clase Hub](#callfromhub)

    - [Selección de los clientes que recibirán la RPC](#selectingclients)
    - [Ninguna validación en tiempo de compilación para los nombres de método](#dynamicmethodnames)
    - [Coincidencia de nombres de método entre mayúsculas y minúsculas](#caseinsensitive)
    - [Ejecución asincrónica](#asyncclient)
- [Cómo administrar la pertenencia a grupos desde la clase Hub](#groupsfromhub)

    - [Ejecución asincrónica de los métodos Add y Remove](#asyncgroupmethods)
    - [Persistencia de pertenencia a grupo](#grouppersistence)
    - [Grupos de usuario único](#singleusergroups)
- [Cómo controlar eventos de duración de la conexión en la clase Hub](#connectionlifetime)

    - [Cuando se llama a OnConnected, OnDisconnected y OnReconnected](#onreconnected)
    - [Estado del llamador no rellenado](#nocallerstate)
- [Cómo obtener información sobre el cliente de la propiedad de contexto](#contextproperty)
- [Cómo pasar el estado entre los clientes y la clase Hub](#passstate)
- [Cómo controlar los errores en la clase Hub](#handleErrors)
- [Cómo llamar a los métodos de cliente y administrar grupos de fuera de la clase Hub](#callfromoutsidehub)

    - [Llamar a métodos de cliente](#callingclientsoutsidehub)
    - [Administrar la pertenencia a grupos](#managinggroupsoutsidehub)
- [Cómo habilitar el seguimiento](#tracing)
- [Cómo personalizar la canalización de concentradores](#hubpipeline)

Para obtener documentación sobre cómo los clientes del programa, consulte los siguientes recursos:

- [Guía de API de concentradores de SignalR: cliente JavaScript](index.md)
- [Guía de API de concentradores de SignalR: cliente .NET](index.md)

Son vínculos a temas de referencia de API a la versión 4.5 de .NET de la API. Si usa .NET 4, consulte [la versión 4 de .NET de los temas de la API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a>Cómo registrar la ruta de SignalR y configurar las opciones de SignalR

Para definir la ruta que los clientes usarán para conectarse a su centro, llame a la [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) método cuando se inicia la aplicación. `MapHubs` es un [método de extensión](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) para el `System.Web.Routing.RouteCollection` clase. El ejemplo siguiente muestra cómo definir la ruta de concentradores de SignalR en el *Global.asax* archivo.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

Si va a agregar funcionalidad de SignalR a una aplicación ASP.NET MVC, asegúrese de que la ruta de SignalR se agrega delante de las otras rutas. Para obtener más información, consulte [Tutorial: Introducción a SignalR y MVC 4](index.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>La dirección URL de /signalr

De forma predeterminada, es la dirección URL de ruta que los clientes usarán para conectarse a su centro de "/ signalr". (No confunda esta dirección URL con la dirección URL "/ signalr/hubs", que es para el archivo JavaScript generado automáticamente. Para obtener más información sobre el proxy generado, vea [Guía de la API SignalR Hubs: cliente JavaScript: el proxy generado y lo que hace por usted](index.md).)

Puede haber circunstancias excepcionales que hacen que esta dirección URL base no se puede usar para SignalR; Por ejemplo, tiene una carpeta en el proyecto denominado *signalr* y no desea cambiar el nombre. En ese caso, puede cambiar la dirección URL base, como se muestra en los ejemplos siguientes (reemplace "/ signalr" en el código de ejemplo con la dirección URL deseada).

**Código de servidor que especifica la dirección URL**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

**Código de cliente de JavaScript que especifica la dirección URL (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Código de cliente de JavaScript que especifica la dirección URL (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

**Código de cliente de .NET que especifica la dirección URL**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Configurar las opciones de SignalR

Las sobrecargas de los `MapHubs` método permiten especificar una dirección URL personalizada, una resolución de dependencia personalizadas y las opciones siguientes:

- Permitir llamadas entre dominios desde clientes de explorador.

    Normalmente, si el explorador carga una página de `http://contoso.com`, la conexión de SignalR está en el mismo dominio, en `http://contoso.com/signalr`. Si la página de `http://contoso.com` realiza una conexión a `http://fabrikam.com/signalr`, que es una conexión entre dominios. Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada. Para obtener más información, consulte [Guía de la API de ASP.NET SignalR Hubs: cliente JavaScript - cómo establecer una conexión entre dominios](index.md).
- Permitir que los mensajes de error detallado.

    Cuando se producen errores, el comportamiento predeterminado de SignalR es enviar a los clientes de un mensaje de notificación sin detalles sobre lo que sucedió. Enviar información detallada del error a los clientes no se recomienda en producción, porque es posible que los usuarios malintencionados pueda utilizar la información de los ataques contra la aplicación. Para solucionar el problema, puede usar esta opción para permitir temporalmente informes de error más informativo.
- Deshabilitar los archivos del proxy de JavaScript generados automáticamente.

    De forma predeterminada, se genera un archivo de JavaScript con servidores proxy para las clases de Hub en respuesta a la dirección URL "/ signalr/hubs". Si no desea usar a los servidores proxy de JavaScript, o si desea generar manualmente este archivo y hacer referencia a un archivo físico en los clientes, puede usar esta opción para deshabilitar la generación de proxy. Para obtener más información, consulte [Guía de la API SignalR Hubs: cliente JavaScript - creación de un archivo físico para SignalR genera proxy](index.md).

El ejemplo siguiente muestra cómo especificar la dirección URL de conexión de SignalR y estas opciones en una llamada a la `MapHubs` método. Para especificar una dirección URL personalizada, reemplace "/ signalr" en el ejemplo con la dirección URL que desea usar.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Cómo crear y usar clases de centro

Para crear un concentrador, cree una clase que derive de [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). El ejemplo siguiente muestra una clase de Hub simple para una aplicación de chat.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

En este ejemplo, puede llamar un cliente conectado la `NewContosoChatMessage` método, y al mismo tiempo, los datos recibidos se difunden a todos los clientes conectados.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Duración del objeto hub

No crear una instancia de la clase Hub o llamar a sus métodos desde su propio código en el servidor. todo lo que se hace automáticamente por la canalización de concentradores de SignalR. SignalR crea una nueva instancia de la clase Hub cada vez que necesita para controlar una operación de concentrador, como cuando un cliente se conecta, desconecta o realiza un llamada de método en el servidor.

Dado que las instancias de la clase Hub son transitorias, no se pueden utilizar para mantener el estado de una llamada al método a la siguiente. Cada vez que el servidor recibe una llamada al método desde un cliente, una nueva instancia de los procesos de la clase Hub el mensaje. Para mantener el estado a través de varias conexiones y las llamadas a métodos, utilizar algún otro método, como una base de datos o una variable estática en la clase Hub o una clase diferente que no se deriva de `Hub`. Si conservar datos en memoria, utilizando un método como una variable estática en la clase de Hub, los datos se perderán cuando se recicla el dominio de aplicación.

Si desea enviar mensajes a los clientes desde su propio código que se ejecuta fuera de la clase de Hub, no podrá hacerlo creando una instancia de la clase de concentrador, pero puede hacerlo mediante la obtención de una referencia al objeto de contexto de SignalR para la clase de concentrador. Para obtener más información, consulte [cómo llamar a los métodos de cliente y administrar grupos de fuera de la clase Hub](#callfromoutsidehub) más adelante en este tema.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Grafía Camel de nombres del centro en clientes de JavaScript

De forma predeterminada, los clientes de JavaScript consulte Hubs mediante el uso de una versión de mayúsculas y minúsculas camel del nombre de clase. SignalR realiza automáticamente este cambio para que el código JavaScript puede cumplir las convenciones de JavaScript. El ejemplo anterior podría denominarse `contosoChatHub` en código de JavaScript.

**Servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

**Cliente de JavaScript mediante el proxy generado**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

Si desea especificar un nombre diferente para los clientes usar, agregue el `HubName` atributo. Cuando se usa un `HubName` atributo, no hay ningún cambio de nombre en mayúsculas y minúsculas en los clientes de JavaScript.

**Servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

**Cliente de JavaScript mediante el proxy generado**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Varios centros

Puede definir varias clases de Hub en una aplicación. Al hacerlo, se comparte la conexión pero grupos son independientes:

- Todos los clientes usarán la misma dirección URL para establecer una conexión de SignalR con el servicio ("/ signalr" o la dirección URL personalizada si ha especificado uno), y que la conexión se utiliza para todos los concentradores definen por el servicio.

    No hay ninguna diferencia de rendimiento de varios centros en comparación con la definición de toda la funcionalidad de Hub en una sola clase.
- Todos los centros de obtención la misma información de la solicitud HTTP.

    Puesto que todos los concentradores comparten la misma conexión, la única información de la solicitud HTTP que obtiene el servidor es lo que viene en la solicitud HTTP original que establece la conexión de SignalR. Si la solicitud de conexión se usa para pasar información del cliente al servidor mediante la especificación de una cadena de consulta, no puede proporcionar las cadenas de consulta diferentes en centros diferentes. Todos los concentradores recibirán la misma información.
- El archivo de los servidores proxy de JavaScript generado contendrá a los servidores proxy para todos los centros de un archivo.

    Para obtener información sobre servidores proxy de JavaScript, consulte [Guía de la API SignalR Hubs: cliente JavaScript: el proxy generado y lo que hace por usted](index.md).
- Se definen grupos de concentradores.

    En SignalR, que puede definir grupos para difundir a los subconjuntos de clientes conectados con nombre. Los grupos se mantienen por separado para cada concentrador. Por ejemplo, un grupo llamado "Administradores" incluye un conjunto de clientes para su `ContosoChatHub` clase y el mismo nombre de grupo haría referencia a un conjunto diferente de los clientes para su `StockTickerHub` clase.

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Cómo definir métodos en la clase de concentrador que se pueden llamar los clientes

Para exponer un método en el centro que desea que se pueda llamar desde el cliente, declare un método público, tal como se muestra en los ejemplos siguientes.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Puede especificar un tipo de valor devuelto y parámetros, incluidos los tipos complejos y matrices, como lo haría en cualquier método de C#. Los datos que recibe en parámetros o devolver al llamador se comunican entre el cliente y el servidor mediante el uso de JSON, y SignalR controla el enlace de objetos complejos y matrices de objetos automáticamente.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Grafía Camel de nombres de métodos en los clientes de JavaScript

De forma predeterminada, los clientes de JavaScript hacen referencia a los métodos de concentrador mediante el uso de una versión de mayúsculas y minúsculas camel del nombre del método. SignalR realiza automáticamente este cambio para que el código JavaScript puede cumplir las convenciones de JavaScript.

**Servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Cliente de JavaScript mediante el proxy generado**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

Si desea especificar un nombre diferente para los clientes usar, agregue el `HubMethodName` atributo.

**Servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Cliente de JavaScript mediante el proxy generado**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Cuándo se debe ejecutar de forma asincrónica

Si el método se se larga o tiene que realizar el trabajo que implican la espera, por ejemplo, una búsqueda de la base de datos o una llamada de servicio web, convertir el método de concentrador a asincrónico devolviendo un [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (en lugar de `void` devolver) o [ Tarea&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) objeto (en lugar de `T` tipo de valor devuelto). Cuando vuelva un `Task` espera a que el objeto desde el método de SignalR el `Task` en completarse, y, a continuación, envía el resultado sin ajustar al cliente, por lo que no hay ninguna diferencia en la manera en que codifica la llamada al método en el cliente.

Hacer que un método de concentrador asincrónicas evita el bloqueo de la conexión cuando utiliza el transporte de WebSocket. Cuando se ejecuta sincrónicamente un método de concentrador y el transporte es WebSocket, las siguientes invocaciones de métodos en el centro del mismo cliente se bloquean hasta que se complete el método de concentrador.

El ejemplo siguiente se muestra el mismo método en el código para ejecutar de forma sincrónica o asincrónicamente, seguido por el código de cliente de JavaScript que funciona para llamar a cualquiera de las versiones.

**Sincrónica**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

**Asincrónica - ASP.NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Cliente de JavaScript mediante el proxy generado**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

Para obtener más información sobre cómo usar los métodos asincrónicos en ASP.NET 4.5, vea [utilizando los métodos asincrónicos en ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Definir sobrecargas

Si desea definir sobrecargas para un método, el número de parámetros en cada sobrecarga debe ser diferente. Si diferenciar una sobrecarga simplemente mediante la especificación de distintos tipos de parámetro, la clase de concentrador se compilarán, pero el servicio SignalR iniciará una excepción en tiempo de ejecución cuando los clientes intentan para llamar a una de las sobrecargas.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Cómo llamar a métodos de cliente de la clase Hub

Para llamar métodos de cliente desde el servidor, use el `Clients` propiedad en un método en la clase de concentrador. El ejemplo siguiente muestra código de servidor que llama a `addNewMessageToPage` en los clientes conectados todo y código de cliente que define el método en un cliente de JavaScript.

**Servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

**Cliente de JavaScript mediante el proxy generado**

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

No se puede obtener un valor devuelto de un método de cliente; sintaxis como `int x = Clients.All.add(1,1)` no funciona.

Puede especificar los tipos complejos y matrices para los parámetros. El siguiente ejemplo pasa un tipo complejo al cliente en un parámetro de método.

**Código de servidor que llama a un método de cliente mediante un objeto complejo**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

**Código de servidor que define el objeto complejo**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

**Cliente de JavaScript mediante el proxy generado**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Selección de los clientes que recibirán la RPC

La propiedad no devuelve los clientes un [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) objeto que proporciona varias opciones para especificar que los clientes recibirán la RPC:

- Todos los clientes conectados.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- Solo el cliente que realiza la llamada.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- Todos los clientes, excepto el cliente que realiza la llamada.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- Un cliente específico identificado por el identificador de conexión.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    Este ejemplo llama a `addContosoChatMessageToPage` en el cliente que realiza la llamada y tiene el mismo efecto que usar `Clients.Caller`.
- Todos los clientes conectados, excepto los clientes especificados, identificados por el identificador de conexión.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- Todos los clientes en un grupo especificado conectados.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- Todos los clientes conectados en un grupo especificado excepción los clientes especificados, identificados por el identificador de conexión.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- Todos los clientes conectados en un grupo especificado, excepto el cliente que realiza la llamada.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Ninguna validación en tiempo de compilación para los nombres de método

El nombre del método que especifique se interpreta como un objeto dinámico, lo que significa que hay ningún IntelliSense ni la validación en tiempo de compilación para él. La expresión se evalúa en tiempo de ejecución. Cuando se ejecuta la llamada al método, SignalR envía el nombre del método y los valores de parámetro al cliente y, si el cliente tiene un método que coincide con el nombre, que se llama al método y los valores de parámetros se pasan a él. Si no se encuentra ningún método coincidente en el cliente, se produce ningún error. Para obtener información sobre el formato de los datos que SignalR se transmite al cliente en segundo plano cuando se llama a un método de cliente, consulte [Introducción a SignalR](index.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Coincidencia de nombres de método entre mayúsculas y minúsculas

Coincidencia de nombres de método distingue mayúsculas de minúsculas. Por ejemplo, `Clients.All.addContosoChatMessageToPage` se ejecutará en el servidor `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, o `addContosoChatMessageToPage` en el cliente.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Ejecución asincrónica

El método al que llama se ejecuta de forma asincrónica. Cualquier código que viene después de una llamada de método a un cliente se ejecutará inmediatamente sin esperar a SignalR transmitir datos a los clientes a menos que especifique que las siguientes líneas de código deben esperar la finalización del método de fin. Los ejemplos de código siguiente se muestra cómo dos métodos de cliente se ejecutan secuencialmente, uno con el código que funciona en .NET Framework 4.5 y uno con el código que funciona en .NET 4.

**Ejemplo de .NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

**Ejemplo de .NET 4**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

Si usas `await` o `ContinueWith` para esperar hasta que un método de cliente finalice antes de que ejecute la siguiente línea de código, eso no significa que los clientes recibirán realmente el mensaje antes de que ejecute la siguiente línea de código. "Conclusión" de una llamada al método de cliente sólo significa que SignalR ha hecho todo lo necesario enviar el mensaje. Si necesita que los clientes reciben el mensaje de comprobación, deberá programar ese mecanismo usted mismo. Por ejemplo, podríamos codificar un `MessageReceived` método del concentrador y, en el `addContosoChatMessageToPage` método en el cliente puede llamar a `MessageReceived` después de hacer el trabajo que se debe hacer en el cliente. En `MessageReceived` en el concentrador puede hacer cualquier trabajo depende de la recepción de cliente real y el procesamiento de la llamada al método original.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Cómo usar una variable de cadena como el nombre del método

Si desea invocar un método de cliente mediante el uso de una variable de cadena como el nombre del método, convertir `Clients.All` (o `Clients.Others`, `Clients.Caller`, etc.) a `IClientProxy` y, a continuación, llame a [Invoke (methodName, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Cómo administrar la pertenencia a grupos desde la clase Hub

Los grupos en SignalR proporcionan un método para difundir mensajes a subconjuntos especificados de los clientes conectados. Un grupo puede tener cualquier número de clientes y un cliente puede ser un miembro de cualquier número de grupos.

Para administrar la pertenencia a grupos, use el [agregar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) y [quitar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) métodos proporcionados por el `Groups` propiedad de la clase de concentrador. El ejemplo siguiente se muestra el `Groups.Add` y `Groups.Remove` métodos utilizados en los métodos de concentrador que son llamados por código de cliente, seguido por el código de cliente de JavaScript que llama a ellos.

**Servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

**Cliente de JavaScript mediante el proxy generado**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

No tiene que crear explícitamente grupos. En efecto un grupo se crea automáticamente la primera vez que especifique su nombre en una llamada a `Groups.Add`, y se elimina cuando se quita la última conexión de la pertenencia a él.

No hay ninguna API para obtener una lista de miembros de grupo o una lista de grupos. SignalR envía mensajes a los clientes y grupos según un [modelo pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), y el servidor no mantiene las listas de grupos o pertenencias a grupos. Esto ayuda a maximizar la escalabilidad, ya que cada vez que agrega un nodo a una granja de servidores web, cualquier estado que mantiene SignalR se propaguen al nuevo nodo.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Ejecución asincrónica de los métodos Add y Remove

El `Groups.Add` y `Groups.Remove` métodos se ejecutan de forma asincrónica. Si desea agregar un cliente a un grupo y enviar inmediatamente un mensaje al cliente mediante el grupo, deberá asegurarse de que el `Groups.Add` método finaliza primero. Los ejemplos de código siguientes muestran cómo hacer esto, uno con un código que funciona en .NET 4.5 y uno mediante el uso de código que funciona en .NET 4

**Ejemplo de .NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

**Ejemplo de .NET 4**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Persistencia de pertenencia a grupo

SignalR realiza un seguimiento de las conexiones, no a los usuarios, por lo que si desea que un usuario esté en el mismo grupo cada vez que el usuario establece una conexión, se debe llamar a `Groups.Add` cada vez que el usuario establezca una conexión nueva.

Después de una pérdida temporal de conectividad, en ocasiones, SignalR puede restaurar la conexión automáticamente. En ese caso, SignalR está restaurando la misma conexión, sin establecer una conexión nueva y, por lo que se restaura automáticamente pertenencia a grupos de clientes. Esto es posible incluso cuando la interrupción temporal es el resultado de un reinicio del servidor o un error, porque el estado de conexión para cada cliente, incluida la pertenencia a grupos, es de ida y vuelta al cliente. Si un servidor deja de funcionar y se reemplaza por un nuevo servidor antes de la conexión agota el tiempo, un cliente puede volver a conectar automáticamente al nuevo servidor y volver a inscribirte en es un miembro de los grupos.

Cuando una conexión no se puede restaurar automáticamente después de una pérdida de conectividad, o cuando se agota el tiempo de espera de la conexión, o cuando se desconecta el cliente (por ejemplo, cuando un explorador se desplaza a una nueva página), se pierden las pertenencias a grupos. La próxima vez que se conecta el usuario será una nueva conexión. Para mantener las pertenencias a grupos cuando el mismo usuario establezca una conexión nueva, la aplicación tiene que realizar un seguimiento de las asociaciones entre usuarios y grupos y restaurar las pertenencias a grupos cada vez que un usuario establece una conexión nueva.

Para obtener más información sobre las conexiones y reconexiones, consulte [cómo controlar eventos de duración de la conexión en la clase Hub](#connectionlifetime) más adelante en este tema.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Grupos de usuario único

Las aplicaciones que usan SignalR normalmente tienen que realizar un seguimiento de las asociaciones entre usuarios y conexiones con el fin de saber qué usuario ha enviado un mensaje y qué usuarios deben recibir un mensaje. Los grupos se usan en uno de los dos patrones usados para hacerlo.

- Grupos de usuario único.

    Puede especificar el nombre de usuario como el nombre del grupo y agregar el identificador de conexión actual al grupo cada vez que el usuario se conecta o se vuelve a conectar. Para enviar mensajes al usuario enviar al grupo. Una desventaja de este método es que el grupo no le proporciona una manera de averiguar si el usuario está conectado o desconectado.
- Realizar un seguimiento de las asociaciones entre los nombres de usuario e identificadores de conexión.

    Puede almacenar una asociación entre cada nombre de usuario y una o más identificadores de conexión en un diccionario o una base de datos y actualizar los datos almacenados cada vez que el usuario se conecta o desconecta. Para enviar mensajes al usuario especificar los identificadores de conexión. Una desventaja de este método es que consume más memoria.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Cómo controlar eventos de duración de la conexión en la clase Hub

Algunos motivos habituales para controlar los eventos de duración de conexión son para realizar un seguimiento de si un usuario está conectado o no y realizar un seguimiento de la asociación entre los nombres de usuario e identificadores de conexión. Para ejecutar su propio código cuando los clientes conexión o desconexión, invalidar el `OnConnected`, `OnDisconnected`, y `OnReconnected` métodos virtuales del centro de la clase, como se muestra en el ejemplo siguiente.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Cuando se llama a OnConnected, OnDisconnected y OnReconnected

Cada vez que un explorador se desplaza a una página nueva, una nueva conexión tiene que establecerse, lo que significa que se ejecutará SignalR el `OnDisconnected` método seguido por el `OnConnected` método. SignalR crea siempre un nuevo identificador de conexión cuando se establece una conexión nueva.

El `OnReconnected` se llama al método cuando se ha producido una interrupción temporal de conectividad que SignalR puede recuperarse automáticamente, por ejemplo, cuando un cable está temporalmente desconectado y vuelto a conectar antes de la conexión agota el tiempo. El `OnDisconnected` se llama al método cuando el cliente se desconecta y SignalR no se puede volver a conectar automáticamente, por ejemplo, cuando un explorador navega a una página nueva. Por lo tanto, es una secuencia de eventos de un cliente determinado posible `OnConnected`, `OnReconnected`, `OnDisconnected`; o `OnConnected`, `OnDisconnected`. No verá la secuencia `OnConnected`, `OnDisconnected`, `OnReconnected` para una conexión determinada.

El `OnDisconnected` método que no se llama en algunos escenarios, como cuando un servidor deja de funcionar u obtiene Reciclando el dominio de aplicación. Cuando otro servidor que se incluye en la línea o el dominio de aplicación se completa su reciclaje, algunos clientes pueden que pueda volver a conectar y activar el `OnReconnected` eventos.

Para obtener más información, consulte [comprensión y control de eventos de duración de conexión en SignalR](index.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Estado del llamador no rellenado

Los métodos de controlador de eventos de duración de conexión se llaman desde el servidor, lo que significa que cualquier estado que se coloca en el `state` objeto en el cliente no se rellenarán en la `Caller` propiedad en el servidor. Para obtener información sobre la `state` objeto y el `Caller` propiedad, vea [cómo pasar el estado entre los clientes y la clase Hub](#passstate) más adelante en este tema.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Cómo obtener información sobre el cliente de la propiedad de contexto

Para obtener información sobre el cliente, use el `Context` propiedad de la clase de concentrador. El `Context` propiedad devuelve un [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) objeto que proporciona acceso a la siguiente información:

- El identificador de conexión del cliente que realiza la llamada.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    El identificador de conexión es un GUID asignado por SignalR (no se puede especificar el valor en su propio código). Hay un identificador de conexión para cada conexión y la misma conexión de que identificador es usado por todos los concentradores si tiene varios centros en la aplicación.
- Datos de encabezado HTTP.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    También puede obtener los encabezados HTTP desde `Context.Headers`. El motivo para varias referencias a la misma cosa es que `Context.Headers` se crean en primer lugar, el `Context.Request` propiedad la agregó más tarde, y `Context.Headers` se conserva por compatibilidad con versiones anteriores.
- Datos de cadena de consulta.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    También puede obtener datos de cadena de consulta de `Context.QueryString`.

    La cadena de consulta que se obtiene en esta propiedad es el que se usó con la solicitud HTTP que se estableció la conexión de SignalR. Puede agregar parámetros de cadena de consulta en el cliente mediante la configuración de la conexión, que es una manera cómoda para pasar datos sobre el cliente desde el cliente al servidor. El ejemplo siguiente muestra una forma de agregar una cadena de consulta en un cliente de JavaScript cuando se usa el proxy generado.

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    Para obtener más información sobre cómo establecer parámetros de cadena de consulta, consulte las guías de API para el [JavaScript](index.md) y [.NET](index.md) los clientes.

    Puede encontrar el método de transporte utilizado para la conexión en los datos de cadena de consulta, junto con algunos otros valores que se usa internamente SignalR:

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    El valor de `transportMethod` será "webSockets", "serverSentEvents", "foreverFrame" o "longPolling". Tenga en cuenta que si comprobar este valor el `OnConnected` método de controlador de eventos, en algunos escenarios inicialmente podría obtener un valor de transporte que no es el método de transporte negociada final para la conexión. En ese caso, el método producirá una excepción y se llamará de nuevo más tarde cuando se establece el modo de transporte final.
- Cookies.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    También puede obtener las cookies de `Context.RequestCookies`.
- Información del usuario.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- El objeto HttpContext para la solicitud:

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    Use este método en lugar de obtener `HttpContext.Current` para obtener el `HttpContext` objeto para la conexión de SignalR.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Cómo pasar el estado entre los clientes y la clase Hub

El proxy de cliente proporciona un `state` objeto en el que puede almacenar datos que desea que se transmite al servidor con cada llamada al método. En el servidor puede tener acceso a estos datos en el `Clients.Caller` propiedad en los métodos de concentrador que lo llaman los clientes. El `Clients.Caller` propiedad no se rellena para los métodos de controlador de eventos de duración de conexión `OnConnected`, `OnDisconnected`, y `OnReconnected`.

Crear o actualizar datos en el `state` objeto y el `Clients.Caller` propiedad funciona en ambas direcciones. Puede actualizar valores en el servidor y se pasan al cliente.

El ejemplo siguiente muestra código de cliente de JavaScript que almacena el estado para la transmisión al servidor con cada llamada al método.

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

El ejemplo siguiente muestra el código equivalente en un cliente. NET.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

En la clase de Hub, puede tener acceso a estos datos en el `Clients.Caller` propiedad. El ejemplo siguiente muestra código que recupera el estado mencionado en el ejemplo anterior.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> Este mecanismo para conservar el estado no está diseñada para grandes cantidades de datos, desde todo lo coloca en el `state` o `Clients.Caller` propiedad es de ida y vuelta con cada invocación del método. Es útil para los elementos más pequeños, como los nombres de usuario o los contadores.


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Cómo controlar los errores en la clase Hub

Para controlar los errores que se producen en los métodos de clase de Hub, use uno o ambos de los métodos siguientes:

- Ajustar el código del método en bloques try-catch y registrar el objeto de excepción. Con fines de depuración puede enviar la excepción al cliente, pero para la seguridad no se recomienda motivos enviar información detallada a los clientes en producción.
- Crear un módulo de canalización de concentradores controla la [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) método. El ejemplo siguiente muestra un módulo de la canalización que registra los errores, seguidos por el código en Global.asax que inserta el módulo en la canalización de concentradores.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

Para obtener más información acerca de los módulos de la canalización de concentrador, consulte [cómo personalizar la canalización de concentradores](#hubpipeline) más adelante en este tema.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Cómo habilitar el seguimiento

Para habilitar el seguimiento de servidor, agregue un elemento de system.diagnostics al archivo Web.config, tal como se muestra en este ejemplo:

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

Al ejecutar la aplicación en Visual Studio, puede ver los registros en el **salida** ventana.

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Cómo llamar a los métodos de cliente y administrar grupos de fuera de la clase Hub

Para llamar al cliente métodos de una clase diferente que la clase de Hub, obtener una referencia al objeto de contexto de SignalR para el concentrador y usarlo para llamar a métodos en el cliente o administración de grupos de.

El ejemplo siguiente `StockTicker` clase obtiene el objeto de contexto, lo almacena en una instancia de la clase, almacena la instancia de clase en una propiedad estática y usa el contexto de la instancia de la clase singleton para llamar a la `updateStockPrice` método en los clientes que están conectado a un concentrador denominado `StockTickerHub`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

Si tiene que usar el contexto varias veces en un objeto de larga duración, obtener la referencia una vez y guardar, en lugar de obtenerlo nuevo cada vez. Obtener el contexto de una vez garantiza que SignalR envía mensajes a los clientes en la misma secuencia en la que los métodos de concentrador realizan a cliente las invocaciones de método. Para ver un tutorial que muestra cómo usar el contexto de SignalR para un concentrador, consulte [difusión de servidores con ASP.NET SignalR](index.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Llamar a métodos de cliente

Puede especificar que los clientes recibirán la RPC, pero tiene menos opciones que cuando se llama a partir de una clase de concentrador. La razón de esto es que el contexto no está asociado con una determinada llamada desde un cliente, por lo que todos los métodos que requieren conocer el identificador de conexión actual, como `Clients.Others`, o `Clients.Caller`, o `Clients.OthersInGroup`, no están disponibles. Están disponibles las siguientes opciones:

- Todos los clientes conectados.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- Un cliente específico identificado por el identificador de conexión.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- Todos los clientes conectados, excepto los clientes especificados, identificados por el identificador de conexión.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- Todos los clientes en un grupo especificado conectados.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- Clientes conectados todo en un grupo especificado, excepto los clientes especificados, identificado por el identificador de conexión.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

Si se llama en la clase que no sea centro de métodos en la clase de Hub, puede pasar el identificador de conexión actual y úsela con `Clients.Client`, `Clients.AllExcept`, o `Clients.Group` para simular `Clients.Caller`, `Clients.Others`, o `Clients.OthersInGroup`. En el ejemplo siguiente, la `MoveShapeHub` clase pasa el identificador de conexión para el `Broadcaster` clase para que la `Broadcaster` puede simular la clase `Clients.Others`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Administrar la pertenencia a grupos

Para administrar grupos tienen las mismas opciones como se hace en una clase de Hub.

- Agregar a un cliente a un grupo

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- Quitar a un cliente de un grupo

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Cómo personalizar la canalización de concentradores

SignalR le permite insertar su propio código en la canalización de concentrador. El ejemplo siguiente muestra un módulo personalizado de canalización de concentrador que registra cada llamada de método entrante recibido desde el cliente y la llamada al método salientes que se invoca en el cliente:

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

El siguiente código en el *Global.asax* archivo registra el módulo se ejecuten en la canalización de concentrador:

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

Hay muchos métodos diferentes que puede invalidar. Para obtener una lista completa, consulte [HubPipelineModule métodos](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
