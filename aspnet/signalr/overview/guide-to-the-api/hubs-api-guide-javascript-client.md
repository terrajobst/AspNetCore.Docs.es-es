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
ms.openlocfilehash: 65e369a393a8c5d2d1bba11b5c71347df8f9c69d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>Guía de API de bases de datos centrales de ASP.NET SignalR - cliente de JavaScript
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 2 en los clientes de JavaScript, como exploradores y aplicaciones de la tienda de Windows (WinJS).
> 
> La API de concentradores de SignalR permite realizar llamadas a procedimiento remoto (RPC) desde un servidor a los clientes conectados y desde clientes en el servidor. En el código de servidor, definir métodos que puedan llamar los clientes y llamar a métodos que se ejecutan en el cliente. En el código de cliente, definir métodos que se pueda llamar desde el servidor y llamar a métodos que se ejecutan en el servidor. SignalR se encarga de todas la mecánica de cliente a servidor para usted.
> 
> SignalR también ofrece una API de nivel inferior denominada conexiones persistentes. Para obtener una introducción a SignalR, concentradores y conexiones persistentes, consulte [Introducción a SignalR](../getting-started/introduction-to-signalr.md).
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

- [Guía de API de concentradores de SignalR - servidor](hubs-api-guide-server.md)
- [Guía de API de concentradores de SignalR - cliente .NET](hubs-api-guide-net-client.md)

El componente de servidor de SignalR 2 solo está disponible en .NET 4.5 (aunque no hay un cliente de .NET para SignalR 2 en .NET 4.0).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>El proxy generado y lo que hace automáticamente

Puede programar un cliente de JavaScript para comunicarse con un servicio de SignalR con o sin un proxy que genera SignalR para usted. Lo que hace el proxy automáticamente es simplificar la sintaxis del código que utiliza para conectar, métodos de escritura que el servidor llama, y llamar a métodos en el servidor.

Cuando se escribe código para llamar a métodos de servidor, el proxy generado le permite utilizar la sintaxis que parece ser que se estaban ejecutando una función local: puede escribir `serverMethod(arg1, arg2)` en lugar de `invoke('serverMethod', arg1, arg2)`. La sintaxis de proxy generado también permite un error de lado cliente inmediato e inteligible si se escribe incorrectamente el nombre de un método de servidor. Y si crea manualmente el archivo que define a los servidores proxy, también puede obtener compatibilidad con IntelliSense para escribir código que llama a métodos de servidor.

Por ejemplo, imagine que tiene la siguiente clase de base de datos central en el servidor:

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Los siguientes ejemplos de código muestran lo que parece de código de JavaScript que para invocar la `NewContosoChatMessage` método en el servidor y recibir las invocaciones de la `addContosoChatMessageToPage` método desde el servidor.

**Con el proxy generado**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Sin el proxy generado**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Cuándo se debe utilizar al proxy generado

Si desea registrar varios controladores de eventos para un método de cliente que llama el servidor, no puede usar al proxy generado. En caso contrario, puede optar por usar al proxy generado o no en función de su preferencia de codificación. Si decide no usarlo, no tienes que hacer referencia a la dirección URL de "/ concentradores de signalr" en un `script` elemento en el código de cliente.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Programa de instalación de cliente

Un cliente de JavaScript requiere referencias a jQuery y el archivo de JavaScript de SignalR core. Debe ser la versión de jQuery 1.6.4 o versiones posteriores importantes, como 1.7.2, 1.8.2 o 1.9.1. Si decide usar al proxy generado, también se necesita una referencia al proxy de SignalR genera archivos de JavaScript. En el ejemplo siguiente se muestra el aspecto que podrían las referencias en una página HTML que utiliza al proxy generado.

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

Estas referencias deben incluirse en este orden: jQuery en primer lugar, la última SignalR core después de eso y los servidores proxy de SignalR.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Cómo hacen referencia al proxy generado dinámicamente

En el ejemplo anterior, la referencia al proxy SignalR generado es código JavaScript generado dinámicamente, no a un archivo físico. SignalR crea el código de JavaScript para el proxy sobre la marcha y sirve para el cliente en respuesta a la dirección URL "/ signalr/concentradores". Si especifica una dirección URL base diferente para las conexiones de SignalR en el servidor en su `MapSignalR` método, la dirección URL para el archivo de proxy generados de forma dinámica es la dirección URL personalizada con "/ concentradores" anexada al mismo.

