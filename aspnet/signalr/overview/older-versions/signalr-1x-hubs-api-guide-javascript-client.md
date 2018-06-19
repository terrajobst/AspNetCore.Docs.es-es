---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: Guía de API de concentradores SignalR 1.x - cliente de JavaScript | Documentos de Microsoft
author: pfletcher
description: Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 1.1 en los clientes de JavaScript, como los exploradores y la tienda de Windows (WinJS) applic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: f92470b2022f343cfd6d822abb255dc19947b4d1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036926"
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a>Guía de API de concentradores SignalR 1.x - cliente de JavaScript
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 1.1 en los clientes de JavaScript, como exploradores y aplicaciones de la tienda de Windows (WinJS).
> 
> La API de concentradores de SignalR permite realizar llamadas a procedimiento remoto (RPC) desde un servidor a los clientes conectados y desde clientes en el servidor. En el código de servidor, definir métodos que puedan llamar los clientes y llamar a métodos que se ejecutan en el cliente. En el código de cliente, definir métodos que se pueda llamar desde el servidor y llamar a métodos que se ejecutan en el servidor. SignalR se encarga de todas la mecánica de cliente a servidor para usted.
> 
> SignalR también ofrece una API de nivel inferior denominada conexiones persistentes. Para obtener una introducción a las conexiones persistentes, concentradores y SignalR, o para ver un tutorial que muestra cómo crear una aplicación de SignalR completa, consulte [SignalR - Getting Started](../getting-started/index.md).


## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [El proxy generado y lo que hace automáticamente](#genproxy)

    - [Cuándo se debe utilizar al proxy generado](#cantusegenproxy)
- [Programa de instalación de cliente](#clientsetup)

    - [Cómo hacen referencia al proxy generado dinámicamente](#dynamicproxy)
    - [Cómo crear un archivo físico para SignalR genera proxy](#manualproxy)
- [Cómo establecer una conexión](#establishconnection)

    - [$. connection.hub es el mismo objeto crea ese $.hubConnection()](#connequivalence)
    - [Ejecución asincrónica del método de inicio](#asyncstart)
- [Cómo establecer una conexión entre dominios](#crossdomain)
- [Cómo configurar la conexión](#configureconnection)

    - [Cómo especificar parámetros de cadena de consulta](#querystring)
    - [Cómo especificar el método de transporte](#transport)
- [Cómo obtener a un proxy para una clase de base de datos central](#getproxy)
- [Cómo definir métodos en el cliente que el servidor puede llamar a](#callclient)
- [Cómo llamar a métodos de servidor desde el cliente](#callserver)
- [Cómo controlar eventos de duración de la conexión](#connectionlifetime)
- [Cómo controlar errores](#handleerrors)
- [Cómo habilitar el registro de cliente](#logging)

Para obtener documentación sobre cómo programar el servidor o los clientes de. NET, vea los siguientes recursos:

- [Guía de API de concentradores de SignalR - servidor](../guide-to-the-api/hubs-api-guide-server.md)
- [Guía de API de concentradores de SignalR - cliente .NET](../guide-to-the-api/hubs-api-guide-net-client.md)

Vínculos a temas de referencia de la API están a la versión de .NET 4.5 de la API. Si usa .NET 4, consulte [la versión 4 de .NET de los temas de la API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>El proxy generado y lo que hace automáticamente

Puede programar un cliente de JavaScript para comunicarse con un servicio de SignalR con o sin un proxy que genera SignalR para usted. Lo que hace el proxy automáticamente es simplificar la sintaxis del código que utiliza para conectar, métodos de escritura que el servidor llama, y llamar a métodos en el servidor.

Cuando se escribe código para llamar a métodos de servidor, el proxy generado le permite utilizar la sintaxis que parece ser que se estaban ejecutando una función local: puede escribir `serverMethod(arg1, arg2)` en lugar de `invoke('serverMethod', arg1, arg2)`. La sintaxis de proxy generado también permite un error de lado cliente inmediato e inteligible si se escribe incorrectamente el nombre de un método de servidor. Y si crea manualmente el archivo que define a los servidores proxy, también puede obtener compatibilidad con IntelliSense para escribir código que llama a métodos de servidor.

Por ejemplo, imagine que tiene la siguiente clase de base de datos central en el servidor:

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Los siguientes ejemplos de código muestran lo que parece de código de JavaScript que para invocar la `NewContosoChatMessage` método en el servidor y recibir las invocaciones de la `addContosoChatMessageToPage` método desde el servidor.

**Con el proxy generado**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Sin el proxy generado**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Cuándo se debe utilizar al proxy generado

Si desea registrar varios controladores de eventos para un método de cliente que llama el servidor, no puede usar al proxy generado. En caso contrario, puede optar por usar al proxy generado o no en función de su preferencia de codificación. Si decide no usarlo, no tienes que hacer referencia a la dirección URL de "/ concentradores de signalr" en un `script` elemento en el código de cliente.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Programa de instalación de cliente

Un cliente de JavaScript requiere referencias a jQuery y el archivo de JavaScript de SignalR core. Debe ser la versión de jQuery 1.6.4 o versiones posteriores importantes, como 1.7.2, 1.8.2 o 1.9.1. Si decide usar al proxy generado, también se necesita una referencia al proxy de SignalR genera archivos de JavaScript. En el ejemplo siguiente se muestra el aspecto que podrían las referencias en una página HTML que utiliza al proxy generado.

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

Estas referencias deben incluirse en este orden: jQuery en primer lugar, la última SignalR core después de eso y los servidores proxy de SignalR.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Cómo hacen referencia al proxy generado dinámicamente

En el ejemplo anterior, la referencia al proxy SignalR generado es código JavaScript generado dinámicamente, no a un archivo físico. SignalR crea el código de JavaScript para el proxy sobre la marcha y sirve para el cliente en respuesta a la dirección URL "/ signalr/concentradores". Si especifica una dirección URL base diferente para las conexiones de SignalR en el servidor en su `MapHubs` método, la dirección URL para el archivo de proxy generados de forma dinámica es la dirección URL personalizada con "/ concentradores" anexada al mismo.

> [!NOTE]
> Para los clientes de JavaScript de Windows 8 (tienda Windows), use el archivo de proxy físico en lugar del generado dinámicamente. Para obtener más información, consulte [cómo crear un archivo físico para SignalR genera proxy](#manualproxy) más adelante en este tema.


En una vista de ASP.NET MVC 4 Razor, use la tilde para hacer referencia a la raíz de la aplicación en la referencia del archivo de proxy:

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

Para obtener más información sobre el uso de SignalR en MVC 4, consulte [Getting Started with SignalR y MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

En una vista de ASP.NET MVC 3 Razor, use `Url.Content` para la referencia del archivo de proxy:

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

En una aplicación de formularios Web Forms de ASP.NET, utilice `ResolveClientUrl` para el proxy de la referencia de archivo o registrar mediante el objeto ScriptManager mediante una ruta relativa de raíz de aplicación (a partir de una tilde):

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

Como norma general, utilice el mismo método para especificar la dirección URL "/ signalr/concentradores" que usa para archivos CSS o JavaScript. Si especifica una dirección URL sin utilizar una tilde, en algunos escenarios de la aplicación funcionará correctamente al probar en Visual Studio mediante IIS Express, pero se producirá un error con un error 404 cuando se implementa en la versión completa de IIS. Para obtener más información, consulte **resolver referencias a recursos de nivel de raíz** en [servidores Web en Visual Studio para proyectos Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) en el sitio MSDN.

Cuando se ejecuta un proyecto web en Visual Studio 2012 en modo de depuración y, si utiliza Internet Explorer como explorador, puede ver el archivo de proxy en **el Explorador de soluciones** en **documentos de Script**, tal y como se muestra en el ilustración siguiente.

![Archivo de proxy generado de JavaScript en el Explorador de soluciones](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

Para ver el contenido del archivo, haga doble clic en **concentradores**. Si no utiliza Internet Explorer y Visual Studio 2012, o si no está en modo de depuración, también puede obtener el contenido del archivo a través de la dirección URL "/ signalR/concentradores". Por ejemplo, si el sitio se ejecuta en `http://localhost:56699`, vaya a `http://localhost:56699/SignalR/hubs` en el explorador.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Cómo crear un archivo físico para SignalR genera proxy

Como alternativa al proxy generado dinámicamente, puede crear un archivo físico que tenga el código de proxy y hacer referencia a ese archivo. Puede hacerlo para un control sobre el almacenamiento en caché o el comportamiento de agrupación, u obtener IntelliSense cuando escribe código llamadas a métodos de servidor.

Para crear un archivo de proxy, siga los pasos siguientes:

1. Instalar el [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) paquete NuGet.
2. Abra un símbolo del sistema y vaya a la *herramientas* carpeta que contiene el archivo SignalR.exe. La carpeta de herramientas está en la siguiente ubicación:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. Escriba el comando siguiente:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    La ruta de acceso a la *.dll* suele ser el *bin* carpeta en la carpeta del proyecto.

    Este comando crea un archivo denominado *server.js* en la misma carpeta que *signalr.exe*.
4. Coloque el *server.js* un archivo en una carpeta correspondiente en el proyecto, cámbiele el nombre según corresponda para su aplicación y agregue una referencia a ella en lugar de la referencia de "/ concentradores de signalr".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Cómo establecer una conexión

Antes de que puede establecer una conexión, tendrá que crear un objeto de conexión, crear a un proxy y registran controladores de eventos para los métodos que pueden llamarse desde el servidor. Cuando se configuran el proxy y controladores de eventos, establecer la conexión mediante una llamada a la `start` método.

Si está utilizando al proxy generado, no tienes que crear el objeto de conexión en su propio código porque hace en el código de proxy generado automáticamente.

<a id="nogenconnection"></a>

**Establecer una conexión (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Establecer una conexión (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

El código de ejemplo usa el valor predeterminado "/ signalr" dirección URL para conectarse a su servicio de SignalR. Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [Guía de API de concentradores de ASP.NET SignalR - Server - la dirección URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

> [!NOTE]
> Normalmente se registran controladores de eventos antes de llamar a la `start` método para establecer la conexión. Si desea registrar algunos controladores de eventos después de establecer la conexión, puede hacerlo, pero debe registrar al menos uno de los handler(s) de evento antes de llamar a la `start` método. Una razón para esto es que puede haber muchos concentradores en una aplicación, pero que no desea que activen la `OnConnected` eventos en cada concentrador si solo va a usar para uno de ellos. Cuando se establece la conexión, la presencia de un método de cliente en proxy de una base de datos central es lo que indica SignalR para desencadenar la `OnConnected` eventos. Si no registra los controladores de eventos antes de llamar a la `start` método, podrá invocar métodos en el concentrador, pero el concentrador `OnConnected` no se llamará al método y no se invocará ningún método de cliente desde el servidor.


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. connection.hub es el mismo objeto crea ese $.hubConnection()

Como puede ver en los ejemplos, cuando se usa el proxy generado, `$.connection.hub` hace referencia al objeto de conexión. Este es el mismo objeto que obtiene mediante una llamada a `$.hubConnection()` cuando no está usando el proxy generado. El código de proxy generado crea la conexión automáticamente mediante la ejecución de la siguiente instrucción:

![Crear una conexión en el archivo de proxy generado](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

Cuando se usa el proxy generado, puede hacer cualquier cosa con `$.connection.hub` que puede hacer con un objeto de conexión cuando no lo esté usando el proxy generado.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Ejecución asincrónica del método de inicio

El `start` método se ejecuta de forma asincrónica. Devuelve un [jQuery diferida objeto](http://api.jquery.com/category/deferred-object/), lo que significa que puede agregar funciones de devolución de llamada mediante una llamada a métodos como `pipe`, `done`, y `fail`. Si tiene código que desea ejecutar una vez establecida la conexión, como una llamada a un método de servidor, agregue el código en una función de devolución de llamada o llamarlo desde una función de devolución de llamada. El `.done` método de devolución de llamada se ejecuta después de que se ha establecido la conexión y, después de cualquier código que tiene en su `OnConnected` método de controlador de eventos en el servidor termina de ejecutarse.

Si coloca la instrucción "Ahora conectados" del ejemplo anterior, como la siguiente línea de código después de la `start` llamada al método (no en un `.done` devolución de llamada), la `console.log` línea se ejecutará antes de establece la conexión, tal y como se muestra en el siguiente ejemplo:

![Medio incorrecto para escribir código que se ejecuta después de establecer conexión](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Cómo establecer una conexión entre dominios

Por lo general si el explorador carga una página de `http://contoso.com`, la conexión de SignalR está en el mismo dominio, en `http://contoso.com/signalr`. Si la página de `http://contoso.com` realiza una conexión a `http://fabrikam.com/signalr`, que es una conexión entre dominios. Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada. Para establecer una conexión entre dominios, asegúrese de que las conexiones entre dominios están habilitadas en el servidor y especificar la dirección URL de conexión al crear el objeto de conexión. SignalR usará la tecnología adecuada para las conexiones entre dominios, como [JSONP](http://en.wikipedia.org/wiki/JSONP) o [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

En el servidor, habilite las conexiones entre dominios seleccionando esta opción cuando se llama a la `MapHubs` método.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

En el cliente, especifique la dirección URL cuando se crea el objeto de conexión (sin el proxy generado) o antes de llamar al método start (con el proxy generado).

**Código de cliente que especifica una conexión entre dominios (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

**Código de cliente que especifica una conexión entre dominios (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

Cuando se usa el `$.hubConnection` constructor, no tiene que incluir `signalr` en la dirección URL porque se agrega automáticamente (a menos que especifique `useDefaultUrl` como `false`).

Puede crear varias conexiones a puntos de conexión diferentes.

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - No establece `jQuery.support.cors` en true en el código.
> 
>     ![No establece jQuery.support.cors en true](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR controla el uso de JSONP o CORS. Establecer `jQuery.support.cors` a true deshabilita JSONP porque hace que SignalR asuma el explorador es compatible con CORS.
> - Cuando se está conectando a una dirección URL de localhost, Internet Explorer 10 no Considérelo una conexión entre dominios, por lo que la aplicación funcionará localmente con Internet Explorer 10 incluso si no ha habilitado las conexiones entre dominios en el servidor.
> - Para obtener información sobre el uso de las conexiones entre dominios con Internet Explorer 9, consulte [este subproceso StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Para obtener información sobre el uso de las conexiones entre dominios con Chrome, consulte [este subproceso StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - El código de ejemplo usa el valor predeterminado "/ signalr" dirección URL para conectarse a su servicio de SignalR. Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [Guía de API de concentradores de ASP.NET SignalR - Server - la dirección URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Cómo configurar la conexión

Antes de establecer una conexión, puede especificar parámetros de cadena de consulta o especifique el método de transporte.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Cómo especificar parámetros de cadena de consulta

Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta para el objeto de conexión. Los ejemplos siguientes muestran cómo establecer un parámetro de cadena de consulta en código de cliente.

**Establecer un valor de cadena de consulta antes de llamar al método start (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

**Establecer un valor de cadena de consulta antes de llamar al método start (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

En el ejemplo siguiente se muestra cómo leer un parámetro de cadena de consulta en código del servidor.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Cómo especificar el método de transporte

Como parte del proceso de conexión, un cliente de SignalR normalmente se negocia con el servidor para determinar el mejor transporte que se admite por servidor y cliente. Si ya sabe qué transporte que se va a utilizar, puede omitir este proceso de negociación especificando el modo de transporte cuando se llama a la `start` método.

**Código de cliente que especifica el método de transporte (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Código de cliente que especifica el método de transporte (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Como alternativa, puede especificar varios métodos de transporte en el orden en el que desea SignalR para intentarlo de ellos:

**Código de cliente que especifica un esquema de reserva de transporte personalizado (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

**Código de cliente que especifica un esquema de reserva de transporte personalizado (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

Puede usar los siguientes valores para especificar el método de transporte:

- "webSockets"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Los ejemplos siguientes muestran cómo averiguar qué método de transporte está usándola una conexión.

**Código de cliente que se muestra el método de transporte utilizado por una conexión (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

**Código de cliente que se muestra el método de transporte utilizado por una conexión (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

Para obtener información sobre cómo comprobar el método de transporte en el código de servidor, consulte [Guía de API de concentradores de ASP.NET SignalR - Server - cómo obtener información sobre el cliente de la propiedad de contexto](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Para obtener más información acerca de los transportes y reservas, consulte [Introducción a SignalR - transportes y reservas](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Cómo obtener a un proxy para una clase de base de datos central

Cada objeto de conexión que cree encapsula información sobre una conexión a un servicio de SignalR que contiene una o más clases de concentrador. Para comunicarse con una clase de base de datos central, utilice un objeto de proxy que crea usted mismo (si no está utilizando al proxy generado) o que se genera automáticamente.

En el cliente el nombre del proxy es una versión de grafía de camel del nombre de clase del concentrador. SignalR automáticamente realiza este cambio para que el código de JavaScript puede ajustarse a las convenciones de JavaScript.

**Clase de base de datos central en servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

**Obtener una referencia al proxy de cliente generado para el concentrador**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

**Crear el proxy de cliente para la clase de base de datos central (sin proxy generado)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

Si decorar la clase de concentrador con un `HubName` de atributo, use el nombre exacto sin cambiar mayúsculas y minúsculas.

**Clase de base de datos central en el servidor con el atributo HubName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

**Obtener una referencia al proxy de cliente generado para el concentrador**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

**Crear el proxy de cliente para la clase de base de datos central (sin proxy generado)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Cómo definir métodos en el cliente que el servidor puede llamar a

Para definir un método que el servidor pueda llamar desde un concentrador, agregar un controlador de eventos para el proxy de concentrador mediante el `client` propiedad del proxy generado, o llame a la `on` método si no está usando el proxy generado. Los parámetros pueden ser objetos complejos.

Agregue el controlador de eventos antes de llamar a la `start` método para establecer la conexión. (Si desea agregar controladores de eventos después de llamar a la `start` método, vea la nota de [cómo establecer una conexión](#establishconnection) anteriormente en este documento y utilizar la sintaxis mostrada para definir un método sin utilizar el proxy generado.)

Coincidencia de nombres de método distingue mayúsculas de minúsculas. Por ejemplo, `Clients.All.addContosoChatMessageToPage` se ejecutará en el servidor `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, o `addcontosochatmessagetopage` en el cliente.

**Definir el método de cliente (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

**Manera alternativa para definir el método de cliente (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

**Defina el método de cliente (sin el proxy generado, o cuando se agrega después de llamar al método de inicio)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

**Código de servidor que se llama al método de cliente**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

Los ejemplos siguientes incluye un objeto complejo como un parámetro de método.

**Defina el método de cliente que toma un objeto complejo (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

**Defina el método de cliente que toma un objeto complejo (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

**Código de servidor que define el objeto complejo**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

**Código de servidor que se llama al método de cliente mediante un objeto complejo**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Cómo llamar a métodos de servidor desde el cliente

Para llamar a un método de servidor desde el cliente, use el `server` propiedad del proxy generado o `invoke` método en el proxy de concentrador, si no está usando el proxy generado. El valor devuelto o los parámetros pueden ser objetos complejos.

Pasar de una versión de mayúscula y minúscula camel del nombre del método en el concentrador. SignalR automáticamente realiza este cambio para que el código de JavaScript puede ajustarse a las convenciones de JavaScript.

Los ejemplos siguientes muestran cómo llamar a un método de servidor que no tiene un valor devuelto y cómo llamar a un método de servidor que tiene un valor devuelto.

**Método de servidor con ningún atributo HubMethodName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

**Código de servidor que define el objeto complejo que se pasa en un parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

Si decora el método de concentrador con un `HubMethodName` de atributo, use ese nombre sin cambiar mayúsculas y minúsculas.

**Método de servidor** con un atributo HubMethodName

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

Los ejemplos anteriores muestran cómo llamar a un método de servidor que no tiene ningún valor devuelto. Los ejemplos siguientes muestran cómo llamar a un método de servidor que tiene un valor devuelto.

**Código de servidor para un método que tiene un valor devuelto**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

**La clase de existencias utilizada para la** valor devuelto

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Cómo controlar eventos de duración de la conexión

SignalR proporciona la siguiente conexión de eventos de duración que puede controlar:

- `starting`: Genera antes de que se envían los datos a través de la conexión.
- `received`: Se genera cuando se reciben los datos en la conexión. Proporciona los datos recibidos.
- `connectionSlow`: Se genera cuando el cliente detecta una conexión lenta o quitar con frecuencia.
- `reconnecting`: Se genera cuando el transporte subyacente comienza conectarse de nuevo.
- `reconnected`: Se genera cuando se ha vuelto a conectar el transporte subyacente.
- `stateChanged`: Se produce cuando cambia el estado de conexión. Proporciona el estado anterior y el nuevo estado (conexión, conectado, volviendo a conectarse o Disconnected).
- `disconnected`: Se genera cuando se ha desconectado la conexión.

Por ejemplo, si desea mostrar mensajes de advertencia cuando hay problemas de conexión que pueden producir retrasos notables, controlar el `connectionSlow` eventos.

**Controlar el evento connectionSlow (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

**Controlar el evento connectionSlow (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

Para obtener más información, consulte [descripción y control de eventos de duración de conexión de SignalR](index.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Cómo controlar errores

El cliente de SignalR JavaScript proporciona un `error` eventos que puede agregar un controlador. También puede usar el método fail para agregar un controlador de errores que son el resultado de una invocación de método de servidor.

Si no habilita explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que SignalR devuelve después de un error contiene información básica sobre el error. Por ejemplo, si una llamada a `newContosoChatMessage` se produce un error, el mensaje de error en el objeto de error contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" enviar mensajes de error detallados a los clientes de producción no se recomienda por motivos de seguridad, pero si desea habilitar los mensajes de error detallados para solución de problemas, utilice el código siguiente en el servidor.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

En el ejemplo siguiente se muestra cómo agregar un controlador para el evento de error.

**Agregar un controlador de errores (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

**Agregar un controlador de errores (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

En el ejemplo siguiente se muestra cómo controlar un error de una invocación de método.

**Identificador de error de una invocación de método (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

**Identificador de error de una invocación de método (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Si se produce un error en una invocación de método, el `error` también se genera el evento, por lo que el código en el `error` controlador de método y en el `.fail` ejecutaría la devolución de llamada de método.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Cómo habilitar el registro de cliente

Para habilitar el registro de cliente en una conexión, establecer el `logging` propiedad en el objeto de conexión antes de llamar a la `start` método para establecer la conexión.

**Habilitar el registro (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

**Habilitar el registro (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

Para ver los registros, abrir las herramientas de desarrollador de su explorador y vaya a la pestaña de la consola. Para obtener un tutorial que muestra las instrucciones paso a paso y pantalla capturas que muestran cómo hacerlo, consulte [Server difusión con ASP.NET Signalr - Enable Logging](index.md).
