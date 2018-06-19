---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Guía de API de bases de datos centrales de ASP.NET SignalR - Server (C#) | Documentos de Microsoft
author: pfletcher
description: Este documento proporciona una introducción a la programación del lado del servidor de la API de concentradores de SignalR de ASP.NET para SignalR versión 2, con ejemplos de código que muestran...
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
ms.locfileid: "28039214"
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a>Guía de API de bases de datos centrales de ASP.NET SignalR - Server (C#)
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Este documento proporciona una introducción a la programación del lado del servidor de la API de concentradores de SignalR de ASP.NET para SignalR versión 2, con ejemplos de código que muestran las opciones habituales.
> 
> La API de concentradores de SignalR permite realizar llamadas a procedimiento remoto (RPC) desde un servidor a los clientes conectados y desde clientes en el servidor. En el código de servidor, definir métodos que puedan llamar los clientes y llamar a métodos que se ejecutan en el cliente. En el código de cliente, definir métodos que se pueda llamar desde el servidor y llamar a métodos que se ejecutan en el servidor. SignalR se encarga de todas la mecánica de cliente a servidor para usted.
> 
> SignalR también ofrece una API de nivel inferior denominada conexiones persistentes. Para obtener una introducción a SignalR, concentradores y conexiones persistentes, consulte [Introducción a 2 SignalR](../getting-started/introduction-to-signalr.md).
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
> ## <a name="topic-versions"></a>Versiones de tema
> 
> Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [Cómo registrar el middleware de SignalR](#route)

    - [La dirección URL /signalr](#signalrurl)
    - [Configurar las opciones de SignalR](#options)
- [Cómo crear y utilizar clases de base de datos central](#hubclass)

    - [Duración de objetos de base de datos central](#transience)
    - [Grafía Camel de nombres de base de datos central en clientes de JavaScript](#hubnames)
    - [Varios centros](#multiplehubs)
    - [Concentradores fuertemente tipados](#stronglytypedhubs)
- [Cómo definir métodos en la clase de base de datos central que pueden llamar los clientes](#hubmethods)

    - [Grafía Camel de nombres de método en clientes de JavaScript](#methodnames)
    - [Cuándo se debe ejecutar de forma asincrónica](#asyncmethods)
    - [Definir sobrecargas](#overloads)
    - [Notificar el progreso de las invocaciones de método de concentrador](#progress)
- [Cómo llamar a métodos de cliente de la clase de base de datos central](#callfromhub)

    - [Al seleccionar a los clientes que recibirán la RPC](#selectingclients)
    - [Ninguna validación en tiempo de compilación para los nombres de método](#dynamicmethodnames)
    - [Coincidencia de nombres de método entre mayúsculas y minúsculas](#caseinsensitive)
    - [Ejecución asincrónica](#asyncclient)
- [Cómo administrar la pertenencia a grupos de la clase de base de datos central](#groupsfromhub)

    - [Ejecución asincrónica de métodos Add y Remove](#asyncgroupmethods)
    - [Persistencia de pertenencia a grupo](#grouppersistence)
    - [Grupos de usuario único](#singleusergroups)
- [Cómo controlar los eventos de duración de conexión en la clase de base de datos central](#connectionlifetime)

    - [Cuando se llama a OnConnected, OnDisconnected y OnReconnected](#onreconnected)
    - [Estado del llamador no rellenado](#nocallerstate)
- [Cómo obtener información sobre el cliente de la propiedad de contexto](#contextproperty)
- [Cómo pasar información de estado entre los clientes y la clase de base de datos central](#passstate)
- [Cómo controlar los errores en la clase de base de datos central](#handleErrors)
- [Cómo llamar a los métodos de cliente y administrar grupos desde fuera de la clase de base de datos central](#callfromoutsidehub)

    - [Llamar a métodos de cliente](#callingclientsoutsidehub)
    - [Administrar la pertenencia a grupos](#managinggroupsoutsidehub)
- [Cómo habilitar el seguimiento](#tracing)
- [Cómo personalizar la canalización de concentradores](#hubpipeline)

Para obtener documentación sobre cómo los clientes del programa, consulte los siguientes recursos:

- [Guía de API de concentradores de SignalR - cliente de JavaScript](hubs-api-guide-javascript-client.md)
- [Guía de API de concentradores de SignalR - cliente .NET](hubs-api-guide-net-client.md)

Los componentes de servidor de SignalR 2 solo están disponibles en .NET 4.5. Servidores que ejecutan .NET 4.0 deben usar v1.x de SignalR.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Cómo registrar el middleware de SignalR

Para definir la ruta que los clientes usarán para conectar con el centro, llame a la `MapSignalR` método cuando se inicia la aplicación. `MapSignalR`es un [método de extensión](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) para el `OwinExtensions` clase. En el ejemplo siguiente se muestra cómo definir la ruta de los concentradores SignalR utilizando una clase de inicio OWIN.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

Si va a agregar funcionalidad de SignalR a una aplicación de ASP.NET MVC, asegúrese de que se ha agregado la ruta de SignalR antes de las otras rutas. Para obtener más información, consulte [Tutorial: Introducción a SignalR 2 y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>La dirección URL /signalr

De forma predeterminada, es la dirección URL de la ruta que los clientes usarán para conectar con el centro "/ signalr". (No confunda esta dirección URL con la dirección URL "/ signalr/concentradores", que es para el archivo JavaScript generado automáticamente. Para obtener más información sobre el proxy generado, consulte [Guía de API de concentradores de SignalR - cliente JavaScript - el proxy generado y lo que hace que](hubs-api-guide-javascript-client.md#genproxy).)

Puede haber circunstancias extraordinarias que hacen que esta dirección URL base no se puede usar para SignalR; Por ejemplo, tiene una carpeta en el proyecto denominado *signalr* y no desea cambiar el nombre. En ese caso, puede cambiar la dirección URL base, como se muestra en los ejemplos siguientes (reemplazar "/ signalr" en el código de ejemplo con su dirección URL deseado).

**Código de servidor que especifica la dirección URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**Código de cliente de JavaScript que especifica la dirección URL (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**Código de cliente de JavaScript que especifica la dirección URL (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Código de cliente de .NET que especifica la dirección URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Configurar las opciones de SignalR

Las sobrecargas de los `MapSignalR` método permiten especificar una dirección URL personalizada, una resolución de dependencia personalizados y las opciones siguientes:

- Habilitar las llamadas entre dominios mediante CORS o JSONP los clientes de explorador.

    Por lo general si el explorador carga una página de `http://contoso.com`, la conexión de SignalR está en el mismo dominio, en `http://contoso.com/signalr`. Si la página de `http://contoso.com` realiza una conexión a `http://fabrikam.com/signalr`, que es una conexión entre dominios. Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada. Para obtener más información, consulte [Guía de API de concentradores ASP.NET SignalR - cliente JavaScript - cómo establecer una conexión entre dominios](hubs-api-guide-javascript-client.md#crossdomain).
- Habilitar mensajes de error detallados.

    Cuando se producen errores, el comportamiento predeterminado de SignalR es enviar a los clientes un mensaje de notificación sin detalles sobre lo que ocurrió. Enviar información detallada del error a los clientes no se recomienda en producción, porque los usuarios malintencionados podrían llevar a la utilice la información de los ataques contra la aplicación. Para solucionar problemas, puede usar esta opción para habilitar temporalmente informes de error más informativo.
- Deshabilite generados automáticamente archivos de proxy de JavaScript.

    De forma predeterminada, se genera un archivo de JavaScript con servidores proxy para las clases de base de datos central en respuesta a la dirección URL "/ signalr/concentradores". Si no desea usar a los servidores proxy de JavaScript, o si desea generar este archivo manualmente y hacer referencia a un archivo físico en los clientes, puede usar esta opción para deshabilitar la generación de proxy. Para obtener más información, consulte [Guía de API de concentradores de SignalR - cliente JavaScript - cómo crear un archivo físico para SignalR genera proxy](hubs-api-guide-javascript-client.md#manualproxy).

En el ejemplo siguiente se muestra cómo especificar la dirección URL de conexión de SignalR y estas opciones en una llamada a la `MapSignalR` método. Para especificar una dirección URL personalizada, reemplace "/ signalr" en el ejemplo con la dirección URL que se va a utilizar.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Cómo crear y utilizar clases de base de datos central

Para crear un concentrador, cree una clase que deriva de [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). En el ejemplo siguiente se muestra una clase simple de concentrador para una aplicación de chat.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

En este ejemplo, puede llamar un cliente conectado el `NewContosoChatMessage` método, y cuando lo hace, los datos recibidos se difunden a todos los clientes conectados.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Duración de objetos de base de datos central

No crear una instancia de la clase de concentrador o llamar a sus métodos desde su propio código en el servidor; todo lo que es necesario encargarse de la canalización de concentradores de SignalR. SignalR crea una nueva instancia de la clase de base de datos central cada vez que necesita para administrar una operación de base de datos central, como cuando un cliente se conecta, desconecta o realiza un método de llamada al servidor.

Dado que las instancias de la clase de base de datos central son transitorias, no puede usarlos para mantener el estado de una llamada al método a la siguiente. Cada vez que el servidor recibe una llamada al método desde un cliente, una nueva instancia de los procesos de clase de base de datos central el mensaje. Para mantener el estado a través de varias conexiones y llamadas a métodos, utilizar algún otro método, como una base de datos o una variable estática en la clase de base de datos central, o en una clase diferente que no se deriva de `Hub`. Si conservar los datos en memoria, utilizando un método como una variable estática en la clase base de datos central, los datos se perderán cuando se recicla el dominio de aplicación.

Si desea enviar mensajes a los clientes desde su propio código que se ejecuta fuera de la clase de base de datos central, no puede hacerlo creando una instancia de la clase de base de datos central, pero puede hacerlo mediante la obtención de una referencia al objeto de contexto de SignalR para la clase de base de datos central. Para obtener más información, consulte [cómo llamar a los métodos de cliente y administrar grupos desde fuera de la clase de base de datos central](#callfromoutsidehub) más adelante en este tema.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Grafía Camel de nombres de base de datos central en clientes de JavaScript

De forma predeterminada, los clientes de JavaScript hacen referencia a los concentradores mediante una versión de grafía de camel del nombre de clase. SignalR automáticamente realiza este cambio para que el código de JavaScript puede ajustarse a las convenciones de JavaScript. El ejemplo anterior se denominará `contosoChatHub` en código de JavaScript.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**Cliente de JavaScript mediante el proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Si desea especificar un nombre diferente para que los clientes, agregue el `HubName` atributo. Cuando se usa un `HubName` atributo, no hay ningún cambio de nombre en mayúsculas y minúsculas camel en los clientes de JavaScript.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**Cliente de JavaScript mediante el proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Varios centros

Puede definir varias clases de base de datos central en una aplicación. Al hacerlo, se comparte la conexión, pero los grupos son independientes:

- Todos los clientes usarán la misma dirección URL para establecer una conexión SignalR con su servicio ("/ signalr" o la dirección URL personalizada si ha especificado uno), y que se utiliza la conexión para todos los centros definen por el servicio.

    No hay ninguna diferencia de rendimiento para varios centros en comparación a la definición de toda la funcionalidad de base de datos central en una sola clase.
- Todos los concentradores de obtener la misma información de la solicitud HTTP.

    Puesto que todos los concentradores comparten la misma conexión, la única información de la solicitud HTTP que el servidor obtiene es qué se incluye en la solicitud HTTP original que establece la conexión de SignalR. Si usas la solicitud de conexión para pasar información del cliente al servidor mediante la especificación de una cadena de consulta, no puede proporcionar las cadenas de consulta diferentes a los concentradores diferentes. Todos los concentradores recibirá la misma información.
- El archivo de proxy de JavaScript generado contendrá a todos los centros de un archivo de los servidores proxy.

    Para obtener información sobre servidores proxy de JavaScript, consulte [Guía de API de concentradores de SignalR - cliente JavaScript - el proxy generado y lo que hace que](hubs-api-guide-javascript-client.md#genproxy).
- Se definen grupos de concentradores.

    En SignalR que puede definir grupos para difundirlo a los subconjuntos de clientes conectados con nombre. Los grupos se mantienen por separado para cada concentrador. Por ejemplo, un grupo llamado "Administradores" incluye un conjunto de clientes para su `ContosoChatHub` clase y el mismo nombre de grupo haría referencia a un conjunto diferente de los clientes de su `StockTickerHub` clase.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Concentradores fuertemente tipados

Para definir una interfaz para los métodos de concentrador que el cliente puede referencia (y habilitar Intellisense en los métodos de concentrador), derivar el centro de `Hub<T>` (introducida en SignalR 2.1) en lugar de `Hub`:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Cómo definir métodos en la clase de base de datos central que pueden llamar los clientes

Para exponer un método en el concentrador al que desea que se pueda llamar desde el cliente, declarar un método público, tal y como se muestra en los ejemplos siguientes.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Puede especificar un tipo de valor devuelto y parámetros, incluidos los tipos complejos y matrices, como lo haría en cualquier método de C#. Los datos que recibe en parámetros o devolver al llamador se comunican entre el cliente y el servidor mediante el uso de JSON y SignalR controla el enlace de objetos complejos y matrices de objetos automáticamente.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Grafía Camel de nombres de método en clientes de JavaScript

De forma predeterminada, los clientes de JavaScript hacen referencia a los métodos de concentrador mediante una versión de grafía de camel del nombre del método. SignalR automáticamente realiza este cambio para que el código de JavaScript puede ajustarse a las convenciones de JavaScript.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Cliente de JavaScript mediante el proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Si desea especificar un nombre diferente para que los clientes, agregue el `HubMethodName` atributo.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Cliente de JavaScript mediante el proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Cuándo se debe ejecutar de forma asincrónica

Si el método se puede o de larga ejecución tiene que trabajar que implican espera, por ejemplo, una búsqueda de la base de datos o una llamada de servicio web, hacer que el método de concentrador asincrónica devolviendo un [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (en lugar de `void` devolver) o [ Tarea&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) objeto (en lugar de `T` tipo de valor devuelto). Cuando vuelva un `Task` objeto desde el método de SignalR espera el `Task` en completarse, y, a continuación, envía el resultado sin ajustar al cliente, así que no hay ninguna diferencia en cómo código la llamada al método en el cliente.

Hacer que un método de concentrador asincrónica evita bloqueando la conexión cuando se utiliza el transporte de WebSocket. Cuando un método de concentrador se ejecuta de forma sincrónica y el transporte es WebSocket, las siguientes llamadas a métodos en el concentrador del mismo cliente se bloquean hasta que se complete el método de concentrador.

El ejemplo siguiente se muestra el mismo método en el código para ejecutar de forma sincrónica o asincrónica, seguido por el código de cliente de JavaScript que funciona para llamar a cualquiera de las versiones.

**Sincrónico**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Asincrónica**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Cliente de JavaScript mediante el proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Para obtener más información sobre cómo usar métodos asincrónicos en 4.5 de ASP.NET, vea [mediante los métodos asincrónicos en ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Definir sobrecargas

Si desea definir sobrecargas para un método, el número de parámetros en cada sobrecarga debe ser diferente. Si es posible diferenciar una sobrecarga simplemente mediante la especificación de distintos tipos de parámetro, la clase de base de datos central se compilará, pero el servicio de SignalR iniciará una excepción en tiempo de ejecución cuando los clientes intentan para llamar a una de las sobrecargas.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Notificar el progreso de las invocaciones de método de concentrador

SignalR 2.1 agrega compatibilidad para la [patrón para informar del progreso](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introducidas en .NET 4.5. Para implementar informes de progreso, defina un `IProgress<T>` parámetro para el método de concentrador que pueda acceder el cliente:

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Al escribir un método de servidor de ejecución prolongada, es importante que use un modelo de programación asincrónico como asincrónicas / Await en lugar de bloquear el subproceso de la base de datos central.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Cómo llamar a métodos de cliente de la clase de base de datos central

Para llamar métodos de cliente desde el servidor, utilice el `Clients` propiedad en un método en la clase de base de datos central. En el ejemplo siguiente se muestra el código de servidor que llame `addNewMessageToPage` en todos los clientes y el código de cliente que define el método en un cliente de JavaScript.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

**Cliente de JavaScript mediante el proxy generado**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

No se puede obtener un valor devuelto de un método de cliente; sintaxis como `int x = Clients.All.add(1,1)` no funciona.

Puede especificar los tipos complejos y matrices para los parámetros. En el ejemplo siguiente se pasa un tipo complejo al cliente en un parámetro de método.

**Código de servidor que llama a un método de cliente mediante un objeto complejo**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Código de servidor que define el objeto complejo**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**Cliente de JavaScript mediante el proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Al seleccionar a los clientes que recibirán la RPC

La propiedad no devuelve los clientes un [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) objeto que proporciona varias opciones para especificar que los clientes recibirán la RPC:

- Todos los clientes conectados.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- Solo el cliente que realiza la llamada.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- Todos los clientes, excepto el cliente que realiza la llamada.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- Un cliente específico identificado por el identificador de conexión.

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    Este ejemplo llama `addContosoChatMessageToPage` en el cliente que realiza la llamada y tiene el mismo efecto que usar `Clients.Caller`.
- Todos los clientes conectados excepción los clientes especificados, identificados por el identificador de conexión.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Todos los clientes en un grupo especificado conectados.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Todos los clientes conectados en un grupo especificado, excepto los clientes especificados, identificados por el identificador de conexión.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Todos los clientes conectados en un grupo especificado, excepto el cliente que realiza la llamada.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Un usuario específico, identificado por el identificador de usuario.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    De forma predeterminada, es `IPrincipal.Identity.Name`, pero se puede cambiar [registrar una implementación de IUserIdProvider con el host global](mapping-users-to-connections.md#IUserIdProvider).
- Todos los clientes y grupos de una lista de identificadores de conexión.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Una lista de grupos.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Un usuario por su nombre.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Una lista de nombres de usuario (introducida en SignalR 2.1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Ninguna validación en tiempo de compilación para los nombres de método

El nombre del método que especifique se interpreta como un objeto dinámico, lo que significa que no hay ningún IntelliSense o validación en tiempo de compilación para él. La expresión se evalúa en tiempo de ejecución. Cuando se ejecuta la llamada al método, SignalR envía el nombre del método y los valores de parámetro para el cliente, y si el cliente tiene un método que coincide con el nombre, que se llama el método y los valores de parámetro se pasan a él. Si no se encuentra ningún método coincidente en el cliente, se produce ningún error. Para obtener información sobre el formato de los datos que SignalR se transmite al cliente en segundo plano cuando se llama a un método de cliente, consulte [Introducción a SignalR](../getting-started/introduction-to-signalr.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Coincidencia de nombres de método entre mayúsculas y minúsculas

Coincidencia de nombres de método distingue mayúsculas de minúsculas. Por ejemplo, `Clients.All.addContosoChatMessageToPage` se ejecutará en el servidor `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, o `addContosoChatMessageToPage` en el cliente.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Ejecución asincrónica

El método que se llama a ejecuta de forma asincrónica. Cualquier código que viene después de una llamada de método a un cliente se ejecutará inmediatamente sin tener que esperar para que SignalR finalizar la transmisión de datos a los clientes a menos que especifique que las siguientes líneas de código deben esperar la finalización del método. En el ejemplo de código siguiente se muestra cómo dos métodos de cliente se ejecutan secuencialmente.

**Usar Await (.NET 4.5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Si usa `await` para esperar hasta que un método de cliente finalice antes de que se ejecuta la siguiente línea de código, esto no significa que los clientes realmente recibirá el mensaje antes de que se ejecuta la siguiente línea de código. "Completar" de una llamada al método de cliente sólo significa que ha hecho SignalR todo lo necesario para enviar el mensaje. Si necesita que los clientes reciben el mensaje de comprobación, tendrá que programar ese mecanismo usted mismo. Por ejemplo, puede codificar un `MessageReceived` método del concentrador y en el `addContosoChatMessageToPage` método en el cliente puede llamar a `MessageReceived` después de trabajo necesita hacer en el cliente. En `MessageReceived` en el concentrador se puede realizar cualquier trabajo depende de la recepción de cliente real y el procesamiento de la llamada al método original.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Cómo utilizar una variable de cadena como el nombre del método

Si desea invocar un método de cliente mediante el uso de una variable de cadena como el nombre del método, convierte `Clients.All` (o `Clients.Others`, `Clients.Caller`, etc.) a `IClientProxy` y, a continuación, llame a [Invoke (methodName, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Cómo administrar la pertenencia a grupos de la clase de base de datos central

Grupos de SignalR proporcionan un método para difundir mensajes a subconjuntos especificados de los clientes conectados. Un grupo puede tener cualquier número de clientes y un cliente puede ser un miembro de cualquier número de grupos.

Para administrar la pertenencia a grupos, use la [agregar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) y [quitar](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) métodos proporcionados por el `Groups` propiedad de la clase de base de datos central. El siguiente ejemplo se muestra la `Groups.Add` y `Groups.Remove` métodos utilizados en los métodos de concentrador que se llaman al código de cliente, seguido por el código de cliente de JavaScript que llama.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**Cliente de JavaScript mediante el proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

No tienes que crear explícitamente grupos. En vigor un grupo se crea automáticamente la primera vez que especifique su nombre en una llamada a `Groups.Add`, y se elimina cuando se quita la última conexión de pertenencia en ella.

No hay ninguna API para obtener una lista de pertenencia a grupos o una lista de grupos. SignalR envía mensajes a los clientes y grupos basados en un [modelo pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), y el servidor no mantiene las listas de grupos o las pertenencias a grupos. Esto ayuda a maximizar la escalabilidad, ya que cada vez que agregue un nodo a una granja de servidores web, cualquier estado que mantiene SignalR debe se propaguen en el nuevo nodo.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Ejecución asincrónica de métodos Add y Remove

El `Groups.Add` y `Groups.Remove` métodos ejecutan de forma asincrónica. Si desea agregar un cliente a un grupo y enviar inmediatamente un mensaje al cliente mediante el grupo, se tiene que asegurarse de que el `Groups.Add` método finaliza primero. En el ejemplo de código siguiente se muestra cómo hacerlo.

**Agregar a un cliente a un grupo y, a continuación, ese cliente de mensajería**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Persistencia de pertenencia a grupo

SignalR realiza un seguimiento de las conexiones, no a los usuarios, por lo que si desea que un usuario esté en el mismo grupo cada vez que el usuario establece una conexión, tiene que llamar a `Groups.Add` cada vez que el usuario establece una nueva conexión.

Después de una pérdida temporal de conectividad, en ocasiones SignalR puede restaurar la conexión automáticamente. En ese caso, SignalR está restaurando la misma conexión, sin establecer una nueva conexión y, por lo que se restaura automáticamente pertenencia de grupo del cliente. Esto es posible incluso cuando la interrupción temporal es el resultado de un reinicio del servidor o un error, porque el estado de la conexión para cada cliente, incluida la pertenencia a grupos, es de ida y vuelta al cliente. Si un servidor deja de funcionar y se reemplaza por un nuevo servidor antes agote el tiempo de espera de la conexión, un cliente puede volver a conectarse automáticamente al nuevo servidor y volver a inscribir en es un miembro de los grupos.

Cuando una conexión no se puede restaurar automáticamente después de una pérdida de conectividad, o cuando se agota el tiempo de espera de la conexión, o cuando el cliente se desconecta (por ejemplo, cuando un explorador se desplaza a una nueva página), se pierden las pertenencias a grupos. La próxima vez que el usuario se conecta será una nueva conexión. Para mantener las pertenencias a grupos cuando el mismo usuario establece una nueva conexión, la aplicación tiene que realizar el seguimiento de las asociaciones entre usuarios y grupos y restaurar las pertenencias a grupos cada vez que un usuario establece una nueva conexión.

Para obtener más información sobre las conexiones y reconexiones, consulte [cómo controlar los eventos de duración de conexión en la clase de base de datos central](#connectionlifetime) más adelante en este tema.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Grupos de usuario único

Las aplicaciones que utilizan SignalR normalmente tienen que realizar un seguimiento de las asociaciones entre usuarios y las conexiones para saber qué usuario ha enviado un mensaje y qué usuarios deberían recibir un mensaje. Grupos se utilizan en uno de los dos patrones de uso frecuente para hacerlo.

- Grupos de usuario único.

    Puede especificar el nombre de usuario como el nombre del grupo y agregue el identificador de conexión actual al grupo cada vez que el usuario se conecta o se vuelve a conectar. Para enviar mensajes al usuario que enviar al grupo. Una desventaja de este método es que el grupo no le proporciona una manera de averiguar si el usuario está conectado o desconectado.
- Realizar un seguimiento de las asociaciones entre los nombres de usuario y los identificadores de conexión.

    Puede almacenar una asociación entre cada nombre de usuario y los identificadores de uno o más conexiones en un diccionario o una base de datos y actualizar los datos almacenados cada vez que el usuario se conecta o desconecta. Para enviar mensajes al usuario especificar el Id. de conexión. Una desventaja de este método es que se tarda más memoria.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Cómo controlar los eventos de duración de conexión en la clase de base de datos central

Motivos habituales para el control de eventos de duración de conexión son para realizar un seguimiento de si un usuario está conectado o no y realizar un seguimiento de la asociación entre los nombres de usuario y los identificadores de conexión. Para ejecutar su propio código cuando los clientes conectan o desconexión, invalidar la `OnConnected`, `OnDisconnected`, y `OnReconnected` métodos virtuales del centro de la clase, como se muestra en el ejemplo siguiente.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Cuando se llama a OnConnected, OnDisconnected y OnReconnected

Cada vez que un explorador se desplaza a una nueva página, una nueva conexión tiene que establecerse, lo que significa que se ejecutará SignalR el `OnDisconnected` método seguido por el `OnConnected` método. SignalR siempre crea un nuevo identificador de conexión cuando se establece una nueva conexión.

El `OnReconnected` método se llama cuando se ha producido una interrupción temporal de conectividad que SignalR puede recuperarse automáticamente de, por ejemplo, cuando un cable está desconectado y vuelto a conectar antes agote el tiempo de espera de la conexión temporalmente. El `OnDisconnected` método se llama cuando el cliente se desconecta y SignalR no se puede volver a conectar automáticamente, por ejemplo, cuando un explorador se desplaza a una nueva página. Por lo tanto, es una secuencia de eventos para un determinado cliente posibles `OnConnected`, `OnReconnected`, `OnDisconnected`; o `OnConnected`, `OnDisconnected`. No verá la secuencia `OnConnected`, `OnDisconnected`, `OnReconnected` para una conexión dada.

El `OnDisconnected` método que no se llama en algunos escenarios, como cuando un servidor deja de funcionar o se encienda y el dominio de aplicación. Cuando otro servidor se pone en línea o el dominio de aplicación completa su reciclaje, algunos clientes podrán que pueda volver a conectar y activar el `OnReconnected` eventos.

Para obtener más información, consulte [descripción y control de eventos de duración de conexión de SignalR](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Estado del llamador no rellenado

Se llaman a los métodos de controlador de eventos de duración de conexión desde el servidor, lo que significa que cualquier estado que coloca en el `state` objeto en el cliente no incluirá ningún dato en el `Caller` propiedad en el servidor. Para obtener información sobre la `state` objeto y el `Caller` propiedad, vea [cómo pasar información de estado entre los clientes y la clase de base de datos central](#passstate) más adelante en este tema.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Cómo obtener información sobre el cliente de la propiedad de contexto

Para obtener información acerca del cliente, use el `Context` propiedad de la clase de base de datos central. El `Context` propiedad devuelve un [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) objeto que proporciona acceso a la siguiente información:

- El identificador de conexión del cliente que realiza la llamada.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    El identificador de conexión es un GUID asignado por SignalR (no se puede especificar el valor en su propio código). Hay un identificador de conexión para cada conexión y la misma conexión que identificador es usado por todos los concentradores, si tiene varios centros en la aplicación.
- Datos del encabezado HTTP.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    También puede obtener encabezados HTTP desde `Context.Headers`. La razón para varias referencias a la misma acción que es `Context.Headers` se creó en primer lugar, el `Context.Request` propiedad se agregó más tarde, y `Context.Headers` se conserva por compatibilidad con versiones anteriores.
- Consultar datos de cadena.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    También puede obtener datos de cadena de consulta de `Context.QueryString`.

    La cadena de consulta que se obtiene en esta propiedad es el que se usó con la solicitud HTTP que se estableció la conexión de SignalR. Puede agregar parámetros de cadena de consulta en el cliente mediante la configuración de la conexión, que es una manera cómoda de pasar datos sobre el cliente desde el cliente al servidor. En el ejemplo siguiente se muestra una manera de agregar una cadena de consulta en un cliente de JavaScript cuando se utiliza el proxy generado.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Para obtener más información acerca de cómo establecer parámetros de cadena de consulta, consulte las guías de API para el [JavaScript](hubs-api-guide-javascript-client.md) y [.NET](hubs-api-guide-net-client.md) los clientes.

    Puede encontrar el método de transporte utilizado para la conexión en los datos de cadena de consulta, junto con algunos otros valores utilizadas internamente por SignalR:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    El valor de `transportMethod` será "webSockets", "serverSentEvents", "foreverFrame" o "longPolling". Tenga en cuenta que si selecciona este valor el `OnConnected` método de controlador de eventos, en algunos escenarios inicialmente podría obtener un valor de transporte que no es el método de transporte negociada final para la conexión. En ese caso, el método producirá una excepción y se llamará de nuevo más tarde cuando se establece el modo de transporte final.
- Cookies.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    También puede obtener las cookies de `Context.RequestCookies`.
- Información del usuario.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- El objeto HttpContext para la solicitud:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Utilice este método en lugar de ver `HttpContext.Current` para obtener la `HttpContext` objeto para la conexión de SignalR.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Cómo pasar información de estado entre los clientes y la clase de base de datos central

El proxy de cliente proporciona un `state` objeto en el que puede almacenar datos que se van a transmitir al servidor con cada llamada al método. En el servidor puede tener acceso a estos datos en el `Clients.Caller` propiedad en los métodos de concentrador que se llama a los clientes. El `Clients.Caller` propiedad no se rellena para los métodos de controlador de eventos de duración de conexión `OnConnected`, `OnDisconnected`, y `OnReconnected`.

Crear o actualizar datos en el `state` objeto y el `Clients.Caller` propiedad funciona en ambas direcciones. También puede actualizar los valores en el servidor y se pasan al cliente.

En el ejemplo siguiente se muestra código de cliente de JavaScript que almacena el estado de transmisión en el servidor con cada llamada al método.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

En el ejemplo siguiente se muestra el código equivalente en un cliente. NET.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

En la clase base de datos central, puede tener acceso a estos datos en el `Clients.Caller` propiedad. En el ejemplo siguiente se muestra código que recupera el estado que se hace referencia en el ejemplo anterior.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Este mecanismo para mantener el estado no está diseñado para grandes cantidades de datos, porque todo lo que coloque en la `state` o `Clients.Caller` propiedad es de ida y vuelta con cada invocación del método. Es útil para los elementos más pequeños como nombres de usuario o los contadores.


En VB.NET o en un concentrador fuertemente tipada, el objeto de estado del autor de la llamada no puede obtenerse a través `Clients.Caller`; en su lugar, utilice `Clients.CallerState` (introducida en SignalR 2.1):

**Usar CallerState en C#**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Usar CallerState en Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Cómo controlar los errores en la clase de base de datos central

Para controlar los errores que se producen en los métodos de clase de base de datos central, utilice uno o varios de los métodos siguientes:

- Ajustar el código del método en bloques try-catch y registrar el objeto de excepción. Para fines de depuración puede enviar la excepción al cliente, pero para la seguridad no se recomienda motivos enviar información detallada para los clientes de producción.
- Crear un módulo de la canalización de concentradores que administra el [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) método. En el ejemplo siguiente se muestra un módulo de canalización que registra los errores, seguidos por el código de Startup.cs que inserta el módulo en la canalización de concentradores.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Use la `HubException` clase (introducida en SignalR 2). Este error se puede producir de cualquier invocación de concentrador. El `HubError` constructor toma un mensaje de cadena y un objeto para almacenar datos de error adicionales. SignalR se auto-serializar la excepción y enviarlo al cliente, donde se utilizará para rechazar o producirá un error en la invocación del método de concentrador.

    Los ejemplos de código siguiente muestran cómo producir una `HubException` durante una invocación de concentrador y cómo controlar la excepción en los clientes de JavaScript y. NET.

    **Código de servidor que muestra la clase HubException**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **Código de cliente de JavaScript que muestra la respuesta para producir una HubException en un centro de**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **Código de cliente de .NET que muestra la respuesta para producir una HubException en un centro de**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Para obtener más información acerca de los módulos de la canalización de concentrador, consulte [cómo personalizar la canalización de concentradores](#hubpipeline) más adelante en este tema.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Cómo habilitar el seguimiento

Para habilitar el seguimiento de servidor, agregue un elemento de system.diagnostics al archivo Web.config, tal y como se muestra en este ejemplo:

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Al ejecutar la aplicación en Visual Studio, puede ver los registros en la **salida** ventana.

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Cómo llamar a los métodos de cliente y administrar grupos desde fuera de la clase de base de datos central

Para llamar a cliente métodos de una clase diferente que la clase de base de datos central, obtener una referencia al objeto de contexto de SignalR para el concentrador y usarlo para llamar a métodos en el cliente o administrar grupos.

El ejemplo siguiente `StockTicker` clase obtiene el objeto de contexto, lo almacena en una instancia de la clase, almacena la instancia de clase en una propiedad estática y usa el contexto de la instancia de clase singleton para llamar a la `updateStockPrice` método en los clientes que están conectado a un concentrador denominado `StockTickerHub`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Si tiene que utilizar los tiempos de varios de contexto en un objeto de larga duración, obtener la referencia de una vez y guardar en lugar de obtenerlo nuevo cada vez. Obtener el contexto de una vez, se garantiza que SignalR envía mensajes a los clientes en la misma secuencia en la que los métodos de concentrador realizan a cliente las invocaciones de método. Para obtener un tutorial que muestra cómo utilizar el contexto de SignalR para un concentrador, consulte [Server difusión con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Llamar a métodos de cliente

Puede especificar que los clientes recibirán la RPC, pero tiene menos opciones que cuando se llama a partir de una clase de base de datos central. El motivo es que el contexto no está asociado a una determinada llamada de un cliente, por lo que los métodos que requieren conocimientos del identificador de conexión actual, como `Clients.Others`, o `Clients.Caller`, o `Clients.OthersInGroup`, no están disponibles. Están disponibles las siguientes opciones:

- Todos los clientes conectados.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Un cliente específico identificado por el identificador de conexión.

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Todos los clientes conectados excepción los clientes especificados, identificados por el identificador de conexión.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Todos los clientes en un grupo especificado conectados.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Todos los clientes en un grupo especificado, excepto los clientes especificados, identificado por el identificador de conexión.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

Si está llamando a la clase de concentrador no de métodos en la clase de base de datos central, puede pasar el identificador de conexión actual y usarlo con `Clients.Client`, `Clients.AllExcept`, o `Clients.Group` para simular `Clients.Caller`, `Clients.Others`, o `Clients.OthersInGroup`. En el ejemplo siguiente, la `MoveShapeHub` clase pasa el identificador de conexión para el `Broadcaster` clase para que la `Broadcaster` puede simular la clase `Clients.Others`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Administrar la pertenencia a grupos

Para administrar grupos de tener las mismas opciones tal y como haría en una clase de base de datos central.

- Agregar a un cliente a un grupo

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Quitar a un cliente de un grupo

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Cómo personalizar la canalización de concentradores

SignalR le permite insertar su propio código en la canalización de concentrador. En el ejemplo siguiente se muestra un módulo personalizado de canalización de concentrador que registra cada llamada de método entrante recibido desde el cliente y la llamada de método de salida se invoca en el cliente:

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

El siguiente código en el *Startup.cs* archivo registra el módulo se ejecuten en la canalización de concentrador:

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

Hay muchos métodos diferentes que se pueden reemplazar. Para obtener una lista completa, consulte [HubPipelineModule métodos](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