> [!NOTE]
> Para los clientes de JavaScript de Windows 8 (tienda Windows), use el archivo de proxy físico en lugar del generado dinámicamente. Para obtener más información, consulte [cómo crear un archivo físico para SignalR genera proxy](#manualproxy) más adelante en este tema.


En un programa de ASP.NET MVC 4 o 5 Razor, use la tilde para hacer referencia a la raíz de la aplicación en la referencia del archivo de proxy:

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

Para obtener más información sobre el uso de SignalR en MVC 5, consulte [Getting Started with SignalR y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

En una vista de ASP.NET MVC 3 Razor, use `Url.Content` para la referencia del archivo de proxy:

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

En una aplicación de formularios Web Forms de ASP.NET, utilice `ResolveClientUrl` para el proxy de la referencia de archivo o registrar mediante el objeto ScriptManager mediante una ruta relativa de raíz de aplicación (a partir de una tilde):

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

Como norma general, utilice el mismo método para especificar la dirección URL "/ signalr/concentradores" que usa para archivos CSS o JavaScript. Si especifica una dirección URL sin utilizar una tilde, en algunos escenarios de la aplicación funcionará correctamente al probar en Visual Studio mediante IIS Express, pero se producirá un error con un error 404 cuando se implementa en la versión completa de IIS. Para obtener más información, consulte **resolver referencias a recursos de nivel de raíz** en [servidores Web en Visual Studio para proyectos Web ASP.NET](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx) en el sitio MSDN.

Cuando se ejecuta un proyecto web en Visual Studio 2013 en modo de depuración y, si utiliza Internet Explorer como explorador, puede ver el archivo de proxy en **el Explorador de soluciones** en **documentos de Script**, tal y como se muestra en el ilustración siguiente.

![Archivo de proxy generado de JavaScript en el Explorador de soluciones](hubs-api-guide-javascript-client/_static/image1.png)

Para ver el contenido del archivo, haga doble clic en **concentradores**. Si no utiliza Internet Explorer y Visual Studio 2012 o 2013, o si no está en modo de depuración, también puede obtener el contenido del archivo a través de la dirección URL "/ signalR/concentradores". Por ejemplo, si el sitio se ejecuta en `http://localhost:56699`, vaya a `http://localhost:56699/SignalR/hubs` en el explorador.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Cómo crear un archivo físico para SignalR genera proxy

Como alternativa al proxy generado dinámicamente, puede crear un archivo físico que tenga el código de proxy y hacer referencia a ese archivo. Puede hacerlo para un control sobre el almacenamiento en caché o el comportamiento de agrupación, u obtener IntelliSense cuando escribe código llamadas a métodos de servidor.

Para crear un archivo de proxy, siga los pasos siguientes:

1. Instalar el [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) paquete NuGet.
2. Abra un símbolo del sistema y vaya a la *herramientas* carpeta que contiene el archivo SignalR.exe. La carpeta de herramientas está en la siguiente ubicación:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
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

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Establecer una conexión (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

El código de ejemplo usa el valor predeterminado "/ signalr" dirección URL para conectarse a su servicio de SignalR. Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [Guía de API de concentradores de ASP.NET SignalR - Server - la dirección URL /signalr](hubs-api-guide-server.md#signalrurl).

De forma predeterminada, la ubicación de la base de datos central es el servidor actual; Si se conecta a un servidor diferente, especifique la dirección URL antes de llamar a la `start` método, tal como se muestra en el ejemplo siguiente:

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> Normalmente se registran controladores de eventos antes de llamar a la `start` método para establecer la conexión. Si desea registrar algunos controladores de eventos después de establecer la conexión, puede hacerlo, pero debe registrar al menos uno de los handler(s) de evento antes de llamar a la `start` método. Una razón para esto es que puede haber muchos concentradores en una aplicación, pero que no desea que activen la `OnConnected` eventos en cada concentrador si solo va a usar para uno de ellos. Cuando se establece la conexión, la presencia de un método de cliente en proxy de una base de datos central es lo que indica SignalR para desencadenar la `OnConnected` eventos. Si no registra los controladores de eventos antes de llamar a la `start` método, podrá invocar métodos en el concentrador, pero el concentrador `OnConnected` no se llamará al método y no se invocará ningún método de cliente desde el servidor.


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. connection.hub es el mismo objeto crea ese $.hubConnection()

Como puede ver en los ejemplos, cuando se usa el proxy generado, `$.connection.hub` hace referencia al objeto de conexión. Este es el mismo objeto que obtiene mediante una llamada a `$.hubConnection()` cuando no está usando el proxy generado. El código de proxy generado crea la conexión automáticamente mediante la ejecución de la siguiente instrucción:

![Crear una conexión en el archivo de proxy generado](hubs-api-guide-javascript-client/_static/image3.png)

Cuando se usa el proxy generado, puede hacer cualquier cosa con `$.connection.hub` que puede hacer con un objeto de conexión cuando no lo esté usando el proxy generado.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Ejecución asincrónica del método de inicio

El `start` método se ejecuta de forma asincrónica. Devuelve un [jQuery diferida objeto](http://api.jquery.com/category/deferred-object/), lo que significa que puede agregar funciones de devolución de llamada mediante una llamada a métodos como `pipe`, `done`, y `fail`. Si tiene código que desea ejecutar una vez establecida la conexión, como una llamada a un método de servidor, agregue el código en una función de devolución de llamada o llamarlo desde una función de devolución de llamada. El `.done` método de devolución de llamada se ejecuta después de que se ha establecido la conexión y, después de cualquier código que tiene en su `OnConnected` método de controlador de eventos en el servidor termina de ejecutarse.

Si coloca la instrucción "Ahora conectados" del ejemplo anterior, como la siguiente línea de código después de la `start` llamada al método (no en un `.done` devolución de llamada), la `console.log` línea se ejecutará antes de establece la conexión, tal y como se muestra en el siguiente ejemplo:

![Medio incorrecto para escribir código que se ejecuta después de establecer conexión](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Cómo establecer una conexión entre dominios

Por lo general si el explorador carga una página de `http://contoso.com`, la conexión de SignalR está en el mismo dominio, en `http://contoso.com/signalr`. Si la página de `http://contoso.com` realiza una conexión a `http://fabrikam.com/signalr`, que es una conexión entre dominios. Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada.

En SignalR 1.x solicitudes entre dominios se controla mediante una sola marca de EnableCrossDomain. Este indicador controla las solicitudes de JSONP y CORS. Para una mayor flexibilidad, compatibilidad con todos los CORS se ha quitado del componente de servidor de SignalR (clientes de JavaScript seguir usan CORS normalmente si se detecta que el explorador admite), y nueva middleware OWIN pone a su disposición admitir estos escenarios.

Si JSONP es obligatorio en el cliente (para admitir las solicitudes entre dominios en exploradores de versiones anteriores), será necesario habilitar de forma explícita mediante el establecimiento `EnableJSONP` en el `HubConfiguration` el objeto a `true`, tal y como se muestra a continuación. JSONP está deshabilitado de forma predeterminada, ya que es menos segura que CORS.

**Agregar Microsoft.Owin.Cors a un proyecto:** para instalar esta biblioteca, ejecute el siguiente comando en la consola de administrador de paquetes:

`Install-Package Microsoft.Owin.Cors`

Este comando agregará el 2.1.0 versión del paquete a su proyecto.

### <a name="calling-usecors"></a>Al llamar a UseCors

 El siguiente fragmento de código muestra cómo implementar las conexiones entre dominios en el 2 de SignalR. 

**Implementación de las solicitudes entre dominios en SignalR 2**

El código siguiente muestra cómo habilitar CORS o JSONP en un proyecto de SignalR 2. Este ejemplo de código usa `Map` y `RunSignalR` en lugar de `MapSignalR`, de modo que se ejecuta el middleware CORS solo para las solicitudes de SignalR que requieren compatibilidad con CORS (en lugar de para todo el tráfico en la ruta de acceso especificada en `MapSignalR`.) Asignación también se puede usar para cualquier otro middleware que necesite ejecutar para un prefijo de dirección URL concreto, en lugar de en toda la aplicación.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - No establece `jQuery.support.cors` en true en el código.
> 
>     ![No establece jQuery.support.cors en true](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR controla el uso de CORS. Establecer `jQuery.support.cors` a true deshabilita JSONP porque hace que SignalR asuma el explorador es compatible con CORS.
> - Cuando se está conectando a una dirección URL de localhost, Internet Explorer 10 no Considérelo una conexión entre dominios, por lo que la aplicación funcionará localmente con Internet Explorer 10 incluso si no ha habilitado las conexiones entre dominios en el servidor.
> - Para obtener información sobre el uso de las conexiones entre dominios con Internet Explorer 9, consulte [este subproceso StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Para obtener información sobre el uso de las conexiones entre dominios con Chrome, consulte [este subproceso StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - El código de ejemplo usa el valor predeterminado "/ signalr" dirección URL para conectarse a su servicio de SignalR. Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [Guía de API de concentradores de ASP.NET SignalR - Server - la dirección URL /signalr](hubs-api-guide-server.md#signalrurl).


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Cómo configurar la conexión

Antes de establecer una conexión, puede especificar parámetros de cadena de consulta o especifique el método de transporte.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Cómo especificar parámetros de cadena de consulta

Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta para el objeto de conexión. Los ejemplos siguientes muestran cómo establecer un parámetro de cadena de consulta en código de cliente.

**Establecer un valor de cadena de consulta antes de llamar al método start (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**Establecer un valor de cadena de consulta antes de llamar al método start (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

En el ejemplo siguiente se muestra cómo leer un parámetro de cadena de consulta en código del servidor.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Cómo especificar el método de transporte

Como parte del proceso de conexión, un cliente de SignalR normalmente se negocia con el servidor para determinar el mejor transporte que se admite por servidor y cliente. Si ya sabe qué transporte que se va a utilizar, puede omitir este proceso de negociación especificando el modo de transporte cuando se llama a la `start` método.

**Código de cliente que especifica el método de transporte (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**Código de cliente que especifica el método de transporte (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

Como alternativa, puede especificar varios métodos de transporte en el orden en el que desea SignalR para intentarlo de ellos:

**Código de cliente que especifica un esquema de reserva de transporte personalizado (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Código de cliente que especifica un esquema de reserva de transporte personalizado (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Puede usar los siguientes valores para especificar el método de transporte:

- "webSockets"
- "SiempreFrame"
- "serverSentEvents"
- "longPolling"

Los ejemplos siguientes muestran cómo averiguar qué método de transporte está usándola una conexión.

**Código de cliente que se muestra el método de transporte utilizado por una conexión (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**Código de cliente que se muestra el método de transporte utilizado por una conexión (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

Para obtener información sobre cómo comprobar el método de transporte en el código de servidor, consulte [Guía de API de concentradores de ASP.NET SignalR - Server - cómo obtener información sobre el cliente de la propiedad de contexto](hubs-api-guide-server.md#contextproperty). Para obtener más información acerca de los transportes y reservas, consulte [Introducción a SignalR - transportes y reservas](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Cómo obtener a un proxy para una clase de base de datos central

Cada objeto de conexión que cree encapsula información sobre una conexión a un servicio de SignalR que contiene una o más clases de concentrador. Para comunicarse con una clase de base de datos central, utilice un objeto de proxy que crea usted mismo (si no está utilizando al proxy generado) o que se genera automáticamente.

En el cliente el nombre del proxy es una versión de grafía de camel del nombre de clase del concentrador. SignalR automáticamente realiza este cambio para que el código de JavaScript puede ajustarse a las convenciones de JavaScript.

**Clase de base de datos central en servidor**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**Obtener una referencia al proxy de cliente generado para el concentrador**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**Crear el proxy de cliente para la clase de base de datos central (sin proxy generado)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

Si decorar la clase de concentrador con un `HubName` de atributo, use el nombre exacto sin cambiar mayúsculas y minúsculas.

**Clase de base de datos central en el servidor con el atributo HubName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**Obtener una referencia al proxy de cliente generado para el concentrador**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**Crear el proxy de cliente para la clase de base de datos central (sin proxy generado)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Cómo definir métodos en el cliente que el servidor puede llamar a

Para definir un método que el servidor pueda llamar desde un concentrador, agregar un controlador de eventos para el proxy de concentrador mediante la propiedad `client` del proxy generado, o llame al metodo `on`,  si no está usando el proxy generado. Los parámetros pueden ser objetos complejos.

Agregue el controlador de eventos antes de llamar a la `start` método para establecer la conexión. (Si desea agregar controladores de eventos después de llamar a la `start` método, vea la nota de [cómo establecer una conexión](#establishconnection) anteriormente en este documento y utilizar la sintaxis mostrada para definir un método sin utilizar el proxy generado.)

Coincidencia de nombres de método distingue mayúsculas de minúsculas. Por ejemplo, `Clients.All.addContosoChatMessageToPage` se ejecutará en el servidor `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, o `addcontosochatmessagetopage` en el cliente.

**Definir el método de cliente (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**Manera alternativa para definir el método de cliente (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**Defina el método de cliente (sin el proxy generado, o cuando se agrega después de llamar al método de inicio)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**Código de servidor que se llama al método de cliente**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

Los ejemplos siguientes incluye un objeto complejo como un parámetro de método.

**Defina el método de cliente que toma un objeto complejo (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**Defina el método de cliente que toma un objeto complejo (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**Código de servidor que define el objeto complejo**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**Código de servidor que se llama al método de cliente mediante un objeto complejo**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Cómo llamar a métodos de servidor desde el cliente

Para llamar a un método de servidor desde el cliente, use el `server` propiedad del proxy generado o `invoke` método en el proxy de concentrador, si no está usando el proxy generado. El valor devuelto o los parámetros pueden ser objetos complejos.

Pasar de una versión de mayúscula y minúscula camel del nombre del método en el concentrador. SignalR automáticamente realiza este cambio para que el código de JavaScript puede ajustarse a las convenciones de JavaScript.

Los ejemplos siguientes muestran cómo llamar a un método de servidor que no tiene un valor devuelto y cómo llamar a un método de servidor que tiene un valor devuelto.

**Método de servidor con ningún atributo HubMethodName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**Código de servidor que define el objeto complejo que se pasa en un parámetro**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

Si decora el método de concentrador con un `HubMethodName` de atributo, use ese nombre sin cambiar mayúsculas y minúsculas.

**Método de servidor** con un atributo HubMethodName

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

Los ejemplos anteriores muestran cómo llamar a un método de servidor que no tiene ningún valor devuelto. Los ejemplos siguientes muestran cómo llamar a un método de servidor que tiene un valor devuelto.

**Código de servidor para un método que tiene un valor devuelto**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**La clase de existencias utilizada para la** valor devuelto

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

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

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**Controlar el evento connectionSlow (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

Para obtener más información, consulte [descripción y control de eventos de duración de conexión de SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Cómo controlar errores

El cliente de SignalR JavaScript proporciona un `error` eventos que puede agregar un controlador. También puede usar el método fail para agregar un controlador de errores que son el resultado de una invocación de método de servidor.

Si no habilita explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que SignalR devuelve después de un error contiene información básica sobre el error. Por ejemplo, si una llamada a `newContosoChatMessage` se produce un error, el mensaje de error en el objeto de error contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" enviar mensajes de error detallados a los clientes de producción no se recomienda por motivos de seguridad, pero si desea habilitar los mensajes de error detallados para solución de problemas, utilice el código siguiente en el servidor.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

En el ejemplo siguiente se muestra cómo agregar un controlador para el evento de error.

**Agregar un controlador de errores (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**Agregar un controlador de errores (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

En el ejemplo siguiente se muestra cómo controlar un error de una invocación de método.

**Identificador de error de una invocación de método (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**Identificador de error de una invocación de método (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Si se produce un error en una invocación de método, el `error` también se genera el evento, por lo que el código en el `error` controlador de método y en el `.fail` ejecutaría la devolución de llamada de método.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Cómo habilitar el registro de cliente

Para habilitar el registro de cliente en una conexión, establecer el `logging` propiedad en el objeto de conexión antes de llamar a la `start` método para establecer la conexión.

**Habilitar el registro (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**Habilitar el registro (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Para ver los registros, abrir las herramientas de desarrollador de su explorador y vaya a la pestaña de la consola. Para obtener un tutorial que muestra las instrucciones paso a paso y pantalla capturas que muestran cómo hacerlo, consulte [Server difusión con ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).
